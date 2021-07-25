---
layout: post
title: CSS Basics and Flex Box Continued
---

### Flexbox Wrapping
Another Flexbox property that is available is [flex-wrap][4], this gives us the ability to set whether flex items inside the container are forced into one line or can wrap around to multiple lines, and allows us to set the direction of the items.

```
flex-wrap: nowrap; (Default Value) // wrap; //wrap-reverse;
```
[Wrapped Examples][5]

### Flex Properties
Flex items within a flexbox layout have their own [properties][6] that allow us to target them directly and usually rely on the spacing in between the box.  These properties work along the main axis of the container so for rows it would be horizontally and for columns it would be vertically.  These three properties are flex-grow, flex-shrink and flex-basis.

The [flex-grow][11] and flex-shrink concepts rely on the space distribution of the container and the flex items inside.  
![container](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Controlling_Ratios_of_Flex_Items_Along_the_Main_Ax/basics7.png)

For three flex items with a width of 100 pixels in a 500 pixels container, there is 200 pixels of available space for it to "grow" into. Without specifying flex properties the space will be added to the end of the last item.

![overflow](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Controlling_Ratios_of_Flex_Items_Along_the_Main_Ax/ratios1.png)

Conversely if there is an overflow within the container there is "negative" space for the flex items to shrink if it is defined.  

The [flex-basis][8] property defines the initial sizing of each item before any space distribution takes place, by default this property is set to auto and is determined by the main size of the item if it is defined.  If your item is defined with a 100 pixel width/height, flex-basis becomes 100px.  If there is no explicitly set size then flexbox utilizes min/max-content[9] to determine the flex-basis.  

All of this can be represented by the short hand flex property within an item:  

```
// Sets the grow factor, shrink factor, and flex-basis respectively.
flex: 1, 1, auto
```

#### Min-content / Max-Content
![min/max-content](https://i7x7p5b7.stackpathcdn.com/codrops/wp-content/uploads/2014/10/min-max-content.png)
[Min-content][10] size refers to the smallest size a box can be without overflowing, and max-content is usually determined by the "ideal" size a box should take to fit its content given infinite space.






### Float
The [CSS Float][3] property allows us to place an element on either side of a container allowing text and other inline elements to wrap around it giving us a away to put an element in the flow of a container as opposed to absolute positioning which can overlay elements over each other.  

```
float: none; (default) \\ left; \\ right;
```


### CSS Shapes
As we've discussed previously CSS can be used to control layouts, fonts, colors, backgrounds, scaling, positions, creating shapes, and more.  
In this [tutorial][1] there are great examples and code snippets that describe how simple geometric shapes can be created by adjusting width/height, and border (radius, left, right, top, bottom). 
[Codepen][3]

A square can be defined as an object with the same height and width measurements:

````
#square {
  width: 100px;
  height: 100px;
  background: red;
}
````

A circle is the same with the exception of a border-radius[2] (rounds the edges) of 50%.  
````
#circle {
  width: 100px;
  height: 100px;
  background: red;
  border-radius: 50%
}
````
A triangle can be made and oriented in any cardinal direction by adjusting the border properties:  

````
#triangle-right {
      //By defining the height and width we keep all properties within scope of the largest borders 
      width: 0;
      height:0;

      //Fills top corner with a transparent block that covers half of the height, but does not override the border-left
      border-top: 50px solid transparent;
      
      //Our triangle faces right so the left border is the "base" 
      border-left: 100px solid red;

      //Fills the bottom corner with a transparent block that covers half of the height, but does not override the border-left
      border-bottom: 50px solid transparent;
    }
````






[1]:https://css-tricks.com/the-shapes-of-css/
[2]:https://developer.mozilla.org/en-US/docs/Web/CSS/border-radius
[3]:https://codepen.io/thereisnoneo/pen/eYWyvJy
[4]:https://developer.mozilla.org/en-US/docs/Web/CSS/flex-wrap
[5]:https://codepen.io/thereisnoneo/pen/eYWyvJy
[6]:https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox#properties_applied_to_flex_items
[7]:https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Controlling_Ratios_of_Flex_Items_Along_the_Main_Ax
[8]:https://developer.mozilla.org/en-US/docs/Web/CSS/flex-basis 
[9]:https://drafts.csswg.org/css-sizing-3/#max-content
[10]:https://drafts.csswg.org/css-sizing-3/#min-content