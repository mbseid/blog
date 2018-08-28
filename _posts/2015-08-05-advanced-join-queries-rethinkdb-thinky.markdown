---
layout: post
title: Advanced Query Joins With RethinkDB and Thinky
date: 2015-08-05
categories: [rethinkdb, thinky]
tags: [tech]
status: publish
type: post
published: true
author: Mike Seid
---
In my spare time, I have been learning [RethinkDB](http://rethinkdb.com/). RethinkDB is a JSON database with some familiar relational database features and a ton of other goodies. One of the most interesting features is joins, as it allows you to keep your data model very normalized. **BUT** the join syntax and execution is quite different than you may be used to, especially when you are selecting specific data. 

_note: I use an ORM [thinky.js](http://thinky.io) to abstract a lot of the modeling away._

Here is a query for fetching data with a condition in the joined document. We are fetching published books and their authors who are from California.

{% gist mbseid/a91bc7e250ee5d680527 %}
 
Quick breakdown of the query and the mindset behind it. 

First you must select your table and filter your preliminary conditions. In this case, we are only fetching published books, so we do a filter on the published field. This initial filter will speed up your queries a ton, as RethinkDB doesn't have to join all the rows, just the rows that pass the published condition. Then you enrich the documents with the join. Once the documents are joined you can then filter again on the state condition resulting in the set of documents you want.

The biggest difference between an RDB and RethinkDB is that RDB queries group steps together and aren't concerned about order as much where as RethinkDB is performed in a strict steps and get built up functionally. When you execute the run function at the end of the RethinkDB query, it will execute all the functions in that order. There are a lot of queries where you will run filter multiple times separately. The filter doesn't need to happen in one place or at one time.

Once you understand the RethinkDB mindset it is actually quite easy to build queries. Think about functions and steps as opposed to set theory.