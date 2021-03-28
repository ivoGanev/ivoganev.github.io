---
layout: post
title:  "Know your data structures"
date:   2021-03-17 16:45:08 +0000
categories: hash, hash tables
---

## What is a data structure?

Data is a collection of facts or description of things, for example, a list of names. Data structures are containers that are used to <i>efficiently</i>  store, organize(sort) and manage(insert, update, remove) the collected data. 

A sheet of paper could be a simple example of what a data structure is. It can store data but organizing and managing the data needs to be done manually by someone, and could become a tedious and inefficient process really quick. 

{:refdef: style="text-align: center;"}
![what is data](/assets/data-structures/what-is-data.png)
{: refdef}
<br>

## Why data structures?

Arrays, lists, hash tables and many more, are all data structures but why do we need them all? Can't we just use a list-like data structure for everything? There are two main reasons to use different data structures which are: <i>efficiency</i> and <i>communicating intention</i>.

##### Efficiency

Computers have speed limits and, for example, searching for your friends name in array of 1 000 000 names in the worst case could take 1 000 000 searches depending on how the array was sorted. 


##### Communicating intention

Imagine you are making a 2D infinite runner game where you need to generate floor tiles. As the player runs, new tiles are being created at the front and removed from the back. You can use any data structure to store the current tiles but why not use a queue? That way you clearly communicate with other developers that blocks are only to be added to the front and removed from the back.

##### Composability

Yet another interesting feature of data structures is that they can be made from other data structures.

### Arrays

Imagine an array as a bunch of ordered boxes.

{:refdef: style="text-align: center;"}
![arrays as boxes](/assets/data-structures/arrays-as-boxes.png)
{: refdef}
<br>

The boxes are neatly arranged next to each other and each box can store a single item, which is usually called <i>element</i>. To access a stored element you need to provide a number called the array's <b>index</b>. The count of the boxes is called the array's <b>length</b>.

{:refdef: style="text-align: center;"}
![arrays breakdown](/assets/data-structures/arrays-breakdown.png)
{: refdef}

<br>

#### Creating an array

Instead of physical boxes, computers store the array elements inside a memory address. When you create an array, say in Java, a memory address needs to be reserved to store the required amount of elements. Let's create an array to hold the integer -- ten.

{% highlight java %}
// reserve a space for one element in memory
anArray = new int[1];
    
// set the integer of 10 to be the first item
anArray[0] = 10;
{% endhighlight %}

Eventually behind the scenes deep inside computer's memory, the data can be stored only in bits, e.g. 0's and 1's. Memory addresses have space only for eight bits but integers in Java are 32 bits. To store all the 32 bits the system reserves four addresses. 

