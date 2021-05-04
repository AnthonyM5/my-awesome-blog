---
layout: post
title: Dynamic Programming - Decision Procedures, and Optimizations Part 2
---

In a similar vein to our summing problems from last week, we are tweaking our approach to tackle string construction.  Our decision procedure will be an algorithm that determines whether a word can be constructed from a array bank of strings.  

For example if we want to construct the word skateboard using the bank of strings ["bo", "rd", "ate", "t", "ska", "sk", "boar"] we can construct a recursive tree to test each element in the array.  Our initial branches will have to be a prefix of the original word, so in our case "ska", or "sk", and then we would test the remaining elements until we return a base case of an empty string (meaning we have constructed our word), or we end up with a string that we can no longer remove a prefix from.  

![tree](https://drive.google.com/uc?id=1XmwF8pCz-gtFtWdwOgD1nwoHK2qdrPDj)

Our recursive function would look something like this:
Javascript strings have a prototype method [indexOf()][1] that we can use to determine if the search term is in the string, and return the starting index, and combined with the [slice()][2] method we can also create our suffix like in our tree.

{% highlight javascript %}

const canConstruct = (target, wordBank) => {

//Base case

    if (target === '') {
        return true
    }

//We then want to iterate through the word bank

    for (let string of wordBank) {

//If our element is a prefix of the target, this should return 0

    if (target.indexOf(string) === 0) {

//Using slice with the length of the string to find our suffix

    let suffix = target.slice(string.length)

//We recursively call our *canConstruct* on this suffix 
//return early if we hit base case

        if (canConstruct(suffix, wordBank) === true) {
            return true
            }
        } 
    }
//After everything runs and we don't reach base case
    return false
}

{% endhighlight %}

<br>
By now we realize that the our time complexity (our branches we have to check) can be in the worst case, each element of the word bank *n*.  Our maximum length of the tree would similarly depend on the length of the target *m*, so at each node we potentially have to check *n* branches for the entire length of *m* that results in O(n<sup>m</sup>) time.  We also create a new suffix in each of our loops so ultimately our worst time complexity is O(n<sup>m</sup> * m).
<br>

#### Make it dynamic

We can make this a little faster by introducing a memo object, and potentially save from repeatedly making the same recursive calls.  

{% highlight javascript %}

const canConstruct = (target, wordBank) => {
    let memo = {}
    if (target in memo) return memo[target]
    if (target === '') {
        return true
    }

    for (let string of wordBank) {


    if (target.indexOf(string) === 0) {

    let suffix = target.slice(string.length)

        if (canConstruct(suffix, wordBank) === true) {
    
    //When we return in our recursive call, we should also store this in the memo 

            memo[target] = true
            return true
            }
        } 
    }
    //When we return in our recursive call, we should also store this in the memo 

    memo[target] = false
    return false
}

{% endhighlight %}

Instead of brute force time complexity of O(n<sup>m</sup> * m), we now only have to calculate a maximum of *n* branches.  We do add an additional memo object in addition to the sliced variable so ultimately our time complexity is O(n * m<sup>2</sup>)

#### Slight Tweak to Count Combinations
Our canConstruct decision procedure can also then be tweaked to give us the total number of combinations that can construct our target word.

{% highlight javascript %}

const countConstruct = (target, wordBank) => {
    let memo = {}

    if (target in memo) return memo[target]
    
    //Base case now returns a count of 1 meaning we have constructed the word

    if (target === '') return 1

    //We track the total count throughout each successive recursive call

    let totalCount = 0

    for (let string of wordBank) {
        if (target.indexOf(string) === 0) {
        let suffix = target.slice(string.length)

    //We recursively call on our suffix, and update *totalCount*

        const restNum = countConstruct(suffix, wordBank)
        totalCount += restNum
        } 
    }
  memo[target] = totalCount
}

{% endhighlight %}

#### Keeping track of the combinations 

Similar to our countConstruct (and the howSum solution from last week), we can apply an additional variable and return the total combinations that can construct our target word.  As we recursively test our combinations we can store the elements in an array and return that instead of the total count.  


{% highlight javascript %}

const allConstruct = (target, wordBank) => {

  //Our base case  

  if (target === '') return [[]]

  //We store our substrings in a result array to return at the end
  const result = []

  for (let word of wordBank) {
    if (target.indexOf(word) === 0) {
      const suffix = target.slice(word.length)
      
      //The remaining ways to construct our suffix  

      const suffixWays = allConstruct(suffix, wordBank)

      //The completed target can be constructed by combining 
      //the current word and the array of ways to construct the suffix

      //Spread the array of suffix pairs to add the current word

      const targetWays = suffixWays.map(way => [word, ...way])

      //Push the new *targetWays* array into our results array

      result.push(...targetWays)
    }
  }
      //The end result should either be an array of our *targetWays* combinations
      //or our original 1-D results variable if no *targetWays* are found 
    
    return result
}

{% endhighlight %}

[1]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf
[2]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice