---
layout: post
title: Dynamic Programming - Tabulation Pt. 2
---



#### Recap (Bottom Up vs Top Down)
We've seen how we can use recursion to explore decision problems and we've learned about the concept of dynamic programming which combines the recursive solution with a way to re-use solved data to efficiently solve larger problems.  The first concept to re-use data we've discussed was through memoization, we used a memo object to store keys (our recursive call), and a value (the result of the call) to avoid repeating patterns and greatly improve our algorithms' efficiency.  Recursive solutions are by their nature a bottom-up approach because we start with a defined base case (the bottom of our recursive tree diagrams).  Last week we learned about tabulation which is considered a top-down approach because we start by filling up a table with the first index and use that to solve for subsequent indices until we reach our target. 

