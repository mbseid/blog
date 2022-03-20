---
layout: post
title: "Acing a Technical Interview, Part III"
date: 2019-05-31
categories: [javascript]
tags: [tech]
status: publish
type: post
published: true
author: Mike Seid
---

This week we will be go over the most popular algorithms that are asked during the interview. In the interview setting, the interviewer will not ask about the algorithms directly. You will be required to know which algorithm is best for the problem at hand, how to use it to solve the problem, and then write out the solution. Knowing the algorithms and their time/space complexities will be the building blocks to solve the problems at hand.

### Algorithms

* Breadth First Search
* Depth First Search
* Quick Sort
* Merge Sort
* Dynamic Programming
* Backtracking
* Binary Search
* K Way Merge
* Union Find (Disjoint Set)
* Bucket Sort

#### Breadth First Search

A tree traversal algorithm iterating through each level of the tree. Uses a queue to keep track of the iterating position. This can also be used for graphs. In a graph setting, keep track of visited nodes in a set to ensure loops aren't being run.

**Complexity:**

* Time: O(n)
* Space: O(n/2)
* n is the number of nodes

```js
class TreeNode{
	constructor(value){
		this.value = value;
		this.left = null
		this.right = null;
	}
}


function breadthFirstSearch(treeRoot){
	let queue = [];

	queue.push(treeRoot);

	while(queue.length > 0){
		let node = queue.shift();

		if(node.left){
			queue.push(node.left)
		}

		if(node.right){
			queue.push(node.right)
		}
	}
}
```

*note: It can be helpful to keep track of rows using a Symbol. This symbol is added pushed onto the queue at the start, then is added again after it's found. Useful for printing out the row.*

#### Depth First Search

Similar to BFS except it goes deep instead of wide. To do this, the algorithm uses a stack instead of a queue. The left and right child ordering are important to the execution of the algorithm. Make sure to think about that ordering when solving the problem. Some useful applications are finding graph cycles, printing paths, solving mazes or other trie based data structures.

**Complexity:**

* Time: O(n)
* Space: O(n/2)
* n is the number of nodes

```js
class TreeNode{
	constructor(value){
		this.value = value;
		this.left = null
		this.right = null;
	}
}


function breadthFirstSearch(treeRoot){
	let stack = [];

	queue.push(treeRoot);

	while(queue.length > 0){
		let node = queue.pop();

		if(node.right){
			queue.push(node.right)
		}

		if(node.left){
			queue.push(node.left)
		}
	}
}
```

#### Quick Sort

Comparison based sort. Takes the divide and conquer approach to sorting the input. Broken down the algorithm takes these general recursive steps:

1. Pick Pivot Point
2. Reorder all the elements less than the pivot on the left and all the elements greater than the pivot on the right.
3. Recursively sort the left and right sub arrays.

**Complexity:**

* Time: O(n log n)
* Space: O(n)
  * There is also an in place version of the algorithm which modifies the original input.
* n is the size of the array

```js
export default class QuickSort extends Sort {

  sort(originalArray) {
    // Clone original array to prevent it from modification.
    const array = [...originalArray];

    // If array has less than or equal to one elements then it is already sorted.
    if (array.length <= 1) {
      return array;
    }

    // Init left and right arrays.
    const leftArray = [];
    const rightArray = [];

    // Take the first element of array as a pivot.
    const pivotElement = array.shift();
    const centerArray = [pivotElement];

    // Split all array elements between left, center and right arrays.
    while (array.length) {
      const currentElement = array.shift();

      // Call visiting callback.
      this.callbacks.visitingCallback(currentElement);

      if (currentElement == pivotElement) {
        centerArray.push(currentElement);
      } else if (currentElement < pivotElement) {
        leftArray.push(currentElement);
      } else {
        rightArray.push(currentElement);
      }
    }

    // Sort left and right arrays.
    const leftArraySorted = this.sort(leftArray);
    const rightArraySorted = this.sort(rightArray);

    // Let's now join sorted left array with center array and with sorted right array.
    return leftArraySorted.concat(centerArray, rightArraySorted);
  }
}
```

