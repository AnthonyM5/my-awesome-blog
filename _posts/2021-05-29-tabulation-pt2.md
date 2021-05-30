---
layout: post
title: Dynamic Programming - Tabulation Pt. 3
---

#### Revisiting canSum, howSum, and bestSum 
Previously we had utilized a top-down recursive strategy to solve our summing algorithms - we recursively call our function until we "bottom" out and reach a base case that we already know the answer to.  By using this strategy we could also decide what information to return: a boolean for canSum, an array for howSum, and even add additional logic to determine bestSum.  This strategy required us to memoize our results in order to fully optimize the results, thereby reducing unnecessary recursive calls.  With tabulation we are using a bottom-up approach where we utilize a table structure that starts with a bottom base case that we use to calculate the next result, reaching our target.  We saw this with the fibonacci approach, we built an array the size of our target fibonacci sequence (accounting for the 0th index, our bottom case), and calculate the next index until we reach the end of the array which yields our target.  This strategy will be the same blueprint we will use to calculate our summing algorithms.  



For our summing algorithm since we will require a nested array to iterate through our numArray, and use those values to fill out our table itself.  We know that each of our individual numbers can be derived, so we fill them with a true value.  Our individual indices represent possible sums up to the last index which is our target - so we can update our table by traversing index *num* spaces.  Our initial pass will then yield true for all indices corresponding to the numArray.  We can then use this to iterate through the rest of the table and return all possible sums.

#### canSum(7, [3,4,5])
![canSum](https://drive.google.com/uc?id=1txz8eK5A4wje5bKsrTEuGCA2xa6CVRGz)



<br>
{% highlight javascript %}
const canSum = (target, numArray) => {
//1. We create an array table with a length of our target + 1
  const table = Array(target + 1).fill(false)
//2. We want to return a boolean so we will fill our array initially with false 
//3. We set 0th index to true, because we can sum always sum to 0
  table[0] = true
//4. We iterate through the table to fill out our table
  for (let i = 0; i <= target; i++) {
//5. We iterate through the numArray and calculate the rest of our table
    if (table[i] === true) {
      for (let num of numArray) {
        table[i + num] = true
      }
    }
  }
//6. Our final index yields target 
  return table[target]
}

{% endhighlight %}


#### howSum
<br>
We can use this technique to solve for the howSum and bestSum algorithms by changing the values we want to store in our table.  Since we want to return an array of possible combinations we want to fill our table with null values, with our bottom case in index 0 as an empty array.  Then as before, we use the *numArray* to fill out the table, passing along two values: the original value (starting from the 0th index empty array), and the number of steps required to reach target index.
<br>

![howSum](https://drive.google.com/uc?id=1Cyep8wUWItxIVH6S98wvTDVGra7ExEkh)

<br>
{% highlight javascript %}
const howSum = (target, numArray) => {
//1. We create table with target length + 1
  const table = Array(target + 1).fill(null)
//2. We fill with null values
  table[0] = []
//3. We set our 0th index to an empty array 
  for (let i = 0; i <= target; i++) {
//4. We iterate through table to update values
    if (table[i] !== null) {
      for (let num of numArray) {
//5. We use *numArray* to fill in indices with *num* combinations
        table[i + num] = [...table[i], num]
      }
    }
  }
//6. Our target index will yield any possible combinations 
  return table[target]
}
{% endhighlight %}

#### bestSum
With an additional check for current combination length we can also solve for bestSum.  We make sure to check for null values, or whether the existing combination in the array is shorter than our current combination before updating the index.

<br>
{% highlight javascript %}
const bestSum = (target, numArray) => {
//1. We create table with length of targetSum + 1
  const table = Array(target + 1).fill(null)
//2. We fill our table with null values
  table[0] = []
//3. Set our bottom case to empty array 

  for (let i = 0; i <= target; i++) {
//4. Iterate through table to update values
    if (table[i] !== null) {
      for (let num of numArray) {
//5. We use *numArray* to fill in indices with *num* combinations
        const combination = [...table[i], num]
//6. We save variable for current combination 
        if (!table[i + num] || table[i + num].length > combination.length)
//7. We check if our table index value is null, or if our combination is shorter than current
        table[i + num] = combination
      }
    }
  }
//8. Return our target which should only have the shortest possible combination
  return table[target]
}
{% endhighlight %}


In all our problem sets some common themes occur:
1. Our table length is always our *target* + 1 
2. We fill our array with empty values
3. Our base case represents the type of data we want to return
4. Our last index represents our *target* and contains our return value