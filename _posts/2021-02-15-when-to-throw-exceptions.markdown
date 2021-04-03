---
layout: post
title:  "Exceptions -- your last resort to handling errors"
date:   2021-02-15 12:25:08 +0000
categories: OOP, Kotlin, exceptions
--- 

### Code that went wrong

I had the pleasure of someone to use one of my library functions and he was quite surprised that an exception was being thrown for a required argument

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

<br>
A function that is better designed could look like this

{% highlight kotlin %}
fun getRandomSurprise(coin: GoldCoin, extraCoins: Collection<Coin>)
    : Surprise {
        // consume the coins and..
        // materialize a specific reward
        return Surprise("Materializing..a pecking gorilla")
}
{% endhighlight %}

#### Don't think in error terms

If exceptions have performance impact and they could clutter your code with try and catch blocks then why throw an exception?

Because -- a function is a unit that performs some work and if it can't complete for some reason it should notify for an error. 

But if you think about it for a second, an error could be a very broad term to describe a situation where things go wrong, which in the first example above was when the function is not receiving proper input arguments - a GoldCoin.

### When to use exceptions?

Exception are a <b>language feature</b> that expands on error codes and can be used to design your functions, but it comes with drawbacks and should be used ideally -- 

<i>only when all other programming <b>language features</b> cannot provide you with anything to design your function to work correctly.</i>