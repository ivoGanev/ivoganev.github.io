---
layout: post
title:  "Know your data structures"
date:   2021-03-17 16:45:08 +0000
categories: hash, hash tables
---

### What is a data structure?
Data is just a bunch of collected information used for some purpose. Imagine that you have a list of your friends age in you head being there for the purpose to be sorted so you know who is the oldest one; your friends age would be your data and surprise, your data structure is your brain.

An example of a physical data structure is a sheet of paper containing a list of names. That list of names becomes and the sheet of paper holding that data is your data structure.

Data structures are used to store the collected data, organize(sort) it and manage(insert, update, remove) it. You can imagine your data being sorted when you work on it with your brain and can sort it manually when its on paper but lets talk about computers because the term data structure itself is meant to be used with them.

### Many data structures

There are many created data structures like: arrays, lists, hash tables and so on, but why do we need them all? Can't we just use an array for everything? Yes we can but it turns out that computers have speed limits and, for example, searching for your friends name in array of 1 000 000 names in the worst case could take 1 000 000 searches. 

Also, data structures can communicate intention and abstract some of their functionality away. Imagine you are making an infinite runner game where you need to generate floor tiles. The player runs to the right, the camera follows him and decide to remove the block to the left side which is no longer visible and generate another on the right side of the screen so that it looks like the terrain is incredibly long. Yes you can use a list to add and remove tiles but why not use a queue? That way you clearly communicate with other developers that blocks are only to be added to the front and removed from the back.

#### Arrays

Arrays are great containers to store your data and retrieve it by an index. Creating an array like `int arr[4]` would reserve space in computers memory, say at location 1000, and each integer would take 4 bytes so the addresses become something like this:

```
 1000 1004 1008 1012 1016
┌────┬────┬────┬────┬────┐
│    │    │    │    │    │
│    │    │    │    │    │
└────┴────┴────┴────┴────┘
```
You can easily access an element in the defined memory address by providing the `base_address + (index * element_byte_size)`. 

Question: You now see how an array data structure is created and accessed but how would you insert or delete an item? 

Answer: By creating a brand new array and that means copying the values one by one from the first array to the second. 

And what if you need to search an array by the data and not by the index, for example, an array of million names? Searching, insertion and is not really efficient using arrays, to be more specific it takes O(n)(linear time complexity).


#### ArrayList (Java)

Java's/Oracle programmers are kind enough to provide an `ArrayList` which is just an array of objects and you get operations like `add()`, `addAll()`, `remove()`, etc., which are eventually enables you to use the arrays as list. Because you are still dealing with an array data structure, time complexity remains the same as the topic above has mentioned.

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

Open Addressing

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

Closed Addressing

One thing that is really great about data structures is that we can mix them together. Closed addressing is another way to solve collisions by instead directly adding the item in the array slot we use a linked list nodes and if there is a collision we can add more nodes in the same slot.

To find an item you just need to generate the hash function and get the index and that way the lookup would be fast.

Objective of Hash Function

- Minimize collisions
- Uniform distribution of hash functions
- Easy to calculate
- Resolve any collisions

Sources:

[Hash Tables and Hash Functions](https://www.youtube.com/watch?v=KyUTuwz_b7Q&ab_channel=ComputerScience)