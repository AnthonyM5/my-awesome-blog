---
layout: post
title:  "Maximum SubArray Problem"
---

#### Code Challenges & Optimizations Part 3

Given a one dimensional array of numbers ([1...n]), return the sum of a continuous subarray of numbers with the largest sum.  


For example given the array:  [−2, 1, −3, 4, −1, 2, 1, −5, 4] the continuous subarray with the largest sum is [4, −1, 2, 1], with a sum of 6.

My Initial Solution: 

{% highlight javascript %}



function largestSubarraySum(array){

// Declare variables to store the current max sum, current subarray sum, 
// and an array to collect the max sums for all subarrays.
    let max_so_far = 0
    let max_ending_here = 0
    let max_collection = [];


// We iterate through all elements to collect sums
  for ( let i = 0; i < array.length; i++) 
// If the current max sum of the elements we've visited is greater than 0,
// we want to track this information and push into our sums array.
    if (max_ending_here + array[i] > 0){
      max_ending_here = max_ending_here + array[i]
      max_collection.push(max_ending_here)
    } else {
// If the sum of current max and current element is negative we reset the 
// current sum
      max_ending_here = 0;
    }
// If the sum of the current max and current element yields 
// a higher sum, the current subarray max is updated
  if (max_so_far < max_ending_here)
    max_so_far = max_ending_here
  
// For cases where our array is empty, we want to return 0,
// otherwise, we return the highest value in our subarray sums collection 
    return max_collection.length ? Math.max(...max_collection) : 0;
}
{% endhighlight %}

Optimizations: 
This problem set can be solved via several different approaches including brute force, divide and conquer, and dynamic programming.  A brute force solution would be a nested loop similar to last week's initial solution.  We iterate through array with an initial position and check all possible combinations and only update our max_so_far whenever a loop returns a higher sum.  


O(n<sup>2</sup>) Time: 
{% highlight javascript %}
function largestSubarraySum(array){
    let max_so_far = 0;
    for (let i = 0; i < array.length; i++) {
        let max_ending_here = array[i]
        max_so_far = Math.max(max_so_far, max_ending_here)
        for (let j = i + 1; j < array.length; j++){
            max_ending_here += array[j]
            max_so_far = Math.max(max_so_far, max_ending_here)
        }
    }
    return max_so_far

}
{% endhighlight %}

The most efficient solution is something called [Kadane's Algorithm][1] which helps to break down the problem into smaller pieces but still following the same principal of collecting a "global" max sum (max_so_far), and a localized sum (max_ending_here) of the current iterative loop.  This is part of a larger optimization principle called Dynamic Programming which relies on breaking problems down to smaller sub-problems, finding optimal solutions these sub-problems that will yield an optimal solution to the overall problem.  

O(n) Time:
{% highlight javascript %}
      // We initialize our local and global sum variables 
      let max_so_far = 0
      let max_ending_here = 0
      // Iterate through the array, and we reduce our previous if/else statement into one line
      for(let i = 0; i < array.length; i++){ 
      // We are checking to see if subarray sum is greater than initial element
      // and we store the higher of the two which reduces our need to recalculate
      // the previous element sums.  If the the subarray yields a lower result we know to 
      // restart the localized sum.
          max_ending_here = Math.max(array[i], max_ending_here + array[i])

      // compare max_so_far with max_ending_here and store the greater value
      // we also account for all negative numbers by returning 0
          max_so_far = Math.max(max_ending_here, max_so_far, 0)
          
      }
      return max_so_far

{% endhighlight %}


Since we know that the subarray has to be continuous we really only need one loop.  We no longer need an array to collect our localized sum (max_ending_here) because we only use this value to calculate the next local max and only update the global max (max_so_far) when our subproblem (subarray) returns a higher value.  Our max_so_far at the end of this loop by default will only be the highest value after the iterations.






[1]:https://en.wikipedia.org/wiki/Maximum_subarray_problem#Kadane's_algorithm