*note: Helpful visualization to understand what is happen. Especially useful for the in place version. [http://www.algomation.com/algorithm/quick-sort-visualization](http://www.algomation.com/algorithm/quick-sort-visualization)*

#### Merge Sort

General purpose sort. Takes each element and merges them together to build a larger sorted set.

**Complexity:**

* Time: O(n log n)
* Space: O(n).
  * There is also an in place version of the algorithm which modifies the original input.
* n is the size of the array

~~~js
export default class MergeSort extends Sort {
  sort(originalArray) {
    // If array is empty or consists of one element then return this array since it is sorted.
    if (originalArray.length <= 1) {
      return originalArray;
    }

    // Split array on two halves.
    const middleIndex = Math.floor(originalArray.length / 2);
    const leftArray = originalArray.slice(0, middleIndex);
    const rightArray = originalArray.slice(middleIndex, originalArray.length);

    // Sort two halves of split array
    const leftSortedArray = this.sort(leftArray);
    const rightSortedArray = this.sort(rightArray);

    // Merge two sorted arrays into one.
    return this.mergeSortedArrays(leftSortedArray, rightSortedArray);
  }

  mergeSortedArrays(leftArray, rightArray) {
    let sortedArray = [];

    // In case if arrays are not of size 1.
    while (leftArray.length && rightArray.length) {
      let minimumElement = null;

      // Find minimum element of two arrays.
      if (this.comparator.lessThanOrEqual(leftArray[0], rightArray[0])) {
        minimumElement = leftArray.shift();
      } else {
        minimumElement = rightArray.shift();
      }

      // Push the minimum element of two arrays to the sorted array.
      sortedArray.push(minimumElement);
    }

    // If one of two array still have elements we need to just concatenate
    // this element to the sorted array since it is already sorted.
    if (leftArray.length) {
      sortedArray = sortedArray.concat(leftArray);
    }

    if (rightArray.length) {
      sortedArray = sortedArray.concat(rightArray);
    }

    return sortedArray;
  }
}
~~~

#### Dynamic Programming

Solves optimal problems by breaking down the problem into recursive subproblems. By using a table to cache results, you can efficiently build fast algorithms to find the optimal solution. All problems must contain an optimal sub structure. Some examples of problems are:

* Longest Substring
* Max Sums
* Max Stock Market Gains
* Diffing Algorithms
* Spell Checkers

**Generic Steps**:

1. Initialize Array
2. Find Base Case
	`T[0,0] = 0`
3. Find modification based on problem
  `T[m, n] = Math.max( T[M - 1, N], T[M][N-1] )`
4. Find solution from table T. Either the max number in the table or often times the last element in the array.

This algorithm and set of problems is extremely popular in interviews. I would spend a lot of time reviewing articles, videos and practice problems.

* [Intro do Dynamic Programming](https://www.hackerearth.com/practice/algorithms/dynamic-programming/introduction-to-dynamic-programming-1/tutorial/)
* [Good Video Explanation](https://youtu.be/vYquumk4nWw)

#### Backtracking

Finds all, optimal, or first solution to problems by incrementally building candidates and checking to see if they satisfy the solution. If a candidate isn't a valid solution it is abandoned. Example problems are:

* Crosswords
* Sudoku
* Puzzles
* Combinations
* Subsets

**Complexity**: Depends on the problem but generally O(n!)

```js
var permute = function(nums) {
    let list = [];

    backtrack(list, [], nums);

    return list;
};

function backtrack(list, tempList, nums){
    if(tempList.length == nums.length){
        list.push([...tempList]);
    } else {
        for(let i = 0; i < nums.length; i++){
            if(tempList.includes(nums[i])) {
                continue;
            }

            tempList.push(nums[i]);
            backtrack(list, tempList, nums);
            tempList.pop();
        }
    }
}
```

#### Binary Search

Finds the target position within a **sorted array**. Jumps around the array by comparing the desired value to the middle element of the remaining section and recursively searching the remaining valid section until the solution is found.

**Complexity:** Time: O(log n)

```js
export default function binarySearch(sortedArray, seekElement, comparatorCallback) {
  // These two indices will contain current array (sub-array) boundaries.
  let startIndex = 0;
  let endIndex = sortedArray.length - 1;

  // Let's continue to split array until boundaries are collapsed
  // and there is nothing to split anymore.
  while (startIndex <= endIndex) {
    // Let's calculate the index of the middle element.
    const middleIndex = startIndex + Math.floor((endIndex - startIndex) / 2);

    // If we've found the element just return its position.
    if (sortedArray[middleIndex] == seekElement)) {
      return middleIndex;
    }

    // Decide which half to choose for seeking next: left or right one.
    if (sortedArray[middleIndex] > seekElement) {
      // Go to the right half of the array.
      startIndex = middleIndex + 1;
    } else {
      // Go to the left half of the array.
      endIndex = middleIndex - 1;
    }
  }

  // Return -1 if we have not found anything.
  return -1;
}
```

#### K Way Merge

Algorithm designed to sort multiple sorted lists. If two lists are provided, this algorithm is called binary merge. Uses a priority queue to keep track and do the sort of the elements. The amount of nodes on the priority queue is the same as the numbers of arrays being merged.

**Complexity:**

* Time: O(n log k)
* Space: O(n)
* n is the length of the input arrays
* k is the amount of arrays being merged

```js
function kwayMerge(arrays){

	let pq = new PriortyQueue(); // Let's assume we have this already built and we are importing it.
	let size = 0;

	for(let i = 0; i < arrays.length; i++){
		size += arrays[i].length;

		pq.push({
			index: 0,
			array: r,
			value: arrays[i][0]
		})
	}

	let output = [];

	while(pq.length > 0){
		let nextNode = pq.pop();
		output.push(nextNode.value);

		// Add back the next value on the PQ
		let nextArrayValue = arrays[nextNode.array][nextNode.index + 1];

		if(!!nextArrayNode){
			pq.push({
				index: nextNode.index + 1,
				array: nextNode.array,
				value: nextArrayValue
			})
		}
	}

	return output;
}
```

#### Union Find (Disjoint Set)

Algorithm (and resulting data structure) designed to keep track of set of elements portioned into a number of **non-overlapping** subsets. It has near constant time operations to add new sets, to merge existing, and most importantly to check to see if two nodes are in the same set.

<iframe src="https://www.youtube.com/embed/wU6udHRIkcc?list=PLLXdhg_r2hKA7DPDsunoDZ-Z769jWn4R8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### Bucket Sort

Sorting algorithm that works by distributing the elements into a number of buckets. By breaking down the sort into smaller buckets, the algorithm is able divide and conquer. This approach is most useful when you are looking for a subset of the problem or a max difference between numbers in an unsorted array.

Bucket sort works as follows:

1. Set up an array of initially empty "buckets".
2. Scatter: Go over the original array, putting each object in its bucket.
3. Sort each non-empty bucket.
4. Gather: Visit the buckets in order and put all elements back into the original array.

**Complexity:**

* Time: O( l n<sup>2</sup> )


### Conclusion

Algorithms are the work horses to solve real world problems. Knowing these general algorithms will help you solve most problems solved in interviews. To get good at the interview problems, you should do a bunch of problems. Using platforms like [https://leetcode.com](https://leetcode.com) can give you a good set of example problems to practice using these algorithms.

**Good Luck, you will do great in the interviews! You got this.**

### Resources

There is a wonderful repository of JS algorithms and data structures. [https://github.com/trekhleb/javascript-algorithms](https://github.com/trekhleb/javascript-algorithms). A lot of my code is copied and adapted from those sources. I would highly recomend you giving that resource a read.
