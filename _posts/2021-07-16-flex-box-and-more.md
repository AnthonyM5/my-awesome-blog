---
layout: post
title: CSS Basics and Flex Box
---

We're taking a detour from React Native to review some CSS basics and the Flex Box layout model that is popular with CSS3.  Our react native applications use CSS styling as well so this is directly extrapolated to how we can style our screens and items within the screens to fit across all devices.  

[CSS][1] is a styling language used to describe how HTML (or XML and other similar markup languages) and is responsible for neatly organizing how content is presented with great flexibility.  CSS can be used to control layouts, fonts, colors, backgrounds, scaling, positions, creating shapes, and more!  Combined with HTML and JavaScript CSS serves as cornerstones of how most sites are created on the Web.  We will review an exercise in which we will learn how to dynamically size containers according to the screen size (viewport)[2].  Below is an example of how we can combine JavaScript and CSS to dynamically resize 3 components, and scale a container to maintain aspect ratio.

[Codepen][7]


```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
width-device-width => Sets the width of the page (and as a result, our content) to the width of the device. 
[initial-scale][3] => Sets the zoom level during page loading and scales content, with default 1.0 not being zoomed in.


#### Display: Flex
The display property for CSS lets us define an element in terms of block (one on top of another), in-line (within the same line), and the layout for it's child components (grid, flex, etc).  ```The display:flex``` property sets up the [Flexible Box][4] or FlexBox layout which allows children components of a flex container to lay out in rows/columns, and "flex" to grow or shrink to fill in space in relation to the parent container.  We set up below a container element that will have it's child components display in a row.  ```flex-direction``` controls the main axis of the items (row: in a line, or column: stacked), and can be set to display the items in [reverse][6].



```
.container {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 100%;
  margin: 0px auto;
  background: white;
```

The [flex property][5] then can also define how a flex element within the container will grow/shrink to fit the available space.  The property can receive up to 3 values controlling ```flex-grow, flex-shrink, flex-basis```.  Flex-Basis defines the initial main size of the item, ```flex-grow``` and ```flex-shrink``` controls how the item will grow/shrink in relation to the main size.  Inside of the flex-item component we can define how much of the container the main size should be, and individually set each item to a specific grow/shrink size.  

```
margin: 10px;
overflow: auto;
flex: 0 0 75%;
```

#### Aligning/Centering 
Alignment properties let us define how the browser distributes spaces around content across the main axis and the cross-axis.  The [main axis][8] for rows are in-line, (left <=> right) and for columns are vertical (up <=> down).  The cross axis is the perpendicular axis to the main axis - for rows it is down the columns or block-level (up <=> down), and for columns it's inline (left <=> right).

Alignment properties include:

```
align-content: start; \ center; \ space-between; \ space-around;
align-items: stretch: \ center; \ start; \ end;
align-self: stretch: \ center; \ start; \ end;
justify-content: start; \ center; \ space-between; \ space-around;
place-content: end space-between; \ start space-evenly; \ space-around start; \ end center; 
```

```align-content``` defines the [spacing around][9] the cross axis of a flexbox so for rows it defines whether the items are centered vertically, at the start of the box, end, spaced between each other, or spaced around evenly.  
```justify-content``` defines the spacing around[10] the main axis so for our rows it will let us center items horizontally with the same parameters as above.
```align-items``` sets all the cross-axis alignments on children within a container at once so it can be used to align elements vertically within the parent.
```align-self``` over-rides the defined align items within the parent container to individually align elements
```place-content``` lets you align content across the block and in-line levels at once (both axes) so you can combine the align-content and justify-content properties together.

#### Height/Width Resizing
In our code pen example we set our elements to have their width as a percentage, and utilizing the flex properties of the children we use JavaScript to dynamically resize the height of the container according to the height of the image container - this allows us to keep the aspect ratio of the image while resizing the text blocks as well.  

Some examples of [width][10] and [height][11] values:

```
width: 300px; \ 20em; \ 75% \ auto; \ vw
height: 300px; \ 20em; \ 75% \ auto; \ vh
```

The length value can be absolute (px, cm, mm), or relative (em, vh, vw).  The vw / vh units are to 1% scale of the width and height of the viewport respectively.  The auto value allows browser to calculate and select the measurement for the element.  We can also define maximum or minimum heights/widths to ensure uniform resizing whenever the viewport changes.

Lastly we use the [windows-resize event][12] to set the height of our container, and the width of our individual text blocks.  Resizing the window triggers the resizing of the container height, which will trigger the resizing of the flex items heights.  In order to also enable the text blocks to completely fill the containers empty space, we resize the blocks according to the width of the container minus the image block - since we want the text blocks to be evenly sized we can set them to exactly half of the remaining container width without the image block.



[1]:https://en.wikipedia.org/wiki/CSS#CSS_3
[2]:https://www.w3schools.com/css/css_rwd_viewport.asp
[3]:https://dev.opera.com/articles/an-introduction-to-meta-viewport-and-viewport/#initial-scale
[4]:https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout
[5]:https://developer.mozilla.org/en-US/docs/Web/CSS/flex
[6]:https://developer.mozilla.org/en-US/docs/Web/CSS/flex-direction
[7]:https://codepen.io/thereisnoneo/pen/abWJMJo
[8]:https://developer.mozilla.org/en-US/docs/Glossary/Main_Axis
[9]:https://developer.mozilla.org/en-US/docs/Web/CSS/align-content
[10]:https://developer.mozilla.org/en-US/docs/Web/CSS/width
[11]:https://developer.mozilla.org/en-US/docs/Web/CSS/height
[12]:https://developer.mozilla.org/en-US/docs/Web/API/Window/resize_event