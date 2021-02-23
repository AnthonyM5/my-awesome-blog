---
layout: post
title:  "Fast Lookup with Hashes"
---

#### Code Challenges, and Optimizations: Part 2

Last week we discussed the concept of time complexity when it comes to our algorithims - and how this is likely to come up during technical interviews.  For additional context we will be using the Inflight Entertainment problem from Interview Cake[1].  

The Background: Customers on long-haul flights light to queue up a second movie after their first ends, but are annoyed that their flight lands before they get to the end.  So you are tasked with building a new feature that will allow users to select two movies with total runtimes equal to the length of the flight.  

Your function will receive an integer flight_length (in minutes), and a list of integers of movie_lengths (in minutes) that and will return true/false if any two individual movies in the list equal the total run time of the flight.  

My first pass and solution turned out to be the brute force path. I would iterate through the list of movie lengths and compare all possible pairs until we reach a pair where the runtime is equal to the flight time, or return false at the end.  The solution looks something like this in Javascript:



{% highlight javascript %}
function inflightMovies(flight_length, movie_lengths) {
    for (let i = 0; i < movie_lengths.length - 1; i++ ) {
        for (let j = 0; < movie_lengths.length; j++ ) {
            if ( movie + secondMovie === flight_length ) {
                return true;
            }
        }
    }
    return false
}

{% endhighlight %}

This solution uses O(N * N) or O(n<sup>2</sup>) Time - our algorithim starts with the first loop that selects the first movie_length and runs a nested loop to compare that movie_length with a second movie_length, until all pairs are evaluated. Time complexity can be thought of in terms of worst-case complexity - like iterating through our entire array until the end runnig through all possible combinations, or best-case complexity - like if we were to find our match within the first two movie_lengths checked.  

For optimization we can think about reducing what we are solving for - if we have the flight_length and the first movie_length, we already know that any match would have to equal flight_length - the first movie_length.  But the crux of our problem is our nested loop in which we iterate through the array.  For a more efficient data structure, we must look to a type of associate array, or a hash table (Javscript Object) - where we can map our movie_length values to keys of our own choosing.  With a Javascript object or a hash table, we can "look up" values by their keys, returning either the value stored, or a falsy (undefined). 

So we could do something like: 

{% highlight javascript %}

function inflightMovies(flight_length, movie_lengths) {
    const movieObj = {}

    for (let i = 0; i < movie_lengths.length -1; i++ ) {
        let match = flight_length - movie_lengths[i]
            if ( movieObj[match] !== undefined ) {
                    return true
                }
                  movieObj[movie_lengths[i]] = i
            }
    return false
}


{% endhighlight %}


In this solution we iterate through the same list of movies, but after each iteration, we populate our object with a key that is equal to the runtime of the movie.  This allows us to ensure that each pair would be unique movies because the key/value pair for the current movie is only included after the loop.  This also allows us to eliminate one of the loops entirely, since we no longer are relying in indices to check for remaining run times.  By setting our keys to the runtime of the movies, we can simply check through our object of cataloged runtimes to see if there exists a key that is a match.  In this way, we are able to get our solution down to O(n), where time complexity is only increased linearly as our input grows.  
 





[1]: https://www.interviewcake.com/question/python/inflight-entertainment?course=fc1&section=hashing-and-hash-tables