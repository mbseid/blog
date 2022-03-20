---
layout: post
title: Creating a Play! Framework 2 Module
date: 2012-08-29 16:11:00.000000000 -07:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---
As any project grows, it becomes cumbersome to manage, organize and compile your source code as a single project. Play! Framework gives you an easy way to separate your concerns and create reusable modules. Its pretty easy to create a module, heres how.

<script src="https://gist.github.com/mbseid/3519352.js"></script>
First, A Play! Module is just a play application itself. Defined as a PlayProject, this module is more or less a standalone Play! application. As you can see in the Build.scala above, you can separate your different site areas. Then, in your main PlayProject, you just add .dependsOn to fuse in module. Easy, right? This is the most common use case for modules, but you can be a whole lot more creative.

As a teaser for my next post, you can also have your PlayProjects depend on plain Scala projects. These can be stand alone libraries, or you can have them be plug-ins. For another day.

Now its up to you to figure out how you want to use it. Hope this helps!