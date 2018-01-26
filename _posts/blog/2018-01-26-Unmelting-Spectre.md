---
layout: post
title: "Unmelting Spectre - Part 2"
excerpt: "Fixes for Meltdown and Spectre"
categories: blog
date: 2018-01-26T02:00:00-06:00
---

<center><figure>
<img src="/images/meltdown.jpg">
</figure>
<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@sweeticecreamwedding?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="_blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Sweet Ice Cream Photography"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Sweet Ice Cream Photography</span></a>
</center>

Last week, I briefly explained the exploitations of the Meltdown and Spectre bug.  This week, I'll talk about the patches and some of the impact the fixes have had on the operating systems.

Patches have been rolled out for Meltdown, the easier of the two issues to exploit.  It involves an upgrade and fix to the kernel by introducing Kernel Page Table Isolation (KPTI).[^1]  KPTI separates the memory so that secure data cannot be loaded into the internal cache, where it can be reached.  This means that every time an applications asks the processor to do something on its behalf, there's an additional check for data security and access.  There were initial warnings about how it could slow down older processors, but the rollout has been more impactful and disastrous than that. [^2]

It appears to affect the processing power of EVERY computer, and not just the older ones.

<center>
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">The <a href="https://twitter.com/hashtag/Meltdown?src=hash&amp;ref_src=twsrc%5Etfw">#Meltdown</a> patch (presumably) being applied to the underlying AWS EC2 hypervisor on some of our production Kafka brokers [d2.xlarge]. Ranges from 5-20% relative CPU increase. Ooof. <a href="https://t.co/fXM0OzfdKx">pic.twitter.com/fXM0OzfdKx</a></p>&mdash; Ian Chan (@chanian) <a href="https://twitter.com/chanian/status/949457156071288833?ref_src=twsrc%5Etfw">January 6, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script></center>

Additionally, some of the patches caused random reboot issues on computers.  As fun as it is when work force patches your computer and it reboots, so you lose all your work, you really don't want that to happen in production.  

The patches have had so many problems, Linus Torvalds, the creator of Linux and Git, has been quoted as saying, "[t]he patches are COMPLETE AND UTTER GARBAGE. ... They do things that do not make sense." (emphasis his)  Windows machines have also experienced similar slowdowns for the patches.

It seems these slowdowns can be somewhat controlled by leveraging the PCID functionality on newer chips.[^3]  Basically, PCID prevents the processor from having to flush the entire buffer regularly when you switch between contexts (or between secure and non-secure memory locations).  It keeps IDs of the pages for user mode processes and kernel mode processes, and limits lookups only to particular contexts.  So instead of having to load the full context every time is switches from user to kernel mode and vice versa, and flushing the old context, it can simply look at that one table. 

Hooray, there is some good news out of this!  Wait -- turns out maybe not.  It looks like a lot of computers are not actually using PCID, because it wasn't deemed necessary before Meltdown, so many guest OS versions didn't have it.  So while the process exists, it's not really leveraged.

An additional problem for Meltdown and Spectre: more chips were impacted than originally stated.  Other companies, like AMD, who initially said they were not impacted by the problem, actually were affected by the problem.  The problem is ubiquitous, and reaches into pretty much every computer processor everywhere - Android, IOS, Windows, Linux.

Spectre is more difficult to mitigate.  Most of the academic papers on the vulnerability do not even address a full mitigation approach, so plans are still evolving.[^4] The explanations are highly technical, so if you are interested in specifics, I refer you to the references at the bottom of the page.  Spectre security is only partially covered by the aforementioned bios updates.  

You will also need to upgrade your browser[^5].  The <a href="https://chromereleases.googleblog.com/search/label/Stable%20updates">Chrome update</a> was released just yesterday, and <a href="https://www.mozilla.org/en-US/security/advisories/mfsa2018-01/">Firefox 57.0.4</a> has also been released.

If you are a developer, and you work in JavaScript, you will also need to evaluate your code for Spectre and see whether or not it contains code which can be exploited.[^6]  Unfortunately, this problem also means you'll likely also have to do some refactoring.

So those are all the technical problems and solutions currently available to "solving" Spectre and Meltdown.  But there are other impacts as well.

Perhaps the most interesting article I've read about Meltdown and Spectre, are the potential legal repercussions.[^7]  I am not sure if the argument is a good one, but the author suggests that due to these patches, the processors are no longer performing as advertised.  The processor is no longer holds the same value, and thus may be open to a few different types of claims: tort, contract, trespass to chattel.  Over the next few months, we could see some settlements as a result of this performance degradation, although I find it unlikely.  If it were just one set of chips impacted perhaps, but the hit to performance has been across the board.

The problem is not that 3.4GHz processor chips are working like 2.4GHz, but that we had an imperfect understanding of what a SECURE 3.4GHz processor looked like.  And so on down the line.  The problem is too systemic.

So update your computer if you haven't (provided you have a 64-bit chip.  And if you don't have a 64-bit chip, I'm surprised you read a tech blog).  And keep your ear turned towards the news, because I'm sure there will be more kernel updates to address these problems in the upcoming days.

<font color="cyan"><u>Resources</u></font>
[^1]: <a href="https://www.redhat.com/en/blog/what-are-meltdown-and-spectre-heres-what-you-need-know">RedHat Blog: What are Meltdown and Spectre? Here’s what you need to know.</a>
[^2]: <a href="https://www.wired.com/story/meltdown-spectre-patching-total-train-wreck/">Meltdown and Spectre Patching Has Been a Total Train Wreck</a>
[^3]: <a href="https://groups.google.com/forum/m/#!topic/mechanical-sympathy/L9mHTbeQLNU">PCID is now a critical performance/security feature on x86</a>
[^4]: <a href="https://medium.com/@mattklein123/meltdown-spectre-explained-6bc8634cc0c2">Meltdown and Spectre, Explained</a>
[^5]: <a href="https://blog.barkly.com/meltdown-spectre-patches-list-windows-update-help#linux-updates"> A Clear Guide to Meltdown and Spectre Patches</a>
[^6]: <a href="https://medium.com/@evanderkoogh/our-response-to-meltdown-spectre-850801f5d1b3">Our Response to Meltdown/Spectre</a>
[^7]: <a href="https://www.darkreading.com/vulnerabilities---threats/meltdown-spectre-patches-performance-and-my-neighbors-sports-car/a/d-id/1330863?_mc=sm_dr&hootPostID=6db5dc3798e74da750bd3870778d36f3">Meltdown, Spectre Patches, Performance & My Neighbor's Sports Car</a>
