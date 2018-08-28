---
layout: post
title: Play! 2 SQL Debugging
date: 2012-09-25 15:59:00.000000000 -07:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---
SQL errors are always a whole lot of fun to debug when you are working with an ORM . To help this, I always have my generated SQL statements logged during development. Add these two lines to your application.conf

`  
db.default.logStatements=true  
 logger.com.jolbox=DEBUG  
`

Short post, hope it helps.