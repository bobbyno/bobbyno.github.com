---
layout: post
title: Getting Past the Initial Hurdle to TDD with Shubox
category: continuous integration
---

Sam Newman recently blogged that <a href="http://www.magpiebrain.com/2010/02/16/struggling-with-test-driven-clojure/" target="_blank">he was struggling with Test-Driven Clojure</a>:
<blockquote>"Stuart Halloway said during his Clojure talk at Qcon SF that despite being a TDD fan he finds it hard to TDD in a new language, and I get exactly what he means. A big part of it is that you’re getting to grips with the idioms, capabilities, libraries and tools associated with your new language – and a lack of this knowledge is going to impact on your ability to write good tests, let alone worry about implementing them."</blockquote>
I thought the same thing, so I packaged up a set of scripts in a gem called Shubox. Shubox provides the ability to generate a TDD sandbox with all the libs and scripts you need for an automated suite of regressions tests.  I have environments for Ruby, Java, and Clojure at the moment. The name comes from <a href="http://en.wikipedia.org/wiki/Shuhari" target="_blank">Shu Ha Ri</a> - when you find yourself in a novice Shu state with all or part of a language, I hope that Shubox will provide a training ground for you to experiment, learn, and eventually move to the next level.

All the documentation you need to get started is with the code on Github at <a href="http://github.com/bobbyno/shubox" target="_blank">http://github.com/bobbyno/shubox</a>. Shubox is built with RubiGen, the same generator framework in Rails. Adding support for a new environment really comes back to answering Sam's question about finding an idiomatic approach for TDD in each language. I plan on adding many more environments and would like to at least keep up with <a href="http://www.pragprog.com/wikis/wiki/SevenLanguages" target="_blank">Bruce Tate's Seven Language in Seven Weeks</a>. Patches for new language support or updates to the existing environments would be welcome.

Those of you who saw my presentation about Test-Driven Learning at <a href="http://scna.softwarecraftsmanship.org/schedule#bobby_norton" target="_blank">Software Craftsmanship North America</a> last summer know that I'm interested in using learning tests to better learn languages, patterns, idioms, etc. through repeated practice. I've found test-driven learning of this sort to be an excellent compliment to other forms of experimentation, such as <a href="http://codekata.pragprog.com/2007/01/kata_kumite_koa.html#more" target="_blank">katas</a> or small applications.

I've been using TDL to experiment with functional programming as of late: Shubox contains an example in Clojure from Stu Halloway's Programming Clojure. More on that soon.