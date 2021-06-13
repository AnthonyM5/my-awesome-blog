---
layout: post
title: Introduction To React Native 
---


#### Introduction
React Native is a mobile development framework that allows developers to build apps in JavaScript but render with Native platform user-interfaces - this can be used to effectively develop for iOS and Android simultaneously.  React Native utilizes the React JavaScript library we can use JSX, which then is rendered into the native language for the platform you're developing for.  For an introductory example - [Basic Tutorial][1]

#### [Native Components vs Core Components][2]
React Native allows you to create platform specific components with JavaScript based react components.  These Native Components are compiled as the corresponding platform specific components giving you access to their native APIs, and allows you to code the behavior/appearance of your app in JavaScript based react components.  React Native's library contains ready to use Native Components called Core Components that are essential to building apps.  

{% highlight javascript %}
React Native: <View> => Android - <ViewGroup> => iOS - <UIView> => Web - <div> 
{% endhighlight %}

#### [Set Up - Local vs Expo][3]
To get started there are two options for setting up development: 
1. [Expo][4] which allows you build, develop, and deploy your apps from your terminal or their web-based GUI.  Apps can then be run on your own device via the ExpoGo mobile app.  
2. Locally with tools like xCode for iOS, and Android Studio for Android - 
this method allows you to simulate the mobile environment on your machine.

For our purposes Expo is the fastest way to get set up, and their [Snacks][4] web-app provides a quick code-pen style editor that can provide quick live-examples and snippets.

#### [Views and More][5]
![Views](https://reactnative.dev/docs/assets/diagram_ios-android-views.svg)
The most basic component of a User Interface is the View container that can hold text, images, user input, etc.  React Views can contain other views and are rendered as their platform specific versions at runtime.  Just like React.js, React Native supports the use of [hooks][6] so functional components have the same functionality as class components with React Hooks like useState, useContext, and your own custom hooks.  React Native also supports [Fast Refresh][7] that gives you quick feedback to your edits just in React.js, with some exceptions for state changes.  

#### Navigation (and React Libraries)
Even though React Native is developed by Facebook it is open-source and as such has a large community that contributes to libraries that add useful functionality.  One example of such library is React Navigation.  When developing mobile apps you'll work mostly in screens, and more often than not multiple screens which require a concise way to track history.  With web browsers there is a built-in history stack that allows for tracking URLs and popping off the last visited page form the stack.  React Navigation provides this functionality and is composed of a core Navigation Container that holds our stack, and individual screens that define our routes.   
[Introduction to React Navigation][9]

<br>


1. createStackNavigator function returns an object that gives us Navigator component that wraps individual screens
2. The screens are children of the Navigator component and help to configure the routes in the Navigation Stack, and it accepts a React Component 


{% highlight javascript %}
function DetailsScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
    </View>
  );
}

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
{% endhighlight %}

Each screen then has access to a [navigation prop][10] which contains various functions that allow us to move between screens as defined by our Navigator.  In our Expo Snack example below we use the navigate function (which accepts a route) to move between our screens, and the goBack() function to create a back button.  Using the React headers, we also see that there is a built-in button to return to your previous screen.  Once we go into our details screen, the header updates to return to our home screen and is named after the title we defined in the header.


[Expo Snack Example:][8]











[1]:https://reactnative.dev/docs/tutorial
[2]:https://reactnative.dev/docs/intro-react-native-components
[3]:https://reactnative.dev/docs/environment-setup
[4]:https://snack.expo.io/
[5]:https://reactnative.dev/docs/components-and-apis
[6]:https://reactjs.org/docs/hooks-intro.html
[7]:https://reactnative.dev/docs/fast-refresh
[8]:https://snack.expo.io/@anthonym5/header-button
[9]:https://reactnavigation.org/docs/hello-react-navigation
[10]:https://reactnavigation.org/docs/navigation-prop