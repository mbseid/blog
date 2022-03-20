---
layout: post
title: Decommissioning A Kafka Node
date: 2015-11-05
categories: [kafka, devops]
tags: [tech]
status: publish
type: post
published: true
author: Mike Seid
---

Decommissioning a Kafka node isn't talked about very much. Unlike other distributed systems, Kafka doesn't provide an automated way to decommission a node. Whether you want to downsize a cluster or take a node down for maintenance, removing a node from the cluster is an important part of the DevOps life cycle. After taking [Naytev's](https://www.naytev.com) cluster from 6 to 4 nodes, I have created a safe process to remove nodes without losing any data. 

To remove a node you must first reassign all the primary partitions on the node then shutdown the node using the scripts. Unfortunately, you can't do this all in one automated step, so you follow these steps for success


### Steps to removing nodes

It all starts with a custom script written by [Michael Noll](https://gist.github.com/miguno) that helps craft the reassignment script. Here is the modified version that worked for us.

{% gist mbseid/8ec6271a63315cfbe6cc %}

Add this script to your running node.

Run it to create the reassignment script.

```
sudo sh kafka-move-leadership.sh --broker-id 3 --first-broker-id 0 --last-broker-id 4 --zookeeper {{zookeeper-ip}}:2181
```

Copy the output into a text file, `partition-reassign.json`.

Run the reassign script. This script comes bundled with Kafka version 0.8.x.

```
sudo sh kafka-reassign-partitions.sh --zookeeper {{zookeeper-ip}}:2181 --reassignment-json-file partition-reassign.json --execute
```

Then you check the progress with a similar command.

```
sudo sh kafka-reassign-partitions.sh --zookeeper {{zookeeper-ip}}:2181 --reassignment-json-file partition-reassign.json --verify
```

Once the reassignment is done, you can shutdown the node.
```
sudo sh kafka-server-stop.sh
```

Once you are done, ensure that you restart all of the services consuming from the Kafka cluster. Sometimes it will have cached pointers to partitions and you can prevent those errors by restarting your consumers.

Hope this helps :)