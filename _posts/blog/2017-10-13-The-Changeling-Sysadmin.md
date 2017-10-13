---
layout: post
title: "The Changeling SysAdmin"
excerpt: "Technology is evolving and so is the job"
categories: blog
date: 2017-10-13T02:00:00-06:00
---

<center><figure>
<img src="/images/changeling.png">
<figcaption><a href="https://pre00.deviantart.net/bab6/th/pre/i/2013/076/b/9/free_hugs_changeling_by_xyotic-d5fyeje.png">Source: DeviantArt.net</a></figcaption>
</figure></center>

There have been all kind of articles over the past 8 years about how the job of SysAdmin is dying.  Whether it's <a href="https://www.cio.com/article/2419896/virtualization/cloud-and-the-death-of-the-sysadmin.html">2010 CIO</a>, the <a href="https://news.ycombinator.com/item?id=2137494">Hacker News Chain</a>, or the <a href="https://developers.slashdot.org/story/16/04/08/188242/opinion-devops-is-dead">Slashdot thread</a>, many people seem to think that the role of System Administrator has passed the stage of usefulness.

Now, we all know that's not true, but the SysAdmin role is changing -- and being supplanted by the 'DevOps' or 'Site Reliability Engineer' titles.  And these jobs are fundamentally different than what they looked like 10 years ago, or even 5 years ago.

When I first left testing/development, it was because while I didn't mind coding, I really didn't enjoy doing it all the time.  I really enjoyed the time spent living on the command line, figuring things out, and not devoting my entire life to re-writing the same program over and over again for enhancements, etc.

However, the longer I have been in Operations, the more and more it's evolved to becoming something closer to the Dev work I left behind.  Simply reading <a href="https://jvns.ca">Julia Evans's totally awesome blogs</a> about Kubernetes and having to dig into the go code shows me enough of that. Other code-like tools have risen up over time; Cfengine first, and then Puppet, Chef, Ansible, all based on scripting languages, are standard tools for managing configuration.  And they make things easier for sure.  But other tools, like Docker and Kubernetes, require a bit more in depth knowledge of development.  And many other technicians have taken to writing their own Python/Ruby/Go tools to handle some of the operations tasks themselves.

Everything being offered right now "as a Service", equivocally meaning "software".  So Platform as a Service, Networking as a Service, or Infrastructure as a Service - while all these things are meant to make life easier, make customization reliant on software, developing.

Public and private cloud has also changed the way servers are managed.  They days of physical servers are gone -- instead you have AWS controlling your network through Route 53, using an Elastic Load Balancer, building your EC2 servers where your S3 storage lives.  Everything "lives" in one place, and you containerization has changed the need to log on and troubleshoot an errant or inefficient server (or indeed, even the want).

So too, have the ideas of specialization changed.  If you work in a big enough company sure, you can have a Network person, a Security person, a Database person, and then a SysAdmin.  But more and more these jobs are being encapsulated into just one or two people -- because orchestration tools can help the one Ops person "do it all", or can enable the developers to do the work themselves.

But therein lies the problem.  As Operations people become more and more generalized, as tools become more and more prolific, and as developers start taking the helm of managing operations themselves, is this where things begin to suffer?  Are we going to start sacrificing security or good decision for a quick time-to-market?  Or are these tools "simply better"?

It may be some time before we know for sure.  I don't have the 30 year tenure in the industry like <a href="https://www.itworld.com/article/2987198/operating-systems/30-years-a-sysadmin.html">this guy</a>.  There are definitely things that can make deployment easier, but I'm not so great with layers upon layers of abstraction.  Perhaps I will be left behind -- my cursor still on the command line.