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

Found here https://leetcode.com/discuss/general-discussion/786126/python-powerful-ultimate-binary-search-template-solved-many-problems

- Correctly initialize the boundary variables left and right to specify search space. Only one rule: set up the boundary to include all possible elements.
- Decide return value. Is it return left or return left - 1? Remember this: after exiting the while loop, left is the minimal k​ satisfying the condition function.
- Design the condition function. This is the most difficult and most beautiful part. Needs lots of practice.

```js
var BS = function() {
  // could be [0, n], [1, n] etc. Depends on problem
    let left = // min range
    let right = // max range

    while (left < right) {
        const mid = left + Math.floor((right - left) / 2);
        console.log(left, right, mid);

        if (condition(mid, H, piles)){
            right = mid;
        } else {
            left = mid + 1;
        }
    }

    return left; // or left+1
};
```

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

Choose best solution at each level instead of choising the overall best solution.

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

## maxSubArray

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

At first, I think the sub problem should look like: maxSubArray(int A[], int i, int j), which means the maxSubArray for A[i: j]. In this way, our goal is to figure out what maxSubArray(A, 0, A.length - 1) is. However, if we define the format of the sub problem in this way, it's hard to find the connection from the sub problem to the original problem(at least for me). In other words, I can't find a way to divided the original problem into the sub problems and use the solutions of the sub problems to somehow create the solution of the original one.

So I change the format of the sub problem into something like: maxSubArray(int A[], int i), which means the maxSubArray for A[0:i ] which must has A[i] as the end element. Note that now the sub problem's format is less flexible and less powerful than the previous one because there's a limitation that A[i] should be contained in that sequence and we have to keep track of each solution of the sub problem to update the global optimal value. However, now the connect between the sub problem & the original one becomes clearer:

`maxSubArray(A, i) = (maxSubArray(A, i - 1) > 0 ? maxSubArray(A, i - 1) : 0) + A[i];`

```java
public int maxSubArray(int[] A) {
  int n = A.length;
  int[] dp = new int[n];//dp[i] means the maximum subarray ending with A[i];
  dp[0] = A[0];
  int max = dp[0];

  for(int i = 1; i < n; i++){
    dp[i] = A[i] + (dp[i - 1] > 0 ? dp[i - 1] : 0);
    max = Math.max(max, dp[i]);
  }

  return max;
}
```

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

https://blog.bitsrc.io/implementing-heaps-in-javascript-c3fbf1cb2e65

Childs of i = 2i+1 AND 2i+2
Parents of i = Math.floor((i-1)/2)

```js
class MinHeap {
  constructor() {
    /* Initialing the array heap and adding a dummy element at index 0 */
    this.heap = [null];
  }

  getMin() {
    /* Accessing the min element at index 1 in the heap array */
    return this.heap[1];
  }

  insert(node) {
    /* Inserting the new node at the end of the heap array */
    this.heap.push(node);

    /* Finding the correct position for the new node */

    if (this.heap.length > 1) {
      let current = this.heap.length - 1;

      /* Traversing up the parent node until the current node (current) is greater than the parent (current/2)*/
      while (
        current > 1 &&
        this.heap[Math.floor(current / 2)] > this.heap[current]
      ) {
        /* Swapping the two nodes by using the ES6 destructuring syntax*/
        [this.heap[Math.floor(current / 2)], this.heap[current]] = [
          this.heap[current],
          this.heap[Math.floor(current / 2)],
        ];
        current = Math.floor(current / 2);
      }
    }
  }

  remove() {
    /* Smallest element is at the index 1 in the heap array */
    let smallest = this.heap[1];

    /* When there are more than two elements in the array, we put the right most element at the first position
            and start comparing nodes with the child nodes
        */
    if (this.heap.length > 2) {
      this.heap[1] = this.heap[this.heap.length - 1];
      this.heap.splice(this.heap.length - 1);

      if (this.heap.length === 3) {
        if (this.heap[1] > this.heap[2]) {
          [this.heap[1], this.heap[2]] = [this.heap[2], this.heap[1]];
        }
        return smallest;
      }

      let current = 1;
      let leftChildIndex = current * 2;
      let rightChildIndex = current * 2 + 1;

      while (
        this.heap[leftChildIndex] &&
        this.heap[rightChildIndex] &&
        (this.heap[current] > this.heap[leftChildIndex] ||
          this.heap[current] > this.heap[rightChildIndex])
      ) {
        if (this.heap[leftChildIndex] < this.heap[rightChildIndex]) {
          [this.heap[current], this.heap[leftChildIndex]] = [
            this.heap[leftChildIndex],
            this.heap[current],
          ];
          current = leftChildIndex;
        } else {
          [this.heap[current], this.heap[rightChildIndex]] = [
            this.heap[rightChildIndex],
            this.heap[current],
          ];
          current = rightChildIndex;
        }

        leftChildIndex = current * 2;
        rightChildIndex = current * 2 + 1;
      }
    } else if (this.heap.length === 2) {
      /* If there are only two elements in the array, we directly splice out the first element */
      this.heap.splice(1, 1);
    } else {
      return null;
    }

    return smallest;
  }
}
```

