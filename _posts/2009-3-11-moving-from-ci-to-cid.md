---
layout: post
title: Moving from CI to CID
category: continuous integration
---

About two years ago when I started working on <a href="http://www.thoughtworks.com/cruise">Cruise</a>, I set up a <a href="http://www.google.com/alerts">Google Alert</a> for "continuous deployment". At first, there was a lot of noise about troop movement in Iraq. Lately, however, things have become <a href="http://www.infoq.com/news/2009/03/Continuous-Deployment">much</a> <a href="http://timothyfitz.wordpress.com/2009/02/10/continuous-deployment-at-imvu-doing-the-impossible-fifty-times-a-day/">more</a> <a href="http://programmerjoe.com/2009/02/12/continuous-deployment-with-thick-clients/">relevant</a>.

I'm happy to see this happening: It validates a lot of the theory we built into Cruise. The Cruise team and others at ThoughtWorks are as interested in deployment as they are in keeping the builds green. Getting code to production after it passed through successive stages of automated and manual testing fits with a <a href="http://studios.thoughtworks.com/assets/2007/5/11/The-Deployment-Pipeline-by-Dave-Farley-2007.pdf">build pipeline</a> metaphor, so the pipeline became the core abstraction in Cruise.

The Cruise team uses Cruise as a CI and deployment server: There's a UAT environment used by the Cruise development team, a pre-production environment used by about 100 ThoughtWorks developers, and the public release that's sold to the world. Although the deployment was automated through Ant scripts, we left the <a href="http://studios.thoughtworks.com/cruise-continuous-integration/1.2/help/pipelines_page.html">gates</a> between the deployment stages set to manual to make things a bit more predictable for the development team and, more importantly, our early users. A post on the <a href="http://tech.groups.yahoo.com/group/extremeprogramming/message/137067">XP mailing list</a> I received at the beginning of my Cruise tenure presaged this outcome:
<blockquote>
<pre>On November 27, 2007,  

The biggest barrier to continuous deployment
is gaining the trust that what you're going to
deploy isn't going to destabilize the environment.
This takes time and effort.

John Roth</pre>
</blockquote>
Indeed. No developer wants the CI server to go down during the workday. We were using Cruise to build itself within a couple of iterations of starting the project, but of course the product took some time to stabilize. We rolled out our first production release two weeks after we started using Cruise in UAT. Once our user base increased, many felt that the risks of the production server being down due to a failed deployment outweighed the benefits of getting new features and bug fixes out as they became available: Users were perfectly content to wait until we'd fielded it in UAT, and even the dev team didn't necessarily want to use every check in; it was enough to know that we could trigger a deployment on demand, as we frequently did throughout an iteration.

Should a CI server be concerned with deployment? I think so. Why do you use a CI server in the first place? Immmediate feedback. Visibility. Reporting. The peace of mind that follows. CI also implies automation, and that means executable documentation, repeatability, reduction in errors.

Don't we also need all of these benefits in our deployment process? For many new to CI, the continuous integration process leaves off after testing, contributing to the <a href="http://www.pragprog.com/titles/twa/thoughtworks-anthology">"Last Mile" problem</a>. I've seen some cases where the supposed CI build doesn't have any tests! I hoped these were isolated incidents, but there are enough experience reports to the contrary that lead some to ask whether or not we need <a href="http://www.cmcrossroads.com/content/view/12735/120/">standards for CI</a>. Assuming you've got a working CI build, why not go to the next level and deploy using the CI server?

<a href="http://studios.thoughtworks.com/cruise-continuous-integration/deployment-pipelines">Cruise</a>, <a href="http://www.anthillpro.com/html/solutions/deployment-management.html">AnthillPro</a>, and <a href="http://open.controltier.com/">ControlTier</a> have the concept of deployment baked right in. There are doubtless other servers offering these features. Either we're all off on a tangent, or were on to something and Continuous Integration (CI) is becoming Continuous Integration &amp; Deployment (CID). Maybe in a few years, leaving deployment out of your CI solution will seem gauche as leaving out tests.

There's a lot more to say about how the challenge of continuous deployment affects the software lifecycle and how it fits with more established configuration management practices in CMMI and ITIL. Although I've since left ThoughtWorks, I'm working on a continuous deployment solution for my new employer, <a href="http://www.drwtrading.com">DRW Trading</a>. I'll continue to post more of my thoughts on CI and CD, and I'm looking forward to discussing more of these ideas with members of the community at <a href="http://citconf.com/msp2009/">CITCON North America</a> in April.