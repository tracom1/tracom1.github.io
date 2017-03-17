---
layout: post
title: "HTTP2: Better, Faster, Stronger"
excerpt: "An Introduction to the Latest Protocol"
categories: blog
date: 2017-03-17T07:00:00-07:00
---

Some of you have probably heard about the HTTP2 Protocol that was embraced over two years ago now, and you might already be intimately familiar.  I have only recently begun learning about the protocol when running across the implementation by <a href="https://http2.akamai.com/">akamai</a> (Akamai is a content delivery provider for a lot of the media that exists on the web).

So what does HTTP provide anyway besides the beginning part of our website url, and how does this next generation improve upon the former?  

NOTE: I'm going to be leveraging a lot of information from multiple websites.  To make the post cleaner, I will just be posting all sources at the bottom of this article, and have no in-text citations.

<h2>Background </h2>

This background is a high level overview for people who have never studied or cared to study about how it works.

HTTP stands for 'Hypertext Transfer Protocol' and was designed back in 1989 by Tim Berners Lee.  The current gold standard of HTTP1.1 was introduced in 1997 and then later ratified, adopted, and implemented onto websites since 1999.

HTTP is comprised of 3 main parts - Header, Payload, and Footer.  The protocol is basically a mail delivery system for the internet, the USPS that doesn't take Sundays off.  The content (Payload) is wrapped or enveloped in the Header.  All instructions for delivery are contained in the Footer.  

<center><figure>
   <img src="http://s.hswstatic.com/gif/question525-packet-250x150.gif">
   <figcaption><a href="http://s.hswstatic.com/gif/question525-packet-250x150.gif">Example Breakdown of Information Sent in HTTP Request</a></figcaption>
</figure></center>

HTTP allows for communication across networks, by establishing a TCP connection (meaning all communication requires an ACK back from the destination) across servers, and communicating salient information back and forth while connected.

HTTP leverages TCP connections mostly on a one-off basis; each TCP connection usually has only one request outstanding.  So if a site needed to make multiple calls, it would open multiple TCP connections.  This could create a concurrency issue of parallel requests on the server, a major reason that people shard, concatenate, data inlining, sprites, and cookies exist today.

HTTP was the first REST API, containing methods like GET and POST in it's first iteration.  Http1.1 added the PUT, OPTIONS, DELETE, TRACE, and CONNECT that we have all come to know and love (potentially loathe) today.

In summation, HTTP works basically like this:

<center><figure>
    <img src="https://ruslanspivak.com/lsbaws-part1/LSBAWS_HTTP_request_response.png">
    <figcaption><a href="https://ruslanspivak.com/lsbaws-part1">HTTP For the Win, on 'Let's Build a WebServer', © 2016 Ruslan Spivak</a></figcaption>
</figure></center>

<h2>How HTTP2 Works and Improves HTTP1.1</h2>

HTTP was designed in a time when most websites were text-based, before the era of touch screens and instagram.  As a result, page loads were both much less labor intensive and more siloed than they are today.  Today's internet cannot subsist on single threaded TCP connections to load websites that contain ever more resources.

HTTP2 has introduced the idea of "streams," meaning more complex two-way streams between client and server.  Instead of the connection closing and reopening with every individual request, it keeps the connection between client and server open during the entire session.  The protocol is multi-plexed, allowing multiple messages to be in flight along the same connection at time.  Multiplexing can even intermingle messages with each other along the wire.

Think of it this way.  Instead of having your food delivered piecemeal, it comes all at the same time, increasing efficiencies.

<p><center><a href='https://kinsta.com/learn/what-is-http2/'><img src='https://kinsta.com/wp-content/themes/kinsta/images/learn/what-is-http2/http1_vs_http2_2_both.png' alt='HTTP2 vs HTTP1' width='760' border='0' /></a></center></p>

The protocol is binary, making the data more efficient to parse.  Previous versions of HTTP were based in text, which as we know is not the language of the computers.  Implementation in binary requires much less computing power and processing time to translate into binary.

HTTP2 also optimizes their headers.  Instead of requiring the same header information over and over again in a single session, HTTP2 drops all redundant transport information that doesn't need to be repeated.

Finally, HTTP2 also implements something called server push. This allows the protocol to simply push messages to the client that it believes it needs in the cache, irrespective of whether the client has requested it.  Not all 3rd parties support Server Push, so it seems to be the least adopted method of HTTP2

