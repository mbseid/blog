---
layout: post
title: Custom Namecheap Domain with Heroku
date: 2012-08-06 16:11:00.000000000 -07:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---
So I just got my new Heroku Cedar Stack set up with a custom URL, and surpassingly it was a bit more confusing than I thought ( on the Namecheap side).

Below are the steps I used to configure my custom domain, mbseid.com with my Heroku instance.

From the console run this heroku toolbelt command:  
 `  
$ heroku domains:add www.mbseid.com  
`

Then in Namecheap, log into your admin and select your domain:

*   Navigate to All Host Records
*   Then for the www host name, enter mbseidblog.herokuapp.com.
    *   **(KEY: must end with a .) with a CNAME record type.**

You will then be good to go.
