---
layout: post
title:  "Know your data structures"
date:   2021-03-17 16:45:08 +0000
categories: hash, hash tables
---

### What is a data structure?
Data is just a bunch of collected information used for some purpose. Imagine that you have a list of your friends age in you head being there for the purpose to be sorted so you know who is the oldest one; your friends age would be your data and surprise, your data structure is your brain.

Data structures are used to store the collected data and organize(sort) and manage(insert, update, remove) it efficiently. 

An example of a physical data structure is a sheet of paper containing a list of names. That list of names becomes the data and the sheet of paper holding is your data structure which you can effectively modify with your pen.


### Why are there so many data structures?

Arrays, lists, hash tables and so on, are all data structures but why do we need them all? Can't we just use an array for everything? Let's take a step back and look at what makes an array a data structure -- it is a container for data and can organize and manage the data. But is it <i>efficient</i> ?

It turns out that computers have speed limits and, for example, searching for your friends name in array of 1 000 000 names in the worst case could take 1 000 000 searches depending on how the array was sorted. 

Also, data structures can communicate intention. Imagine you are making an infinite runner game where you need to generate floor tiles. The player runs to the right, the camera follows him and decide to remove the block to the left side which is no longer visible and generate another on the right side of the screen so that it looks like the terrain is incredibly long. Yes you can use a list to add and remove tiles but why not use a queue? That way you clearly communicate with other developers that blocks are only to be added to the front and removed from the back.

#### Arrays

Arrays are great containers to store your data and retrieve it by an index. Creating an array like `int arr[4]` would reserve space in your computer's memory, say at location 1000, and each integer would take 4 bytes so the addresses would look something like this:

```
 1000 1004 1008 1012 1016
┌────┬────┬────┬────┬────┐
│    │    │    │    │    │
│    │    │    │    │    │
└────┴────┴────┴────┴────┘
```
You can easily access an element in the defined memory address by using `base_address + (index * element_byte_size)`. 

Question: You now see how an array data structure is created and accessed but how would you insert or delete an item? 

Answer: By creating a brand new array and that means copying the values one by one from the first array to the second. 

And what if you need to search an array by the data and not by the index, for example, an array of million names? Searching, insertion and is not really efficient using arrays, to be more specific it takes O(n)(linear time complexity).


#### ArrayList (Java)

Java's/Oracle programmers are kind enough to provide an `ArrayList` which is just an array of objects and you get operations like `add()`, `addAll()`, `remove()`, etc., which eventually enables you to use the arrays as list. Because you are still dealing with an array data structure, time complexity for operations remains the same.

#### Singly Linked List

A singly linked list looks like this:

```
┌─────┬┬────┬┬────┬┐
│ Jon ││Tim ││Zoe ││
│     │┼─►  │┼─►  │┼─► Null
│  ▲  ││    ││    ││
└──┬──┴┴────┴┴────┴┘
   │
  head
```

Each element in this data structure contains a special node which points to the previous one, and the last points to null. Items are efficiently added and removed at the front of the list.

The <i>head</i> is used to store a reference to the first item of the list. If say, we want to add a new item to the list called Jim the list would look like this:

```
┌─────┬┬─────┬┬────┬┬────┬┐
│ Jan ││ Jon ││Tim ││Zoe ││
│     │┼─►   │┼─►  │┼─►  │┼─► Null
│  ▲  ││     ││    ││    ││
└──┬──┴┴─────┴┴────┴┴────┴┘
   │
  head
```

Adding and removing an element to the front only is done in O(1) time complexity but thats about it.

#### Doubly linked list

Here is how the doubly linked list looks like:
```
       ┌─┬──────┬─┬────────┬─┬────────┬─┬────────┬─┐
       │ │      │ ├─►      │ ├─►      │ ├─►      │ ├──►null
       │ │ Jane │ │ │ John │ │ │ Mike │ │ │ Jess │ │
null◄──┤ │      ◄─┤ │      ◄─┤ │      ◄─┤ │      │ │
       └─┴────────┴─┴────────┴─┴────────┴─┴──────┴─┘
```

Compared to singly linked list now we can add and remove items to the <i>front and back</i> with O(1) time complexity but we still can't search and access the data structure in a very efficient way.

#### Hash Tables
Behold the hash tables.

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

###### Open Addressing
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

###### Closed Addressing
--- 

One thing that is really great about data structures is that we can mix them together. Closed addressing is another way to solve collisions by instead directly adding the item in the array slot we use a linked list nodes and if there is a collision we can add more nodes in the same slot.

To find an item you just need to generate the hash function and get the index and that way the lookup would be fast.

Objective of Hash Function

- Minimize collisions
- Uniform distribution of hash functions
- Easy to calculate
- Resolve any collisions

#### Stacks

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

###### Implementation Summary

Stacks can be backed by any data structure but the one that makes most sense is the linked list.

###### Time Complexity

For the linked list implementation of a stack, the push and pop operations take constant time, i.e. O(1).

###### Stack Operations

- <b>Push</b>: Add an element to the top of a stack
- <b>Pop</b>: Remove an element from the top of a stack
- <b>IsEmpty</b>: Check if the stack is empty
- <b>IsFull</b>: Check if the stack is full
- <b>Peek</b>: Get the value of the top element without removing it

###### Use case examples

- Undoing commands
- To reverse a word - Put all the letters in a stack and pop them out. Because of the LIFO order of stack, you will get the letters in reverse order.
- In compilers - Compilers use the stack to calculate the value of expressions like 2 + 4 / 5 * (7 - 9) by converting the expression to prefix or postfix form.
- In browsers - The back button in a browser saves all the URLs you have visited previously in a stack. Each time you visit a new page, it is added on top of the stack. When you press the back button, the current URL is removed from the stack, and the previous URL is accessed.

#### Queues

You can imagine this data structure as a real queue for a roller coaster

- One person comes first waiting on line, and then it comes another one. - This operation is called enqueue.
- The first person has priority to get on the train and is <i>dequeued</i> when he gets in.

###### Basic Operations of Queue
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

###### Time Complexity

For the linked list implementation of a queue, the queue and dequeue operations take constant time, i.e. O(1).

###### Implementation Summary

Queues can be backed by any data structure but the one that makes most sense is a doubly linked list.

###### Use case examples

- CPU scheduling, Disk Scheduling
- When data is transferred asynchronously between two processes.The queue is used for synchronization. For example: IO Buffers, pipes, file IO, etc
- Handling of interrupts in real-time systems.
- Call Center phone systems use Queues to hold people calling them in order.
- In games - Creating an infinite runner game to generate queue floor tiles when the camera moves forward.

#### Trees

This one is also easy if you imagine a tree with branches. 

Other data structures such as arrays, linked list, stack, and queue are linear data structures that store data sequentially. When you perform an operation in a linear data structure, the time complexity increases with the increase in the data size. 

###### Where do trees become useful?

Trees can be used to model: Ancestry Trees, Mind Maps, HTML nodes(DOM), and so on.  

Sources:

[Hash Tables and Hash Functions](https://www.youtube.com/watch?v=KyUTuwz_b7Q&ab_channel=ComputerScience)

