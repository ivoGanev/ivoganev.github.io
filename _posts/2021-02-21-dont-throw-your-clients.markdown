---
layout: post
title:  "Assumptions and throwing functions"
date:   2021-02-21 10:46:08 +0000
categories: OOP
---
Every function asks a question.

One day I found myself writing something similar to this:

{% highlight kotlin %}
abstract class Coin
class SilverCoin : Coin()
class GoldCoin: Coin()

fun main(args: Array<String>) {
    getRandomSurprise(listOf(SilverCoin()))
}

// A random surprise costs minimum a golden coin and
// adding extra coins would yield an even better outcome.
fun getRandomSurprise(coins: List<Coin>) : String {
    // A surprise costs minimum a golden coin.
    val goldCoin = coins.find { coin -> coin is GoldCoin }
        ?: throw IllegalArgumentException("At least 1 coin is required.")

    // consume the coins and..
    // materialize a specific reward
    return "Materializing..a holy donkey."
}
{% endhighlight %}

Maybe you can already see the problem, but at that time I didn't. The decision of throwing exception when the minimum requirement, in this case one gold coin, was not met didn't appear wrong to me. Now it does.

When a function asks a question it would assume something.

In this case the function above is asking: <i>"Do we have a list of coins?"</i>. There is an assumption made from this question - there should be a list of coins; any coin would do. It should execute flawlessly when provided with that argument.

{% highlight kotlin %}
// A random surprise costs minimum a golden coin and
// adding extra coins would yield an even better outcome.
fun getRandomSurprise(coin: GoldCoin, extraCoins: List<Coin>) : String {
    // consume the coins and..
    // materialize a specific reward
    return "Materializing..a pecking gorilla"
}
{% endhighlight %}

Now the function is asking: <i>"Do we have a golden coin and a bag of extra coins on the side?"</i>. The assumption is that there should be a golden coin and a bag(even empty would do) of extra coins on the side.

I decided to look at the problem from another point of view. 

A function answers these questions to its user:
* What does it do -- function name.
* What does it require to run -- function arguments.
* What does it produce -- return type. 

When a user of the function sees that information he then makes an assumption that if he provides the required arguments the function would return.

Make it simple as possible, but not simpler.

Sources:
* [When to throw an exception? (Stack-overflow)][source-one]

[source-one]: https://stackoverflow.com/questions/77127/when-to-throw-an-exception