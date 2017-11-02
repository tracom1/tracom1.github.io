---
layout: post
title: "Keeping on Top of the Tech Game"
excerpt: "There are all these apps of which you've probably never heard"
categories: blog
date: 2017-11-03T02:00:00-06:00
---

<center>
<a data-flickr-embed="true"  href="https://www.flickr.com/photos/robinhutton/72741893/in/photolist-7qPCz-6PTv6L-azCMhC-WQjnuj-ntQMZy-TdkLms-bRXupv-Uk9Fqu-nzhUz3-nDR3Qc-WmqBkE-SGjLfY-rTsX4u-EiD3UR-6xbL8n-8jv1w3-78LVe8-aDN4J7-vpDAaY-iCU8QB-qbZc5m-e1PSgk-aqEpDb-9PakSt-rAWqkr-6WLp6v-X99BYF-hYaGrT-6hRXmD-4CkZMH-uP9f9-b9DpXc-nqkvLu-UpqD1M-4LduBh-nPiCUe-6tH6Bp-7s1dkC-b9DhHP-dHGpqH-4Syo6P-iHHUb3-6gCsv7-5pLyar-4Gz5te-r6tZZK-49eeGq-4mYnRY-rshVvw-9qZwoM" title="Do try to keep up son!"><img src="https://farm1.staticflickr.com/35/72741893_55449cd148.jpg" width="500" height="333" alt="Do try to keep up son!"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>
</center>

Since I've started my new job, I've been barraged by new tools and technologies that I have never used before.  Some of them, I have never even heard of before -- for example, the tool <a href="https://github.com/jorgebastida/gordon">gordon</a>, which is a deployment tool for managing AWS Lambda functions.  (I've also never heard of Airflow, Snowplow, rbenv, chef-librarian, druid, sbt)

Other tools I have heard definitely of before, but never used: Chef, Terraform, Docker, Kubernetes, Helm, Prometheus, AWS.  Before you message me, "you know Tracy, AWS is a whole host of services. not just a tool," I know this but the case still holds, I hadn't leveraged most of the sundry of things in it before.

It can be difficult to stay on the cutting edge, or even the informed edge of tools.  Sometimes you don't learn about something until it comes up in something else.  I've asked around my Ops friends, and most of them haven't heard about Gordon, although it seems like a cool way to manage deployments.  Incidentally, my coworker only found out about it because Snowplow implemented Kinesis Tee into their pipeline, and it leverages Gordon.

But, it can be a bit daunting, and I often feel overwhelmed.  I long for the good old days sometimes when all I had to know was Java 7 (actually, I think we were using Java 6 back then).  

<center><iframe src="https://giphy.com/embed/1FMaabePDEfgk" width="480" height="444" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/panic-stressed-1FMaabePDEfgk">via GIPHY</a></p></center>

So how do you keep on top of all the new tools coming out, and how are do you make sure that you aren't missing out on something that makes your life easier?  And how do you keep the stress levels down in your life?  Is this why so many people I know in Operations don't have any hair?

There are a few good tools you can leverage -- one of them being having good colleagues who can introduce you to new tools.  Right there, that limits how much research you have to do, and means you can relax a little bit more.

There are also a few good websites you can look at.  XebiaLabs, has a <a href="https://xebialabs.com/periodic-table-of-devops-tools/">periodic table</a> of tools that list a bunch of the most popular ones.  One of the benefits of their page is they list whether the tool is OpenSource.  Keep in mind that XebiaLabs have their own product, so they are definitely trying to sell you something.

Stackify also semi-regularly publishes a list of the <a href="https://stackify.com/top-devops-tools/">Top 50 Devops tools</a>. I've ended up on their website a few times looking at their lists.  However, they also seem to be a company with some sort of developer tool -- and given that I've never heard of them, maybe their lists shouldn't be trusted.

However, my favorite page to look at is the <a href="https://www.thoughtworks.com/radar/a-z">ThoughtWorks Technology Radar</a>.  They have a long litany of things that are semi-currently on the radar, and what they think of them.  They break things down into tools, techniques, platforms, and languages & frameworks.  You can poke around and look at different things.  

But I really enjoy their graphs, they are so visual, and their recommendations about what is in it's foundling stages versus what has reached mainstream adoption.  You can also do a search for your tools and see whether you should perhaps consider retiring it.  (Piece of advice, retire Java 6)

Keep in mind, the graph appears not to have been maintained or updated since March 2017.  As an example of the changing world of technology, the graph mentions Angular 2 being something new to assess, but Angular 5 came out <a href="https://blog.angular.io/version-5-0-0-of-angular-now-available-37e414935ced">Wednesday</a>.  

You can also look at previous radar charts to see where your tool may have been, because it seems they don't always keep everything on there from year to year -- obviously the radar graph would be too full with little dots of technologies.

So those are some of my best references.  I would urge caution though.  It's good to keep up with technology, but don't become mired down in the constantly changing world of tech.  If you have a tool that works for you, and hasn't reached end of life, and it easy to maintain -- keep it.  You don't need to go out and replace all your Puppet configuration management with Ansible or Terraform.  If you're constantly trying to keep up with the technology world, your actual work and systems are going to be a big mess of spaghetti.

I cannot overstate how important sanity is.  Stay sane!  Moderate your technological implementation.  You don't need to be bleeding edge.  But you shouldn't be fully legacy either.  It's important to find and strike a balance.

