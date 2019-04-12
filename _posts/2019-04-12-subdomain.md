---
layout: post
title: How to create and link a subdomain
date: 2019-04-12 12:01:00 -0700
categories: explanatory
math: true
comments: true
author: Aaron
---


Sometimes a static blogging platform is unable to accommodate all the features that you'd like. For instance, Jekyll (the platform on which this site is hosted) does not have an easy implementation of a photo gallery to use without some backend Ruby hacking on top of the Minima theme. It turns out Google Sites is a simple-to-use and feature-rich tool that can supplement a primarily blogging platform like Jekyll.  

I ended up using Google sites for my photography. But I wanted to add my Google site to my current domain (aaroncheng.me) instead of having another disjoint domain that was separate for my photos. Instead, I wanted my photography website to live in a subdomain of my already-existing URL, in the form of photos.aaroncheng.me, which would help to unify these two websites under a single domain.  

For future reference and for others aiming to achieve the same task, these are the steps toward making and linking a subdomain to an external service like Google sites:  

1. Verify your host identity through Google sites (there is a simple pipeline through Google Accounts to verify your administrative status with hosting sites like GoDaddy).  

2. Create a CNAME record, where the Name field is the subdomain (for instance, my Name field would be "photos"); this record should point to "ghs.googlehosted.com", and the TTL should be 1 hour.  

![]({{ site.url }}/images/subdomain.png)  

3. Register the subdomain with Google sites.  

Note that this is *not* the same as Subdomain forwarding! Google sites (and many other hosting platforms) discourage subdomain forwarding with masking, and will serve either 404 errors or blank pages. Always create a DNS CNAME record to link subdomains.