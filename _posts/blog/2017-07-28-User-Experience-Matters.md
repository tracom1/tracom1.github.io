---
layout: post
title: "User Experience Matters"
excerpt: "That Means You Paypal"
categories: blog
date: 2017-07-28T05:00:00-06:00
---

<center><figure>
<img src="https://image.freepik.com/free-vector/user-experience-design-with-hands_23-2147543848.jpg">
<figcaption><a href="https://image.freepik.com/free-vector/user-experience-design-with-hands_23-2147543848.jpg">Created by Freepik</a></figcaption> 
</figure></center>

As I've spent more and more time in tech, I have started spending more and more time judging the user experience of different websites.  Since my company is predominantly based in retail, user experience (UX) is really important, and directly translates to earned income.

Lately, I've been having trouble with the UX of Paypal, which to me seems absolutely ridiculous because it's a mature company.  It just doesn't make sense to me. One thing I always have to keep in the back of my mind though is that things have a tendency to break in my presence.  I think it's for that reason that I went into development initially as an automated tester, I've been privy to situations that nobody else has ever experienced; often my husband or close friends will go "I've... never seen that before.  How did that happen?"

My trouble with Paypal has been in trying to get money out of Paypal and into my own personal account.  I have had no troubles paying people <i>from</i> Paypal.

In my first vain attempt to get money out of my account, I set up a bank account like normal.  I verified the account with the two tiny deposits.  Ok, no problem.

<center>
	<iframe src="https://giphy.com/embed/k39w535jFPYrK" width="480" height="271" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/how-i-met-your-mother-thumbs-up-neil-patrick-harris-k39w535jFPYrK">via GIPHY</a></p>
</center>

However, when I went to the web UI and attempted to transfer money INTO my account I ran into problems.  First is directed me to their "legacy" page.  I would enter in all relevant information and click <font color="cyan">Transfer</font>.  The page proceeded to start to load, and then... nothing.  So I clicked again.  I see the page start to load and nothing.  I open up inspection and watched as I clicked the button; no errors.

I figured then it was just an issue with their redirect on the POST, and went about my day.  A few days later, I noticed the money had not been transferred and tried again.  When I saw the same behavior I decided to capitulate and call their phone number.  It was then that they notified me that they do not accept transfers from PayPal into a purely online bank.

Which is fine.

Except I was never notified about this caveat.  Honestly, if you're going to have something like this, why not warn me when I add the account, or when I try to transfer money into that account?  Why make me call?  What kind of user experience is this?

They told me I needed to add a local bank in order to get the money out of my account, and that they would not waive the $1.50 charge to cut a check.  They also would not let me provide feedback to their tech department.

So, a few days later, I added a bank that has brick and mortar facilities.  I go through the entire process again.  A few days later, there are the two micro deposits, so I go online to validate the bank.

But my bank is no longer on my PayPal account.

<center><figure>
	<img src="http://images.clipartpanda.com/poof-clipart-poof-460x280.jpg">
</figure></center>

I try and add it again and get a generic error "Your request could not be completed at this time".  So once again, I call PayPal to try and figure out what's going on.  This time they tell me they received a message from my bank about not being able to find the account (which is ironic because I have the micro deposits in my account), and let's try again with the same information.  He also didn't understand why I was upset at them about an issue provided that it was from my bank.

But my issue was that they didn't tell me about the problem, not that there was one (although I admit, it was a small part).

It was then I just decided to eat the $1.50 for the check, as I had spent hours and hours trying to get my money, and my time is worth more than that.

So here's my problem with this entire situation -- these are easy fixes.  It's simple to have a modal window pop up and tell me that you won't allow transfers to occur to my bank, or to send me an email that there was a problem.  They obviously knew enough about the latter problem to <font color="cyan">remove</font> the account, so why not just trigger an email fire?

I feel like companies are not as interested in UX like they used to be.  If they figure that they have a service that people have to use, like PayPal, then UX doesn't really matter.  So what you end up with is very bad user experiences, and people frustrated with the service they get.  It seems to me like this is next generation Comcast -- people hate it but in a lot of ways it's the only viable option.

I think sometimes there are basic usability features that companies just ignore, disregarding a positive experience for the user.  PayPal is far from the only company guilty of this.  And I just don't understand it.  As a former tester, Paypal's situations are easy to catch, because in one case it is simply testing against your business rules.  (Maybe it's going full automation suite that's at fault)

Now as a catch-all, I certainly don't know a ton about UX; it's an incredibly complicated and not something I'm going to promise that I can design well.  I'll leave that to the experts.  But I am a user, and I know when I have a bad experience.  So let's all be a little more thoughtful about UX design when we're building a tool.  Or at least let me provide feedback.
