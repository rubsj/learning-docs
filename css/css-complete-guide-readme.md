# Reference - CSS - The Complete Guide (incl. Flexbox, Grid and Sass) by Manuel Lorenz Maximilian Schwarzmüller

### Basic CSS Concepts
#### Fonts
- Default fonts are provided by browser and you can change it in browser settings to change the defaults
- to add custom fonts add `link` in html header with path for custom font and then use that custom font in font style. 
i.e. 
`<link href="https://fonts.googleapis.com/css2?family=Anton&display=swap" rel="stylesheet">`
`font-family: 'Anton', sans-serif;`
#### Bckground
- multiple background images can be set to the container
- only one background color can be set.
- If both background color and background image are set the image will be in front of the color.
- different background images can be stacked on top of each other by defining each background ans sepeating by comma.
##### Bckground-size
- background size : this property is useful when setting image in the background and sizing the image.  size can be set in three ways
  -> single value - single value represents width of the image and height will be auto
  -> two values - first valur represent width and second value represent height
  -> key - can be cover / contain 
     - cover : the image will cover the full container size , it can mean image's aspect ratio is not maintained
     - contain : the container will fit the image while maintaining its aspect ratio which mean that there could be white space left in the container. this would also mean full image is always visible wthout cropping
     - auto : scales the image in corresponding direction maintaining the aspect ratio. which means white space can be left in container and this also mean the whole image might not fir and is cropped
- background size : space not covered by background image in background size gets filled by background color. and the bg color will be visible behind bg image that have transpaency/translucency.
- background size : for single/two value possible value can be defined  as <length> , <percentage> or auto.
  -> length streches the image to meet the specified length. if length is smaller than original image length image gets tiled unless background-repeat is set to no-repeat.
  -> percentage - sets the width and height if image in percent of the parent element. 50% will mean 50% of the size of parent container.

 ##### Bckground-poistion
 - background-position :  bg position property sets the startiing position of the bg image. position is relative to the position set by background-origin
 - poistion defines x/y coordinate. It can be defined using one to four values.
    - 1-value syntax : 
        -> center : `background-position: center;` centers the image
        -> top/left/bottom/right : `background-position: left;` edge against which to place the item and other domention is set to 50% so the item is palced in the middle of edge specified
        ->A <length> or <percentage>. This specifies the X coordinate relative to the left edge, with the Y coordinate set to 50%
    - 2-value syntax: one value defines X and the other defines Y    
    - 3-value syntax: Two values are keyword values, and the third is the offset for the preceding value.
    - 4-value syntax: The first and third values are keyword value defining X and Y. The second and fourth values are offsets for the preceding X and Y keyword values.
 - Regarding Percentages:   e.e `background-position: 25% 75%;` 
    -> The percentage offset of the given background image's position is relative to the container. 
    -> Essentially what happens is the background image dimension is subtracted from the corresponding container dimension, and then a percentage of the resulting value is used as the direct offset from the left (or top) edge. which can also be said to mean that when image is placed in container any cropping that needs to be done will be done based on the % defined. default is 50% which means equal cropping from both side. 25% mean only 25% cropping from say top and 75% cropping from bottom
    -> (container width - image width) * (position x%) = (x offset value)
    -> (container height - image height) * (position y%) = (y offset value)
  - to remember - if only percent value is provided it means its about chopping . other values means offsetting the image position corresponding to defined edge  
##### Bckground-origin
- it sets the origin of the background from border start , inside the border or inside the padding. 
- possible values are border box , padding-box , content-box
-background origin is ignored when backgground-attachment is fixed.
##### Bckground-clip
- defines whether the element's background extends underneath its border-box , padding-box or con content-box.This is applicable when image needs to be cropped.
- background clip overrides background-origin if they have different values.
##### Bckground-attachment
- defines whether background image's position is fixed within the viewport or scrolls with its containing block.

#### Color
#### Specificity
- Specificity follows following order
    -> first:  inline style
    -> second: ID
    -> third: Class , attribute and :pseudoclass
    -> fourth: elements/tag, ::pseudoelements
    -> fifth: browser defaults
    -> last: inheritence
 - Universal selector (*), combinators (+, >, ~, ' ', ||) , matches-any pseudo-class(:is()) and negation pseudo-class (:not()) have no effect on specificity. (The selectors declared inside :not() do, however.)  
 - The specificity-adjustment pseudo-class :where()  always has its specificity replaced with zero.
 - Styles for a directly targeted element will always take precedence over inherited styles 
#### Selectors

#### Miscellaneous
- styling the <img> tag.
    -> all the cool properties offiered for background image is *not* applicable to img tag
    -> only size can be set using width and height.
    -> to set the size it is not sufficient to have height/width defined for containing container as by default it will show its regular size.
    -> to set size on image the img tag has to be selected and height/width needs to be explicitly set.
    -> if containing container has fixed height/width then to be able to use img selector with height/width value of 100% or any other % value the containing container must be of block or inline-block type else the % value will not be honored
    -> *TIP* when styling image sometimes white background shows up below image because its inline element. to remove it either make the img selector to display block or add verticle alignment top or bottom.
 - filter - it applies graphical effects like blur or color shift to element. Filters are commonly used to adjust the rendering of image , background or borders   