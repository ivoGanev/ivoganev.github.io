---
layout: post
title:  "today-I-learned #1"
date:   2021-02-15 12:25:08 +0000
categories: OOP, Kotlin, exceptions
---

This is the first article of a series that are intended to be a diary of my journey in programming. 

A lot of the times you are learning by making mistakes. There is always something to be gained and remembered when mistakes are made and I believe that is nothing to be ashamed from. 

On the contrary I feel that when you are not accepting your failures as a chance to learn but as an embarrassment and covering it up you are locking up a great potential of discoveries. 

### When you require it, put it as an argument

I had the pleasure of someone to use one of my library functions and he was quite surprised that an exception was being thrown for a required argument. I won't post my library code but will show a pretty accurate example of what was wrong:

{% highlight kotlin %}
fun getRandomSurprise(coins: Collection<Coin>): Surprise {
    // A surprise costs minimum a golden coin.
    val goldCoin = coins.find { coin -> coin is GoldCoin }
        ?: throw IllegalArgumentException(
            "At least one golden coin is required."
            )
    // consume the coins and..
    // materialize a specific reward
    return Surprise("Materializing..a pecker gorilla.")
}
{% endhighlight %}

A function that is better designed could look like this:

{% highlight kotlin %}
fun getRandomSurprise(coin: GoldCoin, extraCoins: Collection<Coin>)
    : Surprise {
        // consume the coins and..
        // materialize a specific reward
        return Surprise("Materializing..a pecking gorilla")
}
{% endhighlight %}

I knew that exceptions have performance impact and they could clutter your code with try and catch blocks but why I decided to throw an exception there?

I believe it was a flaw in my logic which led me to this situation and more specifically: A function is a unit that performs some work and if it can't complete for some reason it should notify for an error. 

At that point when creating the function I was thinking "This function will return a surprise from a bag of coins and can only work when at least one gold coin is present.". Notice the part: "..can only work when.." decides that an error needs to be thrown if someone forgets to put that golden coin in.

### Today I learned

Exceptions are to be used in exceptional circumstances, more specifically -- 

<i>only when all other programming language features cannot provide you with anything that can make your function work correctly.</i>  