<h2>How To Implement it</h2>

Here's my down and dirty explanation of HTTP2 implementation.  ORRRRRR, you can have somebody else host part or all of your site, and just flip a switch.  If you choose option 2, you can skip this section.

<b>CAVEAT</b>: First, it's worth nothing that if you want to implement HTTP2 on your site, it has to be fully HTTPS (as of right now, it would not work on my site, future site improvements to come!).

<b>WEBSERVER SUPPORT</b>: Almost all webservers support HTTP2, like Apache, nginx, IIS.  To check whether you have a version that supports http2, a full list can be found <a href="https://github.com/http2/http2-spec/wiki/Implementations">here</a>, but at this point there are only a few that do not.  [Side note: Haskell is not listed in the above list, but libraries for it can be found <a href="https://hackage.haskell.org/package/http2">here</a>]

<b>SERVER CHECKING</b>: You can verify whether or not your server supports HTTP2 by running the following:

```bash
echo | openssl s_client -alpn h2 -connect google.com:443 | grep ALPN
ALPN protocol: h2
DONE
```

<b>CLIENT SIDE MINIMUM REQUIREMENTS</b>: Mimimum versions required on the client side to leverage all HTTP2 goodness:
-- Chrome 40+
-- Firefox 36+
-- IE 11+
-- Safari 9+
-- Opera 21+

<b>QUICK START AIDES</b>: Please refer to both <a href="https://www.nginx.com/blog/nginx-1-9-5/">Nginx's blog</a> and <a href="https://httpd.apache.org/docs/2.4/howto/http2.html">Apache's HowTo</a>.

<b>DEBUGGING</b>: A recommended tool for debugging is WireShark, which I'm just starting to learn about.  Because the protocol is binary instead of text, telnet won't give you all the answers you're looking for.  There's some setup required to get WireShark configured to use HTTP2, so I recommend looking at <a href="https://blog.newrelic.com/2016/02/17/http2-production/">Clay Smith's blog</a> about it.  While a year ago, most of the information there is still relevant and incredibly useful.

<h2>Final Thoughts</h2>
Is anybody else going to miss the days of "Http/1.1 Service Unavailable" errors though?  Http/2 Service Unavailable just doesn't have the same ring to it.  Maybe we can just replace it with this -
<center><figure>
    <img src="http://cdn2.holytaco.com/wp-content/uploads/images/2009/12/2030539771_20a9a1ca81_o.jpg">
    <figcaption><a href="http://cdn2.holytaco.com/wp-content/uploads/images/2009/12/2030539771_20a9a1ca81_o.jpg"></a></figcaption>
</figure></center>
It's clear there are advantages to switching your site over to HTTP2, but it's not an easy decision WHEN to implement.  If you host your own content, and keep most of servers and networking on site, it requires a decent amount of research on how to implement in the best possible way.  The protocol requires a minimum software version, so your systems have to support that upgrade (and not all enterprise companies can).  

If you interconnect with a bunch of different content providers and services, there's no guarantee that they will support HTTP2, although it's more likely than not that they do.  HTTP2 is backwards compatible with HTTP1.1, good news for implmentation. A lot of the back-end configuration remains the same.

So do your research.  Implement the upgrade if at all possible, sit back, and watch the money and clicks roll in from your much faster website!


<h3>References</h3>
<a href="https://kinsta.com/learn/what-is-http2/">What is Http2?</a><br>
<a href="https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol">Wikipedia Hypertext Transfer Protocol</a><br>
<a href="https://http2.github.io/faq/">HTTP/2 Frequently Asked Questions (Github)</a><br>
<a href="https://medium.com/the-web-crunch-publication/what-you-need-to-know-about-http-2-ab4a6f3bf468#.bdohe36ug">What you need to know about HTTP/2</a><br>
<a href="https://blog.stackpath.com/glossary/http2/">What is HTTP/2?</a><br>
<a href="https://www.engadget.com/2015/02/24/what-you-need-to-know-about-http-2/">What you need to know about HTTP/2</a><br>
<a href="https://auth0.com/blog/what-is-http2-all-about/">What is HTTP2 All About?</a><br>
<a href="https://blog.newrelic.com/2016/02/17/http2-production/">Implementing HTTP/2 in Production Environments</a><br>
