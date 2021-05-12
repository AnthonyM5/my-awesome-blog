---
layout: post
title: Dynamic Programming - When to Memoize 
---

Last post we discussed the ability of a dynamic programming approach (recursion + re-use) to drastically improve run time and eliminate unnecessary function calls in a classical recursive solution like our original fibonacci function.  The introduction of memoization allowed us to store and re-use already solved sub-problems (branches in our fib-tree) without requiring our function to recursively call repeatedly - this is sometimes called a lazy solution where terms are only calculated the first time they are asked for.

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


In the Leetcode example [Unique Paths:][1]

[1]:https://leetcode.com/problems/unique-paths/


We are tasked with finding the number of unique paths a robot (top left corner) can take to reach the finish (bottom right corner) - the robot can only traverse down or right.

![grid](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

Our inputs m,n represent the size of the grid, and our start point.  From each point in the grid there are only two options: down, or to the right -

![tree](https://drive.google.com/uc?id=1HZ8zqfLyh5YFJ1FIzfXthUtvjgfZ_WVL)

We have our base cases:

If our grid is 1x1, there is only 1 path, and if there are any 0 coordinates we've reached a negative base case, you can't travel in a 0th row or column.  We can then set up our recursive function:



{% highlight javascript %}

const uniquePaths = (m, n) => {
// Our base cases in our diagram 
//(1,1 grid), and coordinates containing 0
    if ( m === 1 && n === 1) return 1
    if ( m === 0 || n === 0) return 0;
// We sum the total possible paths, going down, and going right to hit every path 
    return uniquePaths(m - 1, n) + uniquePaths(m, n - 1)
}

{% endhighlight %}


If we run this in Leetcode we do pass the test cases, but upon submitting the solution we run into a Time Limit Exceeded status.  For larger grids the number of paths our function must travel and test is exponential, similarly to our recursive fibonacci function.

Our brute force solution would have a runtime of O(2<sup>(m + n)</sup>) so larger grids will run too slowly (like when we ran fib(50)).

![recursive](https://drive.google.com/uc?id=1veo1sfsWeCGeXyU9tjvc9tIywOYcbc8F)

  
Since our tree solution highlighted a recursive way to test paths, and our recursive solution pass the test cases we can assume the brute force solution maybe be a candidate for dynamic programming with memoization.  From looking at our diagram we can see some branches are repeated, and there are patterns that can be memoized to save us extra calculations - for example, we have 2 branches for (1,2) that can be optimized.  

By adding a memo object like we did with fibonacci we can store our grid coordinates, and the return value of those coordinates to bring us to our "lazy" solution - each set of unique coordinate arguments are calculated the first time only.

{% highlight javascript %}

//Declare global memo
let memo = {};
const uniquePaths = (m, n) => {
//We want to save the coordinates (arguments) as the key
const key = m + "," + n;
//Check if we've already stored this set of arguments
if ( key in memo ) return memo[key]; 
// Our base cases in our diagram 
//(1,1 grid), and coordinates containing 0
    if ( m === 1 && n === 1) return 1
    if ( m === 0 || n === 0) return 0;
// We still sum all the total paths, but also store the coordinates 
// as keys in the memo object 
    memo[key] = unqiuePaths(m - 1, n) + uniquePaths(m, n -1);
    return memo[key];
}

{% endhighlight %}

The result: (50% decrease in runtime)
![memo](https://drive.google.com/uc?id=1me1qTrFZhi-_-i16BFqifpxbuCffScd8)


#### When to memoize?

1. You have a recursive solution - start by drawing the tree and outlining base cases.
2. Test the solution, a brute force solution that works can be optimized.
3. Add a memo object! Use the arguments as keys and save the return values.


