---
layout: post
title: "LDAP Authentication"
excerpt: "Where is NIST when you need them?"
categories: blog
date: 2017-06-09T05:00:00-06:00
---

<center>
<iframe src="https://giphy.com/embed/aWxbEGCqkiZFK" width="480" height="244" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/secret-secrets-secretive-aWxbEGCqkiZFK">via GIPHY</a></p></center>

One of the things I'm constantly frustrated with is the unnecessary complexity of Lightweight Directory Access Provider (LDAP) integration into your applications.  Not only with LDAP, but also implementing Okta into your tools can be cumbersome, annoying, and tricky.

For a brief primer on LDAP, which is basically a database that manages users and logins, I recommend this article <a href="https://hynek.me/articles/ldap-a-gentle-introduction/">here</a>, or of course <a href="https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol">Wikipedia</a>, for something more a little more in-depth.  LDAP is an open industry standard developed as a lightweight response to <a href="https://en.wikipedia.org/wiki/X.500">X.500 networking standards</a>, helping organizations to manage user access to multiple things -- files, servers, databases -- without necessarily knowing where in the hierarchical structure that user is located.

LDAP is mistakenly used interchangeably with Active Directory (AD), even by me.  The key difference between the two is that LDAP is a protocol, and AD is an implementation of that protocol. (As usual, there's a <a href="https://stackoverflow.com/questions/663402/what-are-the-differences-between-ldap-and-active-directory">StackOverflow</a> question about this.)

This also should not be confused with SSO services like <a href="https://developer.okta.com/standards/SAML/">SAML</a>, which again can (or cannot) leverage the LDAP protocol.  Okta or OneLogin are further removed, as SAML/AD implementations.

Lately, we've been dealing with all kinds of LDAP integration issues.  Part of those problems have been caused by the data center migration. But some have also been caused by changing our root cert on our domain controllers.  I have been battling for several weeks with ever changing requirements for one of our applications that integrates LDAP through Apache due to our changing landscape.  Plus, the problem is exacerbated by having several different LDAP vips -- all which leverage different domain controllers, and have different configurations on them.

LDAP integration through apache is... interesting.  I've been working with automating our Icinga2 monitoring system for teams that are being onboarded onto pipeline.  Our pipeline teams are going to be automating the entire workflow from soup to nuts, and also will receive a separate Icinga2/Thruk instance that pulls data just for servers/applications relevant to them.  Today I spent some time trying to understand why LDAP with the exact same configuration worked on one server, and not on another.

In case you were curious, the solution was apparently a 4th restart of apache solved the problem.

<center>
<iframe src="https://giphy.com/embed/bDTtPo3HyEluE" width="480" height="328" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/sigh-cat-bDTtPo3HyEluE">via GIPHY</a></p></center>

It requires use of the <a href="http://httpd.apache.org/docs/2.0/mod/mod_auth.html">mod_auth</a>, mod_authz, and <a href="https://httpd.apache.org/docs/2.4/mod/mod_ldap.html">mod_ldap</a> modules within your standard httpd.conf.  There's also <a href="https://httpd.apache.org/docs/2.0/mod/mod_auth_ldap.html">mod_auth_ldap</a>, so I'm sure there's multiple ways it can be implemented within apache itself. I highly recommend implementing your configuration within a separate conf.d directory for your application.  You can put references to your certs in your *.conf file itself, or rely on your ldap installation (we use openldap, so /etc/openldap/ldap.conf) to manage your root CA.  If your certs are self-signed, you need to account for that as well, potentially by adding

```code
LDAPVerifyServerCert Off
```

to your configuration.

Also, depending on your distribution, you need to ensure your SELinux settings are configured correctly (see <a href="http://www.linuxquestions.org/questions/linux-server-73/ldap-authentication-error-%5Bcan%27t-contact-ldap-server%5D-from-apache-httpd-920907/">LDAP authentication error</a>, where it LDAP leads people to self-harm).

Another complication with LDAP: integration is different for a sundry of applications.  We have a few applications (Nexus Repository, JIRA, Tectonic, Confluence, Jenkins) that leverage LDAP integration.  Actually, Confluence is Okta, which is a completely DIFFERENT annoyance, but it also uses LDAP/AD configuration.  And without a doubt, the implementation of the usage/filtering has been different; different enough beyond just how we allow users access.  The LDAP username and Base DN are usually similar.  However, the user schema settings are wildly different.  For example, the username attribute that works for one application won't work for another.  Sometimes we use sAMAccountName, other times uid, and switching them doesn't work.  It's not one configuration fits all.

So, why continue to use LDAP when SSO protocols like SAML are taking over the world?  Honestly, there's still much to be said for the classics.  LDAP implementation can be very complicated and not really standard across applications, and something into which I've only taken a few steps.  But there's something to be said for the configuration you can do with LDAP, and the fact that it's free.

 I just wish they leveraged something more standard across applications -- but that may be too much to ask with something that's open source.