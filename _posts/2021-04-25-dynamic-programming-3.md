---
layout: post
title: Dynamic Programming - Decision Procedures, and Optimizations
---

A decision problem is essentially a problem that can be represented as a yes/no question, and an algorithm tasked with solving the problem is called a decision procedure.

![Decision Problem](https://upload.wikimedia.org/wikipedia/commons/0/06/Decision_Problem.svg)

An example of a decision problem would be determining if a target number can be summed up with an array of input (positive) integers.

{% highlight javascript %}

canSum(10, [7,2,3,1])
canSum(5, [2,4,1,6,1])

{% endhighlight %}

Our problem should return yes/no, or in this case a boolean true/false.  Our decision procedure will be an algorithm that will determine whether the elements in the input array can sum to our target number.  We can visualize the problem as a recursive tree by checking each element of the array against the target number, with the end of our tree yielding 0 if it there is a possible combination. 

![tree](https://drive.google.com/uc?id=1BlztedSRiPzRprnflVDojFUV5pM3F5P8)

We know that if we reach 0 we reach a base case where the combinations of input numbers can add up to the target number.  Similarly, if we return a negative number we know that it is not possible to sum exactly to the target number.  


This gives us a recursive outline:


{% highlight javascript %}

const canSum = (targetNum, numbers) => {
    if( targetNum === 0 ) return true
    if( targetNum < 0 ) return false
}

{% endhighlight %}

From there we can loop through the elements and recursively subtract each element of the array until we reach a base case:


{% highlight javascript %}

for (let num of numbers) {
//Iterate through each element, and subtract from target
    const remainder = targetNum - num
//Remainder becomes the new targetNum and we recursively call canSum
//Return early if remainder can be summed
    if (canSum(remainder, numbers)) === true {
        return true;
    }
}
//We return false after every element is checked

{% endhighlight %}

We have a recursive solution that provides us a brute force way to test every combination.  This is perfect for memoization to help us avoid running through branches in our tree that have already been solved. 

#### Memoized
{% highlight javascript %}

const canSum = (targetNum, numbers) => {
//Declare memo object
    let memo = {}
//Check memo object to see if branch has already been solved
    if( targetNum in memo ) return memo[targetNum]
    if( targetNum === 0 ) return true
    if (targetNum < 0 ) return false

    for (let num of numbers) {

        const remainder = targetNum - num
//Store value of branch into memo object 
        if (canSum(remainder, numbers) === true) {
            memo[targetNum] = true
            return true
        }
    }
    memo[targetNum] = false
    return false
}

{% endhighlight %}

We can further increase our algorithm's complexity by returning the values used to sum to the target sum and utilizing additional data during each call.  

{% highlight javascript %}

const howSum = (targetNum, numbers) => {

    let memo = {}

    if( targetNum in memo ) return memo[targetNum]
//We're building an array of numbers that combine into our targetNum
//Our return values will be an array, or null
    if( targetNum === 0 ) return []
    if (targetNum < 0 ) return null

    for (let num of numbers) {
//We're tracking the remainder, and keeping the result stored
        const remainder = targetNum - num
        const remainderResult = howSum(remainder, numbers)
//If we reach a case where our remainder is 0, we want to save this value
//We use spread operator to assign a value of an array to our targetNum
//Spread operator allows us to build array of integer combinations

        if (remainderResult !== null ) {
            memo[targetNum] = [...remainderResult, num]
            return memo[targetNum]
        }
    }
    memo[targetNum] = null
    return null
}
//howSum(100, [7,14]) => null
//howSum(27, [7,14,6,17]) => [6,7,7,7]

{% endhighlight %}

From here we can see that there is a way for us to find the best possible combinations (i.e the shortest number of integers) that can derive our *targetNum*.  Since our above algorithm returned early once it found any combination, we can further tweak this procedure to track an additional variable for the *shortestCombination*

{% highlight javascript %}

const bestSum = (targetNum, numbers) => {
    let memo = {}

    if (targetNum in memo) return memo[targetNum]
    if (targetNum === 0) return []
    if (targetNum < 0) return null
//Our new variable to keep track of the shortest length array seen so far
    let shortest = null

    for (let num of numbers) {
        const remainder = targetNum - num 
//Our remainderCombination is built by recursively calling with our remainder
        const remainderCombination = bestSum(remainder, numbers) 
//If we reach a non-null base case we have found a combination
        if (remainderCombination !== null) {
            const combination = [...remainderCombination, num] 
//We only care about the shortest combination so for each targetNum we check
//If the shortest combination has been found, and comparing our current combination
            if (shortest === null || combination.length < shortest.length) {
                shortest = combination
            }
        }
    }
//We can use our memo object to store the shortest combination seen for each targetNum

    memo[targetNum] = shortest

//Our returned array will be the shortest length array for our targetNum
    return shortest
}

{% endhighlight %}


If our canSum algorithm was an example of a decision procedure, our howSum is an example of a combination problem, and the bestSum is an optimization problem.  canSum was trying to figure out whether something was simply true/false.  howSum required us to track the combinations (just the first combination), and the final iteration asked us to optimize and find the shortest possible combination. Each is an example of using dynamic programming to help track and re-use values (branches) that have already been calculated and memoized.
