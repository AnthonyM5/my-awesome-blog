---
layout: post
title:  "My First Post"
---


#### Code Challenges, and Optimizations: Part 1

At some point in the job search, we encounter technical interviews that aim to assess our level of understanding and/or depth of knowledge.  One of the most common, or the most commonly practiced aspects of a technical interview is the code challenge. 

Code challenges are often a simple one on one interaction with an interviewer and a series of problems/scenarios that you are required to solve.  There are various resources for the structure of these challenges, and libraries of problem sets to practice with:

List of Resources:
1. [HackerRank][1]
2. [Leetcode][2]
3. [Interview Cake][3]


A common strategy after reviewing and confirming the parameters of the problem is to pseudo-code a solution, and use this time to ask any questions or clarifications to discover edge cases.  Once a path has been decided, your solution will benefit greatly from the framework provided in your pseudo-code/brainstorming session.  

A secondary part of the code challenge is the optimization portion, where your interviewer may ask you to review your solution, and hink about ways to decrease time and space complexity. ([Big O Notation][4])  In Computer Science, Big O Notation refers to how quickly our functions run relative to our input, as input increases.  In other words, Big O Notation helps us to classify how efficent our code is when faced with arbitrarily large inputs.  

There are run-times that are constant, the "steps" required in our code is unchanging relative to input.

O(1) Time:

{% highlight javascript %}
function printFirst(arr) {
    console.log(arr[0])
}
{% endhighlight %}


There are run-times that are linear, as in our code iterates linearly.

O(n) Time:

{% highlight javascript %}
function printAll(items) {
    for (let item in items) {
        console.log(items)
    }
}
{% endhighlight %}


There are run-times referred to as quadratic time, where the run time is equivalent to the squared input. We're nesting two loops, our inner loop runs n times each iteration of the outer loop (n x n or n<sup>2</sup>).


O(n<sup>2</sup>) Time: 

{% highlight javascript %}
function printPairs(items) {
    for (let item in items) {
        for (let secondItem in items) {
            console.log(item, secondItem)
        }
    }
}
{% endhighlight %}

Whether your solution is the most optimal or a brute force solution, be prepared to discuss how/why that is the case in terms of time complexity.  Next week, we'll discuss a quick problem with brute force solutions, and an optimized solution.







[1]: https://www.hackerrank.com/dashboard
[2]: https://leetcode.com/
[3]: https://www.interviewcake.com/
[4]: https://www.interviewcake.com/article/python/big-o-notation-time-and-space-complexity?

