---
layout: post
title: "Do You Openstack?  My First Try Hasn't Gone So Well"
excerpt: "My first foray in the cloud, and what happens when you fail"
categories: blog
date: 2017-04-07T07:00:00-06:00
---

About two months ago now, I took a new role as a Cloud Engineer.  It was a great opportunity, particularly since I had no previous experience working "in the cloud".  With the cloud?  Yes.  ON the cloud?  Nope.

<center><figure>
<img src="https://i.imgflip.com/dugkb.jpg"><figcaption><a href="https://i.imgflip.com/dugkb.jpg">source: imgflip</a></figcaption>
</figure></center>

Before I started working in OPS, I never really considered the infrastructure behind the cloud.  I mean, most people think of it as similar to "serverless" management, but they don't own the hardware, but there is still hardware.  Since our company does on-prem cloud, we actually manage and own all our own infrastructure.

Have you ever worked on something that you feel like you just beat your head against a wall?  That's sort of how this project felt.  Add into the mix that I was still going on call for my last job about every 14 days, and it made it difficult to focus on the task, causing further fragmentation problems.

I got involved with the implementation of Openstack Mitaka, a release about one year back (like Ubuntu, Openstack releases 2 versions a year).  Openstack is an opensource solution for the cloud, and has some of the fun documentation concerns involved with open source projects.  On the whole, the documentation is fairly good, but I was interested in <a href="https://docs.openstack.org/mitaka/install-guide-rdo/ceilometer-install.html">ceilometer</a> (their metering application) and <a href="http://gnocchi.xyz/">gnocchi</a> (a fairly new way of storing metrics).

<i>Side Note: I'm not a fan of the name gnocchi.  Every time I tried to do research I kept getting results for potato dumplings.  Then I'd be hungry.</i>

I first attempted installing gnocchi in test, getting it up and running with statsd.  Because I wasn't integrated with Openstack's authentication infrastructure, Keystone, I used a no_auth method.  The no_auth method worked great.

Then I had to try to get it set up with actual Openstack and all it's moving parts (including auth).  I kept running into problems.  I couldn't get data to flow through gnocchi into ceilometer.  I couldn't get the status command to work; I only got 401s.  Every other gnocchi command worked.  I submitted my problem to the forum and they couldn't determine what was wrong with my configuration.  

The documentation lacked some essential steps -- for example, if you're using a certain version of gnocchi, you won't get the ceilometer stats you're used to. (This used to be in their docs, but was <a href="https://github.com/openstack/ceilometer/commit/caf47178af181e7a1ba025827a865a9ffe23b6a5">removed</a>)  It also failed to mention some changes that needed to be implemented in the pipeline.yaml file, and some additional lines that were required in the ceilometer.conf file to work correctly with keystone.  I spent a lot time googling and translating pages from Chinese. This resource in particular was helpful: <a href="http://www.cnblogs.com/multi-task/p/5553830.html">Ceilometer + Gnocchi + Aodh</a>.

So then I went to mongodb, the recommended base install for ceilometer.  But even then I ran into problems.  I struggled finding the documentation I needed -- it was in a myriad of different places.  I couldn't figure out why only some of my data was getting into mongo.  Where was the rest of it?  My pipeline.yaml file said it was all going in.

It took about 3 days of banging my head against a wall until my boss, who has much more experience with these kinds of things, found that they switched the default return size on the <font color="cyan"><i>ceilometer meter-list</i></font> command to only return 100 results.  That'll teach me.

<center><figure>
<img src="http://i1.kym-cdn.com/entries/icons/original/000/011/786/homer-simpson-doh.jpg">
</figure></center>

While I was struggling with what I thought was my mongodb problem, I went back to gnocchi.  I figured if nothing worked, might as well go with the original plan.

It didn't happen.  I had to call in the big guns (my boss).  He told me not to feel badly, because he fought with this for a while himself.  He eventually got it sending data (leveraging a bunch of information <a href="https://github.com/tigerlinux/tigerlinux-extra-recipes/blob/master/recipes/openstack/ceilometer-with-gnocchi-backend/RECIPE-ceilometer-with-gnocchi-backend.md">here</a>), but it hobbled our servers.  It tanked our 40CPU boxes by taking ALL OF THE CPUS.  The queues on RabbitMQ would get so backed up we'd be missing data points for hours on a metric.  We had to restrict the data we sent down to one metric.

It's kind of funny, because the entire reason for switching to gnocchi was to enable quicker access to data.  And yet we had this problem where we had to severely limit the data we were publishing at all.

This project for me was really tough because I wanted to be able to figure something out on my own, but invariably I just couldn't do it.  The documentation was too fragmented, my knowledge of Openstack in general too shallow.  I felt like a failure.  My first task in my new job and I couldn't complete ANY of it.  What did that mean for my future in the cloud?  Should I stick to the ground?

I think the lesson I need to take away from failures is that sometimes failure happens.  Sometimes you cannot figure things out on your own.  It's ok to ask for help, but make sure you've put in the work first.  And it's also important to realize when you're learning, and when you're an expert.  I'm definitely not an expert.  It's been a humbling experience for sure, but I know next time it will be easier.

I'm hoping I'll feel comfortable enough to contribute to the openstack documentation soon.  Because I CAN document.

