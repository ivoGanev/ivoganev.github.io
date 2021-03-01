---
layout: post
title:  "Errors everywhere, but not where they should be"
date:   2021-02-21 10:46:08 +0000
categories: OOP
---

How do you write a function?

A while ago I would simply start like this: <i>"This function has to return a random surprise when any amount of coins are provided."</i> and then write the implementation.

{% highlight kotlin %}
abstract class Coin
class SilverCoin : Coin()
class GoldCoin: Coin()
class Surprise(val surprise: String)

fun main(args: Array<String>) {
    getRandomSurprise(listOf(SilverCoin()))
} 

fun getRandomSurprise(coins: List<Coin>): Surprise {
    // consume the coins and..
    // materialize a specific surprise
    return Surprise("Materializing.. three weeks of summer vacation.")
}
{% endhighlight %}

A week later, the function needed to require at least one gold coin so I found myself expanding the codebase like this: 

{% highlight kotlin %}

fun getRandomSurprise(coins: List<Coin>): Surprise {
    // A surprise costs minimum a golden coin.
    val goldCoin = coins.find { coin -> coin is GoldCoin }
        ?: throw IllegalArgumentException("At least one golden coin is required.")

    // consume the coins and..
    // materialize a specific surprise
    return Surprise("Materializing.. a holy gorilla.")
}
{% endhighlight %}

Honestly I could of continued this way..

{% highlight kotlin %}

fun getRandomSurprise(coins: List<Coin>): Surprise {
    // A surprise costs minimum a golden coin.
    val goldCoin = coins.find { coin -> coin is GoldCoin }
        ?: throw IllegalArgumentException("At least one golden coin is required.")

    if (coins.size < 2)
        throw IllegalArgumentException("Minimum two coins are required.")
    // consume the coins and..
    // materialize a specific reward
    return Surprise("Materializing.. a pickled cucumber.")
}

fun materializeSurprises(coinsFromSomewhere: List<Coin>) {
    var surprise: Surprise? = null
    try {
        surprise = getRandomSurprise(coinsFromSomewhere)
    }
    catch (ex: IllegalArgumentException) {
        // Now which error is it? Never mind, I can check the message..
    }
}
{% endhighlight %}

Yes it's all wrong but wait, am I not suppose to send any errors when the function can't figure out a meaningful result? Yes but here what the deal is:

Here's a cliche for you: Don't assume that you are writing a function only for yourself and saying "Doesn't matter, I can do what I like because I know how my code works". 

A solution: A function should be really designed for a buddy-colleague or dad in mind. Let's try the example above but with a different mental approach -- make the function talk to someone.

### Make the function talk

The function says: <i>"If you provide me with coins, any coin would do, I'll give you a surprise. And by the way you need to provide me at least a gold coin."</i>. 

Notice that there are certain amount of <i>things</i> that need to hold true -- <b>invariant</b> -- in order for the function to fulfill its contract. When the function says "provide" it means it literally:

{% highlight kotlin %}
getRandomSurprise(coin: GoldCoin, extraCoins: List<Coin>)
    : Surprise {...}
{% endhighlight %}

Do I need to notify an error now? Not really. 

So when do I provide an error then? When you break the invariant.

### A few examples

Let's start with a few examples:

The function says: "If you provide me with a ballance and tell me a positive number, I'll take that amount from your ballance and give you back what's left.

Translated to code:

{% highlight kotlin %}

class BankAccount {
    private var balance = 10000

    fun withdraw(amount: Int): Int {
        if (amount > balance) {
            balance = 0
            return balance
        }
        if (amount < 0) {
            throw IllegalArgumentException(
                "This bank doesn't add the money " + 
                "which you are trying to withdraw.")
        }
        else {
            balance -= amount
        }
        return balance
    }

    override fun toString(): String {
        return balance.toString()
    }
}

fun main(args: Array<String>) {
    val bankAccount = BankAccount()
    try {
        bankAccount.withdraw(-10)
    }
    catch(ex: IllegalArgumentException) {
        println(ex)
    }
}
{% endhighlight %}

Now, if you are the user of this function and provide a negative number as an argument, say -10000, what you really are doing is breaking the invariant by making false this function's assertion: <i>"..tell me a positive number.."</i> and therefore need to be notified that whatever you tried to achieve won't happen.

Let's examine a simple piece of code from java.io and see how the author designed that function.

{% highlight java %}

/**
    * Creates a new {@code File} instance by converting the given
    * pathname string into an abstract pathname.  If the given string is
    * the empty string, then the result is the empty abstract pathname.
    *
    * @param   pathname  A pathname string
    * @throws  NullPointerException
    *          If the {@code pathname} argument is {@code null}
    */
public File(String pathname) {
        if (pathname == null) {
            throw new NullPointerException();
        }
        //..
 }

 {% endhighlight %}

The function says: "If you provide me with a string pathname, even with an empty one, I'll make my thing..". Notice how the author is very explicit what the function invariant is. You can only break the invariant by making the assertion "..provide me with a string pathname.." false. The only way this is going to happen in Java is by passing null as an input.

Remember, when you are designing a function that you are telling it what to say to the other person using it.