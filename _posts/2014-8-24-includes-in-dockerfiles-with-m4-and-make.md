---
layout: post
title: INCLUDES in Dockerfiles with m4 and make
category: devops
---

Say I work in a company with multiple teams all building Dockerfiles based on [Ubuntu 14.04](https://registry.hub.docker.com/_/ubuntu/). Some teams might want to include the Docker client in their containers along with a particular installation of Ruby:

<pre>
FROM ubuntu:trusty

ENV DEBIAN_FRONTEND noninteractive

INCLUDE "docker_latest"
INCLUDE "ruby_2_1_2"

# ...
</pre>

Some might want Java 8 and Ruby 1.9.3 without the docker client:

<pre>
FROM ubuntu:trusty

ENV DEBIAN_FRONTEND noninteractive

INCLUDE "ruby_1_9_3"
INCLUDE "java_8_sdk"

# ...
</pre>

If you know Docker, you know that these Dockerfiles don't exist. As of the time of this writing, Dockerfiles don't have an INCLUDE command. From the discussions in [an issue filed in May 2013](https://github.com/docker/docker/issues/735), there's no guarantee that support will be added anytime soon.

Enter m4. This quote from the [m4 manual](http://www.gnu.org/software/m4/manual/m4.html) sums up my own experience with the tool: "The m4 macro processor is widely available on all UNIXes, and has been standardized by POSIX. Usually, only a small percentage of users are aware of its existence. However, those who find it often become committed users."

We can use `m4` and `make` as the basis for a simple Dockerfile preprocessing solution. Start with `Dockerfile.m4`:

<pre>
FROM ubuntu:trusty

ENV DEBIAN_FRONTEND noninteractive

include(`ruby_2_1_2.m4')
include(`docker_latest.m4')
</pre>

A look at the contents of `ruby_2_1_2.m4` and `docker_latest.m4` explains why we might want to save these off to share them:

`ruby_2_1_2.m4`:

<pre>
RUN apt-get -y update
RUN apt-get -y -q install zlib1g-dev libssl-dev libreadline6-dev libyaml-dev
WORKDIR /tmp
RUN wget ftp://ftp.ruby-lang.org/pub/ruby/2.1/ruby-2.1.2.tar.bz2
RUN tar xjf ruby-2.1.2.tar.bz2
WORKDIR /tmp/ruby-2.1.2
RUN ./configure --prefix=/usr/local --disable-install-doc
RUN make
RUN echo "gem: --no-document" > /.gemrc
RUN make install

# Update basic gems
RUN gem install rubygems-update bundler
WORKDIR /
</pre>

`docker_latest.m4`:
<pre>
# Based on http://docs.docker.com/installation/ubuntulinux/#ubuntu-trusty-1404-lts-64-bit

# Ensure HTTPS transport is available to APT
RUN apt-get -y update && apt-get install -y apt-transport-https

# Add the repository to your APT sources
RUN echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list

# Then import the repository key
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

# Install docker
RUN apt-get -y update && apt-get install -y lxc-docker
</pre>

The call to m4 to emit our Dockerfile is simply: `m4 ./Dockerfile.m4 > Dockerfile`

## Automating with make

It would make for a clumsy workflow if we changed any of the m4 templates and forget to regenerate the Dockerfile before a build. Enter the venerable `make` to keep us honest.

`makefile`:

<pre>
lib = ./dockerfiles

dockerfile: $(lib)/*.m4
    m4 -I $(lib) $(lib)/Dockerfile.m4 > Dockerfile

build: dockerfile
    docker build --rm -t your/image .
</pre>

`build` is dependent on the `dockerfile` target, and `dockerfile` depends on our .m4 templates. Now we can simply run `make build` to generate a Dockerfile, if necessary, along with our image. The `makefile` also is a convenient home for interesting `docker run` targets that tend to accumulate.

Notice how m4 takes a `-I` argument for a list of files to include. This allows us to keep all of our templates in any directory we like. This could provide the means for a personal or team repository of snippets.

You can try this code out for yourself using the [dockerfile-include-spike project](https://github.com/bobbyno/dockerfile-include-spike) on Github.

## Benefits

* Dockerfile.m4 templates offer modularity, readability, and composability.

* Clean separation of concerns between file preprocessing for include files and Dockerfile parsing.

* Snippets could be easily shared among teammates or personal projects. Sharing amongst a larger community might be possible, but maintaining the context in which the snippet was shown to work might be challenging.

* Using include files provides the benefits of reuse without the overhead of a base image with far more than one needs or another copy and paste from a related Dockerfile in the [docker-library repo](https://github.com/docker-library).

* The simple include and macro syntax of m4 is very appealing. I'm planning to use this tool in future work when I need to embed source code in an article or a blog post, for example. Regardless of how this experiment turns out, I'm happy I found this tool.

## Risks

* If a team combines their Dockerfile snippets, what's to stop a shared resource from being changed, rendering a build unrepeatable? This can be mitigated by using snippets with explicit versions in the names. If the snippets are being pulled from a shared team library, consumers could include the git commit in the name of the snippet or in the name of a folder in a 'vendor' directory where shared snippets are stored.

* What if a snippet includes a FROM statement that would cause odd side-effects or fail the build? It would make sense to follow a convention where you don't put FROM statements in snippets. A social solution is probably easier than a technical one here.

## To include or not to include?

The purpose of this post was to consolidate a few of the pros and cons of using `include` semantics in Dockerfiles and to present a working method that I'm using to experiment. I hope the lessons learned will inform the implementation of an official INCLUDE in the Docker file if there is to be one at all.

Personally, If you feel the need to break your Dockerfile into included snippets, I would first ask if the container is doing too much. Do you really need Java, Ruby, and Python in the same container? Will you really use those snippets elsewhere? Maybe it would make sense to rethink your approach, breaking your application into multiple containers that each do one thing well. There might be perfectly suitable base images already in the [Docker Hub Registry](https://registry.hub.docker.com).

That said, [dev environment containers](http://blog.docker.com/2014/07/10-docker-tips-and-tricks-that-will-make-you-sing-a-whale-song-of-joy/) are likely to be more complex than a typical application component container. Experimenting with a few dev containers to hold all dependencies needed to develop a project is what prompted me to originally start looking for an include mechanism.

The theory that this approach might be useful at a team level is one yet to be tested. I look forward to hearing feedback about this idea from my colleagues at [Outpace](http://www.outpace.com), as well as from the Docker community. If you have some advice, you can find me on Twitter [@bobbynorton](https://twitter.com/bobbynorton), and I'll also start a thread on the [Docker User mailing list](https://groups.google.com/forum/#!forum/docker-user).

## Acknowledgments

Thanks to [Jens Finkhaeuser](https://github.com/jfinkhaeuser), who made some comments in the [aforementioned Docker issue](https://github.com/docker/docker/issues/735)) alluding to the idea of using m4 to implement include behavior in Dockerfiles. This comment served as the original inspiration for trying out this technique on a rainy Saturday afternoon in Chicago.

## Related issues and pull requests in Docker

The idea for an INCLUDE in the Dockerfile is neither new nor without debate:

* [Proposal: INCLUDE syntax for specifying multiple images per context](https://github.com/docker/docker/issues/7277)
* [please add INCLUDE <file> to dockerfile build so we can build more complex images](https://github.com/docker/docker/issues/735)
* [docker build: initial work on the include command](https://github.com/docker/docker/pull/2108)
* [735 include verb](https://github.com/docker/docker/pull/2337)
* [Proper parser for Dockerfile](https://github.com/docker/docker/pull/2266)
