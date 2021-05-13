---
layout: post
title: Dynamic Programming - Tabulation Pt. 2
---



#### Recap (Bottom Up vs Top Down)
We've seen how we can use recursion to explore decision problems and we've learned about the concept of dynamic programming which combines the recursive solution with a way to re-use solved data to efficiently solve larger problems.  The first concept to re-use data we've discussed was through memoization, we used a memo object to store keys (our recursive call), and a value (the result of the call) to avoid repeating patterns and greatly improve our algorithms' efficiency.  Recursive solutions are by their nature a bottom-up approach because we start with a defined base case (the bottom of our recursive tree diagrams).  Last week we learned about tabulation which is considered a top-down approach because we start by filling up a table with the first index and use that to solve for subsequent indices until we reach our target.  We briefly tested fibonacci with tabulation and found it was quick and efficient even for calculating the *50th* fib number.  We will now re-visit our gridTraveler and eventually our summing algorithms with a top-down tabulation approach.


#### Grid Traveler/Unique Paths

[Unique Paths from Leetcode][1]


1. Arrays have 0 indices so we have to account for those in the table. (Any 0 index is automatically an invalid grid, yielding 0 possible moves)
2. Using our seed value we fill out the rest of the table (boxes to the right and bottom of current position).  
3. Our final array position yields our answer for gridTraveler(3,3).  


![table](https://drive.google.com/uc?id=1TwyF3C8N2wd3vb4Re9oC1YWoEE2l8WWv)

<br>

{% highlight javascript %}

const gridTraveler = (m, n) => {
//Our grid can be represented as 2-D Array
//First array represents *m* direction
  const table = Array(m + 1)
  .fill()
//Fill our first array with another array
//Inside this nested array, we fill with *n* direction 
//We want to map the array because each position value is unique 
//Fill the second array with default values
  .map(() => Array(n + 1).fill(0))
//Initialize seed value (this is our base case)
  table[1][1] = 1


//Travelling in a nested array requires a nested loop
  for (let i = 0; i <= m; i++) {
    for (let j = 0; j <= n; j++) {
//Track our current element, using it to calculate neighbor arrays
      const currentElement = table[i][j]
//We want to avoid filling past our array bounds
      if (j + 1 <= n) {
        table[i][j + 1] += currentElement
      }
//Check for when we are at edges
      if (i + 1 <= m) {
        table[i + 1][j] += currentElement
      }
    }
  }
//Our final table position should yield the answer
  return table[m][n]
}


{% endhighlight %}




#### Tabulation Steps
1. Can problem be represented as a table (similar to how we drew recursive tree)
2. Determine size of table depending on inputs, considering arrays are zero indexed
3. Start with a default values (0s)
4. Determine seed value from the smallest sub-problem (top of our table)
5. Iterate through table calculating remaining values from the seed (top) value


[1]:https://leetcode.com/problems/unique-paths/