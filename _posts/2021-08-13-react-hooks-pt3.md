---
layout: post
title: "React Hooks - API Reference"
---


The React.js library contains an [API Reference][1] for all the built-in hooks that are commonly used and readily available to us in any react app.  The ones we've covered already are [useEffect][3], [useState][2], which allow us to replicate component life cycle methods and initialize state respectively.  As previously explained the reasons for using Hooks is first to do away with the use of classes and make components more streamlined and split different features into their own functions.  Hooks and custom hooks also allow us to share logic across components a little easier and is arguably a more intuitive way to structure your components.  

### Context 
React provides us a way to share data that is considered to be "global" or needs to be accessed by the entire component tree - without having to pass down a prop through every single component.  Prime use cases for a [Context][4] object could be for setting authenticated users, themes, or selecting languages.  Context objects also come with Provider component that contains a value to be consumed by the components that use the Context object, and all components that are "subscribed" to the Provider will re-render when the Provider value changes.  

The example from the React.js documents outline a common use case of applying light/dark themes via the Context object.  The default value is set to 'light', and components can connect to this Context via a Provider.  This example uses a provider that can serve up this value to any component in the tree.  

```
// Context lets us pass a value deep into the component tree
// without explicitly threading it through every component.
// Create a context for the current theme (with "light" as the default).
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Use a Provider to pass the current theme to the tree below.
    // Any component can read it, no matter how deep it is.
    // In this example, we're passing "dark" as the current value.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// A component in the middle doesn't have to
// pass the theme down explicitly anymore.
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // Assign a contextType to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

The [useContext][5] Hook provides us an elegant way to reproduce the Context object and Provider/Subscriber relationship without the use of classes. We still create a Context Object, and pass it an initial value - and we can still access this value via a Provider.  Now the context is not stored in the class component state, but useContext is called with our created Context Object itself.  Whether this way of using Context is better is highly subjective, but it does abstract the classes away from our components.



```
// Define the light/dark themes 
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

//Create the context set to default 'light'
const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

### Other Hooks (useReducer)

For those who have used Redux and the way their data flow is organized with dispatchers and actions will find the [useReducer][6] hook to be familiar.  It is recommended in place of useState when there is complex logic that is involved in updating the state.  The useReducer accepts a reducer that itself takes (state, action) and returns the new state.  It is set up similarly to useState with a deconstructed array, and we pass in our reducer function with an initial state.  The example provided in React.js is an updated button counter that will update an *initialState* object and uses a action/dispatch model to make changes to our component state.  Now instead of using setState() and processing the change to the state directly, we can define specific actions that update our state and call those instead with our dispatch method.

This follows with the Redux model of passing data and manipulating state in a clean and organized way.  The reducer function sets up our state and receives an action that is passed by the dispatch() method.  Whenever the dispatch passes in an 'increment' or 'decrement' action to our reducer we can take the appropriate action and return the correct state.  This allows us to abstract all changes into one function and organize a little better.

```
const [state, dispatch] = useReducer(reducer, initialArg, init);

const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );

```

This combination of useContext and useReducers also allows for a unique ability to pass our dispatch functionality to any component.  Instead of passing in callback functions we can create a Context object and have the Provider pass the dispatch function to any child component no matter how nested they are. 

```
const TodosDispatch = React.createContext(null);

function TodosApp() {
  // Note: `dispatch` won't change between re-renders
  const [todos, dispatch] = useReducer(todosReducer);

  return (
    <TodosDispatch.Provider value={dispatch}>
      <DeepTree todos={todos} />
    </TodosDispatch.Provider>
  );
}


function DeepChild(props) {
  // If we want to perform an action, we can get dispatch from context.
  const dispatch = useContext(TodosDispatch);

  function handleClick() {
    dispatch({ type: 'add', text: 'hello' });
  }

  return (
    <button onClick={handleClick}>Add todo</button>
  );
}

```













[1]:https://reactjs.org/docs/hooks-reference.html
[2]:https://reactjs.org/docs/hooks-reference.html#usestate
[3]:https://reactjs.org/docs/hooks-reference.html#useeffect
[4]:https://reactjs.org/docs/context.html
[5]:https://reactjs.org/docs/hooks-reference.html#usecontext
[6]:https://reactjs.org/docs/hooks-reference.html#usereducer
[7]:https://reactjs.org/docs/hooks-faq.html#how-to-avoid-passing-callbacks-down