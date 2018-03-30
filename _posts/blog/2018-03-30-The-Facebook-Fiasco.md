---
layout: post
title: "The Facebook Fiasco"
excerpt: "When Your Social Media App Talks Too Much"
categories: blog
date: 2018-03-30T02:00:00-06:00
---

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

<center>
<figure>
<img src="/images/facebook.jpg">
</figure>
<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@firmbee?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="_blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from William Iven"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">William Iven</span></a>
</center>


Unless you've been living under a rock for the past 9 days or so, I'm sure you're aware of the latest Facebook Fiasco, which finally supplants the original Facebook Fiasco: their IPO.  NASDAQ, you're off the hook!

The latest scandal involves the leak of millions of users data to a company, Cambridge Analytica, which was then purportedly used for nefarious purposes -- including helping Trump win the election (or at least advertising to people in particular ways to help Trump).  Politics aside, I'm not even sure about the efficacy of the Trump win argument, the bigger problem, asserted in <a href="https://arstechnica.com/tech-policy/2018/03/facebooks-cambridge-analytica-scandal-explained/"> this Ars Technica</a> article, is how private data is being handled by Facebook at all.

On top of the Cambridge Analytica complaint is the even more damning problem that Facebook knew about the problem as early as 2015, but neither alerted their users about it, nor followed through to make sure the company deleted the data.  They simply took their word that the data had been deleted.  That was the stance of Mark Zuckerberg, but it just feels fake; that smacks of incredible naiveté on the part of Facebook.

Since the article, other issues of FB security have come to light.  Indeed, they were warned about their security problems as <a href="https://techcrunch.com/2018/03/24/facebook-was-warned-about-app-permissions-in-2011/">early as 2011</a>.  A privacy advocate and lawyer, started a lawsuit alleging that Facebooks privacy permissions made it too easy for granting permissions to 3rd party applications made it far too easy to give them access to entire user history, which could then be used for any desired purpose.

Facebook also appears to have <a href="https://arstechnica.com/information-technology/2018/03/facebook-scraped-call-text-message-data-for-years-from-android-phones/">scraped data from individuals personal phones</a>, particularly Android, if you consented to allow the Messenger app to access your contacts.  There was quite a Twitterstorm of people who downloaded their FB data, only to find their entire call history listed.  

<div class="alert note">
  <span class="closebtn">&times;</span>  
  <strong>PRO-TIP:</strong> If you're interested in seeing your FB history, you can go to Settings --> General and there should be a link to "Download a copy of your Facebook Data".

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


While the people in charge of Facebook have issues a tepid apology for the Cambridge Analytica scandal, they refuse to apologize for something they insist should have been known by all the users beforehand; indeed it was included in the fine print when you consented to give them access to your phone contacts.

I'm saddened by the fact that things like my personal messages on FB may have been data mined, but I'm not entirely surprised.  In fact, I'm surprised more at how surprised people are that their online data has been used and transformed for targeting.  I mean, look at online advertising now.  I can look at a product on Amazon, and start seeing it in banners on other sites I go to as something I might be interested in.  We used a service to help sell our home, and I <i>still</i> see ads for it that show up in various sites.

In fact, if you want to be terrified, you can read an <a href="https://twitter.com/iamdylancurran/status/977559925680467968">expositive and thoughtful twitter thread</a> about all the things that people know about you.  It's safe to say that nothing you put on the internet remains a secret, particularly to advertisers.  So why the big kerfuffle over Facebook's scandal?  Why the hashtags to #deletefacebook and everybody deciding they want to jump over more fully to Twitter and Instagram, arguably not any less geared towards advertising as they both have sponsorship ads.

I feel the outrage comes from both the severity of the breaches and FB's lax approach to user security.  3rd party apps were apparently given "carte blanche" to all data, and there were no strictures in place at all to limit what they received.  Anybody could write a simple "How Smart Are You?" quiz app that's so ubiquitous and grab entire users data who logged in.

<div class="alert note">
  <span class="closebtn">&times;</span>  
  <strong>PRO-TIP:</strong> If you want to disable to remove certain applications access to Facebook, you can find that data in Settings-->Apps.  You can turn it off completely, or simply remove access to some of the apps to which you have previously granted permission.
</div>

Perhaps the most nefarious practice was how Facebook allowed 3rd party apps to access <a href="https://www.theguardian.com/news/2018/mar/20/facebook-data-cambridge-analytica-sandy-parakilas">data of friends of a person who used a third party app</a>.  That means that if you're friends with your Grandma, and she decides she wants to know which Disney Princess she is, that application could theoretically also mine your personal data.  Even if you already know without a test that you're a Rapunzel.

And quite frankly, that's how Facebook made most of their money.  Their data is what makes them valuable, and it's what has shot their stock price through the roof since they went public in 2012.  It's what makes any social media app worthwhile -- because connecting friends is not inherently a great money making scheme, particularly when it's free.

Data is the main commodity out there on the Internet, and from the largesse of FB, it had a lot of cachet.  Which meant it could make a lot of cash, eh.  <a href="https://www.washingtonpost.com/news/the-intersect/wp/2016/08/19/98-personal-data-points-that-facebook-uses-to-target-ads-to-you/?utm_term=.e6ccfcbca928">This Washington Post article</a> covers a lot of the key components important to a marketer.  And advertising dollars is the way a platform like FB is going to make it's money.

So while I agree with the sentiment that was Facebook allowed was unconscionable, and that they cared more about making money then their users or their security, I cannot say that I'm surprised.  Like a parent, I'm more disappointed.  And while a lot of my friends have deleted Facebook and moved on to other platforms, I'm going to keep it around.  I'd like to think I'm circumspect enough to not trust their "ads".  Plus there are some people I just cannot get in touch with any other way.  So my Facebook is staying, albeit with a bit more care towards what else can access it.  To be fair though, I never really enjoyed 3rd party integrations anywhere anyway.

Something like this is apparently going to change the ways of Facebook; they've already announced due to severe market share droppage and public pressure, that they are going to <a href="https://arstechnica.com/tech-policy/2018/03/facebook-will-soon-yank-third-party-ad-data-in-the-name-of-privacy/">limit third party ad data's access to information</a>.

My main suggestion is that you be smart about what you put on Facebook (or any platform), if you decide to keep it.  And for you to realize, that as much as Facebook knows (and sold) about you, Google probably knows 10 times more.