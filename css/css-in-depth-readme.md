README for CSS Learning
## Book referred CSS in Depth
### Rerence source
- https://github.com/CSSInDepth/css-in-depth
### Reviewing the Fundamentals
Fundamentally, CSS is about declaring rules: Under various conditions, we want certain things to happen. If this class is added to that element, apply these styles. If element X is a child of element Y, apply those styles. The browser then takes these rules, figures out which ones apply where, and uses them to render the page.

#### The Cascade

- The cascade is the name for this set of rules. It determines how conflicts are resolved
- When declarations conflict, the cascade considers three things to resolve the difference:
  - Stylesheet origin : Where the styles come from. Your styles are applied in conjunction with the browser’s default styles.
  - Selector specificity : Which selectors take precedence over which.
  - Source order : Order in which styles are declared in the stylesheet.
  - Flow of cascade showing declaration precedence :
     Conflicting declaration 
     - Check if different origin or importance? 
       - If Yes :  Use Declaration with higher priority origin
     - If no : Is one an inline style (scope) ?
       - If Yes : Use inline declaration
     - If no : Do selectors have different specificity?
       - If yes : Use declaration with higher specificity.
     - If no: Use Declaration that comes later in the source order 
  - Understanding Stylesheet origin     
    - Yours are called author styles; 
    - there are also user agent styles, which are the browser’s default styles. 
    - User agent styles have lower priority, so your styles override 
    - There’s an exception to the style origin rules: declarations that are marked as
      important. A declaration can be marked important by adding !important to the end of the declaration, before the semicolon: `color : red !important;`
    -  Declarations marked !important are treated as a higher-priority origin, so the overall order of preference, in decreasing order, is this:
        - Author important
        - Author
        - User Agent
    - The cascade independently resolves conflicts for every property of every element on the page    
  - A quick review of terminology
    - `color: black;` - This is called a declaration. This declaration is made up of a property (color) and a value (black)
    -  A group of declarations inside curly braces is called a declaration block.
       ```
       body {
         color: black;
         font-family: Helvetica;
       }
       ```
    - A declaration block is preceded by a selector (in this case, body)
    - Together, the selector and declaration block are called a ruleset.  
    - A ruleset is also called a rule 
    - at-rules are language constructs beginning with an “at” symbol, such as @import rules or @media queries.
  - Understanding specificity
    - The browser evaluates specificity in two parts: styles applied inline in the HTML and styles applied using a selector.
    - If you use an HTML style attribute to apply styles, the declarations are applied only to that element. 
       - These are, in effect, “scoped” declarations, which override any declarations applied from your stylesheet or a <style> tag.
       - Inline styles have no selector because they are applied directly to the element they target.
    - To override inline declarations in your stylesheet, you’ll need to add an !important to the declaration, shifting it into a higher-priority origin
    - If the inline styles are marked important, then nothing can override them.    
    - The second part of specificity is determined by the selectors.
    - For instance, a selector with two class names has a higher specificity than a selector with only one. 
    - ID selector has a higher specificity than a class selector
    - for example. In fact, a single ID has a higher specificity than a selector with any number of classes. 
    - a class selector has a higher specificity than a tag selector (also called a type selector).
    - The exact rules of specificity are:
      - If a selector has more IDs, it wins (that is, it’s more specific).
      - If that results in a tie, the selector with the most classes wins.
      - If that results in a tie, the selector with the most tag names wins.
    - Pseudo-class selectors (for example, :hover) and attribute selectors (for example, [type="input"]) each have the same specificity as a class selector. 
    - The universal selector (*) and combinators (>, +, ~) have no effect on specificity. 
    - The root node is the ancestor of all other elements in the document. It has a special pseudo-class selector (:root) that you can use to target it. This is equivalent to using the type selector html with the specificity of a class rather than a tag. 
    - A common way to indicate specificity is in a number form, often with commas between each number. 
      (inline(0/1) , id , class , tag)  e.g. `{body header.page-header h1}` mean 0,0,1,3
      inline value is optional and it can be 1 or 0 for being there or not
    - When you give several declarations an !important, then the origins match and the regular specificity rules apply. 
  - Understanding source order
    - If the origin and the specificity are the same, then the declaration that appears later in the stylesheet—or appears in a stylesheet included later on the page—takes precedence.
  - A declaration that “wins” the cascade is called a cascaded value. 
     
