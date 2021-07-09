---
layout: post
title: More React Hooks
---

As we have covered previously, the React Hooks provides a intuitive API that encapsulates the same concepts of props, state, refs, and lifecycle.  The stated goals of the [Hooks API][1]:

- Hooks let you reuse logic between components without changing your component hierarchy.
- Hooks let you split one component into smaller functions based on what pieces are related.
- Hooks let you use React without classes.


#### React Render Prop & Higher Order Components
React provides a few techniques to share logic between components with the Render prop - this is effectively is a prop whose value is a function that then returns a React element.  The example provided in the React.js documentation describes an example with a *Mouse*  component that keeps track of a mouse position, that is then extracted into a *MouseTracker* component that calls on the *Mouse* component.  The original set up hard codes a <p> tag that displays the current mouse position - but if this needed to be updated to render something else (like another component that renders an image) we would need a way for the *Mouse* render to dynamically determine what to render.  The *render prop* is that solution, instead of implementing its own logic, we can pass in the *Cat* component and share the *handleMouseMove* function.  The new *Mouse* render no longer uses hard coded logic to determine what to display, but instead can rely on the *render prop* to decide what to display.

{% highlight javascript %}

class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>

        {/*
          Instead of providing a static representation of what <Mouse> renders,
          use the `render` prop to dynamically determine what to render.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}

{% endhighlight %}

Higher Order Components provides a similar method of encapsulating shared logic between components by "wrapping" components - 
"[Concretely, a higher-order component is a function that takes a component and returns a new component][2]"
The wrapper function will accept a child component that receives data from its props as the first parameter, and the second parameter deals with the data we want to provide to that component.  This allows an abstraction that can help to share common features like waiting for component mounting to subscribe to a data source, setting state on changes, and clean-up function when un-mounted.

#### Custom React Hooks
React Components are functions and the React Hooks API allows these components to have the same functionality as class components (namely state, props, etc), but React Hooks are functions as well - this means that logic can be shared between two functions (read: components) by extracting into a 3rd function, the custom hook.  As per the documentation - ["A custom Hook is a JavaScript function whose name starts with ”use” and that may call other Hooks."][3].  The example provided utilizes *useEffect* to replicate the data store example in the High Order Component exercise - we have a friends list that needs to be updated when component is loaded, and un-subscribed when component is not in use.  

{% highlight javascript %}

import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}

{% endhighlight %}

In order to re-use this logic to check for online status of an entire contact list, the useEffect can be copied into this new contact list function, but this results in repeated code.  Instead, custom hooks can be structured so that whatever hooks need to be repeated are encapsulated in one function:

{% highlight javascript %}

import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
{% endhighlight %}

This custom hook removes duplicated logic that simply checks to see if a user is online - the result is that this custom hook can be used across multiple components easily.  

{% highlight javascript %}

function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}

function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
{% endhighlight %}

The useFriendStatus custom hook receives an ID and returns a simple state - whether or not the user is online.  This allows the *isOnline* check to be shared easily across our components.  The combination of *useState*, *useEffect*, and the custom hook made checking state and storing state possible without the use of classes and replicates the life cycle of a class component that would check for mounting and un-mounting.






[1]:https://github.com/reactjs/rfcs/blob/master/text/0068-react-hooks.md
[2]:https://reactjs.org/docs/render-props.html
[3]:https://reactjs.org/docs/hooks-custom.html