---
layout : page
title : My CSS Learning Journey
date : 2025-01-21
---

You can head to [w3schools](https://www.w3schools.com/) to play with the basics of CSS. After you know what its about, you can follow my footsteps as follows. The source I referred was primarily [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS). Others will be hyperlinked along the way.

## 1. Flexbox

This is the first important CSS module I learnt. It helps in the alignment and spacing between `items` on a `container` arranged in horizontal and vertical axes. Refer to the [MDN Flexbox Guide](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout#guides). It works with the `display:flex` property within a `<div>` with the class of `container`. The main concepts you should strongly grasp are:  
* The two axes of flexbox
* The flex container
* Key properties like `flex-direction`, `flex-wrap`, `align-items`, `justify-content` and `flex`(combining flex-start, flex-shrink & flex-basis). For these properties, I found the [Codevolution Flexbox Tutorial](https://www.youtube.com/playlist?list=PLC3y8-rFHvwg6rjbiMadCILrjh7QkvzoQ) very helpful.
 
Key Flexbox Concepts are:

`display: flex;` enables flexbox on the parent container.  
`justify-content: space-between;` distributes space evenly between flex items along the main axis.  
`align-items: center;` centers flex items vertically along the cross axis.  
`flex-direction: column;` used in responsive layout to stack elements vertically  
`flex-wrap: wrap;` allows flex items to wrap onto new lines when there's not enough space to fit them all on one line.  
`flex: 1;` distributes available space among flex items.  

_By default_, a flex item has a width which auto-adjusts to its content, and it cannot grow but can shrink, i.e. `flex: 0 1 auto`. By assigning say `flex:1` to an item, we allow it to grow when container width expands. But the `flex: 1` makes sense when compared with another item within the same container with say `flex: 2` which would grow twice as fast !

Check out my project on a Flexbox-based webpage [here](https://codepen.io/galaxyeagle/pen/LEPJQRR). 

## 2. Grid layout

This is the most popular alternative to the flexbox layout. It works with the `display:grid` property within the container. The [SlayingTheDragon Tutorial on Grids](https://www.youtube.com/watch?v=EiNiSFIPIQE) explains it best.