#### The Inheritance

- If an element has no cascaded value for a given property, it may inherit one from an ancestor element.
- Not all properties are inherited, however. By default, only certain ones are. 
  - They are primarily properties pertaining to text: color, font, font-family, font-size, font-weight, font-variant, font-style, line-height, letter-spacing, text-align, text-indent, text-transform, white-space, and word-spacing.
  - A few others inherit as well, such as the list properties :list-style, list-style -type, list-style-position, and list-style-image
  - The table border properties, border-collapse and border-spacing, are also inherited
- a peculiar quirk of inheritance: when an element has a value defined using a length (px, em, rem, and so forth), its computed value is
  inherited by child elements. When units such as ems are specified for a line height, their value is calculated, and that calculated value 
  is passed down to any inheriting children. With the line-height property, this can cause unexpected results if the child element has a different font size, like the overlapping text.  

####  SPECIAL VALUES

- There are two special values that you can apply to any property to help manipulate the cascade: `inherit` and `initial`.
- Using the inherit keyword
  - Sometimes, you’ll want inheritance to take place when a cascaded value is preventing it. 
  - To do this, you can use the keyword inherit. `color : inherit;`
  - You can also use the inherit keyword to force inheritance of a property not normally inherited, such as border or padding. 
- Using the initial keyword
  - Sometimes you’ll find you have styles applied to an element that you want to undo. You can do this by specifying the keyword `initial`.
  - If you assign the value initial to that property, then it effectively resets to its default value.   
  - `initial` resets to the initial value for the property, not the element
  
#### SHORTHAND PROPERTIES

- Shorthand properties are properties that let you set the values of several other properties at one time.
- Most shorthand properties let you omit certain values and only specify the bits you’re concerned with.
- It’s important to know, however, that doing this still sets the omitted values; they’ll be set implicitly to their initial value.
  This can silently override styles you specify elsewhere.
- **Beware shorthands silently overriding other styles**
- Understanding the order of shorthand values
  - Shorthand properties try to be lenient when it comes to the order of the values you specify. You can set border: 1px solid black or border: black 1px solid and either will work.
  - But there are many properties where the values can be more ambiguous. In these cases, the order of the values is significant. 
    - For these properties, the values are in clockwise order, beginning at the top.
    - In fact, the word TRouBLe is an mnemonic you can use to remember the order: top, right, bottom, left.
  - Properties whose values follow this pattern also support truncated notations. 
   i.e. If the declaration ends before one of the four sides is given a value, that side takes its value from the opposite side.  
  - The TRouBLe mnemonic only applies to properties that apply individually to all four sides of the box. Other properties only support up to two values.
  - These include properties like background-position, box-shadow, and text-shadow (although these aren’t shorthand properties, strictly speaking).
  - Compared to the four-value properties like padding, the order of these values is reversed.
    - Whereas padding: 1em 2em specifies the vertical top/bottom values first, followed by the horizontal right/left values,
    - background-position: 25% 75% specifies the horizontal right/left values first, followed by the vertical top/bottom values. 
  - Although it seems counter-intuitive that these are opposite, the reason for this is straightforward: the two values represent a Cartesian grid.
  -  Cartesian grid measurements are typically given in the order x, y (horizontal and then vertical).     


### Working with Relative units

#### THE POWER OF RELATIVE VALUES

- CSS brings a late-binding of styles to the web page: The content and its styles aren’t pulled together until after the authoring of both is complete.
  - it also provides more power—one stylesheet can be applied to hundreds, even thousands, of pages.
  - Furthermore, the final rendering of the page can be altered directly by the user, who, for example, can change the default font size or resize the browser window.
- Less common absolute units are mm (millimeter), cm (centimeter), in. (inch), pt (point—typographic term for 1/72nd of an inch), and pc (pica—typographic term for 12 points). 
  - 1 in. = 25.4 mm = 2.54 cm = 6 pc = 72 pt = 96 px. 
