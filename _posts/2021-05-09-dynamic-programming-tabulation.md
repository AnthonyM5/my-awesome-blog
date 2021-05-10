---
layout: post
title: Dynamic Programming - Tabulation 
---

So far we have explored a top-down approach to dynamic programming by using using recursion and memoization.  Our recursive trees visualize this top-down approach very well:

[canSum][1]
1. We start from our target number and explore combinations with the provided numbers array
2. We create new nodes and try additional combinations
3. We bottom out at either a base case of 0, or a negative meaning we've tried a combination that doesn't work

![tree](https://drive.google.com/uc?id=1BlztedSRiPzRprnflVDojFUV5pM3F5P8)


[canConstruct][2]
1. We reconstruct our target word by exploring combinations starting from the prefix.  
2. We bottom out at either the completed word resulting in an empty string, or a remainder string that we can't deconstruct further.

![tree](https://drive.google.com/uc?id=1XmwF8pCz-gtFtWdwOgD1nwoHK2qdrPDj)

[gridTraveller][3]
1. We explore our grid with the options provided to us (down, or right moves).
3. We bottom out at either the target(1,1) or an index with a 0 meaning we've reached an impossible path.

![tree](https://drive.google.com/uc?id=1HZ8zqfLyh5YFJ1FIzfXthUtvjgfZ_WVL)


Another Dynamic Programming approach is called Tabulation, and is considered a bottom up approach.  This means that instead of working down from the target to a base case, we start solving from the base case on upwards to the target.  One advantage of tabulation is whenever you have an algorithm that requires you to solve every sub-problem - tabulation will beat a top-down memoized algorithm.  A memoized solution will save time to avoid recalculating already solved branches but will not offer an advantage if every sub-problem needs to be traversed.  

![tabulation]https://qph.fs.quoracdn.net/main-qimg-7438f6e5470283b2c699e3ba04b3a502


For example with tabulation we can solve for the fibonacci number *n* without memoization and still enjoy similar efficiency.

{% highlight javascript %}

const fib = (n) => {
    //We create our tabulation table, 
    //we need an additional box for the *nth* number
    //We start by filling them with 0s
    const table = Array(n + 1).fill(0)
    table[1] = 1

    for (let i = 0; i <= n; i++) {
    //We calculate up to our target 
    //Using base case as our start
        table[i + 1] += table[i]
        table[i+ 2] += table[i]
    }

    return table[n]
}

{% endhighlight %}



[1]:https://anthonym5.github.io/my-awesome-blog/2021/04/25/dynamic-programming-3.html
[2]:https://anthonym5.github.io/my-awesome-blog/2021/05/01/dynamic-programming-4.html 
[3]:https://anthonym5.github.io/my-awesome-blog/2021/04/11/dynamic-programming-2.html