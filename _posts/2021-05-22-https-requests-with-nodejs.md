---
layout: post
title: HTTP Requests with Node.js and Returning Data
---

#### What is Node.js
JavaScript is near ubiquitous in its use with near [97%][1] of all webpages utilizing it as a client-side programming language.  Node.js is a runtime environment that allows developers to run JavaScript outside of the web browser so it can be used on the server side, often to produce dynamic web page content before being sent to a user's browser or pre-rendering it for a better user experience.  Node.js allows for unifying the developing process to create entire web applications with just JavaScript instead of multiple languages for server-side and client-side scripts.  Node.js is considered Non-Blocking or Asynchronous meaning that it allows for multiple input/output processing at the same time allowing a single "thread" to handle all requests, which in turn makes it scalable for large I/O applications like servers.  Node Package Manager (npm) gives us a large repository of JavaScript libraries, and Node.js supports modules that you can use to import modules that provide additional functionality (like node-fetch, https, etc).

#### HTTP Request
A lot of front-end JavaScript assessments use Node.js and will often limit you to certain modules.  In this case we will use the 'http' module to fetch some data from an API, parse the data we need and return accordingly.  

The Node.js documents provide us with a guide on how to structure a ['GET'][2] request and log the resulting data.  Also provided is a way to [extract the data from the request][3].  The response object we receive back is a stream of data that we can listen for with the 'data' event, which comes back to us in chunks.  We can combine all the chunks of data into a variable and once the stream ends we receive the 'end' event.  

{% highlight javascript %}
const https = require('https')
const options = {
  hostname: 'whatever.com',
  port: 443,
  path: '/todos',
  method: 'GET'
}

const req = https.request(options, res => {

    let body = []

    res.on('data', (chunk) => {
        body.push(chunk)
    })
})

req.on('error', error => {
  console.error(error)
})
req.end()

{% endhighlight %}

However since Node.js is asynchronous we can't simply store the data in the response body into a variable and wrap it around a function to return that value to us.  In the example below our dataList function returns an empty array in every console.log() call except within the scope of the *response.on()* function, because the console.log() calls are executed before the HTTP request finishes.

{% highlight javascript %}

const fetchMovies = (year) => {
    const url = `https://jsonmock.hackerrank.com/api/movies?Year=${year}`
    const request = (response) => {
        let data = '';
        
        
        response.on('data', (chunk) => {
            data = data + chunk.toString();
            
            // console.log(body)
            
    
            
        });
      
        response.on('end', () => {
            const body = JSON.parse(data);
            dataList = body.data.map(movie => (movie.Title))
        console.log(dataList)
        });
        
        console.log(dataList)
    
    }
        console.log(dataList)
    https.request(url, request).end()

}

{% endhighlight %}


#### Using Promises / Async & Await
If you are familiar with the Fetch API provided by JavaScript, you will know that it has promises functionality built-in, and allows us to use *.then()* to wait for a response before executing the next line of code.  Similarly, Async and Await functionality has promises built-in to allow for our requests to be handled properly before returning.  

{% highlight javascript %}
// Using fetch and promises built-in
const getFirstUserData = () => {
  return fetch('/users.json') // get users list
    .then(response => response.json()) // parse JSON
    .then(users => users[0]) // pick first user
    .then(user => fetch(`/users/${user.name}`)) // get user data
    .then(userResponse => userResponse.json()) // parse JSON
}

getFirstUserData()

{% endhighlight %}

{% highlight javascript %}

// Using Async & Await which provides the same functionality as our promises above
const getFirstUserData = async () => {
  const response = await fetch('/users.json') // get users list
  const users = await response.json() // parse JSON
  const user = users[0] // pick first user
  const userResponse = await fetch(`/users/${user.name}`) // get user data
  const userData = await userResponse.json() // parse JSON
  return userData
}

getFirstUserData()

{% endhighlight %}



In this case with the HTTP module, we do not have access to built-in promises, so we must build it ourselves.  With our movies example we know that our data is accessible in the HTTP request object but we need to wait for the request to finish completely, and return that value.  Promises allow us to return an object that resolves with this value, and from there we can use it any other function!


{% highlight javascript %}

const fetchMovies = (year) => {
    const url = `https://jsonmock.hackerrank.com/api/movies?Year=${year}`

//New promise that will return from this function
//Promise will either resolve with our desired data, or error message

    return new Promise((resolve, reject) => {
        const req = https.request(url, res => {
            let body = []

            res.on('data', (chunk) => {
                body.push(chunk)
            })

            res.on('end', () => {
                body = JSON.parse(body)
//At the end of our HTTP request, resolve our promise with the body value 
                resolve(body)
            })
//If we receive the 'error' event we resolve our promise with that instead
            req.on('error', (e) => {
                reject(e.message)
            })
        })
        req.end()
        
    })
}


async function getMovieList(year) {
    let movieList = []
//We wait for fetchMovies to fully resolve
    await fetchMovies(year)
//This allows us to access that promise and access that data
    .then(res => {
        res.data.map(movie => {
            movieList.push(movie.Title)
        })
    })
//Now our movieList will return what we want
    console.log(movieList) 
    

}

getMovieList("2012")

{% endhighlight %}


By wrapping our HTTP request function into a promise we build in a mechanism for us to wait for the request to complete, and promises allow us to resolve this HTTP request with the data that we want.  In an outer function we can then do what we need to with the response body - in this case we map through the resulting movies array and return a new array of titles.  Instead of having our function return empty arrays, we are awaiting a promise object from our HTTP request that will result in either an error or the HTTP body with the data we want.



[1]:https://www.w3schools.com/nodejs/nodejs_modules.asp
[2]:https://nodejs.dev/learn/making-http-requests-with-nodejs
[3]:https://nodejs.dev/learn/get-http-request-body-data-using-nodejs