---
layout: post
title: "Being On Call"
excerpt: "For masochists, but work sanctioned"
categories: blog
date: 2017-06-30T05:00:00-06:00
---

<center>
<img src="https://image.shutterstock.com/z/stock-photo-mobile-phones-background-pile-of-different-modern-smartphones-d-241404481.jpg">
<figcaption><a href="https://image.shutterstock.com/z/stock-photo-mobile-phones-background-pile-of-different-modern-smartphones-d-241404481.jpg">Shutterstock, Pile of Phones</a></figcaption>
</center>

<style>
.alert {
    padding: 20px;
    background-color: #f44336;
    color: black;
    opacity: 1;
    transition: opacity 0.6s;
    margin-bottom: 15px;
}

.alert.note {background-color: #66ccff;}

.closebtn {
    margin-left: 15px;
    color: white;
    font-weight: bold;
    float: right;
    font-size: 22px;
    line-height: 20px;
    cursor: pointer;
    transition: 0.3s;
}

.closebtn:hover {
    color: black;
}
</style>

<div class="alert note">
  <span class="closebtn">&times;</span>  
  <strong>NOTE:</strong> I'm incredibly tired and not sure this post makes sense.  But I think it's important to see that sometimes Dev/IT (and IT Operations in particular) can make you completely bone weary.  So please enjoy my random meanderings on what life is like on call.
</div>

<script>
var close = document.getElementsByClassName("closebtn");
var i;

for (i = 0; i < close.length; i++) {
    close[i].onclick = function(){
        var div = this.parentElement;
        div.style.opacity = "0";
        setTimeout(function(){ div.style.display = "none"; }, 600);
    }
}
</script>

I've been on call this past week.  Being on call is like playing roulette, but the best you can hope to win is a normal night of sleep.  The worst can be, well, pretty bad.  In my case it generally involves little to no sleep, a lot of bad television watched in the wee hours of the morning, a gradual decline in appearance over the week, and lots and lots of caffeine.

I recently switched teams, and with the switch, my on call procedure changed.  Previously when I was on duty, I was not expected to go into the office during that week.  The expectation for not being in the office was that coverage of our complex production systems generally took many more than 40 hours over the week that you were on call, even excluding normal business hours.  

Also, after we switched off at noon on Friday, I had the glorious freedom of not having to come into work until the following Tuesday.  The previous on call schedule seemed PRETTY sweet. You know, if you ignore the 5-8 alerts you get a night, or if you're really lucky and there's a site issue, the 435 you get in a span of 2 minutes.

Since transitioning to my new team, I have had to get used to a new on call paradigm.  I'm expected to go into the office now!  Like a regular plebian!  And I'm expected to handle all emergency/maintenance duties after hours, after being product.  I no longer get time off after I go off-call either, beyond the normal weekend.

The main rationale for the paradigm shift is that my new team does not get nearly as many alerts as my previous team.  My first few on calls were relatively idyllic -- one or two alerts for the entire week.  However, as many people who handle on call rotations are aware, the dice were about about to be cast against me.

This past week has been really tough.  I have been up every night.  Monday I was up until 5:30AM; Tuesday until 3:30; Wednesday I worked from 9AM-12:00AM Thursday, and tonight there's another maintenance, so who knows what will happen.  The weekend was blessedly quiet.  I've been subsisting at work primarily on sugar and caffeine, mostly to stay awake.  This is what I look like now:

<center>
<img src="https://s-media-cache-ak0.pinimg.com/736x/14/cd/26/14cd26392c5878ddf08c50c3df2b9c73--hilarious-stuff-funny-shit.jpg">
<figcaption><a href="https://s-media-cache-ak0.pinimg.com/736x/14/cd/26/14cd26392c5878ddf08c50c3df2b9c73--hilarious-stuff-funny-shit.jpg">Spongebob on Day 6 of On-Call</a></figcaption>
</center>

I've decided that being on standby is mostly for masochists; only those with some sort of self-hatred could participate in on call on a regular basis.  It's equal parts exhilarating and terrifying: I love being able to solve something, but am completely panic-stricken that there will be a major problem I'm unable to resolve quickly enough and will ruin our income.  It seems to be a fear that's pervasive in the industry; there is always a fear of being the point of contact, the go-to person if there's an issue.  There's almost no way to know everything, so you will invariably come across the situation where you call somebody at 2:30 am for assistance.  Being on call is a big lesson in knowing your (technical knowledge) limits.

One of my favorite things about going off-call is being able to set my phone down and walk into a different room, or even building without having to tote it, my laptop, a mifi, a change of clothes, and emergency food.  We use a tool called OpGenie, which has about a million integrations and uses data on your phone.  I highly recommend it if you're going to use an on call tool.

And I cannot wait until noon on Friday when I can go back to ignoring it again.

