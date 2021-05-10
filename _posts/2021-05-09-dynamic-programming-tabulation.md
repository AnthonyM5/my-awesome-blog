---
layout: post
title: Dynamic Programming - Tabulation 
---

So far we have explored a top-down approach to dynamic programming by using using recursion and memoization.  Our recursive trees visualize this top-down approach very well:

canSum[1]
1. We start from our target number and explore combinations with the provided numbers array
2. We create new nodes and try additional combinations
3. We bottom out at either a base case of 0, or a negative meaning we've tried a combination that doesn't work
![tree](https://drive.google.com/uc?id=1BlztedSRiPzRprnflVDojFUV5pM3F5P8)


canConstruct[2]
1. We reconstruct our target word by exploring combinations starting from the prefix.  
2. We bottom out at either the completed word resulting in an empty string, or a remainder string that we can't deconstruct further.
![tree](https://drive.google.com/uc?id=1XmwF8pCz-gtFtWdwOgD1nwoHK2qdrPDj)

gridTraveller[3]
1. We explore our grid with the options provided to us (down, or right moves).
3. We bottom out at either the target(1,1) or an index with a 0 meaning we've reached an impossible path.
![tree](https://drive.google.com/uc?id=1HZ8zqfLyh5YFJ1FIzfXthUtvjgfZ_WVL)



[1]:https://anthonym5.github.io/my-awesome-blog/2021/04/25/dynamic-programming-3.html
[2]:https://anthonym5.github.io/my-awesome-blog/2021/05/01/dynamic-programming-4.html 
[3]:https://anthonym5.github.io/my-awesome-blog/2021/04/11/dynamic-programming-2.html