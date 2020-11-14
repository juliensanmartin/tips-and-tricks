# Time complexity and Big O Notation (worst case scenario):

- O(k^n) => exponential
- O(n^2) => quadratic
- O(n) => linear
- O(log n) => logarithmic
- O(1) => constant

## Cache and Memoization

```js
const memoize = function (cb) {
  const cache = {};
  return (...args) => {
    const key = JSON.stringify(...args);
    if (!cache[key]) {
      cache[key] = cb(...args);
    }
    return cache[key];
  };
};
```

# Recursions:

## Recursion in 4 steps

1. Identify base case(s).
2. Identify recursive case(s).
3. Return where appropriate.
4. Write procedures for each case that bring you closer to the base case(s).

```js
function callMyself() {
    if () {
        //base case
        return;
    } else {
        // recursive case
        callMyself();
    }
    return;
}
```

# Divide & Conquer

## Binary search

# Sorting

- Naive sort:
  - Bubble Sort: move inside the same array and switch,
  - Insertion Sort: create a new array and insert,
  - Selection Sort.
- Divide & conquer sort:
  - Merge sort: divide until every sub list is ony one item and then merge back (O(nlog(n))). The merge use 2 pointers and compare
  - Quick sort.

## Bubble sort

```js
// basic implementation
function bubbleSortBasic(array) {
  let countOuter = 0;
  let countInner = 0;

  for (let i = 0; i < array.length; i++) {
    countOuter++;
    for (let j = 1; j < array.length; j++) {
      countInner++;
      if (array[j - 1] > array[j]) {
        let temp = array[i];
        array[i] = array[j];
        array[j] = temp;
      }
    }
  }

  return array;
}
```

## Merge Sort

### Pseudo code

```js
    mergeSort(list)
        base case: if list.length 2, return list
        break the list into halves L & R
        Lsorted = mergeSort(L)
        Rsorted = mergeSort(R)
        return merge(Lsorted, Rsorted)
```

This is my own implementation

```js
const merge = (arr1, arr2) => {
  let index2 = 0;
  let index1 = 0;

  let haha = [];

  while (index1 < arr1.length && index2 < arr2.length) {
    if (index1 > arr2.length) {
      haha.push(...arr1.slice(index1));
    } else if (index2 > arr1.length) {
      haha.push(...arr2.slice(index2));
    } else if (arr1[index1] > arr2[index2]) {
      haha.push(arr2[index2]);
      index2 = index2 + 1;
    } else if (arr1[index1] <= arr2[index2]) {
      haha.push(arr1[index1]);
      index1 = index1 + 1;
    }
  }
  if (index1 < arr1.length) {
    haha.push(...arr1.slice(index1));
  }
  if (index2 < arr2.length) {
    haha.push(...arr2.slice(index2));
  }
  return haha;
};

const mergeSort = (list) => {
  if (list.length < 2) {
    return list;
  } else if (list.length === 2) {
    return merge([list[0]], [list[1]]);
  } else {
    const max = list.length;
    const middle = Math.floor(max / 2);
    const first = list.slice(0, middle);
    const second = list.slice(middle);
    const right = mergeSort(first);
    const left = mergeSort(second);
    return merge(right, left);
  }
};

const sortedList = mergeSort([12, 34, 67, 43, 2, 4, 90, 23, 62]);
console.log(sortedList);
```

# Greedy Algorithms

Try to make it work for the example. (not recommended better go with brute force)

# Brute force

Go through all the possible way, usually not optimized (exponential)

# Dynamic Programming (DP) - MIT course

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec19.pdf

=~ "careful brute force"
=~ guessing + recursive + memoization
=~ shortest paths in some DAG (Directed Cyclic Graph)

time = #subproblems \* time/subproblems (treating recursive calls as O(1))

## Fibonacci

F1 = F2 = 1, Fn = Fn-1 + Fn-2

Naive recursive algorithm
fib(n):
if n<=2 f=1
else f=fib(n-1)+fib(n-2)
return f

Exponential time: T(n)=T(n-1)+T(n-2)+O(1)=2T(n-2)=O(2 exp n/2) => BAD

## Memoized DP algo:

memo={}
fib(n):
if n in memo: return memo[n]
if n<=2 f=1
else f=fib(n-1)+fib(n-2)
memo[n]=f
return f

We only recurses the first time it's called (O(n)) and then use the memoized calls (O(1)) so total is O(n) => linear

DP: recursion + memoization + guessing

- memoize (remember) & reuse solutions to subproblems that help solve the actual problem
  => O() => #subproblems + time/subproblems

## Bottom up DP algo:

