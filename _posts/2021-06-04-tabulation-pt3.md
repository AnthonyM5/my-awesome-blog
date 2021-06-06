---
layout: post
title: Dynamic Programming - Tabulation Wrap Up
---

#### Revisiting canConstruct, countConstruct, and allConstruct
We will adapt the tabulation methodology to revisit our previous problems, and review how we can adapt our table's input to return the results we want.  

canConstruct Table:
We want to return a boolean so we can start by creating a table with a length of our *targetString* + 1 with each index representing characters up to but not including our index - this means that our initial index represents an empty string and the final index represents the complete *targetString*.  We fill each index with false values, and set our initial index to true because we can always construct an empty string - we can also fill in true values for the given substrings.  Similar to the canSum tabulation we can iterate through the rest of the table to complete our possible combinations as well.   

{% highlight javascript %}

const canConstruct = (targetString, wordBank) => {
// 1. Initiate a table with length of targetString + 1, filling with booleans
    const table = Array(targetString.length + 1).fill(false)
// 2. Set our initial index to true
    table[0] = true
// 3. Iterate through our table including the last index
    for(let i = 0; i <= targetString.length; i++) {
// 4. We start updating from our last true value
        if (table[i] === true) {
// 5. We iterate through our *wordBank* to check combinations
            for(let word of wordBank) {
              const subString = targetString.slice(i, i + word.length)
// 6. Slicing our targetString from our current index + the length of the *targetWord* to check if our current word matches
                if (subString === word) {
// 7. If we we have a match, we can then update that index value to true
                    table[i + word.length] = true
                }
            }
        }
    }
    return table[targetString.length]
}

{% endhighlight %}

countConstruct Table:

For this version of the construct problem we want to return the number of possible combinations so our array will store an integer instead of a boolean.  Our tabulation will be similar in that we want to represent possible string combinations, but instead we will increment when we reach confirmed cases.  As with our previous tabulations, our first index represents a constant true value (empty string, 0, etc) that we can use to calculate further indices.

{% highlight javascript %}

const countConstruct = (targetString, wordBank) => {
// 1. Initiate a table with length of targetString + 1, filling with 0
    const table = Array(targetString.length + 1).fill(0)
// 2. We set our initial index to 1 (it is always possible to construct empty string)
    table[0] = 1
// 3. Iterate through our table  
    for(let i = 0; i <= targetString.length; i++) {
// 4. Iterate through our targetArray to fill table
            for(let word of wordBank) {
              const subString = targetString.slice(i, i + word.length)
                if (subString === word) {
// 5. If word in word bank matches to a corresponding substring in our *targetString* we can determine that this substring has the same possible combinations
                    table[i + word.length] += table[i]
                }
            }
        }
    return table[targetString.length]
}

{% endhighlight %}