- a CSS pixel does not strictly equate to a monitor’s pixel.
- When in doubt, use rems for font size, pixels for borders, and ems for most other properties.
- EMS AND REMS
  - Ems, the most common relative length unit, are a measure used in typography, referring to a specified font size. 
  -  In CSS, 1 em means the font size of the current element; its exact value varies depending on the element you’re applying it to.
  - Using ems to define font-size
    - ems are defined by the current element’s font size. But, if you declare font size: 1.2em, what does that mean?
    - font-size ems are derived from the inherited font size.
    - What makes ems tricky is when you use them for both font size and any other properties on the same element. 
      When you do this, the browser must calculate the font size first, and then it uses that value to calculate the other values.
      Both properties can have the same declared value, but they’ll have different computed values.
  - Using rems for font-size
    - Rem is short for root em   
    - Instead of being relative to the current element, rems are relative to the root element.
#### VIEWPORT-RELATIVE UNITS
  - viewport-relative units for defining lengths relative to the browser’s viewport
  - viewport—The framed area in the browser window where the web page is visible. This excludes the browser’s address bar, toolbars, and status bar, if present.
  - vh—1/100th of the viewport height
  - vw—1/100th of the viewport width
  - vmin—1/100th of the smaller dimension, height or width (IE9 supports vm instead of vmin)
  - vmax—1/100th of the larger dimension, height or width (not supported in IE or, at the time of writing, Edge)
  - One application for viewport-relative units that may not be immediately obvious is font size. In fact, I find this use more practical than applying vh and vw to element heights or widths.
  - Using calc() for font size 
    -The calc() function lets you do basic arithmetic with two or more values. 
    -This is particularly useful for combining values that are measured in different units.
    - This function supports addition (+), subtraction (-), multiplication (*) and division (/). 
    - The addition and subtraction operators must be surrounded by whitespace, for example, calc(1em + 10px).
#### UNITLESS NUMBERS AND LINE-HEIGHT
  - Some properties allow for unitless values (that is, a number with no specified unit). 
  - Properties that support this include line-height, z-index, and font-weigh
  - You can also use the unitless value 0 anywhere a length unit (such as px, em, or rem) is required because, in these cases, the unit does not matter—0 px equals 0% equals 0 em.
  - A unitless 0 can only be used for length values and percentages, such as in paddings, borders, and widths.
    It can’t be used for angular values, such as degrees or time-based values like seconds.
  - The line-height property is unusual in that it accepts both units and unitless values.  
  - You should typically use unitless numbers because they’re inherited differently.
  - These results are due to a peculiar quirk of inheritance: 
    when an element has a value defined using a length (px, em, rem, and so forth), its computed value is inherited by child elements.
    `When you use a unitless number, that declared value is inherited, meaning its computed value is recalculated for each inheriting child element.`
    When units such as ems are specified for a line height, their value is calculated, and that calculated value is passed down to any inheriting children.
    With the line-height property, this can cause unexpected results if the child element has a different font size, like the overlapping text.
  - length—The formal name for a CSS value that denotes a distance measurement. It’s a number followed by a unit, such as 5 px. Length comes in two flavors: absolute and relative. Percentages are similar to lengths, but strictly speaking, they’re not considered lengths.
#### CUSTOM PROPERTIES (AKA CSS VARIABLES)
  - The name must begin with two hyphens (--) to distinguish it from CSS properties, followed by whatever name you’d like to use.
  `:root {
       --main-font: Helvetica, Arial, sans-serif;
     }`
  - Variables must be declared inside a declaration block. 
  - A function called var() allows the use of variables. 

  `p {                                   
    font-family: var(--main-font , sans-serif);       
  }`
 
  - The var() function accepts a second parameter, which specifies a fallback value. If the variable specified in the first parameter is not defined, then the second value is used instead.
  - If a var() function evaluates to an invalid value, the property will be set to its initial value.
  - But what makes them particularly interesting is that the declarations of custom properties cascade and inherit:
    You can define the same variable inside multiple selectors, and the variable will have a different value for various parts of the page.

### Mastering the box model
#### DIFFICULTIES WITH ELEMENT WIDTH
- default behavior of the box model is that when you set the width or height of an element, you’re specifying the width or height of its content; any padding, border, and margins are then added to that width.
  - when you set 70% size and 30% size Instead of the two columns sitting side by side, they line wrapped. because of padding and border getting added later making total more than 100%
- Fix for the issue
  - Using magic number like 26% instead of 30%. Never do this.
  - using calc() function and reducing padding. `calc(30% -3em)`  - not great either
  - Adjust the box model - recommended