Why use a Heap instead of Sorted Array:

- get min max in O(1)
- insert/remove in O(log n)

Sorted Array:

- insert/remove in O(n log n)

## Binary Search Trees, BST Sort

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec05.pdf

```js
class Node {
  constructor(val) {
    this.val = val;
    this.right = null;
    this.left = null;
    this.count = 0;
  }
}

class BST {
  constructor() {
    this.root = null;
  }

  create(val) {
    const newNode = new Node(val);
    if (!this.root) {
      this.root = newNode;
      return this;
    }
    let current = this.root;

    const addSide = (side) => {
      if (!current[side]) {
        current[side] = newNode;
        return this;
      }
      current = current[side];
    };

    while (true) {
      if (val === current.val) {
        current.count++;
        return this;
      }
      if (val < current.val) addSide('left');
      else addSide('right');
    }
  }

  find(val) {
    if (!this.root) return undefined;
    let current = this.root,
      found = false;

    while (current && !found) {
      if (val < current.val) current = current.left;
      else if (val > current.val) current = current.right;
      else found = true;
    }

    if (!found) return 'Nothing Found!';
    return current;
  }

/**
  First we need a utility function to collect all of the deleted node’s children.
  I’ll use a basic breadth-first search to push everything into an array which we can then loop through to re-add each item to the tree.

  The only difference from a normal search is that it needs to accept a different starting point,
  so we can limit our search only to the subtree of our deleted node’s children.
 * */
  BFS(start) {
    let data = [],
        queue = [],
        current = start ? this.find(start) : this.root;

    queue.push(current);
    while (queue.length) {
      current = queue.shift();
      data.push(current.val);

      if (current.left) queue.push(current.left);
      if (current.right) queue.push(current.right);
    };

    return data;
  }

/**
  Since we can’t traverse back up to the parent we’ll use a variable to store the parent node to current and use that to set current to null after we’ve saved the children.

  In deleteNode we’ll collect our node’s children, set it on its parent to null, then use create on each of the children, restructuring them properly in the tree. Our children array will also include the deleted node, which we can just splice out.
 * */
  delete(val) {
    if (!this.root) return undefined;
    let current = this.root,
        parent;

    const pickSide = side => {
      if (!current[side]) return 'No node found!';

      parent = current;
      current = current[side];
    };

    const deleteNode = side => {
      if (current.val === val && current.count > 1) current.count--;
      else if (current.val === val) {
        const children = this.BFS(current.val);
        parent[side] = null;
        children.splice(0, 1);
        children.forEach(child => this.create(child));
      };
    };

    while (current.val !== val) {
      if (val < current.val) {
        pickSide('left');
        deleteNode('left');
      };
      else {
        pickSide('right');
        deleteNode('right');
      };
    };

    return current;
  }

}

let tree = new BST();
tree.add(10);
tree.add(4);
tree.add(4);
tree.add(12);
tree.add(2);
console.log(tree);
```

Why use BST VS ARRAY:

- search/insert/delete in O(log n) VS O(n) for array
- slower to access O(log n) VS O(1) for array

## AVL Trees

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec06.pdf

