---
layout: post
title: "Acing a Technical Interview, Part II"
date: 2019-05-24
categories: [javascript]
tags: [tech]
status: publish
type: post
published: true
author: Mike Seid
---

This week we will be go over the primary data structures that are used within the coding interview. Most interview questions will not ask you directly about the data structures but will require to use one or more data structures and you must explain how they work. Being able to explain their time/space complexity and operations will be important for success.

### Data Structures

* Hash Tables
* Linked Lists
* Stack
* Queue
* Trees
* Tries
* Graph
* Vector
* Heaps

#### Hash Tables

Structure that maps keys to values. Using a hash function, the keys can be distributed into a buckets. Collision may occur and there are patterns to handle those collisions. Good hashing patterns provide even distribution of the keys. The less collisions the better.

* Space Complexity: O(n) where n is the number of keys
* Time Complexity: Get and Set is O(1)
* Key Metric: Load Factor = `n/k` where n is the number of entries and k is the number of buckets. Higher load leads to more collisions.
* Collisions can be handled through separate chaining or open addressing.


**Javascript**

~~~ js
let hashTable = {}

hashTable["key"] = "value" // Set: O(1)
return hashTable["key"] // Get: O(1)
~~~

**Common Uses**

* Associative Arrays
* Database Index
  * Database Sharding
* Caches
* Sets
* Object Representation

#### Linked List

Linear data structure where each element is a separate object, linked by pointers. Head and tail variables are pointers pointing to their respective objects. These pointers are not nodes themselves. There are multiple flavors of linked lists: singly linked lists that only go from first to last and doubly linked lists that can go in either direction.

Benefits versus arrays:

* Can be added to indefinitely without memory allocation.
* No wasted memory, only consumes for the amount of objects you have.
* Elements are cheaply removed and added
* Not ordered on physical memory

Disadvantages versus arrays:

* Only allows for sequential access O(n)
* More memory usage than arrays. Allocates pointers plus values

**Javascript**

~~~js
class ListNode {
	constructor(value){
		this.value = value;
		this.next = null;
	}
}

const head = new ListNode(1);
const second = new ListNode(2);

head.next = second;
~~~

**Complexities**

* Insert - O(n)
* Head - O(1)
* Tail - O(n) unless there is a tail pointer in which case it's O(1)
* Search - O(n)


#### Stack

A last in first out data structure. Usually stacks are a fixed size and can be overflowed if there are too many items.

**Operations**

* Push: Add item to the top of the stack - O(1)
* Pop: Takes item off the top of the stack - O(1)

**Javascript**

~~~js
let stack = [];

stack.push(1);

stack.push(2);

const item = stack.pop();

console.log(item) // 2
~~~

#### Queue

A first in first out data structure. Queues are the most used data structure if required to order work.

**Operations**

* enqueue: Add item to the end of the queue - O(1)
* dequeue: Takes item off the top of the stack- O(n) - Cause: moving all the array items one to the left.

**Javascript**

~~~js
let queue = [];

queue.push(1);
queue.push(2);

const item = queue.shift();

console.log(item) // 1
~~~~

#### Trees

Has a single root and nodes containing values and a subtree of children. There are two types of trees: unordered trees and ordered trees. Trees are recursive data structures. Binary trees only have two children and get filled left to right. Binary trees are the only typed used in interviews.

**Key Traits**

* Depth - the nodes between the node in question and the root
* Height - the largest depth of the tree
* Leaf - a node without any children

**Common Uses**

* Natural Hierarchical Data - Family tries
* Organizing Data
* Tries - Dictionary
* Network Routing Algorithms

**Javascript**

~~~js
class TreeNode{
	constructor(value){
		this.value = value;
		this.left = null
		this.right = null;
	}
}

const root = new TreeNode(1);
const leaf = new TreeNode(2);
root.left = leaf;

console.log(root.left.value); // 2
~~~


#### Tries

A tree where the value is the sum of the nodes and it's parents. I've never had to write these in code, but have talked about tries in an interview setting.

**Common Uses**

* Routing Tables
* Spell Check
* Autocomplete

#### Graphs

An undirected graph that connects a set of nodes and edges. Nodes and edges can have multiple values. Commonly used for network graphs and social networks.

**Operations**

* g is the graph itself
* adjacent(g, x, y) - edge between x and y
* neighbors(g, x, y) - list nodes that connect x and y
* add_node(g, x, value)
* add_edge(g, x, y, value)
* remove_node(g, x)
* remove_edge(g, x, y)
* get_node_value(g, x)
* get_edge_value(g, x, y)

#### Vectors

Variable sized one dimensional array. Continuously held in memory. Lookups are O(1) like arrays. Vectors can grow or shrink in size. Usually size are added or removed in blocks. Allocation is expensive.

#### Heaps

Specialized type of tree data structure that satisfies the heap property: if P is a parent node of C, P value must be grater than C. A max heap: P => C, P > C, a min heap: P => C, P < C. This data structure is often used for priority queues.

**Operations**

* insert(value) - When inserting a value, the tree must rebalance the tree to ensure it's inserted in the right place. Time Complexity(log n)
* remove(value) - Same as inserting, the tree must rebalance the to ensure it's still satisfies the heap property. Time Complexity(log n)



### Conclusion

Data structures are the foundation of any algorithm. Having deep knowledge of these core data structures will provide the knowledge to build great algorithms. You'll quickly see how these data structures are used in the algorithms next week. If you have any comments, tweet me @mbseid. I would love to hear your feedback and additions.
