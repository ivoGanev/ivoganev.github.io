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

But why did I throw exception here?

My definition of a function -- a unit that performs some work and if it can't complete for some reason it should notify for an error -- led me to that decision. 

But what do we consider an error?

### Don't design thinking in terms of errors

An error could be a very broad term to describe a situation where things go wrong, which in the example code above was when the function is not receiving a required argument. But thinking in terms of errors can make you biased.

Looking at the library function again, there is much better design for it

{% highlight kotlin %}
fun getRandomSurprise(coin: GoldCoin, extraCoins: Collection<Coin>)
    : Surprise {
        // consume the coins and..
        // materialize a specific reward
        return Surprise("Materializing..a pecking gorilla")
}
{% endhighlight %}
<br>

It turns out that just by thinking if you should throw exception or not, your thought process is taking you through a path that maybe you didn't even desired. In this case notice that requiring an argument can be achieved by just a simple language feature (putting the argument in the method body). 

### When to use exceptions?

Exceptions are a <b>language feature</b> that expands on error codes and can be used to design your functions to handle failures, but they come with [drawbacks](https://wiki.c2.com/?AvoidExceptionsWheneverPossible){:target="_blank"} and (to me)

{:refdef: style="text-align: center;"}
 <i>should be used ideally only when all other programming language features and/or design choices leave you no other option.</i>
{: refdef}

When you find yourself asking a question like 'Should I throw an exception?' you should consider turning it around and asking 'How can I design this function without making it fail?'