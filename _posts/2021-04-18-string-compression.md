---
layout: post
title: String Compression Algorithm  
---

This LeetCode question was a good lesson for me and an example of how applying certain parameters can turn an easier problem into one that required more familiarity with various built-in functions in JavaScript.  It was also a helpful reminder to strategize and either run through a pseudo-code scenario or otherwise diagram the problem before writing any code.  

[String Compression][1]

Given an array of characters *chars*, compress it using the following algorithm:

Begin with an empty string *s*. For each group of consecutive repeating characters in *chars*:

{% highlight javascript %}
If the group's length is 1, append the character to *s*.

Otherwise, append the character followed by the group's length.
The compressed string s should not be returned separately, but instead be stored in the input character array chars. Note that group lengths that are 10 or longer will be split into multiple characters in chars.

After you are done modifying the input array, return the new length of the array.
{% endhighlight %}

#### Key Examples

{% highlight javascript %}
Example #1:
Input: chars = ["a","b","b","b","b","b","b","b","b","b","b","b","b"]
Output: Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].
Explanation: The groups are "a" and "bbbbbbbbbbbb". This compresses to "ab12".

Example #2:
Input: chars = ["a"]
Output: Return 1, and the first character of the input array should be: ["a"]
Explanation: The only group is "a", which remains uncompressed since it's a single character.
{% endhighlight %}


#### First Approach
The first instinct is to initialize the string *s* to store the new compressed string.  Next would be to iterate through the input array of chars, and initialize a counter to be able to keep track of how many times a char has been encountered.  


{% highlight javascript %}

var compress = function(chars) {
  let s = ""
  for (let i = 0; i < chars.length; i++){
    let count = 1

{% endhighlight %}

We need to keep track of 3 conditions: 
1. Current index *i*, length of array
2. Equality of chars at index of *i* and adjacent letter
3. Counter of number of repeating chars

A while loop that checks for our above conditions and track the index and counter variables:
{% highlight javascript %}

while (i < chars.length - 1 && chars[i] === chars[i + 1]){
      count++
      i++
    }
{% endhighlight %}

We can then handle our cases to either append our char + count to *s*, or just the char:

{% highlight javascript %}

if (count === 1){
      s += chars[i]
   
    } else {
      s += chars[i] + count  
    }
  }

{% endhighlight %}

The problem then requires us not to return *s* but append to our original input array, and return the length of the new array.  The Array.prototype.splice() method provided to us with JavaScript is the perfect solution because it will modify the original array by allowing us to remove or replace existing elements inside the array.  Since we are essentially constructing a new compressed array we can splice the *chars* array at index of the last repeating char.  

We loop through the compressed string *s* and remove elements from the char array and replace with elements from the compressed string.  We also handle for cases where the *chars* array is longer than the compressed string by removing every additional element. 

{% highlight javascript %}

for (let j = 0; j < s.length; j++) {
       chars.splice(j, j, s[j])
      }

if (chars.length > s.split("").length) {
    chars.splice(s.split("").length)
  }

{% endhighlight %}

We can then return the length of the new *chars* array.  Everything together:

{% highlight javascript %}

var compress = function(chars) {
  let s = ""
  for (let i = 0; i < chars.length; i++){
    let count = 1
    while (i < chars.length - 1 && chars[i] === chars[i + 1]){
      count++
      i++
    }
    if (count === 1){
      s += chars[i]
   
    } else {
      s += chars[i] + count  
    }
  }
   for (let j = 0; j < s.length; j++) {
    //We can splice into the *jth* index and replace *j* number of elements with 
    //the current *jth* element in the compressed string.
       chars.splice(j, j, s[j])
      }

  if (chars.length > s.split("").length) {
    //We split the compressed string to account for digits over 10
    chars.splice(s.split("").length)
  }
    return chars.length
};


{% endhighlight %}





[1]:https://leetcode.com/problems/string-compression/