fib={}
for k in range(k):
if k<=2: f:1
else f=fib[k-1]+fib[k-2] //lookup not function call, already computed values
fib[k]=f
return fib[n]

exactly same computation
topological sort of subproblem dependency DAG Fn-3 -> Fn-2 -> Fn-1 -> Fn

## Shortest paths

Guessing: Don't know the answer? Guess! ...try all the guesses! (And take best one)

shortest path S to V: S > ... > ... > ... > V

S > ... > ... > U > V
sp(S,V)=min(sp(S,U))+edge(U,V)
base case: sp(S,S) = 0
sp(S,U) is recursive but just recursive is bad so we can add memoization
if dict(s,v) return dict(s,v) otherwise to computation

Is memoization good? No because infinite loop on graph with cycles
A
| \
S | V > S(V) look at S(A), S(A) look at S(B), S(B) look at S(V) and S(S) (infinite loop)
\ | /
B

Subproblem dependencies should be acyclic for memoization

To solve we need to make an acyclic graph into cyclic, flatten the graph by duplicating into layer

## 5 easy steps to DP

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec20.pdf

1- Define subproblems (# of subproblems)
2- Guess (part of solution) (# choices for guess)
3- related subproblem solutions (recurence) (time/subproblems)
4- Recurse & memoize (or build DP table bottom-up) -> check that subproblem recurrence is acyclic i.e topologycal order - DAG
5- solve the origin problem

## Text jutification

Split text into "good" lines
Text = list of words
badness(i,j) => how bad it is to use words between i and j for a line
if badness(i,j) don't fit then return infinity
else return (page width - total width of words)^3

1 or 2 can be switched

1. subproblems (hard part): suffixes words[i:]
   #subprobs: n
2. guess (hard part): where to start the 2nd line
   #choices: <= n-i =~ O(n)
3. recurrences: DP(i)= min(badness(i,j) + DP(j)) for every j in range(i+1, n+1)
4. topologic order: i=n, n-1 ..., 0
   total time: O(n^2)
5. original problems: DP(0)

## Blackjack

Perfect-information on Blackjack
Deck = C0, C1, Cn-1
1 player vs dealer
\$1 bet/hand

1. guess: how many hits?
   #choices <=n
2. subproblems: suffix Ci:
   #subproblems = n
3. recurrence: BJ(i)=max(BJ(j)+ outcome in {-$1, $0, +\$1}) for #hits in range (0, n) if valid play
   j=i+4+#hits+#decker hits

## DP - 3

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec21.pdf

## DP - 4

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec22.pdf

####################################################################

# Data Structure

## Heap ans Heap-sort with max heap

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec04.pdf

## Binary Search Trees, BST Sort

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec05.pdf

## AVL Trees

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec06.pdf

## Graph

### BFS - shortest path

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec13.pdf

### DFS - detext cycle - DAG - topological order

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec14.pdf

## Shortest Paths Problems

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec15.pdf

### Dijkstra

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec16.pdf

### Bellman-Ford

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec17.pdf

## Stack and Queue

### Stack

api: push(), pop()

#### Common interview question

- use an array to implement 3 stacks
- implement a getMin() or getMax() method on your stack
- create a queue using 2 stacks,
- sort a stack with the min values in order, on top. You can use another stack as a buffer
- write a function that returns true if a string of brackets is vali/balanced

### Queue

api: enqueue(), dequeue()

## Linked List

to implement dynamic array

Pros: Fast operation on the ends and flexible size
Cons: Costly lookups

a linkedlist is composed of Node with {value, next}

```js
const linkedlist = {
  head: {
    value: 1,
    next: {
      value: 2,
      next: {
        value: 3,
        next: null,
      },
    },
  },
};
```

Example: Least Recenlty Used cache, hash table, queue, stack

### Common interview question

- delete a node in the middle of a linked list
- delete a node with only a variable pointing at that node
- delete a duplicate
- partition a linked list around a value
- write a function for reversing a linked list. Can you do in place?
- check if a linked list conatins a cycle
- find the kth to the last node

## Hash Table

organizes data for quick look up on values for a given key
Pros: fast lookup, flexible keys
Cons: slow worst case lookup, unordered, single directional lookups

HashTable takes a hash function to generate a numerical value from any kind of input (string etc.) and for same input always get same output.
Also a hash function takes a range where to hash [0, 25] or [0, 50] etc. and always hash inside this range.

To deal with collision every value of a hashTable is linkedlist. So 2 values can generate the same hash key.

Once the hashtable reach 50% capacity, we need to double the hashtable.

In JS, hastable are either {}, Map, Set

{} can only have a string as key
Map can have any type as key (function etc.)
Set is just a set of keys with no values

Thing to not, hash table are constant time, even though when their is collision we might have to look through the array to find a specific value
which makes it linear, but because this scenario is unlikely because hash function shoud always return an unique value per passing arg, we can
think that hash table retrieval is constant time.

Same thing for resizing when the hashtable size expends, it happens not so often so we can think as constant time.

### Common interview question

- Count the number of occurences of all characters or words in a body of text or string.
- Delete duplicates in a list
- Find a unique value in a list
- Find if two integer in a list add up to k

## Array & String

### Array

Pros: Fast lookup, Fast append
Cons: Slow insert, slow delete

### String

Immutable

### Common interview question

- Rotate a matrix, string or an array
- Given an mXx bollean matrix, if an element is 0, set its' entire row and column to 0
- Search for a value
- Write a function that encodes a string by turning all spaces into `%20`.
- Merge two sorted list into 1 list

# Trees

Non-linear data structure
Linear data structure are array, hashtable, linkedlist

A tree is made of trees etc... > recursion

## Binary Trees - Traversals

# Graph

Need to store 2 things, object and relationship

## How to model

1-2-3
|/|/
5-4

### Adjacency Matrix

Array inside an array

1-2-3-4-5 (Vertices)
1 0 1 0 0 1
2 1 0 1 1 1
3 0 1 0 1 0 < Edges
4 0 1 1 0 1
5 1 1 0 1 0

### Adjacency List (preferable)

Inside the array is a reference to an object containing a value not the actual value.

```js
const adjList = {
  1: [2, 5],
  2: [1, 5, 3, 4],
  3: [2, 4],
  4: [2, 3, 5],
  5: [1, 2, 4],
};
```

## Traversing

### DFS vs BFS

DFS uses a Stack and BFS uses a Queue

#### Depth First Search

Graph Exploration

1. Add unvisited vertex to stack
2. Mark vertex as visited
3. If vertex has unvited children: repeat 1-2 with child
4. If vertex has no unvisited children: pop from stack
5. repeat until stack empty

```js
depthFirstTraversal(startingNode, func = console.log) {
    let visited = {}
    let stack = []

    stack.push(startingNode);
    visited[startingNode] = true;
    while (stack.length!==0) {
      const current = stack.pop();
      func(current);
      this.adjList[current].forEach(node=> {
        if (!visited[node]){
          stack.push(node);
          visited[node]=true;
        }
      })
    }
  }
```

#### Breath First Search

Graph Exploration

1. Add unvisited vertex to queue
2. Mark vertex as visited
3. If vertex has unvited children: repeat 1-2 with child
4. If vertex has no unvisited children: unqueue first in queue
5. repeat until queue empty

```js
  BreathFirstTraversal(startingNode, func = console.log) {
    let visited = {}
    let queue = []
    queue.push(startingNode);
    visited[startingNode] = true;
    while (queue.length!==0) {
      const current = queue.shift();
      func(current);
      this.adjList[current].forEach(node=> {
        if (!visited[node]){
          queue.push(node);
          visited[node]=true;
        }
      })
    }
  }
```

### Common question BFS/DFS

- Logging or validating the contents of each edge and/or vertex
- Copying a graph, or converting between adjacency matric or list
- Counting the number of edges and/or vertex
- Identifying the connected components
- Finding paths or cycles between two vertices

### Graph Property

- Directed VS Undirected
- Weighted VS Unweighted
- Self loop
- Sparse VS Dense
- Cyclic VS Acyclic

### Binary Search Tree

```js
/** Class representing a Binary Search Tree. */

class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
    this.size = 0;
  }
  add(value) {
    const newNode = new Node(value);
    if (this.root) {
      const { found, parent } = this.findNodeAndParent(value);
      if (found) {
        // duplicated: value already exist on the tree
        found.meta.multiplicity = (found.meta.multiplicity || 1) + 1;
      } else if (value < parent.value) {
        parent.left = newNode;
      } else {
        parent.right = newNode;
      }
    } else {
      this.root = newNode;
    }
    this.size += 1;
    return newNode;
  }
  findNodeAndParent(value) {
    let node = this.root;
    let parent;
    while (node) {
      if (node.value === value) {
        break;
      }
      parent = node;
      node = value >= node.value ? node.right : node.left;
    }
    return { found: node, parent };
  }
}

const myBST = new BinarySearchTree();

myBST.add(1);
console.log(myBST);
```

Complexity: O(h) where h is the height of the tree

if the tree is unbalanced then the complexity tends to more linear complexity
