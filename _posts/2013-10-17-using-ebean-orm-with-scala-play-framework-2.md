---
layout: post
title: Using Ebean ORM with Scala Play! Framework 2
date: 2013-10-17 16:00:00.000000000 -07:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---

I have really enjoyed using Play! Framework 2, especially with the power of scala. As I use it more and explore various aspects, I have come to desire for a real ORM again. Although Anorm provides a nice data access layer, I really don't enjoy writing crud methods and the simple converters for every model I make. Thus, I started exploring the other options provided. Play! 2 comes bundled with Ebean, a state-less java ORM that uses JPA notation. Its pretty simple and provides us with the ORM features that we all know and love. With just a little bit of modification, we can take advantage of Ebean while still using Scala Controllers and Actors.

Over the course of this post, I will explain how to both configure Ebean and show how to use it.

To start using Ebean, you must add the following lines to your application.conf file.

{% gist mbseid/3371839 application.conf %}

Simple, right? The default in Ebean.default specifies which database it is using. You can declare singular models to different databases if you like, its completely up to you.

The next step is to create a subproject for your models. Now why did I decide to do this? Well, Play! does some nifty things on compile to make your life easier. In general, you wouldn't declare your model variables as public, as this is a safety concern, so Play! does a bunch of byte code modification to make the variables private, and generate getters/setters. Besides the safety concerns, its adds a bunch of dirty handlers and other magic to help optimize your queries. Nifty! So, I made the separate projects because I need the model to compile first before the controller uses it. Otherwise, I would always get compile time errors when I changed the model; Then I would have to clean/reload it again before it would work. So, by creating a sub project, and having the main project depend on it, I can ensure the project is build in the proper order. Check out my Build.scala file below:

{% gist mbseid/3371839 build.scala %}

Now to access the data. Ebean lends itself to the DAO pattern, with its finder object. I have a CrudDAO that I have all my main DAOs extend from.

{% gist mbseid/3371839 AbstractDAO.java %}

The awesome sauce here is the last function, Option (Lines 52-55). Using the same syntax, we can create a scala option in java. How cool is that! Now when when we call dao.get( foobarId ), we can return an option instead of a null, so much easier to handle.

Another important conversation is the one from java.util.ArrayList to a Scala List. Luckily there are some converters already packaged with Scala. Taking advantage of those converters with the implicit operator, I made a trait that does all the conversions automatically.

{% gist mbseid/3371839 EbeanConversions.scala %}

Now I don't have to think about the conversions. I get the objects I expect, in the scala flavor that I really enjoy. The trait may not be the best way or place to do the conversions, but it has worked really well for me so far.

I'll make sure to update this post as I learn more. But for now, It works great and I really enjoy the ORM capabilities. Hope this helps you out.

Cheers!