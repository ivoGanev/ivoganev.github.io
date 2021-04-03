---
layout: post
title:  "Exceptions -- your last resort to handling errors"
date:   2021-02-15 12:25:08 +0000
categories: OOP, Kotlin, exceptions
--- 

### Is this code exceptional?

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
<br>

But why did I throw exception in the first code example? My definition of a function led me to that decision.

My definition of a function -- a unit that performs some work and if it can't complete for some reason it should notify for an error. 

But what do you consider an error?

### Don't think in terms of errors

If you think about it for a second, an error could be a very broad term to describe a situation where things go wrong, which in the first example above, for me was when the function is not receiving enough input arguments. 

So I considered the case of a missing argument for an error and this is where the problem lies -- thinking in terms of errors can make you throw exceptions everywhere.

### When to use exceptions?

Exceptions are a <b>language feature</b> that expands on error codes and can be used to design your functions, but they comes with drawbacks and should be used ideally -- 

<i>only when all other programming language features and/or design choices leave you no other option.</i>

When you find yourself asking the question 'Should I throw an exception?' you should consider turning it around and asking 'How can I design this without making it fail?'