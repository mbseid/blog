---
layout: post
title: Play Framework 2.2 - Heroku - New Relic
date: 2014-03-14 18:18:00.000000000 -07:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---
This week I was monkeying around with Play Framework 2.2, Heroku and New Relic. [Heroku](http://www.heroku.com) makes it almost too easy to deploy to a production server.

I wanted to track the performance and errors of my application, so I added New Relic to the mix. Unfortunalty, it wasn't nearly as easy to get set up. If you want to get it set up, follow these steps ( despite everything found on the web ):

1.  Add the add-on to your Heroku app: `heroku addons:add newrelic`
2.  Open your new relic dashboard
3.  Download the Java agent that they provide.
4.  Unzip the folder, and put it in the ROOT of your project. I placed the newrelic folder as a sibling to the app folder. This way it won't interfere with your project code, and still get packaged within your builds.
5.  Extract the newrelic.yml file from the newrelic folder and move it to the conf folder. Keep all your configs together!
6.  Modify your Procfile to add the -J-javaagent and New Relic Config.  
     `  
    web: target/universal/stage/bin/YOUR_APP_NAME -Dhttp.port=${PORT} -J-javaagent:/app/newrelic/newrelic.jar -J-Dnewrelic.config.file=conf/newrelic.yml  
    `

A few notes. The javaagent is marked by a `:`, not an `=`. This is very confusing and to be honest I have no idea why this is the case. Also, the javaaggent must be absolutely refernced in the Procfile, where as the config file is relativly referenced.

Once this is all set up, redeploy your app and you will have New Relic at your service. I hope this saves you a bit of time and causes less hassle than it did to me .

Cheers