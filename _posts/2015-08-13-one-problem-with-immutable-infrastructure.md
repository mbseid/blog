---
layout: post
title: One Problem With Immutable Infrastructure
date: 2015-08-05
categories: [devops, scala]
tags: [tech]
status: publish
type: post
published: true
author: Mike Seid
---

### Dependencies that aren't hosted by you

![Bintray Down](/assets/bintray_down.png)

At [Naytev](https://www.naytev.com), we love things being [immutable](https://en.wikipedia.org/wiki/Immutable_object). We develop in Scala and practice [immutable infrastructure](http://chadfowler.com/blog/2013/06/23/immutable-deployments/). For the most part, it works great, until it doesn't....

One premise of immutable infrastructure is that each time you redeploy, you create a new image from scratch. Start with a base Linux image and download everything you need; then when you redeploy the latest version of your code, you throw away your existing boxes. Your boxes don't change and can be recreated with ease. No potential error for forgotten hot fixes or patches.

The only problem is when you are trying to download your dependencies and the provider is down. You are shit out of luck with no chance of help. This is a problem with any single point of failure but it is exasperated when you can't deploy any code change at all, even when there is no change in dependencies. Every dependency that gets cached on your box gets thrown out. Outages will always happen and have recently with [NPM](http://status.npmjs.org/incidents/8f606wk49gq4), [RubyGems](http://blog.rubygems.org/2015/08/13/postmortem.html) and [Bintray](http://status.bintray.com/incidents/kbrwgs20k12r) yesterday. 

We were able to work around it by modifying our latest image server images with the our newest code but it wasn't fun. Overall, I'm still loving our immutable infrastructure but no methodology is perfect.