- Adjusting the box model
  - you’ll want your specified widths to include the padding and borders. 
  - CSS allows you to adjust the box model behavior with its `box-sizing` property.  
  - By default, `box-sizing `is set to the value of `content-box`. 
     - This means that any height or width you specify only sets the size of the content box. 
  - You can assign a value of `border-box` to the box sizing instead.    
    - That way, the height and width properties set the combined size of the content, padding, and border,
-  Using universal border-box sizing
   - It would be nice to fix it once, universally for all elements
   -  You can do this with the universal selector (*)    
     `*,
      ::before,
      ::after{
        box-sizing: border-box;
      }`
   - above code Applies border box sizing to all elements and pseudo-elements on the page
   - If, however, you add third-party components with their own CSS to your page, you may see some broken layouts for those components, Because the universal border-box fix targets every element in the component with the universal selector, correcting this can be problematic.
   - `:root{
        box-sizing :border-box;
      }
      *,::before, ::after{
        box-sizing :inherit;
      }`
     Box sizing isn’t normally an inherited property, but by using the inherit keyword, you can force it to be.
   -  With the version shown here, you can convert a third-party component into a content-box when necessary by targeting its top-level container. Then all elements inside the component will inherit the box sizing
    `.third-party-component {
       box-sizing: content-box;
     }`     
- Adding a gutter between columns
  - It’s often more visually appealing to have a small gap (or gutter) between columns. It can be achived using multiple ways
  - approach 1 :  add a margin to one of the columns and adjust the widths of your elements to account for the added space `width :29%; margin-left:1%;`
  - to specify the gutter in units other than a percentage use cal();
    - it has the benefit of being a little more explicit in code.   
    - this allow you to use ems rather than percentages for the gutter

#### DIFFICULTIES WITH ELEMENT HEIGHT
- Normal document flow is designed to work with a constrained width and an unlimited height.
- Controlling overflow behavior
  - control the exact behavior of the overflowing content with the overflow property : visible (default value) , hidden , scroll , auto
  - control only horizontal overflow using the overflow-x property, or vertical overflow with overflow-y. 
- Applying alternatives to percentage-based heights
  - For percentage-based heights to work, the parent must have an explicitly defined height.
  - A better approach is to use the viewport-relative vh units
  - Creating columns of equal heights
    - CSS table Layout
    - Flexbox   
- Two properties that can be immensely helpful are `min-height` and `max-height`. Instead of explicitly defining a height, you can use these properties to specify a minimum or maximum value, allowing the element to size naturally within those bounds.
- Similar properties `min-width` and `max-width` constrain an element’s width.
- Vertically centering content
  - A `vertical-align` declaration only affects inline and table-cell elements.      
- Guide to vertical centering
  - Can you use a natural height container?  Apply an equal top and bottom padding to the container to center its contents.
  - Do you need a specific height container, or do you need to avoid using padding? Use display: table-cell and vertical-align: middle on your container.
  - Can you use flexbox? If you don’t need to support IE9, you can center your content with flexbox
  - Is the inner content only one line of text? Set a tall line height equal to the desired container height. This will force the container to grow to contain the line height. If the contents aren’t inline, you may have to set them to inline -block.
  - Do you know the height of both the container and the inner content? Center the contents with absolute positioning. 
  - What if you don’t know the height of the inner element? Use absolute positioning in conjunction with a transform.   
  - When in doubt, see http://howtocenterincss.com.     
  
#### NEGATIVE MARGINS
- If applied to the left or top, the negative margin moves the element leftward or upward, respectively
- If applied to the right or bottom side, a negative margin doesn’t shift the element; instead, it pulls in any succeeding element.
- Using negative margins to overlap elements can render some elements unclickable if they’re moved beneath other elements.

#### COLLAPSED MARGINS
- When top and/or bottom margins are adjoining, they overlap, combining to form a single margin. This is referred to as collapsing.
- The size of the collapsed margin is equal to the largest of the joined margins.
- Elements don’t have to be adjacent siblings for their margins to collapse.
- Margin collapsing only occurs with top and bottom margins. Left and right margins don’t collapse.
- Collapsing outside a container
  - Applying overflow: auto (or any value other than visible) to the container prevents margins inside the container from collapsing with those outside the container. This is often the least intrusive solution.
  - Adding a border or padding between two margins stops them from collapsing.
  - Margins won’t collapse to the outside of a container that is floated, that is an inline block, or that has an absolute or fixed position.
  - When using a flexbox, margins won’t collapse between elements that are part of the flex layout. This is also the case with grid layout .
  - Elements with a table-cell display don’t have a margin, so they won’t collapse. This also applies to table-row and most other table display types. Exceptions are table, table-inline, and table-caption
