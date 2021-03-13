---
layout: post
title:  "Minimum Swap Array Manipulation Problem"
social-icons: 
- title: GitHub
  url: https://github.com/AnthonyM5


---





This array manipulation problem requires you to be able to sort an unordered array, and return the minimum amount of "swaps" it would take to order the array.  

#### [Minimum Swaps 2][1] 
You are given an unordered array consisting of consecutive integers  [1, 2, 3, ..., n] without any duplicates. 

For example, given the array arr = [7, 1, 3, 2, 4, 5, 6]: 

{% highlight javascript %}
i   *arr*                   swap (indices)
0   [7, 1, 3, 2, 4, 5, 6]   swap (0,3)
1   [2, 1, 3, 7, 4, 5, 6]   swap (0,1)
2   [1, 2, 3, 7, 4, 5, 6]   swap (3,4)
3   [1, 2, 3, 4, 7, 5, 6]   swap (4,5)
4   [1, 2, 3, 4, 5, 7, 6]   swap (5,6)
5   [1, 2, 3, 4, 5, 6, 7]

{% endhighlight %}

The minimum amount amount of swaps it took to sort this array was 5.  This is a medium difficulty problem on HackerRank and given the constraints it is not hard to see why. 

Constraints: 
* 1 <= *n* <= 10<sup>5</sup>
* 1 <= arr[i] <= n

The constraints specify integer *n* representing the number of elements in array *arr* can be quite large so time complexity will certainly play a factor in getting the correct answer.  Given the finite amount of memory allocated to us with Javascript's stack data structure we may find that a brute force solution exceeds the [maximum call stack size][2].







[1]:https://www.hackerrank.com/challenges/minimum-swaps-2/problem
[2]:https://glebbahmutov.com/blog/javascript-stack-size/


