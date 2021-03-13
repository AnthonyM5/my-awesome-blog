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

Points to Consider:
* Our sorting method will need to identify the element in place, and perform the swap if necessary
* Arrays are zero-indexed, but the elements are indexed to 1 (we can determine an element's correct position by subtracting 1 from it's value)
* Nesting loops like in a [bubble sort][3] will not be efficient for larger arrays

Since we do need to sort the array we can use an [insertion sort][4] function to arrive at an O(n<sup>2</sup>) solution:
[Source:][4]

{% highlight javascript %}

let insertionSort = (arr) => {
    let length = arr.length;
    for (let i = 1; i < length; i++) {
        let key = arr[i];
        let j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
    return arr;
};

{% endhighlight %}


However, since we already know if our *arr[i]* does not equal to *i + 1* this element is out of position and must be swapped.  We don't need to keep track of a second index in this case.
{% highlight javascript %}
if (arr[i] != i + 1)
{% endhighlight %}


We can then set up a while loop to iterate through the array:
{% highlight javascript %}
function minimumSwaps(arr) {
        let i = 0;
        let count = 0;
        let  n = arr.length;
        while(i < n) {
            if (arr[i] != i + 1) {
            //swapping happens here
            //increment count
            } else {
                i++
            }
        }
        return count++
}
{% endhighlight %}

From there we save the currently indexed value in a temporary variable and assign it to it's correct value (which is just the value - 1 due to the 0-indexed array)

{% highlight javascript %}
 if(arr[i] != i+1){
                temp = arr[i];
                arr[i] = arr[temp-1];
                arr[temp-1] = temp;
                count++;
            }
{% endhighlight %}

Our sort algorithm still selects a single element at at time but we are able to eliminate a nested loop by deriving the correct index directly from the value.  


### Full Solution
{% highlight javascript %}

function minimumSwaps(arr) {
    let length = arr.length;
    let i = 0;
    let temp;
    let count = 0;
    
    while(i < length) {
        if (arr[i] != i + 1) {
            temp = arr[i];
            arr[i] = arr[temp - 1]
            arr[temp - 1] = temp
            count++
        } else {
            i++
        }
    }
    console.log(arr)
    return count;

}
{% endhighlight %}

 


[1]:https://www.hackerrank.com/challenges/minimum-swaps-2/problem
[2]:https://glebbahmutov.com/blog/javascript-stack-size/
[3]:https://stackoverflow.com/questions/7502489/bubble-sort-algorithm-javascript
[4]:https://gist.github.com/robincard/2d0acdf42e4e606ad341d7fdcf264a48


