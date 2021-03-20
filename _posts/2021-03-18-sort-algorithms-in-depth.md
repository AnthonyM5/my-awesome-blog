---
layout: post
title:  "Sorting Algorithms in Depth"
# social-icons: 
# - title: GitHub
#   url: https://github.com/AnthonyM5


---

[Previously][1] we briefly mentioned bubble sorting as a possible solution, but noted that it was likely to not be efficient enough for larger *n* sized arrays. The bubble sort method iterates through an array and compares adjacent values and swaps them if they are out of place.  This process happens throughout the entire array and usually requires us to keep track of two variables in a nested loop.

{% highlight javascript %}

const bubbleSort = (arr) => {
    //Iterate through array *arr*, tracking 1st element with *i*
    for (let i = 0; i < arr.length ; i++) { 
    //We track 2nd element with *j*
        for (let j = 0; j < (arr.length - i - 1); j++) { 
      //Compare the adjacent positions and swap if necessary
            if(arr[j] > arr[j+1]) {
      //Declare variable to temporarily store our current number 
              let tmp = arr[j];  
      //Replace current variable with comparing variable
              arr[j] = arr[j+1]; 
      //Replace adjacent number with current number
              arr[j+1] = tmp; 
            }
        }        
    }
}

{% endhighlight %}

#### A visualization of Bubble Sorting:                                                               
![bubble-sort](https://bs-uploads.toptal.io/blackfish-uploads/sorting_algorithms_page/content/illustration/animated_image_file/animated_image/27839/bubble-sort-fa12d2c89cb8fe2e90fbf85a85ee501b.gif)

Our minimum swap problem was solved with a variation of an insertion sort:

{% highlight javascript %}

const insertionSort = (arr) => {
    let length = arr.length;
  
  //Iterate through the array *arr* 
    for (let i = 1; i < length; i++) {
  //We check current location against previous position 
        let key = arr[i];
  //At position 0, we compare the last element of the array 
        let j = i - 1;
        while (j >= 0 && arr[j] > key) {
  //Swap the values 
            arr[j + 1] = arr[j];
            j = j - 1;
        }
  //Set the new *key* variable
        arr[j + 1] = key;
    }
    return arr;
};


{% endhighlight %}

#### Visualization of Insertion Sort:                                        
![insertion-sort](https://bs-uploads.toptal.io/blackfish-uploads/sorting_algorithms_page/content/illustration/animated_image_file/animated_image/27775/insertion-sort-73d4d2a97b420f9cc1d4b2a6f1c7f4c9.gif)


These methods both involve a O(n<sup>2</sup>) time complexity so we will explore more efficient ways of sorting using strategies like Divide and Conquer, and recursion. 

### Merge Sorting:

We break up our input array until there is only one element left (one element is by default sorted), then we merge the subArrays together until one sorted array remains.

{% highlight javascript %}
//We are looking to split array *arr* in half recursively 
//Return our base case of a single element *arr*

const mergeSort = (arr) => {
  if (arr.length === 1) {
    return arr
  }

//We find the middle index of the array, and round down
  const middle = Math.floor(arr.length / 2) 
//Set variable for left side of the array
  const left = arr.slice(0, middle)
//Set variable for right side of the array
  const right = arr.slice(middle) 
//Helper function to help us merge our subArrays
  return merge(
    mergeSort(left),
    mergeSort(right)
  )
}

//We compare the arrays and merge them into a new array
function merge (left, right) {
  let result = []
  let leftIndex = 0
  let rightIndex = 0

//We iterate through left and right arrays until the end
  while (leftIndex < left.length && rightIndex < right.length) {
//Compare right and left arrays and we build out the sorted array
    if (left[leftIndex] < right[rightIndex]) {
      result.push(left[leftIndex])
      leftIndex++
    } else {
      result.push(right[rightIndex])
      rightIndex++
    }
  }
//We return the results array and add back the elements left over
//as in the case where the arrays are unequal lengths 
  return result.concat(left.slice(leftIndex)).concat(right.slice(rightIndex))
}


{% endhighlight %}









[1]: https://anthonym5.github.io/my-awesome-blog/2021/03/13/minimum-swaps.html 