```js
/**
 * Balance tree doing rotations based on balance factor.
 *
 * Depending on the `node` balance factor and child's factor
 * one of this rotation is performed:
 * - LL rotations: single left rotation
 * - RR rotations: single right rotation
 * - LR rotations: double rotation left-right
 * - RL rotations: double rotation right-left
 *
 * @param {BinaryTreeNode} node
 */
function balance(node) {
  if (node.balanceFactor > 1) {
    // left subtree is higher than right subtree
    if (node.left.balanceFactor < 0) {
      return leftRightRotation(node);
    }
    return rightRotation(node);
  }
  if (node.balanceFactor < -1) {
    // right subtree is higher than left subtree
    if (node.right.balanceFactor > 0) {
      return rightLeftRotation(node);
    }
    return leftRotation(node);
  }
  return node;
}
// end::balance[]

// tag::balanceUpstream[]
/**
 * Bubbles up balancing nodes a their parents
 *
 * @param {BinaryTreeNode} node
 */
function balanceUpstream(node) {
  let current = node;
  let newParent;
  while (current) {
    newParent = balance(current);
    current = current.parent;
  }
  return newParent;
}
/**
 * AVL Tree
 * It's a self-balanced binary search tree optimized for fast lookups.
 */
class AvlTree extends BinarySearchTree {
  /**
   * Add node to tree. It self-balance itself.
   * @param {any} value node's value
   */
  add(value) {
    const node = super.add(value);
    this.root = balanceUpstream(node);
    return node;
  }

  /**
   * Remove node if it exists and re-balance tree
   * @param {any} value
   */
  remove(value) {
    const node = super.find(value);
    if (node) {
      const found = super.remove(value);
      this.root = balanceUpstream(node.parent);
      return found;
    }

    return false;
  }
}

/**
 * Swap parent's child
 *
 * @example Child on the left side (it also work for the right side)
 *
 * p = parent
 * o = old child
 * n = new child
 *
 * p           p
 *  \    =>     \
 *   o           n
 *
 * @param {TreeNode} oldChild current child's parent
 * @param {TreeNode} newChild new child's parent
 * @param {TreeNode} parent parent
 */
function swapParentChild(oldChild, newChild, parent) {
  if (parent) {
    // this set parent child
    const side = oldChild.isParentRightChild ? 'Right' : 'Left';
    parent[`set${side}AndUpdateParent`](newChild);
  } else {
    // no parent? so set it to null
    newChild.parent = null;
  }
}
/**
 * Single Left Rotation (LL Rotation)
 *
 * @example: #1 tree with values 1-2-3-4
 *
 * 1                                 1
 *  \                                 \
 *   2*                                3
 *    \    --left-rotation(2)->      /  \
 *     3                           2*    4
 *      \
 *       4
 *
 * @example: #2 left rotation
 *
 *     1                                          1
 *      \                                          \
 *       4*                                         16
 *      / \                                        / \
 *    2    16    -- left-rotation(4) ->          4   32
 *       /  \                                   / \    \
 *      8   32                                2   8    64
 *            \
 *            64
 * @param {TreeNode} node current node to rotate (e.g. 4)
 * @returns {TreeNode} new parent after the rotation (e.g. 16)
 */
function leftRotation(node) {
  const newParent = node.right; // E.g., node 16
  const grandparent = node.parent; // E.g., node 1
  const previousLeft = newParent.left; // E.g., node 8

  // swap parent of node 4 from node 1 to node 16
  swapParentChild(node, newParent, grandparent);

  // Update node 16 left child to be 4, and
  // updates node 4 parent to be node 16 (instead of 1).
  newParent.setLeftAndUpdateParent(node);
  // set node4 right child to be previousLeft (node 8)
  node.setRightAndUpdateParent(previousLeft);

  return newParent;
}
/**
 * Single Right Rotation (RR Rotation)
 * @example: #1 rotate node 3 to the right
 *
 *       4                                  4
 *      /                                  /
 *     3*                                 2
 *    /                                 /  \
 *   2    ---| right-rotation(3) |-->  1    3*
 *  /
 * 1
 *
 * @example: #2 rotate 16 to the right and preserve nodes
 *       64                                       64
 *       /                                        /
 *      16*                                      4
 *      / \                                     / \
 *     4  32    -- right-rotation(16) -->      2   16
 *    / \                                     /    / \
 *   2   8                                   1    8   32
 *  /
 * 1
 *
 * @param {TreeNode} node
 *    this is the node we want to rotate to the right. (E.g., node 16)
 * @returns {TreeNode} new parent after the rotation (E.g., node 4)
 */
function rightRotation(node) {
  const newParent = node.left; // E.g., node 4
  const grandparent = node.parent; // E.g., node 64
  const previousRight = newParent.right; // E.g., node 8

  // swap node 64's left children (node 16) with node 4 (newParent).
  swapParentChild(node, newParent, grandparent);

  // update node 4's right child to be node 16,
  // also make node 4 the new parent of node 16.
  newParent.setRightAndUpdateParent(node);
  // Update 16's left child to be the `previousRight` node.
  node.setLeftAndUpdateParent(previousRight);

  return newParent;
}
/**
 * Left Right Rotation (LR Rotation)
 *
 * @example LR rotation on node 3
 *        4                       4
 *      /                        /                           4
 *    3                         3*                          /
 *  /                          /                           2
 * 1*   --left-rotation(1)->  2   --right-rotation(3)->  /  \
 *  \                        /                          1    3*
 *   2                      1
 *
 * @param {TreeNode} node
 *    this is the node we want to rotate to the right. E.g., node 3
 * @returns {TreeNode} new parent after the rotation
 */
function leftRightRotation(node) {
  leftRotation(node.left);
  return rightRotation(node);
}
/**
 * Right Left Rotation (RL Rotation)
 *
 * @example RL rotation on 1
 *
 *   1*                           1*
 *    \                            \                          2
 *      3   -right-rotation(3)->    2   -left-rotation(1)->  /  \
 *    /                              \                      1*   3
 *   2                                3
 *
 * @param {TreeNode} node
 * @returns {TreeNode} new parent after the rotation
 */
function rightLeftRotation(node) {
  rightRotation(node.right);
  return leftRotation(node);
}
```

