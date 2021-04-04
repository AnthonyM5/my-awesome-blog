---
layout: post
title:  "Sorting Algorithms in Depth Pt. 2"
# social-icons: 
# - title: GitHub
#   url: https://github.com/AnthonyM5


---


Another efficient sorting method we can implement is Quick Sort.  This algorithm is similar to Merge Sort in that it is a divide and conquer technique that breaks down the problem into smaller parts, and uses recursion to solve sub-problems starting from a base case.  

Instead of splitting our array into two halves and sorting the halves quick sort utilizes a comparision element called a pivot.
The smaller sub-lists are created by comparing all the array elements placing all elements less than the pivot on the left, and greater than the pivot on the right.  We can continue to perform the same sort operation on the left/right elements created.  


The layout should be as follows:
1. Define our base case to allow us to recursively call quickSort
2. Select pivot element out of the array (we'll select the end element since it's easy to find)
3. We compare elements within the array against the pivot value
4. Elements less than the pivot are pushed into the left array, and elements large than the pivot are pushed into the right array
5. We recursively call the sort function on the smaller left and right elements, checking until we hit our base case.
6. Each call to quickSort will return sorted sub arrays until only one sorted array is left.


[Computerphile][1] has a great video example of this in action:
<!-- blank line -->
<figure class="video_container">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/XE4VP_8Y0BU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</figure>
<!-- blank line -->

#### Start by setting our base case
Our base case will return the element in arrays of length 1, and will not break when passed an empty array (it will just return empty array).

{% highlight javascript %}
const quickSort = (array) => {
  if(array.length <= 1) {
    return array;
  }

{% endhighlight %}

#### Declare pivot and create our left/right arrays:

{% highlight javascript %}
// Selecting the ending element as the pivot
const pivot = array[array.length -1];
const leftArr = [];
const rightArr = [];
{% endhighlight %}

#### Iterate through array and compare to the pivot 
We push element into left array if it's smaller than pivot otherwise we push into the right array.  We stop right before the last element as that is our pivot.

{% highlight javascript %}
for ( let i = 0; i < array.length - 1; i++) {
//Ternary checking if current element is smaller than pivot
//If yes, it's pushed into leftArr, otherwise it is pushed into rightArr
 array[i] < pivot ? leftArr.push(array[i]) : rightArr.push(array[i])
}

{% endhighlight %}

#### Recursively call our quickSort function 
After our left/right arrays are created, we can call quickSort on each side, and return the result.  By using the [spread syntax][2] we can have the function return an array that is constructed by the quickSorted leftArr, the pivot, and the sorted rightArr.  Our base case can handle situations where left or right arrays are empty, and the spread operator returns a single sorted array.

{% highlight javascript %}

const quickSort = (array) => {
  if(array.length <= 1) {
    return array;
  }

  const pivot = array[array.length -1];
  const leftArr = [];
  const rightArr = [];

  for ( let i = 0; i < array.length - 1; i++) {
    array[i] < pivot ? leftArr.push(array[i]) : rightArr.push(array[i])
  }
  //We return a desctructured array containing the value of a quicksorted 
  //left array + the pivot + the quicksorted right array - this accounts for uneven arrays
  return [...quickSort(leftArr), pivot, ...quickSort(rightArr)]
}

{% endhighlight %}

Similar to the Merge Sort, the time complexity in a best case scenario is O(nLogn) as we divide our array into halves.  However as noted by the sorted array example given by [Computerphile][1] - the worst case scenario can result in O(n<sup>2</sup>) if you have to run the maximum *n* number of inputs.

#### Quick Sort
Best Case:  O(nLogn)
<br>
Worst Case: O(n<sup>2</sup>)
<br>
![Quick-Sort](https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

#### Insertion Sort
Best Case: O(n<sup>2</sup>)
<br>
Worst Case: O(n<sup>2</sup>)
<br>
![insertion-sort](https://bs-uploads.toptal.io/blackfish-uploads/sorting_algorithms_page/content/illustration/animated_image_file/animated_image/27775/insertion-sort-73d4d2a97b420f9cc1d4b2a6f1c7f4c9.gif)

#### Merger Sort
Best Case: O(nLogn)
<br>
Worst Case: O(nLogn)
<br>
![merge-sort](https://bs-uploads.toptal.io/blackfish-uploads/sorting_algorithms_page/content/illustration/animated_image_file/animated_image/27903/merge-sort-3d471ac722ab52fc3a8cc59162808c60.gif)


Each sort has their own use cases, but note merge sort will always have the same time complexity as it will always divide the array into two halves and takes linear time (growth rate as *n* increases) to merge them. 


















[1]:https://www.youtube.com/channel/UC9-y-6csu5WGm29I7JiwpnA
[2]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax