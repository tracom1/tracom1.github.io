---
layout: post
title: "Melting Down Spectre - Part 1"
excerpt: "A brief history and explanation of the bugs that rocked the kernel"
categories: blog
date: 2018-01-12T02:00:00-06:00
---

<center><figure>
<img src="/images/fire.jpg">
</figure>
<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@ripato?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="_blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Ricardo Gomez Angel"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Ricardo Gomez Angel</span></a>
</center>

Unless you've been living under a tech rock the last few weeks, which I have been and I _still_ heard about them, you've probably heard about the Meltdown and Spectre kernel bugs that came to prominence in the beginning of January.  I've been following them a little in the news, and I want to discuss a little about what they are, and how they work -- as far as I understand it.  Please know that I'm not a security expert, nor a Linux kernel expert, so my knowledge may be incomplete or naive.  I'll include some refereneces at the bottom for those who want further reading about the issues.

In this blog, I'll cover some basic background information as well as an explanation of the vulnerabilities.  Next week, I'll talk about reactions to the news, mitigations of the attacks, and next steps.

<h2>Background</h2>

Perhaps the most fascinating thing about the exploitations is one of the discoverers: a 22 year old[^1] by the name of Jann Horn, who works at Google's Project Zero.  While he was working alone, reading through the thousands and thousands of pages of Intel Corp. processor manuals (which I didn't even know were available for reading), when he spotted an interesting inconsistency in the way chips handle speculative execution.  At the same time, a few other groups around the world were looking into similar vulnerabilities.  After about 6 months of research, they all came forward to announce these kernel vulnerabilities in the beginning of January.

Speculative execution[^2] is something a computer does to improve it's performance.  A computer will make some educated guesses (using predictive analysis), about things that may need to be done in a process before it actually knows from the process itself that it needs to be done.  Because the processes can run in parallel, it can have the result of this guesswork by the time the process has moved forward enough to require it, saving time down the line.

Let's use a simple example.  Let's say I want to do the laundry.  Now typically, doing laundry is a linear process -- clothes go in washer, they wash, [clothes go in dryer], they dry, human folds and puts them away (or tosses them somewhere if they are a slob).  Here in Germany, dryers aren't very common, and most people use a drying rack when they are doing laundry.  So the process of the [cloths go in dryer] is replaced with the following: set up the drying rack, clothes go on drying rack.

While the process is still MOSTLY linear, I can speculatively execute one process beforehand -- setting up the drying rack.  I can do that while the clothes are being washed.  That way, when that process is done, I can go right to putting them on the rack.

However, let's say I'm back in America and I set up a drying rack, but then remember that I have a dryer.  In that case, I've made an error in execution.  The drying rack isn't needed -- and the result of that process gets tossed, as it goes straight to putting clothes in dryer.

Speculative execution is done to speed up the computer and make things run more quickly, rather than in the standard linear fashion they used to work.  It's been very good for processor speed and performing tasks and taking advantage of the memory a computer has.

Everybody heralding speculative execution as the next step in computer processing, and the world was great for 10-15 years.  But then a dark day.  It turns out, speculative execution is vulnerable in a number of ways to kernel side channel attacks.[^3]  

Variant 1: bounds check bypass[^4]<br>
Variant 2: branch target injection[^5]<br>
Variant 3: rogue data cache load[^6]<br>

All three are vulnerable to exploitation by hackers, or black hats.

<center><figure>
<img src="/images/black-hat.png">
</figure>
</center>

<h2>Brief Explanation of the Variants</h2>

The first two variants are known collectively as 'Spectre' and the last is known as 'Meltdown'.  Meltdown is easier to exploit than Spectre, but they both leverage a similar approach to attack.  I'll cover each one briefly so you can understand how they work.  All of my explanations and examples are taken from a few main sources: the project zero post about the vulnerabilities they found[^7], the papers published about Spectre[^8] and Meltdown[^9], and a medium article explaining them[^10].  A semi-technical explanation can be found <a href="https://medium.com/cloudflare-blog/an-explanation-of-the-meltdown-spectre-bugs-for-a-non-technical-audience-49aee7d22cc6">here</a>.

<font color="cyan"><i>Bounds Check Bypass</i></font>

Consider the following code: 

```
if (x < array1_size) {
  y = array2[array1[x] * 256];
}
```

Ok, let's assume that this is the part of the code that's going to be vulnerable.  The computer don't currently know what array1_size is, but we (the black hats) know the value of x.  The processor is going to speculate that x is less than array1_size, and speculatively execute the statement AS IF it were true, assigning a value to y.  This execution is called an out-of-bounds read, and if the first line later turns out to be false, can be rolled back and tossed away.

