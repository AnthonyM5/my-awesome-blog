---
layout: post
title: Dynamic Progrmaming or How I learned to Love Sub-Arrays"
---

With our previous sorting algorithims we've employed the Divide and Conquer technique to recursively break problems into smaller easier to solve problems, before merging (bottom-up approach) to generate the final (initial request) result.  Dynamic Programming can also use recursion to help us solve problems, but with a top-down approach - i.e we only solve the base case sub-problem once and store the value to solve the larger sub-problems and continue storing these values.  A common example to illustrate the difference between the two methods is the classic Fibonacci numbers.

{% highlight javascript %}

function fibNum(n){
    if (n <= 1 ) {
        return n
    } 
    return fib(n - 1) + fib(n - 2)
}

{% endhighlight %}