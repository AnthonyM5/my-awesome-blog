---
layout: post
title: React Navigation - Hooks
---

React Hooks have been a part of the React library since 2018 and was introduced to move way from class syntax and keep track of component state without life cycle methods.  The introduction of React Hooks enables state logic more reusable across different components and can replicate all of the life cycle methods we're used to like componentDidMount, componentWillUnmount componentDidUpdate to make components a little more modular - we can abstract fetches to a separate function instead of nesting them into a lifecycle method.  As of [Release Version 0.59][1] hooks are available in React Native to build our components using functions and manage state.  We'll review some of the most useful hooks that are also available in React Native.


#### Use State
The most familiar hook is [useState][3] which as it's name implies, allows us to use state in our components without writing classes.  It is imported directly from the react library like components and gives us the ability to create a state object, and a function to update the state.  The basic example used in the API documents sets up a counter that updates once a button is clicked.  The format of this hook requires us to set up the name of our state object (count), and a function to update to set the state (setState) usually following the same naming convention as our first variable, but pre-pended with set to denote it is the setter function.

{% highlight javascript %}

//Use state accepts an initial state, in this case 0.
const [count, setCount] = useState(0);

//This is the same as:
  let countStateVariable = useState('banana'); // Returns a pair
  let count = countStateVariable[0]; // First item in a pair
  let setCount = fruitStateVariable[1]; // Second item in a pair

{% endhighlight %}


#### Use Effect
The [UseEffect][2] hook mimics the componentDidMount/componentDidUpdate life cycle methods and allows us to pass in functions that will cause "side-effects" or perform changes to our component after it renders or updates.  In the same example from the react documentation we can use useEffect to display our counter every-time there is an update - and an added advantage of using the hook is that we no longer have to specify this display function in the update and mount life cycle methods. 
The resulting lines of code are also reduced - 

{% highlight javascript %}

useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

{% endhighlight %}

Our "effect" that we are passing into our function is structured in a way that we don't need to "clean-up" our effect, as in we don't need to worry about doing anything during the un-mounting of the component.  Sometimes we want to limit the number of times our effects runs, so useEffect allows us to pass in a second argument, a dependant variable that additionally can trigger our effect.  For effects we only want to run/clean up once (during mounting and un-mounting), we can pass in an empty array:

{% highlight javascript %}

useEffect(() => {
    //Effect happens here
  }, []);
 
{% endhighlight %}

This implies that the effect has no dependencies and does not rely on any values passed in as props or from the state.  The counter example relies on the state of count and triggering the effect once count is updated, so we can add count as a dependency.  Once the state (count) changes, we trigger our effect.

{% highlight javascript %}

useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); 
{% endhighlight %}

So far we've used useEffect for functions that don't require "clean-up" effects, but if we are building a subscription service and need to unsubscribe once our component is completed?  We can pass in a function into our useEffect that also returns a clean-up function that will run before the component is unmounted.


{% highlight javascript %}

useEffect(() => {
//Initiate our subscription service during mount
  const subscription = props.source.subscribe();
//Return a function that will run during un-mounting
  return () => {
// Clean up the subscription
    subscription.unsubscribe();
  };
});

{% endhighlight %}

These are the basic React Hooks, and they are functionally similar in React Native as well.  We our applications can use component state and mimic life cycle updates that come in handily without having to write component classes.  In this timer component we set up a useState that accepts a function that converts minutes to milliseconds, and additionally we update our milliseconds state according to the change in minutes with the minutes variable as a dependency. 

{% highlight javascript %}

let minutes = 20

const [millis, setMillis] = useState(minutesToMil(minutes));
const minute = Math.floor(millis / 1000 / 60) % 60;
const seconds = Math.floor(millis / 1000) % 60;
const minutesToMil = (min) => min * 1000 * 60;

useEffect(() => {
    setMillis(minutesToMil(minutes));
  }, [minutes]);

{% endhighlight %}








[1]:https://reactnative.dev/blog/2019/03/12/releasing-react-native-059
[2]:https://reactjs.org/docs/hooks-effect.html
[3]:https://reactjs.org/docs/hooks-state.html