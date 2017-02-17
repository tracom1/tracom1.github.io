---
layout: post
title: "Tackling Tech's Trials"
excerpt: "Large Obstacles Keeping Tech Departments from Moving Forward"
categories: blog
date: 2017-02-17T07:40:00-07:00
---

More and more, tech has become a requisite important department within a company. Yet, in many cases it seems that it is still the red-headed step child.  Things are improving, but still many companies consider tech a necessary evil.  The result -- dev teams are understaffed, under funded, and under the ground.

<center>
<iframe src="//giphy.com/embed/ImIoV7yW9o8Cs" width="480" height="274" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/it-crowd-ImIoV7yW9o8Cs">via GIPHY</a></p>
</center>

Here’s my thoughts on some of the key challenges facing technology - both the external pressures and internal business dynamics.

<h2>EXTERNAL</h2>
There's a long litany of issues I could cover, but they are covered in some really good articles (see the end for some additional references), so I'll just briefly touch on a few of what I believe are the most important.

-- <b>Data Management</b>: Companies are storing everything these days, and as data grows exponentially, so too come issues of storage, data management and cost.  Will you be storing your data in-house or implementing an off-site solution?  What will you leverage your data to tell you?  What data needs to be backed up or is required to keep around for SOX/PCI compliance? There are a lot of questions that a company needs to answer.

-- <b>Security</b>: There are a lot of components making security difficult, from the increased amount of BYOD, to the rise of the IOT more quickly than security parameters can deal with them, to balancing security rules with innovation.  Security policies need to comply with all regulatory requirements as well.  The key here is to develop a good overall policy and then implement it.  Also, don't let your servers have access to anything beyond what it needs to perform it's task.  Some people suggest implementing policies around data rather than specific technologies.  With recent hacks on large infrastructure services like Dyn, the needs for appropriate security is only increasing.

<center><figure>
    <img src="/images/Security.jpg">
    <figcaption><a href="http://m.memegen.com/ceqkni.jpg">http://m.memegen.com/ceqkni.jpg</a></figcaption>
</figure></center>

-- <b>Complexity</b>: Systems are becoming more and more complex, and services are becoming more narrow in their focus.  An extension of the long-tail philosophy, applications and systems are focusing on one key component, rather than a monolith.  There are more and more integration points, and (different) services speaking to each other.  Within code itself, you also run into problems with backwards compatability (sometimes for very old versions of IE!).  All this results in your IT members needing to know and be experts with more and more systems, and the wide variety of options available to solve a solution. 

<h2>INTERNAL</h2>

-- <b>Tech Subjugated to Changing Priorities</b>: Coupled with this is they lack of equal rights at the table for roadmapping.  The result - applications that are not built for stability or robustness (**ahem**, or good garbage collection).  Even worse, these applications will sometimes be abandoned three months later. The key to resolving this problem; provide a culture from the top down that supports and makes tech part of the conversation, and give tech a seat at the table.  (cf. <a href="https://www.fastcompany.com/3067062/how-to-finally-solve-your-teams-most-annoying-tech-problems">Solve Your Teams Annoying Tech Problems</a>)

-- <b>Sales Promising Products Not Developed</b>:  Whenever you work for a company that has sales people, you will have sales people upselling customers features that do not currently exist. This problem is so prolific, it has even been covered on a <a href="http://softwareengineering.stackexchange.com/questions/197174/is-there-a-name-for-when-a-sales-team-irresponsibly-promises-non-existent-featur">software engineering stack exchange</a>! John Cutler calls this problem <a href="https://hackernoon.com/12-signs-youre-working-in-a-feature-factory-44a5b938d6a2#.ykmvrsdpc">'chasing upfront revenue'</a>.  Secondarily, you have the <font color="aqua">statistically significant</font> number of times sales then forgets to relay that new “feature” promise to tech teams.  Sometimes, even when they do remember to tell dev, there is so little time that priorities have to be dropped to secure this “promise”.  Focusing on core competencies, and current products should be an important component of your sales team.

<center><figure>
    <img src="/images/Dilbert.jpg">
</figure></center>

-- <b>Communication Issues between Dev and Business</b>:  Have you ever seen that <a href="https://www.youtube.com/watch?v=BKorP55Aqvg">YouTube video</a> about drawing 7 red lines all perpendicular, with some in green ink and some transparent?  That’s what I feel like conversation between dev and business is sometimes.  Too often there is not any communication between the two which results in assumptions, and mismatched tech/business requirements.  I have been in a situation where legal, business, and tech all interpreted a legal rule different ways, resulting in wildly variant expected vs actual behavior.  We need really good technical project managers who can straddle both worlds and understand the jargon of each.

-- <b>Lack of Communication between Dev and Ops</b>:  I have seen this happen before, whether it be with big projects on either side, shifts in staffing, email overload, or mismanaged expectations.  As an example, I worked for a company once where dev was reorged and application ownership got jumbled and shipped around.  They never told Ops who owns what app now, so any problems in production resulted in blindly guessing to whom we thought owned it, eventually becoming a big game of telephone.  Even worse, sometimes the prior team ignores the email, since they no longer own the application, causing Ops to be stuck trying to fix a production application that people refuse to claim.  This is the point to having a DevOps team, but it doesn't always integrate the two cleanly, as the team can fall on one side or the other.  I found some great suggestions <a href="http://www.cio.com/article/2405441/collaboration/how-to-improve-collaboration-with-development-and-operations.html">here</a>, so check it out.

-- <b>Messy integrations of code</b>: Messy integrations ties in with complexity mentioned above, but in this case I am specifically discussing in house development that causes bad code.  Due to other forces, there is not always enough time to write code "correctly," so you have to settle for getting it done, period.  Applications are abandoned halfway through development due to a priority shift.  Six months later, a new application is written which does the same thing as that now abandoned app.  Large corporations are not always available to upgrade theis systems as quickly as technology evolves, so new methodology has to be shoe-horned into old infrastructure.  Other items of note:  how is old code deprecated?  Does it forever live on in your code base, vast swaths of code never called on again in production?  Finally, does your company dedicate enough time to tech-debt? 

<h2>Final Thoughts</h2>

These are just a few of the key hurdles facing tech departments today.  Keep in mind, all these issues are coming from the perspective of a company who categorized as a tech company (e.g., they are a finance company, or a retailer, or service company) Tech companies may not experience these problems to the depth other companies do, because their core competency is itself technology.

What are some of the key problems you think exist in tech?  Do you have any solutions for fixing it?

<h2>Additional References</h2>
<a href="http://er.educause.edu/articles/2015/1/top-10-it-issues-2015-inflection-point">10 IT Issues 2015 Inflection Point</a><br>
<a href="http://www.inc.com/ken-lin/the-3-biggest-technical-challenges-facing-growing-companies.html">3 Biggest Technical Challenges Facing Growing Companies</a><br>
<a href="http://www.mrc-productivity.com/blog/2016/11/5-big-challenges-facing-cios-and-it-leaders-in-2017/">Big Challenges Facing CIOs and IT Leaders in 2017</a><br>
<a href="http://www.businessnewsdaily.com/6072-challenges-facing-it-departments.html">Challenges Facing IT Departments</a><br>
<a href="https://blog.financialforce.com/the-top-3-challenges-that-cios-of-technology-companies-are-facing-today/">Top 3 Challenges that CIOs of Technology Companies are Facing Today</a><br>
<a href="https://www.globalknowledge.com/us-en/content/articles/12-challenges-facing-it-professionals/">12 Challenges Facing IT Professionals</a><br>


