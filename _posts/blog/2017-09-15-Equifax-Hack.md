---
layout: post
title: "Equifax Hack"
excerpt: "Where did it all go wrong"
categories: blog
date: 2017-09-15T05:00:00-06:00
---

<center><figure>
<img src="https://thumb7.shutterstock.com/display_pic_with_logo/784078/334355231/stock-photo-data-breach-level-to-maximum-modern-conceptual-meter-isolated-on-white-background-334355231.jpg">
<figcaption><a href="https://thumb7.shutterstock.com/display_pic_with_logo/784078/334355231/stock-photo-data-breach-level-to-maximum-modern-conceptual-meter-isolated-on-white-background-334355231.jpg">Source: Shutterstock</a></figcaption>
</figure></center>

I'm sure none of you have been living under a rock big enough to encapsulate and protect you from the news of the Equifax hack.  Over 143 million Americans data may have been compromised, and Equifax has not been handling the fallout very well.

Now, it's my opinion that every website is culpable and available to be hacked.  Obviously there are steps you can put in place to prevent data breaches, or at least help shut them down when in their midst.  There are also special ways that secure data should be encapsulated -- like PCI (Payment Card Industry) compliance for credit cards, or PII (Personally Identifiable Information) information for health records.  All data records should be encrypted, and preferably hashed as well.

I cannot comment on the strength of Equifax's security measures without knowing their back-end, or their employees ability to shut down known intruders, but given some of the news coming out, it definitely sounds like there are some steps that could have been taken.

-- <b>Keep up to date on their security vulnerabilities</b>:  In an <a href="https://arstechnica.com/information-technology/2017/09/massive-equifax-breach-caused-by-failure-to-patch-two-month-old-bug/">Ars Technica</a> article, it was mentioned that a security vulnerability AND a fix for an Apache tool had been brought to Equifax's attention back in March.  Patching this could have prevented the hack.  It can be difficult to stay on top of all the security patches required for all your tools -- especially if you bundle your own version of an upstream service.  However, you always pay attention to the most critical notifications, and it looks like this <a href="https://www.rapid7.com/db/vulnerabilities/apache-struts-cve-2017-5638">one</a> was certainly a big deal.  It "allows remote attackers to execute arbitrary commands via a #cmd= string in a crafted Content-Type HTTP header".

-- <b>Don't keep the breach a secret from your customers so long</b>: One of the big failings of Equifax, was keeping this exploitation under wraps from their customers for over 5 weeks.  Presumably, some of it was used by three individuals to sell their stock before it tanked (find that <a href="https://techcrunch.com/2017/09/07/equifax-managers-dumped-stock/">here</a>), but it sounds like that only required one day of notice.  So what were they doing for the remaining 4 weeks and 6 days?  Certainly not preparing for the onslaught their system would take after news got out, because their site has acted like healthcare.gov.  Customers have a right to know when their data has been compromised, as expeditiously as possible so they can resolve the problem, or take steps to monitor them.

-- <b>If you're going to delay, delay, delay, at least prepare</b>:  Equifax should have realized the onslaught and load their systems would be under as a result of this news.  But it seems like they either did not prepare, or insufficiently prepared for their systems to handle such requests.  I still cannot put a freeze on my Equifax account, and I'm essentially trying at 2 in the morning.  What are they doing back there?  Have they stood up additional servers, increased their consumer thread count for data processing?  Coming from a retail company, our stuff is constantly stress-tested, and we have servers spun up during time of anticipated extra load.  Other companies that are used more heavily during a 9-5 schedule spin up additional servers during that time.

-- <b>Respond better</b>: Equifax's main flaw throughout this entire situation has been their response.  They don't fix the vulnerability, then they take too long to acknowledge the issue, they don't have adequate resources to handle the problem, then they try and price gouge their customers for their mistake by charging them for things.  What a mess.

Unfortunately, I think Equifax has been able to get away with these sort of things because they don't provide an optional service.  No American can take their business elsewhere, because it's one of 3 major credit bureaus.  So their business won't be hurt by their lack of attentiveness.  Sure, their stock has gone down from ~$142.72 to $96.66, a major loss of market value, but they still have all the american customers in the world.  We are all stuck on this wild ride.