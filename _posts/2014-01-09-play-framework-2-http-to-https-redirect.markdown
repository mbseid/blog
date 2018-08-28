---
layout: post
title: Play Framework 2 HTTP to HTTPS redirect
date: 2014-01-09 17:03:00.000000000 -08:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---
One good feature of Play Framework is that it doesn't require an HTTP server in front of it. Play runs netty under the hood and it takes care of the HTTP service. Unfortunately, the play team decided not to support HTTPS currently. So I am using an Amazon ELB (Elastic Load Balancer) to both balance my servers and terminate the HTTPS connections.

The feature I wanted was to forward all requests from HTTP to HTTPS. This can be done simply by taking advantage of a header set by the ELB and using the Global.scala object.

{% gist mbseid/8327841 %}

No need to modify any of your application code. It's clean and maintains state.

If you are curious on how to set up an ELB for HTTPS, [follow the tutorial I wrote here](http://mbseid.com/setting-up-an-aws-elastic-load-balancer-with-ssl/).

##### Update/Edit:

As mentioned by whirby in the comments, the Play team actually added it in Play! 2.2\. The documentation can be found here: http://www.playframework.com/documentation/2.2.x/ConfiguringHttps

