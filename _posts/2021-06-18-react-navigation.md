---
layout: post
title: React Navigation - Stack Navigation
---

Within React Navigation we have key APIs that help us build a robust application:
- [Stack Navigation][1] (Creates our router, and defines our application's history stack)
- [navigation-prop][3] (Gives us access to the navigation functionality, state, params, etc)
- [navigation-options][5] (Gives us ability to design our headerBar)


The React Navigation our examples have used is Stack Navigation - a way for the app to transition between screens and is similar to the browser history stack where each new screen we visit is placed on top of a stack.  

The Stack Navigation is built using createStackNavigation API that accepts two parameters: RouteConfigs, and StackNavigatorConfig.  This allows us to build a routing mechanism in our application and design the navigational structure.  

The RouteConfigs is an object that maps our route names to the screens that our navigator should present when accessing that route - 
for all the screens we want to make navigatable we want to also pass in specifically the component to render.  

The StackNavigatorConfig provides options for the router like: 
- *initialRouteName* that sets a default screen within our stack
- *initialRouteParams* that defines params for our initial route
- *navigationOptions* that passes default navigation options to use for screens


{% highlight javascript %}
const RootStack = createStackNavigator(
  {
    //We want to have access to a 'Home' route
    Home: {
    //We define the React Native component "HomeScreen" as our screen
      screen: HomeScreen,
    },
    //We want to have access to a 'Details' route
    Details: {
    //We point to the component "DetailsScreen" for navigation prop to render
      screen: DetailsScreen,
    },
  },
  {
    initialRouteName: 'Home',
    defaultNavigationOptions: {
      headerStyle: {
        backgroundColor: '#f4511e',
      },
      headerTintColor: '#fff',
      headerTitleStyle: {
        fontWeight: 'bold',
      },
    },
  }
);
{% endhighlight %}


The React Navigation library gives us the ability to build a dynamic header bar that can be very useful to pass params, change the title, and navigate to other screens.  The [Expo Snack][4] with our header and navigation example displayed the navigation prop that is used for us to navigate through screens.  This navigation-prop is available to us in all of our screen components and contains things like the navigate('route-name') and goBack() functions, state, setParams, getParam, etc.  

The header bar is provided to us within React Navigation and can be set inside the Navigation stack as options: 
{% highlight javascript %}

function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{ title: 'My home' }}
      />
    </Stack.Navigator>
  );
}

{% endhighlight %}

Or within our individual screen component itself:

{% highlight javascript %}

class Home extends React.Component {
  static navigationOptions = {
    title: 'My Home',
  };
  ...

{% endhighlight %}

React Navigation stores the structure of the app (routes, params, etc) within it's [state object][2] that is part of the navigation prop.  The navigation prop can also be passed into our options to dynamically set and update our header:

{% highlight javascript %}
class ProfileScreen extends React.Component {
  static navigationOptions = ({ navigation }) => ({
    title: navigation.state.params.name + "'s Profile!",
  });
  ...

{% endhighlight %}

Through the navigation prop we can create buttons within the screen or the header that have access to update params, and our navigation functions:

{% highlight javascript %}

//Within the header:

static navigationOptions = ({navigation}) => ({
        title: navigation.state.params.name,
        //We are given a back button that is titled with the previous screen by default 
        //but this can also be configured
        headerBackTitle: "Back"
        
    })


//Within the screen: 

return (
      <View>
        <Button
          title="Update the title"
          onPress={() =>
            this.props.navigation.setParams({ otherParam: 'Updated!' })}
        />
        <Button
          title="Go to Details... again"
          onPress={() => this.props.navigation.navigate('Details')}
        />
        <Button
          title="Go back"
          onPress={() => this.props.navigation.goBack()}
        />
      </View>
    
{% endhighlight %}

The navigationOptions can accept core components like *Button*, *Text* and even custom components so there is a lot of versatility to how we can design our screen, and provides us default functionality like the header bar that can be configured with properties like:

- Title 
- headerStyle (style object for header)
- headerRight / HeaderLeft (accepts components)
- headerBackImage / headerBackTitle (customize the back button/title)















[1]:https://reactnavigation.org/docs/2.x/stack-navigator/#navigationoptions-used-by-stacknavigator
[2]:https://reactnavigation.org/docs/navigation-state
[3]:https://reactnavigation.org/docs/1.x/navigation-prop/
[4]:https://snack.expo.io/@anthonym5/header-button
[5]:https://reactnavigation.org/docs/1.x/stack-navigator/#navigationoptions-used-by-stacknavigator