However, it turns out that this can be exploited by an attacker.  During this period of speculation, the attacker can take this value of y and force the computer down a path and re-steer the code to determine the actual value of array1[x] in our example above.

However, to be actually vulnerable in this way, you must have this actually in your code, or there must be an interpreter that can inject this code pattern into the processes.

This is difficult because in order to take advantage of this exploitation, the kernel has to be forced to take a certain line of speculation, which is calcuated internally.

<font color="cyan"><i>Branch Target Injection</i></font>

This vulnerability relies on <a href="https://en.wikipedia.org/wiki/Indirect_branch">indirect branching</a> within the kernel's assembly language.  (The Google Zero post specifically references a virtual machine, but the other paper does not, so I'm unsure if it's relegated to just virtual machines or not, my guess is no.)  Indirect branching instructions have the ability to jump to more than two possible target locations, meaning that the location of the branch is not known by the kernel UNTIL it reaches that point.  While the kernel is waiting to get to the point of decision, the black hat can infiltrate your computer and force the address of the branch so that the computer will speculatively execute a branch of the black hat's choosing until the processor has caught up enough to the actual point of the execution.  This can result in a lot of information stored in the cache of your computer that the black hat can leverage.

It should be noted here that this process of indirect branching is by decision, and not necessarily a flaw.  The idea that it could be exploited in this way suggests that we need to reconsider how the kernel processes things.

<font color="cyan"><i>Rogue Data Cache Load</i></font>

This Meltdown exploitation is markedly easier to leverage than Spectre, and is probably a bigger deal.  It relies on memory isolation -- the fact that all memory allocated to one process can only be used by that process, and can't see where or what memory is being used by something else.  This keeps things like Angry Birds from stealing the memory used by your operating system.  The kernel manages this and keeps a ledger of all the memory being used by which applications.

Meltdown also relies on <a href="https://en.wikipedia.org/wiki/Out-of-order_execution">out-of-order operation</a>, another standard behavior of computers.  Basically the computer will execute tasks based on the availability of the data, and the idle time of a processor.  The goal, like speculative execution, is to mitigate idle time of the computer, and speed up processing.  It allows instructions to run in parallel (in some cases leveraging speculative execution).

The vulnerability allows unprivileged processes to read from kernel memory, and learn information about ALL other processes on the system -- something that should not be allowed due to memory isolation.

Because of out-of-order execution, sometimes things execute that shouldn't.  In the below example (taken from the Meltdown white paper), the exception should prohibit the access, but out-of-order execution could cause the access function to be called _before_ the exception is raised, and could have the information stored in the memory cache, ready to be stolen.

```
raise_exception();
access(probe_array[data * 4096]);
```

This access function is what is termed a _transient instruction_.  By forcing a transient instruction on some sort of secret that the black hats want leaked, creates a side channel.  This secret can be leaked back to the black hat.  The black hat can use a simple probe array in memory, referencing the secret.  As the probe array iterating through various locations, the black hat can determine where it is indexed in memory.  When it gets a fast response, it knows that's where the information is located.  The black hat can then receive the secret, and also the entire physical memory of the target machine.


<font color="cyan"><u>Resources</u></font>
[^1]: <a href="https://www.bloomberg.com/news/articles/2018-01-17/how-a-22-year-old-discovered-the-worst-chip-flaws-in-history">How a 22 Year Old Discovered the Worst Chip Flaw in History</a>
[^2]:<a href="https://www.extremetech.com/computing/261792-what-is-speculative-execution">What is Speculative Execution?</a>
[^3]: <a href="https://access.redhat.com/security/vulnerabilities/speculativeexecution">RedHat: Kernel Side Channel Attacks</a>
[^4]: <a href="https://access.redhat.com/security/cve/cve-2017-5753">RedHat: CVE-2017-5753</a>
[^5]: <a href="https://access.redhat.com/security/cve/cve-2017-5715">RedHat: CVE-2017-5715</a>
[^6]: <a href="https://access.redhat.com/security/cve/cve-2017-5754">RedHat: CVE-2017-5754</a>
[^7]: <a href="https://googleprojectzero.blogspot.de/2018/01/reading-privileged-memory-with-side.html">Project Zero: Reading Privileged Memory With a Side-Channel</a>
[^8]: <a href="https://spectreattack.com/spectre.pdf">Spectre Attacks: Exploiting Speculative Execution</a>
[^9]: <a href="https://meltdownattack.com/meltdown.pdf">Meltdown</a>
[^10]: <a href="https://medium.com/@mattklein123/meltdown-spectre-explained-6bc8634cc0c2">Meltdown and Spectre Explained</a>