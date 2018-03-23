---
layout: post
title: "Dealing with Legacy Code"
excerpt: "Every Devs Worst Nightmare"
categories: blog
date: 2018-03-23T02:00:00-06:00
---


<center>
<figure>
<img src="/images/matrix.jpg">
<figcaption><a href="https://unsplash.com/photos/68ZlATaVYIo">Coming Out 19 Years Ago, The Matrix is Now, Legacy Code (Source: Markus Spiske)</a></figcaption>
</figure>
</center>

Sometimes you work on the bleeding edge of technology, playing with VR and elbows deep in node.js on github, and other times... you don't.  Sometimes you're handed a CVS/SVN repository written in COBOL (or Java, that seems to feel very legacy-ish to a bunch of developers out there) and you have to work with it.  

The first thing you want to do is rewrite that legacy code and refactor it into the latest new language.  Who wants to work in SVN?  But before you dive and have the application rewritten by lunch-time, there are some things you need to consider.


<font color="cyan">1. Understand the Application</font>

You can't rewrite an application before you understand what the application does -- and all the nuances that entails.  It can be easy to understand the primary purpose of an application, but there are often a lot of surrounding minutiae or business logic built into the code.  So try not to judge the code on first glance.  Often, not all these details can be gleaned on first glance.  You'll need to spend some time with your application.  Do a lot of research, and digging into the code, before you begin to rewrite.  I can recall one instance where some devs rewrote an application that for collecting taxes.  They considered everything, except what happened when their new decoupled service was unavailable; there were no fallback values in case of failure.  The first time that new service went unavailable, taxes weren't collected when they should have been because the fallback values weren't in there.

Start small with the code, by actually spending some time working in its native language.  Do some bug fixes.  Heck, write some automated tests (those can be in any language you want).  Writing automated tests will help position you in the future for your rework because you can verify behavior of your new code.

You can also start refactoring code by slowly breaking out pieces into small microservices in the new language.  Maybe you're changing languages totally.  Maybe you need to get off Java 5.  Chances are if your code is legacy, it's also monolithic.

<center>
<figure>
<img src="https://imgs.xkcd.com/comics/good_code.png">
<figcaption><a href="https://imgs.xkcd.com/comics/good_code.png">Source: xkcd</a></figcaption>
</figure>
</center>


<font color="cyan">2. Understand the Dependencies</font>

Before you rush to change that one app to golang, it's important to understand what other dependencies the app relies on.  What sort of databases does it rely on?  What other apps touch it?  Is there hardware that this application touches, or drives?

Sometimes the hardware is only compatible with certain code languages.  Medical systems in particular are prey to these problems.  They are often developed with specialized software, and anything that integrates with them must be tied to their restraints.  The cost of changing these dependencies can be expensive, or just not realistic.  Any effort to refactor your code could simply be a waste of time.

Spend some time understanding the entire application environment, it's going to be essential to your rewrite, and to understanding current implementation.

<font color="cyan">3. Understand the Corporate Culture</font>

One of the biggest hurdles to refactoring and updating code can be the corporate culture itself.  Maybe you are ready to upgrade the code, but the higher ups aren't going to support the time it takes to refactor.  Perhaps you want to change the code, but you're the only developer in the entire company that knows the particular language to which you want to change the application.  Maybe the culture itself is hostile to change -- perhaps the company is trying to sell or downsize.

It's important that you understand your work environment before you attempt to undertake a giant task like updating code. <u>Especially</u> on a mission critical application.  You cannot be the sole person in the company that can support an application.  If you're working with 200 other Pascal developers, and you want to switch the app to <a href="https://en.wikipedia.org/wiki/LOLCODE">lolcode</a>, chances are, you're just making more work for yourself.  Plus, you won't have anybody to review your code, which is essential in a well developed environment.

<font color="cyan">4. Understand the Constraints</font>

In addition to all the things mentioned above, you should be aware of any other constraints that might limit or hinder your ability to upgrade the code.  Maybe the application is simply in "maintenance mode", so there isn't time available to upgrade the code.  Maybe with all your other work, you simply don't have time to rewrite the code.  Perhaps your limited by your higher-ups, or your colleagues, or what your software integrates with.  Perhaps you are simply limited by scope.

I once worked on an integration testing suite that was enormous.  I desperately wanted to refactor the code, but I didn't have the time to build in all the functionality that encompassed the current system.

------

Before you rush headlong into changing that old code, take a step back and get a feel for the environment, the total environment.  Updating code can be a key and essential part to making it more manageable and easier to understand.  But it's not a simple step, and it shouldn't be undertaken lightly.  There don't seem to be many books out there, the only one I could find was <a href="https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052">Working Effectively With Legacy Code</a> from 2005.  So, if you have any other suggestions for updating legacy code, I'd love to hear them!