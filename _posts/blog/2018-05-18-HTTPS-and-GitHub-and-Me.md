---
layout: post
title: "HTTPS and GitHub and Me"
excerpt: "Securing Your Custom Domain GitHub Pages"
categories: blog
date: 2018-05-18T02:00:00-06:00
---



<center>
<figure>
<img src="/images/secure.jpg">
</figure>
<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@jeisblack?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="_blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Jason Blackeye"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Jason Blackeye</span></a>
</center>

There are a lot of different ways to host a website.  You can run it locally on a server.  You can buy Amazon servers.  You can use a service, like WordPress or SquareSpace and not worry about there the servers are.  If you are using a static site, you can actually leverage Amazon S3 buckets, which I think is neat.  My website is a static site that is hosted as a GitHub Page, which is then routed through a custom domain that I own.

I purchased the custom domain through a website that OFFERS to create your own pages for you, but I was too cheap for service.  So I went through this way.  Unfortunately, as a matter of the service I used + GitHub, I was unable to secure the site via HTTPS.

Until now.

Honestly, it seems kind of ridiculous that a site like mine even NEEDS to be served over HTTPS.  With the exception of an email address, there's nothing else to enter.  I take no information from you, and I give only slightly more than that.  However, Google marks you down if your site is not secure in their search engine, and just recently they announced their going to make your website look even more suspicious by a glaring red <a href="https://www.theverge.com/2018/5/17/17365362/google-chrome-secure-indicator-https">not secure</a> label in a few short months.

Luckily, GitHub came to my rescue by partnering with <a href="https://letsencrypt.org/">Let's Encrypt</a> to <a href="https://blog.github.com/2018-05-01-github-pages-custom-domains-https/">support HTTPS for custom domains</a>.  (as you may have noticed, my webpage is now a shiny https://techlady.ninja).

GitHub's documentation on the process is pretty good, but not located all in one place, so I figured I'd do a quick summation, based on a few of their help pages -- but put it all in one location.

<font color="cyan">STEP 1: DNS CONFIGURATION</font>  You need to update your DNS settings wherever you own your domain, as detailed <a href="https://help.github.com/articles/troubleshooting-custom-domains/#https-errors">here</a>.  If all you're using is a CNAME, you should be set.  If you're using an A record, you'll need to update it to point to one of the following IP addresses:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

For some reason, I have both a CNAME and an A record.  I'm pretty sure it's because I had a difficult time getting the routing to work correctly initially so I just tried everything and then it was working so I didn't want to touch it.  (Also, my domain provider is stupid, or I'm stupid so straight CNAME wasn't working).

<font color="cyan">STEP 2: CUSTOM DOMAIN UPDATING</font>  Remove and re-add your custom domain on GitHub, as detailed <a href="https://help.github.com/articles/adding-or-removing-a-custom-domain-for-your-github-pages-site/">here</a>.  I think you have to do this even if you just have a CNAME if your GitHub page is already in use.  It was the only way I could get it to trigger the creation of my certificate.  It's a pretty simple process, and can be found at https://github.com/USERNAME/USERNAME.github.io/settings .  

<center>
<figure>
<img src="/images/github.png">
</figure>
</center>

<font color="cyan">STEP 3: WAIT</font> Once you've done that, if you look at the "Enforce HTTPS" button just below that and you'll probably see a message along the lines of <i>"HTTPS cannot be enforced because the certificate has not been issued"</i>. I thought my certificate would be issued within 24 hours.  After 3 days, I bit the bullet and emailed GitHub support.  The guy responded REALLY quickly and helped to push things along.

<font color="cyan">STEP 4: UPDATE YOUR CODE</font>  Once I followed steps 1 and 2, I was able to see content at HTTPS!  Victory! Except... the formatting was all wrong.  It was not pretty things on HTTPS.  Because I serve a static site, I had to go into my <i>config.yml</i> file and update my url to point to the new, secure techlady.ninja.  If your website is more involved than mine, you may need to update some of your JS/CSS.

After checking in the change and waiting a few minutes, techlady.ninja looked all better!

<font color="cyan">STEP 5: ENFORCEMENT </font>  Enforce HTTPS, as detailed <a href="https://help.github.com/articles/securing-your-github-pages-site-with-https/">here</a>.  It's really just a simple check box back over on your settings side.  I find it's best not to enforce HTTPS until your site is looking like you want it to at that url.  Best not to scare the visitors away.

And that's it!  It's pretty simple, and I'm so grateful to GitHub for making this happen, because it helps me be lazy.  It also keeps GitHubs pages as a great solution for your individual websites, even with a custom domain.

Happy securing!