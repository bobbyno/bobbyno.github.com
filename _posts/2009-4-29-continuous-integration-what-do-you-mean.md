---
layout: post
title: Continuous Integration? What do you mean?
category: continuous integration
---

"You're going to deploy every change to production? Is that every time you save a file? How about every keystroke?"

The downtime and confusion that would ensue could hardly be considered agile.

Of course, this isn't what's intended by the terms 'continuous integration' or 'continuous deployment'. Continual means 'frequent, repeating at intervals' and continuous means 'going on without pause or interruption'. So shouldn't we be talking about 'continual integration'?
<blockquote>"Continuous Integration increases your opportunities for feedback. Through it, you learn the state of the project several times a day. CI can be used to reduce the time between when a defect is introduced and when it is fixed, thus improving overall software quality." - <a href="http://www.amazon.com/Continuous-Integration-Improving-Addison-Wesley-Signature/dp/0321336380%3FSubscriptionId%3D18F0HAA4KWCRBW7SEZG2%26tag%3Dws%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D0321336380" target="_blank">Continuous Integration: Improving Software Quality and Reducing Risk</a></blockquote>
As agile developers, we choose to build and deploy as frequently as we can in order to maximize feedback and the opportunities for learning about the state of a system that is otherwise obfuscated. An interval as often as every check in seems to be a reasonable compromise between frequent feedback and absurdity.

The point of the term 'continuous' isn't to be linguistically precise: It's to act as a challenge to improve our process by automating as many of our release management tasks as is practical. It's about a commitment you make with your team to keep the build green and to fix broken windows as soon as they appear. Is "post check-in integration" going to change your team? Is "pretty frequent deployment" going to revolutionize your organization? Somehow those terms don't quite have the same call to action to them, do they?

The benefits of continuous integration have been <a href="http://www.amazon.com/Continuous-Integration-Improving-Addison-Wesley-Signature/dp/0321336380%3FSubscriptionId%3D18F0HAA4KWCRBW7SEZG2%26tag%3Dws%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D0321336380" target="_blank">well-documented</a>. Applying the same automation challenge to deployment is relatively new, but it makes sense. As continuous integration breaks out of it's agile home ground and goes mainstream, more people are realizing that they're already producing their release candidates as part of the build, so why not actually deploy the same way, using nothing more complicated than scripts and a CI server?

The why's and why not's of continuous deployment took up an hour when I facilitated a discussion with about 50 participants at <a href="http://citconf.com/msp2009/" target="_blank">CITCON NA 2009</a> this weekend; I'll be posting the results of that discussion soon.