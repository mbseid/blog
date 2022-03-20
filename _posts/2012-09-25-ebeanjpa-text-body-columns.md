---
layout: post
title: Ebean/JPA text body columns
date: 2012-09-25 15:57:00.000000000 -07:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---
It's always a fun feeling when you run into the same problem 3 times. Today, I created a Ebean model for my post, writing the description as:

`  
public String description;  
`

This is all fine and dandy, until I added a long descriptions to one of my post. Upon looking at the sql debug logs, I found out that default column description of varchar(255). Only 255 chars? Thats not going to work for me. To fix it just add a column description metatag:

`  
@Column(columnDefinition="TEXT")  
 public String description;  
`

Now your column has the type text, which should be plenty for your description.