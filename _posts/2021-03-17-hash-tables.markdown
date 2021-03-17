---
layout: post
title:  "Hash tables explained simple"
date:   2021-03-17 16:45:08 +0000
categories: hash, hash tables
---

I stumbled upon a great video explaining hash tables that makes it easy to understand them.

[Hash Tables and Hash Functions](https://www.youtube.com/watch?v=KyUTuwz_b7Q&ab_channel=ComputerScience)

### What are they good for?

Hash tables are used for extremely fast retrieval of data no matter how much data there is. Considering this array:

```
  0   1   2   3   4   5   6   7   8   9  10
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│Jan│Tim│Mia│Sam│Leo│Ted│Bea│Lou│Ada│Max│Zoe│
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

You can access easily items from an index but what if you need search to for a name? In the worst case scenario that would require a linear search through the entire array. It turns out that you can use the data to calculate the index number -- <i>a hashing function</i>. For this example lets use the hashing function: 

Index Number = sum of the ASCII codes MOD size of array.

If we then place all elements according to the function above we get:

```
  0   1   2   3   4   5   6   7   8   9  10
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│Bea│Tim│Leo│Sam│Mia│Zoe│Jan│Lou│Max│Ada│Ted│
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

With this function we can easily find let's say Ada by simply adding the ASCII codes MOD size of array.

### What is hashing algorithm

Hashing algorithm also known as a hashing function


