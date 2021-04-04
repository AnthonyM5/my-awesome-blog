---
layout: post
title: Dynamic Progrmaming or How I learned to Love Sub-Problems
---

<p>With our previous sorting algorithims we've employed the Divide and Conquer technique to recursively break problems into smaller easier to solve problems, before merging (bottom-up approach) to generate the final (initial request) result.  Dynamic Programming can also use recursion to help us solve problems, but with a top-down approach - i.e we only solve the base case sub-problem once and store the value to solve the larger sub-problems and continue storing these values. </p>


A common example to illustrate the difference between the two methods is the classic Fibonacci numbers.

### Recursive Solution
{% highlight javascript %}

const fib = (n) => { 
    if (n <= 1 ) {
        return n
    } 
    return fib(n - 1) + fib(n - 2)
}

}


//fib(5) = fib(4) + fib(3)
//fib(4) = fib(3) + fib(2)
//fib(3) = fib(2) + fib(1)
//fib(2) = fib(1) + fib(0)
//fib(1)/fib(0) = 1

{% endhighlight %}

![fib(5)](https://i.stack.imgur.com/HpgSu.png)

We have 2 recursive calls that fire *n* times, so the runtime O(2<sup>n</sup>) and in this case our space complexity (amount of memory reserved) will correspond to the input *n*. The problem with solutions like the purely recursive divde and conquer pattern above is that our time complexity would get needlessly large as input *n* increases.  

The time complexity to calculate fib(10) for example would require 2<sup>10</sup> or 1024 steps to derive. 2<sup>50</sup> results in 1,125,899,906,842,624 steps and actually hangs when we try to run.


<figure class="video_container">
  <iframe width="560" height="315" src="https://youtube.com/embed/I0FdVXFyG-g" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</figure>

Here is where one of the key principals of dynamic programming come into play - from the fibonacci tree above we can see that a lot of the same call is being made multiple times (fib(1), fib(0)) and even entire branches can be repeated as with fib(2), fib(3) branches which occur twice.  


Recall in our very first the very first optimzation post we had dicussed the fast "key lookup" that JavaScript objects can afford us, and that a useful way of optimizing our soluition is to store our values in a way that can be rapidly accessed.  This concept can be used for a dynamic approach to our recursive fib function.  

By storing the values of fib(2), fib(3), and so forth, we arrive at the second (most important) part of a dynmamic programming solution - the ability to sole sub-problems only once and use that to derive more complex problems higher up the tree.  This particular optimization process is called memoization.



### Memoized Solution
{% highlight javascript %}
// Declare an empty JS object (outside of the function) for us to store our data.
let memo = {}
const fib = (n) => { 
 //We can quickly key in to our object checking for solution to fib(n)
    if (n in memo) {
//This eliminates duplicated steps
        return memo[n]
    }
    if (n <= 1 ) {
        return n
    }
//If this is the first time encountering fib(n), we store this value
    memo[n] = fib(n - 1) + fib(n - 2)
//Return the value of fib(n)
    return memo[n]
}

{% endhighlight %}

One major difference to Divide and Conquer techniques and Dynamic Programming is: Recursion + Re-using Data (via Memoization)

Our run time goes from 2<sup>n</sup> to a linear O(n) solution as only unique calls are run (fib(1), fib(2), fib(3), fib(4), fib(5))

![memoized](https://drive.google.com/uc?id=1Py-XkdApC6HV5FL_xBD7CM9cFjIvhCc2)

Voila!
<figure class="video_container">
  <iframe width="560" height="315" src="https://youtube.com/embed/edsRMcrr5oU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</figure>