- Spacing elements within a container
  - a lobotomized owl selector. It looks like this: * + *.
  - That’s a universal selector (*) that targets all elements, followed by an adjacent sibling combinator (+), followed by another universal selector.   
  

## From general Notes etc
### reference https://www.smashingmagazine.com/2019/01/how-to-learn-css/

- Some selectors act as if you had applied a class to something in the document. 
  - `p:first-child` behaves as if you added a class to the first p element, these are known as pseudo-class selectors.
  - The pseudo-element selectors act as if an element was dynamically inserted, for example `::first-line` acts in a similar way to you wrapping a `span` around the first line of text. 

## SCSSS Notes
- Variables can be declared in SCSS as `$var` e.g. `$text-color: DarkGray;`
- partials are created to modularize the CSS development. to create a partial file the name must start with underscore (_) e.g. _variables.scss
- What is the difference between mixin and inheritence?
  - mixin just reduces the code size in scss file but the final css output fil with have code duplicated whereever they are used leading to bigger css files
  - inheritence leads to no code duplication but in exchange it leads to creation of lot of selectors
  - inhenritance currently has limitation of usage insde media query as well as that only one elector can be extended
  - even though file generated by mixin is bigger if the code is pushed into production after _gsipppying_ the mixin performs better because gsippying eliminates duplicate code

### imports
- partial needs to be imported use @import e.g. `@import "variables"` . when importing remove the _ in import statement.
- imports can be regular CSS import types like import url , css file with media , http and another css file
- one import directive can be used to import multiple files as well
- in the context of CSS imports you can use variable interpolation sntax which is `#{var}`
- always first import SCSS imports and then CSS imports
### Media Queries
- media queries can be nested inside scss definitions , whereas in css i has to be its own thing
- media queries can be mised with mixins
### Operators
### Functions
- there are built in functions and you can create your own function too
- to create your own function include `@function` directive
- to return value from a function use `@return` directive
- you can define extend only selector so CSS won't generate code for it until its extended. e.g. `%highlight{}` with create extend only selector
- there is `!optional` import featre which allows you to import a selector and if it does not exists , ignores it
### Inheritance
- inheritence can be achieved by using `@extend` keyword;
- only one selection can be extended at a time i.e. you can not extend a selector that has multiple classes seperated by space
- When using extend inside media query you can extend the rules that are defined inside media query and not from outside
### COnditional Directives
### Loops
### mixins
- Mixin is a way to create partial style rule and then include it is another rule. to create mixing declare a rule with `@mixin` in front e.g. 
```@mixin warning {
  background-color: orange;
  color: white;
}  

 ```
to use it include it. e.g.
```.warning-button {
  @include warning;
  padding: 8px 12px;
}
```
- one mixin can be added into another mixin
- mixin can be include inside a selector like above example or it can be included as standalone in the scss file directly at root e.g.
```
@mixin fancy-links{
  a{
    font-style: italic;
    text-decoration: none;
  }
}
@include fancy-links;
```
- mixin can be declared with argumets and in that case should be called like funtion. Mixin can be called like class variable when they don't have any argumets or as function with empty parenthesis. 
- maxin arguments can have default value which can be defined like `@mixin box($radius: 6px)` 
- if there are multiple argumets with default values and you want to override only one of them then explicitly name the variable at the time of calling mixin. e.g.
```
  @mixin box($radius: 6px, $border :1px solid $error-color) {
    @include rounded($radius);
    border: $border;
  }

  @include box($radius: 4px);
```
- if mixin is called with argumet's names the order of ar
- cool scss syntax to use var name like `font-size` and `font-weight` is as below
    ```
    font: {
    size: 24px;
    weight: bold;
  }
    ```
- `@content` is good directive to pass code blocks into the mixins when calling it. specially useful for browser specific hecks. it can make code more readable



  