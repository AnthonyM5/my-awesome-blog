---
layout: post
title: React Navigation - Drawer Navigation / Tab Navigation
---

The React Navigation library provides us additional navigation features for your app's UI that can be added to your project with NPM/Yarn - 

Drawers: [@react-navigation/drawer][1]
<video width="320" height="240" controls>
  <source src="https://reactnavigation.org/assets/navigators/drawer/drawer.mov" type="video/mp4">
</video>

and Tabs: [@react-navigation/bottom-tabs][2]
<video width="320" height="240" controls>
  <source src="https://reactnavigation.org/assets/navigators/tabs/bottom-tabs-demo.mov" type="video/mp4">
</video>
<br>
### Drawer Navigation

The Drawer Navigation structure is similar to our stack structure, with a main create function that lets us define the Navigator and the screens.  

{% highlight javascript %}
const Drawer = createDrawerNavigator();

export default function App() {
  return (
    <NavigationContainer>
//Main Navigator - We can still pass in options like initialRouteName 
      <Drawer.Navigator initialRouteName="Home">
//We define our screens that navigator will contain, and pass in the React component
        <Drawer.Screen name="Home" component={HomeScreen} />
        <Drawer.Screen name="Notifications" component={NotificationsScreen} />
      </Drawer.Navigator>
    </NavigationContainer>
  );
}
{% endhighlight %}

With Drawer Navigation we still can make use of the navigation options, and navigation props in our screens.  In addition we are given some functionality to open/close and toggle our drawer in addition to using the native gestures.  These functions are:

{% highlight javascript %}
navigation.toggleDrawer();
navigation.openDrawer();
navigation.closeDrawer();
{% endhighlight %}

The default drawer component provides scroll-able functionality, and only renders links to the routes (screens) provided to the navigator.  This can be overridden with a *contentComponent* that allows us to create custom content for the drawer.  To set up, we pass in props from the navigator to give it access to the navigator's props.  It consists of *ScrollView* and *DrawerItems* components - and is wrapped in a [SafeAreaView][4].

{% highlight javascript %}
//We override default drawerContent with our CustomDrawerContent Component
//Passing in props from the Navigator 
 <Drawer.Navigator drawerContent={props => <CustomDrawerContent {...props} />}>
    <Drawer.Screen name="Feed" component={Feed} />
      <Drawer.Screen name="Notifications" component={Notifications} />
</Drawer.Navigator>

const CustomDrawerContent = (props) => {
  return (
    <DrawerContentScrollView {...props}>
    //Our Drawer Items contain the screens from navigator
      <DrawerItemList {...props} />
    //And we can add additional Drawer Items
    //We have access to the navigation object from props
      <DrawerItem
        label="Close drawer"
        onPress={() => props.navigation.closeDrawer()}
      />
      <DrawerItem
        label="Toggle drawer"
        onPress={() => props.navigation.toggleDrawer()}
      />
    </DrawerContentScrollView>
  );
}
{% endhighlight %}

Basic set up with Drawer -
[Expo Snack][3]

### Tab Navigation
The set up for Tab Navigation also follows along with the Stack Navigation structure with a createBottomTabNavigator function.

{% highlight javascript %}
const Tab = createBottomTabNavigator();

const MyTabs = () => {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  );

  export default function App() {
  return (
    <NavigationContainer>
      <MyTabs />
    </NavigationContainer>
  );
}

}
{% endhighlight %}

We have access to the same navigation object we would in Stack and Drawer navigation - and we have the ability to provide options on our screens to customize title, styling, or pass in an icon to render on the tab.  

Usually our bottom tab appears on more than one screen, and thus we might want to be able to navigate within our tab screens.  We can provide stack navigation inside our tab screens to provide additional functionality - for example we want to have a profile tab and be able to navigate to additional details.  We can have a stack navigator for each of our screens:

{% highlight javascript %}
const HomeStack = createStackNavigator();

const HomeStackScreen = () => {
  return (
    <HomeStack.Navigator>
      <HomeStack.Screen name="Home" component={HomeScreen} />
      <HomeStack.Screen name="Details" component={DetailsScreen} />
    </HomeStack.Navigator>
  );
}

const SettingsStack = createStackNavigator();

const SettingsStackScreen = () => {
  return (
    <SettingsStack.Navigator>
      <SettingsStack.Screen name="Settings" component={SettingsScreen} />
      <SettingsStack.Screen name="Details" component={DetailsScreen} />
    </SettingsStack.Navigator>
  );
}

{% endhighlight %}

[Expo Snack Example][5]



[1]: https://github.com/react-navigation/react-navigation/tree/main/packages/drawer
[2]: https://github.com/react-navigation/react-navigation/tree/main/packages/bottom-tabs
[3]: https://snack.expo.io/@anthonym5/drawer-navigation-with-toggle
[4]: https://reactnative.dev/docs/safeareaview
[5]: https://snack.expo.io/@anthonym5/tab-navigation-with-stack