{:refdef: style="text-align: center;"}
![arrays in memory](/assets/data-structures/arrays-memory-creation.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i>four addresses are reserved to store an integer in bits</i>
{: refdef}

<br>


Now let's create an array to store 10,20,30,40 and 50 instead of just only one number.

{% highlight java %}
// reserve a space for five elements in memory
anArray = new int[5];
    
anArray[0] = 10;
anArray[1] = 20;
anArray[2] = 30;
anArray[3] = 40;
anArray[4] = 50;
{% endhighlight %}

{:refdef: style="text-align: center;"}
![items in array](/assets/data-structures/arrays-in-memory.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i>this is how multiple stored elements look like in memory</i>
{: refdef}

#### Accessing an array

For convenience, array elements are accessed by using just their index, not the memory address. This is possible because we can simply use the following formula:

<br>

> Base Memory Address + (Item's Array Index + Item's Size In Bytes) 

<br>

The first integer is therefore retrieved with the expression: 1000 + 0 * 4 = 1000 hence you can see why the first element is accessed with the index 0.

Accessing an array by its index is quite fast and takes a constant time, i.e. O(1). 

#### Inserting and deleting

Arrays are not dynamic data structures meaning that it is not possible to add or remove items without recreating the array which makes the time complexity for these operations linear, i.e. O(n).

#### Searching for an element

An element in an array can be quickly retrieved by providing its index but if you are searching for something specific, for example "Bob" in array of strings, in the worst case you would have to loop the entire array making the time complexity O(n). If you are looking to search quickly for an element inside a data structure take a look at hash tables.

### ArrayList (Java)

`ArrayList` in Java is just an array of objects with operations for adding, inserting and removing elements. Because you are still dealing with an array data structure, what happens behind the scenes is that the array is still copied when these operations are used.

### Singly Linked List

In a singly linked list each new added element has a reference to the previous one and the last points to null. Items are efficiently added and removed only at the front of the list.

{:refdef: style="text-align: center;"}
![singly linked list](/assets/data-structures/singly-linked-list.png)
{: refdef}
<br>

This data structure is not backed by an array but a special <i>head</i> variable that keeps track of the first element for quick access. If say, we want to add a new item to the list called Jim the list would look like this

{:refdef: style="text-align: center;"}
![singly linked list add](/assets/data-structures/singly-linked-list-add.png)
{: refdef}
<br>

Adding and removing an element to the front only is done in O(1) time complexity. Searching is in linear time.

### Doubly linked list

This data structure is an improvement of singly linked list which in addition keeps track of first element with a variable named <i>tail</i>. 
{:refdef: style="text-align: center;"}
![doubly linked list](/assets/data-structures/doubly-linked-list.png)
{: refdef}
<br>

Compared to singly linked list now we can add and remove items to the <i>front and back</i> with O(1) time complexity but we still can't search and access the data structure in a very efficient way.

#### Uses

- A music player which has next and prev buttons.
- Represent a deck of cards in a game.
- Undo-Redo functionality

### Circular Doubly Linked List

This is a doubly linked list which doesn't have null nodes. The last node points to the first node and the first node points to the last.

{:refdef: style="text-align: center;"}
![circular doubly linked list](/assets/data-structures/circular-doubly-linked-list.png)
{: refdef}
<br>

#### Uses

- Playing the next song in a music list if the last track was ended.
 (Singly Circular Linked List)
- Windows tab rotating (e.g Alt+Tab)
- Keep track of players in a multiplayer game

### Hash Tables
Hash tables are used for extremely fast search, insert and delete operations of data no matter how much data there is. Take this array for example:

```
  0   1   2   3   4   5   6   7   8   9  10
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│Bea│Tim│Leo│Sam│Mia│Zoe│Jan│Lou│Max│Ada│Ted│
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```
You know that getting elements by index is easy but how do we get them by looking for the element itself?. It turns out that you can use the data to calculate the index number -- <i>a hashing function</i>. For this example lets use the hashing function: 

Index Number = sum of the ASCII codes MOD size of array.

If we then place all elements according to the function above we get:

```
  0   1   2   3   4   5   6   7   8   9  10
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│Jan│Tim│Mia│Sam│Leo│Ted│Bea│Lou│Ada│Max│Zoe│
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```
With this function we can easily find let's say Ada by simply adding the ASCII codes MOD size of array.

##### Open Addressing
---

The hash function above conveniently didn't cause any problems but needless to say that was unrealistic. Sometimes when you apply a hashing function to two different keys it generates the same index number for both but both items can't go in the same place; this generates a <i>collision</i>.

Lets say that the hashing function generated the indexes for: "Jan","Tim","Mia","Sam","Bea" and "Lou":

```
Indexes generated are: 0,1,2,3,6 and 7.

  0   1   2   3   4   5   6   7   8   9  10
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│Jan│Tim│Mia│Sam│   │   │Bea│Lou│   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

Now imagine that for "Ted" the hashing function generates index 1 but there is already an element "Tim" there so what should we do? 

Resolving a collision by placing an item somewhere than its calculated address is called <i>open addressing</i> because every location is open to any item.

One open addressing technique we can use to resolve our current collisions is <i>linear probing</i>(a linear search) which tries to find where the next available slot is by searching one by one each element after the collided index. If it gets to the end of the array and it still can't find any space to place the item it might cycle around to the beginning of the array and continue searching from there.

To look up for an item in the table the hashing function is used but if there are collisions then linear probing has to be used again. 


The more items there are in a hash table the bigger the chance of collision when inserting more data. One way to deal with this is to create a bigger hash table, perhaps one that it is always 70% occupied. The ratio of the total number stored and the size of the array is called the <i>load factor</i>. If the hash table is implemented as a dynamic data structure then it can be auto-resized when the load factor reaches a certain threshold. 

Open Addressing Solutions:

- Linear probing - One problem that linear probing creates is called <i>primary clustering</i> where keys might bunch inside the array while large portions remain unoccupied. 
- Plus 3 rehash - Instead every next slot we probe every third slot along until a free space is found.
- Quadratic probing - Each time a slot hasn't been found the distance grows rapidly.
- Double hashing - Applies a second hash function when the collision fails

##### Closed Addressing
--- 

One thing that is really great about data structures is that we can mix them together. Closed addressing is another way to solve collisions by instead directly adding the item in the array slot we use a linked list nodes and if there is a collision we can add more nodes in the same slot.

To find an item you just need to generate the hash function and get the index and that way the lookup would be fast.

Objective of Hash Function

- Minimize collisions
- Uniform distribution of hash functions
- Easy to calculate
- Resolve any collisions

### Stacks

{:refdef: style="text-align: center;"}
![Stack of coins](/assets/data-structures/g890.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> A stack is just like a pile of coins</i>
{: refdef}

Imagine what you can do with a stack of coins 

- Put a coin on top
- Remove the top coin
- Examine the top coin without removing it

{:refdef: style="text-align: center;"}
![Stack - How it works](/assets/data-structures/g123.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> push and pop operations</i>
{: refdef}

##### Implementation Summary

Stacks can be backed by any data structure but the one that makes most sense is the linked list.

##### Time Complexity

For the linked list implementation of a stack, the push and pop operations take constant time, i.e. O(1).

##### Stack Operations

- <b>Push</b>: Add an element to the top of a stack
- <b>Pop</b>: Remove an element from the top of a stack
- <b>IsEmpty</b>: Check if the stack is empty
- <b>IsFull</b>: Check if the stack is full
- <b>Peek</b>: Get the value of the top element without removing it

##### Use case examples

- Undoing commands
- To reverse a word - Put all the letters in a stack and pop them out. Because of the LIFO order of stack, you will get the letters in reverse order.
- In compilers - Compilers use the stack to calculate the value of expressions like 2 + 4 / 5 * (7 - 9) by converting the expression to prefix or postfix form.
- In browsers - The back button in a browser saves all the URLs you have visited previously in a stack. Each time you visit a new page, it is added on top of the stack. When you press the back button, the current URL is removed from the stack, and the previous URL is accessed.

#### Queues

You can imagine this data structure as a real queue for a roller coaster

- One person comes first waiting on line, and then it comes another one. - This operation is called enqueue.
- The first person has priority to get on the train and is <i>dequeued</i> when he gets in.

##### Basic Operations of Queue
A queue is an object (an abstract data structure - ADT) that allows the following operations:

- <b>Enqueue</b>: Add an element to the end of the queue
- <b>Dequeue</b>: Remove an element from the front of the queue
- <b>IsEmpty</b>: Check if the queue is empty
- <b>IsFull</b>: Check if the queue is full
- <b>Peek</b>: Get the value of the front of the queue without removing it

{:refdef: style="text-align: center;"}
![queue operations](/assets/data-structures/queue.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> queue operations </i>
{: refdef}

##### Time Complexity

For the linked list implementation of a queue, the queue and dequeue operations take constant time, i.e. O(1).

##### Implementation Summary

Queues can be backed by any data structure but the one that makes most sense is a doubly linked list.

##### Use case examples

- CPU scheduling, Disk Scheduling
- When data is transferred asynchronously between two processes.The queue is used for synchronization. For example: IO Buffers, pipes, file IO, etc
- Handling of interrupts in real-time systems.
- Call Center phone systems use Queues to hold people calling them in order.
- In games - Creating an infinite runner game to generate queue floor tiles when the camera moves forward.

#### Trees

##### Why tree data structures?

Other data structures such as arrays, linked list, stack, and queue are linear data structures that store data sequentially. In order to perform any operation in a linear data structure, the time complexity increases with the increase in the data size. But, it is not acceptable in today's computational world.

Different tree data structures allow quicker and easier access to the data as it is a non-linear data structure using hierarchy.

Node

A node is an entity that contains a key or value and pointers to its child nodes.

The last nodes of each path are called leaf nodes or external nodes that do not contain a link/pointer to child nodes.

The node having at least a child node is called an internal node.

###### Edge
It is the link between any two nodes.

{:refdef: style="text-align: center;"}
![Nodes and edges of a tree](/assets/data-structures/nodes-edges_0.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> Nodes and edges of a tree </i>
{: refdef}

###### Root
It is the topmost node of a tree.

###### Height of a Node
The height of a node is the number of edges from the node to the deepest leaf (ie. the longest path from the node to a leaf node).

###### Depth of a Node
The depth of a node is the number of edges from the root to the node.

###### Height of a Tree
The height of a Tree is the height of the root node or the depth of the deepest node.

{:refdef: style="text-align: center;"}
![Height and depth of each node in a tree](/assets/data-structures/height-depth_0.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> Height and depth of each node in a tree </i>
{: refdef}

###### Degree of a Node
The degree of a node is the total number of branches of that node.

###### Forest
A collection of disjoint trees is called a forest.

{:refdef: style="text-align: center;"}
![Creating forest from a tree](/assets/data-structures/forest_0.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> Creating forest from a tree </i>
{: refdef}


You can create a forest by cutting the root of a tree.

###### Child vs Sibling

A child relationship is when one element (the child) is inside another element (the parent). A sibling relationship is between two elements that are both inside the same parent element.

###### Where do trees become useful?

Trees can be used to model: Ancestry Trees, Mind Maps, HTML nodes(DOM), Compilers, and so on.  

##### Binary Tree

A binary tree is a tree data structure in which each parent node can have at most two children.

[Example Image]

##### Full Binary Tree

A full Binary tree is a special type of binary tree in which every parent node/internal node has either two or no children.

It is also known as a proper binary tree.

[Example Image]

##### Perfect Binary Tree

A perfect binary tree is a type of binary tree in which every internal node has exactly two child nodes and all the leaf nodes are at the same level.

##### Complete Binary Tree

A complete binary tree is a binary tree in which all the levels are completely filled except possibly the lowest one, which is filled from the left.

A complete binary tree is just like a full binary tree, but with two major differences

All the leaf elements must lean towards the left.
The last leaf element might not have a right sibling i.e. a complete binary tree doesn't have to be a full binary tree.

##### Balanced Binary Tree

A balanced binary tree, also referred to as a height-balanced binary tree, is defined as a binary tree in which the height of the left and right subtree of any node differ by not more than 1.

To learn more about the height of a tree/node, visit Tree Data Structure.Following are the conditions for a height-balanced binary tree:

difference between the left and the right subtree for any node is not more than one
the left subtree is balanced
the right subtree is balanced

##### Binary Search Tree

Binary search tree is a data structure that quickly allows us to maintain a sorted list of numbers.

It is called a binary tree because each tree node has a maximum of two children.
It is called a search tree because it can be used to search for the presence of a number in O(log(n)) time.
The properties that separate a binary search tree from a regular binary tree is

All nodes of left subtree are less than the root node
All nodes of right subtree are more than the root node
Both subtrees of each node are also BSTs i.e. they have the above two properties

###### Search Operation

The algorithm depends on the property of BST that if each left subtree has values below root and each right subtree has values above the root.

If the value is below the root, we can say for sure that the value is not in the right subtree; we need to only search in the left subtree and if the value is above the root, we can say for sure that the value is not in the left subtree; we need to only search in the right subtree.

##### AVL tree

###### Advantages

The main advantage of AVL tree over BST(Binary Search Tree), comes when the BST is not balanced.

{:refdef: style="text-align: center;"}
![Binary Search Trees may be balanced, meaning that results like this happen](/assets/data-structures/binary-search-tree_0.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> Binary search trees can produce skewed results like this.</i>
{: refdef}

A search though the BST in the case above would take linear time but a search through an AVL tree would take always take logarithmic. An AVL tree with the same data as above would look like this:

{:refdef: style="text-align: center;"}
![AVL Tree](/assets/data-structures/avl-tree-0.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> An AVL tree is balanced compared to BST.</i>
{: refdef}

###### Disadvantages

When inserting or deleting, a node in an AVL tree may become critical or unbalanced and then the tree has to be reorganized. AVL tree structures can be used in situations which require fast searching but the large cost of balancing may limit the usefulness of it.

###### Uses

- Language dictionaries, as they are loaded once and are frequently used for retrieval.
- They may also be used in Huffman trees for data compression.
- Expression solvers and parsers.
- They are also used in implementations of java.util.Set in Java.

##### Heap Data Structure (Priority Queue)

A Heap is a complete binary tree, that is each level of the tree is completely filled, except possibly the bottom level. At this level, it is filled from left to right. 

In addition to being a complete binary tree,the values in the heap data structure have to be arranged by using one of the two possible ways:

- Max-Heap -- from highest to lowest value or
- Min-Heap -- from lowest to highest value

Take this binary tree with random values for example
{:refdef: style="text-align: center;"}
![Complete binary tree](/assets/data-structures/complete-binary-tree.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> A complete binary tree.</i>
{: refdef}

To create heap data structure from the binary tree we need to use a process known as <i>heapify</i> which just swaps the positions of the elements to create an outcome like this:

{:refdef: style="text-align: center;"}
![Heap data structure](/assets/data-structures/heap.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i> A complete binary tree arranged into a MAX-HEAP heap data structure</i>
{: refdef}

###### Heapify

Heapify is the process of creating a heap data structure from a binary tree. It is used to create a Min-Heap or a Max-Heap.

###### Insert

Insertion is done at the last level, last node from left to right. Has time complexity of O(log n) because in the worst case one swap of elements would be needed on every level.

###### Delete

Delete just swaps the last element with the one to be deleted and heapifies the tree to become heap again. Has time complexity of O(log n)

###### Peek

Get the first element.

###### Search

Searching is like searching an array and the time complexity is linear.

###### Uses

Heaps allow you to get the smallest or the biggest value quickly and know the NEXT smallest or biggest. That's why it's called a Priority Queue.

#### Graphs

Graphs are used to solve real-life problems that involve representation of the problem space as a network. Examples of networks include telephone networks, circuit networks, social networks (like LinkedIn, Facebook etc.).

For example, a single user in Facebook can be represented as a node (vertex) while their connection with others can be represented as an edge between nodes.

Each node can be a structure that contains information like user’s id, name, gender, etc.

###### Adjacency Matrix

An Adjacency Matrix is a 2D array of size V x V where V is the number of nodes in a graph. A slot matrix[i][j] = 1 indicates that there is an edge from node i to node j.

{:refdef: style="text-align: center;"}
![Graph represented as Adjacency Matrix](/assets/data-structures/graph-matrix.png)
{: refdef}
{:refdef: style="text-align: center;"}
<i>Graph represented as Adjacency Matrix</i>
{: refdef}

###### Adjacency List

An adjacency list represents a graph as an array of linked lists.

The index of the array represents a vertex and each element in its linked list represents the other vertices that form an edge with the vertex.

The adjacency list for the graph we made in the first example is as follows:

Sources:

[Hash Tables and Hash Functions](https://www.youtube.com/watch?v=KyUTuwz_b7Q&ab_channel=ComputerScience)

