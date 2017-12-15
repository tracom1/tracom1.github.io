---
layout: post
title: "The Culture Where Everybody Else's Code is Crap"
excerpt: "Subjectivism rears its ugly head in development"
categories: blog
date: 2017-12-15T02:00:00-06:00
---

<center><figure>
<img src="/images/trash.jpg">
<figcaption><a href="https://unsplash.com/photos/YzSZN3qvHeo">Source: Gary Chan</a></figcaption>
</figure></center>

Sturgeon, a deeply pessimistic man, has a <a href="https://en.wikipedia.org/wiki/Sturgeon%27s_law">law</a>.  The law is simply this: "90% of everything is crap".  

It seems that we've taken that to heart, particularly in the area of software development.  With few notable exceptions, it's pretty commonplace to deride other people's code, and I often here exhortations of "this code is crap", when people have to make adjustments to other people's work.

This is a problem in software development.  The incredible hubris it requires for most people to determine the worth of other's code.

Being told your code is garbage it tough, and it is personal, and it can be very emotional.  I once maintained this testing infrastructure; I took over from somebody else who moved on to a different role. In his spare time, he would go through and re-write all the code I wrote -- without telling me.  Now, was my code bad?  It did what it was supposed and was documented.  But I'll never know if there were things I could have done better, because he never told me why he was rewriting it.  I would just learn that my offerings had been found wanting when I would do a pull of the code, and see what had been changed.  It wasn't even his job to look at the code any more.

Why do we have to treat and belittle and undermine people in this manner?  Why do we have to sit around and judge somebody's efforts as wanting?  A lot of times in Operations, the goal was to automate something, so the code is just a little script, a piece of fluff that becomes so integrated into the system it's suddenly mission critical to have it exist.  But it wasn't written in that manner, so why bemoan somebody's effort to make the job easier?

<u>Formal</u> code reviews are an important standard of learning, and a relevant need in any software engineering job.

Therein lies the difficulty, the tightrope.

Standard coding conventions should be used and applied in a person's oeuvre.  Code has to be logical.  It should be legible (meaning, understandable).  It should be documented.

<Side Note: Self-documenting is not a thing.  You can respond with comments about swagger, but it's still not a thing in the truest sense of the word.  /Side Note>

Finally, the code should be written "well".  This is where things get difficult.  Written well, meaning that it is:

 Maintainable <br>
 Dependable <br>
 Efficient <br>
 Usable[^1]

Sometimes code has to be re-written to be more efficient.  Sometimes, while it's cool to condense your code down from 8 lines into 1, it just defies the easy to read principle and makes it more complicated.

As software developers, we need to understand there are good and better ways of doing something.[^2]  I know I'm constantly learning new things, and most people I know look at code they wrote a few years ago and see how it could be better written now.  Be open to constructive criticism, because there is always a better or cleaner way to do something.  So try not to take things too personally.  You were hired to develop things (arguably code), so there must be some level of confidence in your skills. Else, why are you there?

That being said - also realize when that critique is overly nitpicky[^3], or if the developer is just trying to supplant your approach with his coding style. There are many ways of coding a solution, and just because your method is different, doesn't mean it's inherently bad.  Everybody else's code does not suck. 

And maybe, we as humans should stop being so overly critical of others.  We can be helpful without being judgmental.  We can help others grow their skills without derision.  So before calling somebody else's code crap, we take a deep breath and decide to not be a jerk instead.

[^1]: <a href="https://en.wikipedia.org/wiki/Best_coding_practices">Wikipedia: Best Coding Practices</a>
[^2]: <a href="https://opensource.com/article/17/5/30-best-practices-software-development-and-testing">Opensource.com: 30 Best Practices for Software Development and Testing</a>
[^3]: <a href="https://www.brandonsavage.net/the-pitfalls-of-code-review-and-how-to-fix-them/">Brandon Savage: The Pitfalls of Code Review and How to Fix Them</a>