## Graph

### BFS - shortest path

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec13.pdf

```js
let findShortestPath = (graph, startNode, endNode) => {
  // track distances from the start node using a hash object
  let distances = {};
  distances[endNode] = 'Infinity';
  distances = Object.assign(distances, graph[startNode]);
  // track paths using a hash object
  let parents = { endNode: null };
  for (let child in graph[startNode]) {
    parents[child] = startNode;
  }

  // collect visited nodes
  let visited = [];
  // find the nearest node
  let node = shortestDistanceNode(distances, visited);

  // for that node:
  while (node) {
    // find its distance from the start node & its child nodes
    let distance = distances[node];
    let children = graph[node];

    // for each of those child nodes:
    for (let child in children) {
      // make sure each child node is not the start node
      if (String(child) === String(startNode)) {
        continue;
      } else {
        // save the distance from the start node to the child node
        let newdistance = distance + children[child];
        // if there's no recorded distance from the start node to the child node in the distances object
        // or if the recorded distance is shorter than the previously stored distance from the start node to the child node
        if (!distances[child] || distances[child] > newdistance) {
          // save the distance to the object
          distances[child] = newdistance;
          // record the path
          parents[child] = node;
        }
      }
    }
    // move the current node to the visited set
    visited.push(node);
    // move to the nearest neighbor node
    node = shortestDistanceNode(distances, visited);
  }

  // using the stored paths from start node to end node
  // record the shortest path
  let shortestPath = [endNode];
  let parent = parents[endNode];
  while (parent) {
    shortestPath.push(parent);
    parent = parents[parent];
  }
  shortestPath.reverse();

  //this is the shortest path
  let results = {
    distance: distances[endNode],
    path: shortestPath,
  };
  // return the shortest path & the end node's distance from the start node
  return results;
};
```

### DFS - detext cycle - DAG - topological order

https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/MIT6_006F11_lec14.pdf

#### Detect cycle

```js
function Graph() {
  this.adjList = {};
}

Graph.prototype.addVertex = function (vertex) {
  this.adjList[vertex] = [];
};

Graph.prototype.addEdge = function (vertex1, vertex2) {
  this.adjList[vertex1].push(vertex2);
};

Graph.prototype.dfs = function () {
  const nodes = Object.keys(this.adjList);
  const visited = {};
  for (let i = 0; i < nodes.length; i++) {
    const node = nodes[i];
    this._dfsUtil(node, visited);
  }
};

Graph.prototype._dfsUtil = function (vertex, visited) {
  if (!visited[vertex]) {
    visited[vertex] = true;
    console.log(vertex, visited);
    const neighbors = this.adjList[vertex];
    for (let i = 0; i < neighbors.length; i++) {
      const neighbor = neighbors[i];
      this._dfsUtil(neighbor, visited);
    }
  }
};

Graph.prototype.detectCycle = function () {
  const graphNodes = Object.keys(this.adjList);
  const visited = {};
  const recStack = {};

  for (let i = 0; i < graphNodes.length; i++) {
    const node = graphNodes[i];
    if (this._detectCycleUtil(node, visited, recStack))
      return 'there is a cycle';
  }
  return 'no cycle!';
};

Graph.prototype._detectCycleUtil = function (vertex, visited, recStack) {
  if (!visited[vertex]) {
    visited[vertex] = true;
    recStack[vertex] = true;
    const nodeNeighbors = this.adjList[vertex];
    for (let i = 0; i < nodeNeighbors.length; i++) {
      const currentNode = nodeNeighbors[i];
      console.log('parent', vertex, 'Child', currentNode);
      if (
        !visited[currentNode] &&
        this._detectCycleUtil(currentNode, visited, recStack)
      ) {
        return true;
      } else if (recStack[currentNode]) {
        return true;
      }
    }
  }
  recStack[vertex] = false;
  return false;
};

const graph = new Graph();

graph.addVertex('A');
graph.addVertex('B');
graph.addVertex('C');
graph.addVertex('D');
graph.addVertex('E');

graph.addEdge('A', 'B');
graph.addEdge('D', 'E');
graph.addEdge('C', 'E');
graph.addEdge('A', 'D');
graph.addEdge('A', 'C');
graph.addEdge('E', 'B');
graph.addEdge('D', 'B');
graph.addEdge('E', 'A');
```

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

## Binary Search Trees

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
