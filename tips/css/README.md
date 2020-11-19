# Cascade, specificity, inheritance

```css
h1 {
  /* Tag selector */
  font-family: serif;
}
#page-title {
  /* Id selector */
  font-family: sans-serif;
}
.title {
  /* Class selector */
  font-family: monospace;
}
```

## user-agent

Styles from browser

## Priority

1 Author important `important!`
2 Author
3 User agent

## Specifity

The exact rules of specificity are:
1- If a selector has more IDs, it wins (that is, it’s more specific).
2- If that results in a tie, the selector with the most classes wins.
3- If that results in a tie, the selector with the most tag names wins.

Selector IDs Classes Tags Notation
html body header h1 0 0 4 0,0,4
body header.page-header h1 0 1 3 0,1,3
.page-header .title 0 2 0 0,2,0
#page-title 1 0 0 1,0,0

## Two rules of thumb

1- Don’t use IDs in your selector. Even one ID ratchets up the specificity a lot. When you need to override the selector, you often don’t have another meaningful ID you can use, so you wind up having to copy the original selector and add another class to distinguish it from the one you are trying to override.

2- Don’t use !important. This is even more difficult to override than an ID, and once you use it, you’ll need to add it every time you want to override the original declaration—and then you still have to deal with the specificity

## Inheritance

Not all properties are inherited, however. By default, only certain ones are. In general, these are the properties you’ll want to be inherited. They are primarily properties pertaining to text: `color`, `font`, `font-family`, `font-size`, `font-weight`, `font-variant`, `font-style`, `line-height`, `letter-spacing`, `text-align`, `text-indent`, `text-transform`, `white-space`, and `word-spacing`.

### inherit (inherited from parent)

```css
a:link {
  color: blue;
}
.footer {
  color: #666;
  background-color: #ccc;
  padding: 15px 0;
  text-align: center;
  font-size: 14px;
}
.footer a {
  color: inherit; /* Inherits font color from the footer*/
  text-decoration: underline;
}
```

### initial (reset to default value)

# Working with relative units

`responsive`: In CSS, this refers to styles that “respond” differently, based on the size of the browser window. This entails intentional consideration for mobile, tablet, or desktop screens of any size. We’ll take a good look at responsive design in chapter 8, but in this chapter, I’ll lay some important groundwork before we get there.

## EMS and REMS

```css
.padded {
  font-size: 16px;
  padding: 1em; /* equals to 16px because font-size is 16px*/
}
```

```css
:root {
  font-size: 1em; /* relative to default browser font-size (16px in general) */
}
ul {
  font-size: 0.8rem; /* relative to the root element */
}
```

When in doubt, use `rems` for font size, `pixels` for borders, and `ems` for most other properties.

## calc()

```css
:root {
  font-size: calc(0.5em + 1vw);
}
```

## var()

```css
:root {
  --main-font: Helvetica, Arial, sans-serif;
}
p {
  font-family: var(--main-font);
}
```

# Box Model

`box-sizing`: `content-box` | `border-box`

content-box = content
border-box = content + padding + border (no margin)

By default, `box-sizing` is set to the value of `content-box`. This means that any height or width you specify only sets the size of the content box.

## Universal border-box sizing

```css
:root {
  box-sizing: border-box;
}
*,
::before,
::after {
  box-sizing: inherit;
}
```

After applying this to the page, height and width will always specify the actual height and width of an element. Padding won’t change them.

## Equal Height columns with flexbox

```css
.container {
  display: flex;
}
.main {
  width: 70%;
  background-color: #fff;
  border-radius: 0.5em;
}
.sidebar {
  width: 30%;
  padding: 1.5em;
  margin-left: 1.5em;
  background-color: #fff;
  border-radius: 0.5em;
}
```

Never explicitly set the `height` of an element unless you have no other choice. Always seek an alternative approach first. Setting a height invariably leads to further complications.

## Guide to vertical centering

The best approach to centering contents inside a container may depend on a number of factors based on your particular scenario. To help you decide,step through these questions until you encounter a solution that you can use. Some of these techniques will be covered in later chapters, so I’ve directed you to where you can find the answer.

1- Can you use a natural height container? Apply an equal top and bottom padding to the container to center its contents.

2- Do you need a specific height container, or do you need to avoid using padding? Use `display: table-cell` and `vertical-align: middle` on your
container.

3- Can you use flexbox? If you don’t need to support IE9, you can center your content with `flexbox`.

4- Is the inner content only one line of text? Set a tall `line-height` equal to the desired container height. This will force the container to grow to contain the line height. If the contents aren’t inline, you may have to set them to inline-block.

5- Do you know the height of both the container and the inner content? Center the contents with absolute positioning. (I only recommend this when all approaches mentioned here fail.)

6- What if you don't know the height of the inner element? Use absolute positioning in conjunction with a transform. (Again, I only recommend this if you've ruled out all other options.)

When in doubt, see http://howtocenterincss.com. It’s a great resource. You can fill in several options based on your exact scenario, and it will produce the code you can use for vertical centering.

## Collapsing margin

When top and/or bottom margins are adjoining, they overlap, combining to form a single margin. This is referred to as collapsing.

Here are ways to prevent margins from collapsing:

- Applying `overflow: auto` (or any value other than visible) to the container prevents margins inside the container from collapsing with those outside the container. This is often the least intrusive solution.
- Adding a `border` or `padding` between two margins stops them from collapsing.
- Margins won’t collapse to the outside of a container that is floated, that is an `inline block`, or that has an `absolute` or `fixed` position.
- When using a `flexbox`, margins won’t collapse between elements that are part of the flex layout. This is also the case with grid layout (chapter 6).
- Elements with a `table-cell` display don’t have a margin, so they won’t collapse. This also applies to `table-row` and most other table display types.

## display: block

The block element fills the available width and puts each link on its own line.

## the lobotomized owl selector \* + \*

That’s a universal selector (\*) that targets all elements, followed by an adjacent sibling combinator (+), followed by another universal selector. It earns its name because it resembles the vacant stare of an owl. The lobotomized owl is not unlike the selector you used earlier: .social-button+ .social-button. Except, instead of targeting buttons that immediately follow other buttons, it targets any element that immediately follows any other element. That is, it selects all elements on the page that aren’t the first child of their parent.

```css
body * + * {
  margin-top: 1.5em;
}
```

## Summary

- Always use a universal `border-box` fix for predictable element sizing.
- Avoid explicitly setting the `height` of an element to avoid overflow issues.
- Use modern layout techniques like `display: table` or a `flexbox` to produce columns of equal height or to vertically center content.
- If your margins behave oddly, take steps to prevent margins from collapsing.
- Consider using the lobotomized owl selector on your page to globally apply
  margins between stacked elements.

# Floats

A float pulls an element (often an image) to one side of its container,allowing the document flow to wrap around it.

## double container pattern

In our example, <body> serves as the outer container. By default, this is already 100% of the page width, so you won’t have to apply any new styles to it. Inside that, you’ve wrapped the entire contents of the page in a <div class="container">, which servesas the inner container. To that you’ll apply a max-width and auto margins to center the contents. Add this listing to your stylesheet.

```css
.container {
  max-width: 1080px;
  margin: 0 auto; /* Auto left and right margins will grow to fill the available space, centering the element within the outer container.*/
}
```

## clearfix

The problem is that, unlike elements in the normal document flow, floated elements do not add height to their parent elements.

One way you can correct this is with the float’s companion property, clear. If you place an element at the end of the main container and use clear, it causes the container to expand to the bottom of the floats.

```css
.media {
  float: left;
  width: 50%;
  padding: 1.5em;
  background-color: #eee;
  border-radius: 0.5em;
}

.clearfix::after {
  /*Targets the pseudo-element at the end of the container*/
  display: block; /* A non-inline display value and a content value cause the pseudoelement to appear in the document.*/
  content: ' ';
  clear: both; /* Makes the pseudo-element clear all floats in the container */
}
```

## Block formatting context

A block formatting context (sometimes called a BFC) is a region of the page in which elements are laid out. A block formatting context itself is part of the surrounding document flow, but it isolates its contents from the outside context. This isolation does three things for the element that establishes the BFC:
1- It contains the top and bottom margins of all elements within it. They won’t collapse with margins of elements outside of the block formatting context.
2- It contains all floated elements within it.
3- It doesn’t overlap with floated elements outside the BFC.

Put simply, the contents inside a block formatting context will not overlap or interact with elements on the outside as you would normally expect

- `float: left` or `float: right`, anything but none
- `overflow`: hidden, auto, or scroll, anything but visible
- `display`: inline-block, table-cell, table-caption, flex, inline-flex,
  grid, or inline-grid—these are called block containers.
- `position: absolute` or `position: fixed`

Usually `overflow: auto` does the trick!

## Grid system

```html
<main class="main clearfix">
  <h2>Running tips</h2>
  <div class="row">
    <div class="column-6">
      <div class="media">
        <img class="media-image" src="runner.png" />
        <div class="media-body">
          <h4>Strength</h4>
          <p></p>
        </div>
      </div>
    </div>
    <div class="column-6">
      <div class="media">
        <img class="media-image" src="shoes.png" />
        <div class="media-body">
          <h4>Cadence</h4>
          <p></p>
        </div>
      </div>
    </div>
  </div>
  <div class="row">
    <div class="column-6">
      <div class="media">
        <img class="media-image" src="shoes.png" />
        <div class="media-body">
          <h4>Change it up</h4>
          <p></p>
        </div>
      </div>
    </div>
    <div class="column-6">
      <div class="media">
        <img class="media-image" src="runner.png" />
        <div class="media-body">
          <h4>Focus on form</h4>
          <p></p>
        </div>
      </div>
    </div>
  </div>
</main>
```

```css
.row {
  margin-left: -0.75em;
  margin-right: -0.75em;
}
.row::after {
  content: ' ';
  display: block;
  clear: both;
}
[class*='column-'] {
  float: left;
  padding: 0 0.75em;
  margin-top: 0;
}
.column-1 {
  width: 8.3333%;
}
.column-2 {
  width: 16.6667%;
}
.column-3 {
  width: 25%;
}
.column-4 {
  width: 33.3333%;
}
.column-5 {
  width: 41.6667%;
}
.column-6 {
  width: 50%;
}
.column-7 {
  width: 58.3333%;
}
.column-8 {
  width: 66.6667%;
}
.column-9 {
  width: 75%;
}
.column-10 {
  width: 83.3333%;
}
.column-11 {
  width: 91.6667%;
}
.column-12 {
  width: 100%;
}
header {
  padding: 1em 1.5em;
  color: #fff;
  background-color: #0072b0;
  border-radius: 0.5em;
  margin-bottom: 1.5em;
}
.main {
  padding: 0 1.5em 1.5em;
  background-color: #fff;
  border-radius: 0.5em;
}
.container {
  max-width: 1080px;
  margin: 0 auto;
}
.media {
  padding: 1.5em;
  background-color: #eee;
  border-radius: 0.5em;
}
.media-image {
  float: left;
  margin-right: 1.5em;
}
.media-body {
  overflow: auto;
  margin-top: 0;
}
.media-body h4 {
  margin-top: 0;
}
.clearfix::before,
.clearfix::after {
  display: table;
  content: ' ';
}
.clearfix::after {
  clear: both;
}
```

# Flexbox

```css
.site-nav {
  display: flex;
  padding: 0.5em;
  background-color: #5f4b44;
  list-style-type: none;
  border-radius: 0.2em;
}
.site-nav > li {
  margin-top: 0;
}
.site-nav > li > a {
  display: block;
  padding: 0.5em 1em;
  background-color: #cc6b5a;
  color: white;
  text-decoration: none;
}
```

You’ll notice you made the links a `display: block`. If they were to remain inline, the height they’d contribute to their parent would be derived from their `line-height`.

```css
.site-nav > li + li {
  margin-left: 1.5em;
}
.site-nav > .nav-right {
  margin-left: auto; /* Auto margins inside a flexbox will fill the available space */
}
```

## flex-basis

~= `witdh` but more flexible

## flex-grow

If an item has a flex-grow of 0, it won’t grow past its flex basis. If any items have a non-zero growth factor, those items will grow until all of the remaining space is used up. This means the flex items will fill the width of the container.

Declaring a higher flex-grow value gives that element more “weight”; it’ll take a larger portion of the remainder. An item with flex-grow: 2 will grow twice as much as an item with flex-grow: 1

## flex-shrink

The flex-shrink value for each item indicates whether it should shrink to prevent overflow. If an item has a value of flex-shrink: 0, it will not shrink. Items with a value greater than 0 will shrink until there is no overflow. An item with a higher value will shrink more than an item with a lower value, proportional to the flex-shrink values.

## Holy grail layout

```css
.item-side {
  flex: 0 0 200px;
}
.item-center {
  flex: 1; /* grow to fill the remaining space */
}
```

# Grid

```html
<div class="grid">
  <div class="a">a</div>
  <div class="b">b</div>
  <div class="c">c</div>
  <div class="d">d</div>
  <div class="e">e</div>
  <div class="f">f</div>
</div>
```

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  grid-gap: 0.5em;
}
.grid > * {
  background-color: darkgray;
  color: white;
  padding: 2em;
  border-radius: 0.5em;
}
```

Grid item

```css
.container {
  display: grid;
  grid-template-columns: 2fr 1fr;
  grid-template-rows: repeat(4, auto); /* == auto auto auto
auto */
  grid-gap: 1.5em;
  max-width: 1080px;
  margin: 0 auto;
}

nav {
  grid-column: 1 / 3; /* Spans from vertical grid line 1 to grid line 3*/
  grid-row: span 1; /* Spans exactly one horizontal grid track*/
}
.main {
  grid-column: 1 / 2;
  grid-row: 3 / 5;
}
.sidebar-top {
  grid-column: 2 / 3;
  grid-row: 3 / 4;
}
.sidebar-bottom {
  grid-column: 2 / 3;
  grid-row: 4 / 5;
}
```

`span` keyword tells the browser that the item will span one grid track.

## Naming

```css
.container {
  display: grid;
  grid-template-columns:
    [left-start] 2fr
    [left-end right-start] 1fr
    [right-end];
  grid-template-rows: repeat(4, [row] auto); /* Names horizontal
grid lines “row” */
  grid-gap: 1.5em;
  max-width: 1080px;
  margin: 0 auto;
}
header,
nav {
  grid-column: left-start / right-end;
  grid-row: span 1;
}
.main {
  grid-column: left;
  grid-row: row 3 / span 2; /* Places the item beginning at the third row grid line and spanning two grid tracks */
}
.sidebar-top {
  grid-column: right; /* Spans from right-start to right-end */
  grid-row: 3 / 4;
}
.sidebar-bottom {
  grid-column: right;
  grid-row: 4 / 5;
}
```

## Grid area

```css
.container {
  display: grid;
  grid-template-areas:
    'title title'
    'nav nav'
    'main aside1'
    'main aside2';
  grid-template-columns: 2fr 1fr;
  grid-template-rows: repeat(4, auto);
  grid-gap: 1.5em;
  max-width: 1080px;
  margin: 0 auto;
}
header {
  grid-area: title;
}
nav {
  grid-area: nav;
}
.main {
  grid-area: main;
}
.sidebar-top {
  grid-area: aside1;
}
.sidebar-bottom {
  grid-area: aside2;
}
```

## explicit/implicit grid

```html
<div class="portfolio">
  <figure class="featured">
    <img src="images/monkey.jpg" alt="monkey" />
    <figcaption>Monkey</figcaption>
  </figure>
  <figure>
    <img src="images/eagle.jpg" alt="eagle" />
    <figcaption>Eagle</figcaption>
  </figure>
  <figure class="featured">
    <img src="images/bird.jpg" alt="bird" />
    <figcaption>Bird</figcaption>
  </figure>
  <figure>
    <img src="images/bear.jpg" alt="bear" />
    <figcaption>Bear</figcaption>
  </figure>
  <figure class="featured">
    <img src="images/swan.jpg" alt="swan" />
    <figcaption>Swan</figcaption>
  </figure>
  <figure>
    <img src="images/elephants.jpg" alt="elephants" />
    <figcaption>Elephants</figcaption>
  </figure>
  <figure>
    <img src="images/owl.jpg" alt="owl" />
    <figcaption>Owl</figcaption>
  </figure>
</div>
```

```css
.portfolio {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-auto-rows: 1fr;
  grid-gap: 1em;
}
```

By specifying `minmax(200px, 1fr)`, the browser ensures that all tracks are at least 200 px wide.

The `auto-fill` keyword is a special value you can provide for the repeat() function. With this set, the browser will place as many tracks onto the grid as it can fit, without violating the restrictions set by the specified size (the minmax() value).

Together, auto-fill and minmax(200px, 1fr) mean your grid will place as many
grid columns as the available space can hold, without allowing any of them to be smaller than 200 px. And because no track can be larger than 1 fr (our maximum value), all the grid tracks will be the same size.

## Image fill

```css
.portfolio > figure {
  display: flex;
  flex-direction: column;
  margin: 0;
}
.portfolio img {
  flex: 1; /* Uses flex grow to make the image fill the available space in the flex container */
  object-fit: cover; /* Allows the image to fill the box without being stretched (cropping edges instead) */
  max-width: 100%;
}
```

## Feature queries @supports (not working on IE)

```css
@supports (display: grid) {
  /* ... */
}
```

# Positioning and Stacking Contexts

default `position: static`

## fixed positioning

Applying `position: fixed` to an element lets you position the element arbitrarily within the viewport. This is done with four companion properties: `top`, `right`, `bottom`, and `left`

### Modal

```html
<header class="top-banner">
  <div class="top-banner-inner">
    <p>
      Find out what's going on at Wombat Coffee each month. Sign up for our
      newsletter:
      <button id="open">Sign up</button>
    </p>
  </div>
</header>
<div class="modal" id="modal">
  <div class="modal-backdrop"></div>
  <div class="modal-body">
    <button class="modal-close" id="close">close</button>
    <h2>Wombat Newsletter</h2>
    <p>Sign up for our monthly newsletter. No spam. We promise!</p>
    <form>
      <p>
        <label for="email">Email address:</label>
        <input type="text" name="email" />
      </p>
      <p><button type="submit">Submit</button></p>
    </form>
  </div>
</div>
<script type="text/javascript">
  var button = document.getElementById('open');
  var close = document.getElementById('close');
  var modal = document.getElementById('modal');
  button.addEventListener('click', function (event) {
    event.preventDefault();
    modal.style.display = 'block';
  });
  close.addEventListener('click', function (event) {
    event.preventDefault();
    modal.style.display = 'none';
  });
</script>
```

```css
.modal {
  display: none; /* Hides the modal by default; JavaScript will set display: block when it opens the modal.*/
}
.modal-backdrop {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
}
.modal-body {
  position: fixed;
  top: 3em;
  bottom: 3em;
  right: 20%;
  left: 20%;
  padding: 2em 3em;
  background-color: white;
  overflow: auto;
}
.modal-close {
  cursor: pointer;
}
```

## Absolute positioning

Absolute positioning works the same way, except it has a different containing block. Instead of its position being based on the viewport, its position is based on the closest positioned ancestor element. As with a fixed element, the properties top, right, bottom, and left place the edges of the element within its containing block.

Adding a close button to fixed potionned Modal

```css
.modal-close {
  position: absolute;
  top: 0.3em;
  right: 0.3em;
  padding: 0.3em;
  cursor: pointer;
}
```

## Relative positioning

The top, right, bottom, and left properties, if applied, will shift the element from its original position, but it won’t change the position of any elements around it.
The relatively positioned element, and all elements on the page around it, will remain exactly where they were

### Dropdown menu

```html
<div class="container">
  <nav>
    <div class="dropdown">
      <div class="dropdown-label">Main Menu</div>
      <div class="dropdown-menu">
        <ul class="submenu">
          <li><a href="/">Home</a></li>
          <li><a href="/coffees">Coffees</a></li>
          <li><a href="/brewers">Brewers</a></li>
          <li><a href="/specials">Specials</a></li>
          <li><a href="/about">About us</a></li>
        </ul>
      </div>
    </div>
  </nav>
  <h1>Wombat Coffee Roasters</h1>
</div>
```

```css
.container {
  width: 80%;
  max-width: 1000px;
  margin: 1em auto;
}
.dropdown {
  display: inline-block;
  position: relative; /*Establishes the containing block*/
}
.dropdown-label {
  padding: 0.5em 1.5em;
  border: 1px solid #ccc;
  background-color: #eee;
}
.dropdown-menu {
  display: none; /*Hides the menu initially*/
  position: absolute;
  left: 0;
  top: 2.1em;
  min-width: 100%;
  background-color: #eee;
}
.dropdown:hover .dropdown-menu {
  display: block;
}
.submenu {
  padding-left: 0;
  margin: 0;
  list-style-type: none;
  border: 1px solid #999;
}
.submenu > li + li {
  border-top: 1px solid #999;
}
.submenu > li > a {
  display: block;
  padding: 0.5em 1.5em;
  background-color: #eee;
  color: #369;
  text-decoration: none;
}
.submenu > li > a:hover {
  background-color: #fff;
}
```

## Stacking context and z-index

The elements appearing later in the markup are painted over the top of the previous ones.

This behavior changes when you start positioning elements. The browser first paints all non-positioned elements, then it paints the positioned ones. By default, any positioned element appears in front of any non-positioned elements.

Typically, modals are added to the end of the page as the last bit of content before the closing </body> tag.

A stacking context consists of an element or a group of elements that are painted together by the browser. One element is the root of the stacking context, so when you add a z-index to a positioned element that element becomes the root of a new stacking context.

All the elements within a stacking context are stacked in this order, from back to front:

- The root element of the stacking context
- Positioned elements with a negative z-index (along with their children)
- Non-positioned elements
- Positioned elements with a z-index of auto (and their children)
- Positioned elements with a positive z-index (and their children)

## Summary

- Use fixed positioning for modal dialogs.
- Use absolute positioning for dropdown menus, tooltips, and other dynamic interactions.
- Be aware of accessibility concerns when building these features.
- There are two gotchas of z-index: it only works on positioned elements and
  using it creates a new stacking context.
- Be aware of the potential pitfalls when creating multiple stacking contexts on a page.
- Keep an eye out for better browser support of sticky positioning.

# Responsive design

The three key principles to responsive design:

1. A mobile first approach to design. This means you build the mobile version before you construct the desktop layout.
2. The `@media` at-rule. With this rule, you can tailor your styles for viewports of different sizes. This syntax (often called media queries) lets you write styles that only apply under certain conditions.
3. The use of fluid layouts. This approach allows containers to scale to different sizes based on the width of the viewport.

## Mobile first

Don't forget the viewport meta tag.

```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Wombat Coffee Roasters</title>
  <link href="styles.css" />
</head>
```

The meta tag’s content attribute indicates two things. First, it tells the browser to use the device width as the assumed width when interpreting the CSS, instead of pretending to be a full size desktop browser. Second, it uses initial-scale to set the zoom level at 100% when the page loads.

## Media queries

Media queries allow you to write a set of styles that only apply to the page under certain conditions. This lets you tailor your styles differently, based on the screen size.

```css
@media (min-width: 35em) {
  .title > h1 {
    font-size: 2.25rem;
  }
}
```

You should use ems for media query breakpoints. It’s the only unit that performs consistently in all major browsers should the user zoom the page or change the default font size.

```css
@media (min-width: 20em) and (max-width: 35em) { … }
```

- (min-height: 20em)—Targets viewports 20 em and taller
- (max-height: 20em)—Targets viewports 20 em and shorter
- (orientation: landscape)—Targets viewports that are wider than they are tall
- (orientation: portrait)—Targets viewports that are taller than they are wide
- (min-resolution: 2dppx)—Targets devices with a screen resolution of 2 dots
  per pixel or higher; targets retina displays
- (max-resolution: 2dppx)—Targets devices with a screen resolution of up to 2 dots per pixel

https://developer.mozilla.org/en-US/docs/Web/CSS/@media

The two media types you’ll generally need to think about are `screen` and `print`.

```css
@media print {
  * {
    color: black !important;
    background: none !important;
  }
}
```

### breakpoints

```css
.title {
  ...;
}
@media (min-width: 35em) {
  .title {
    ...;
  }
}
@media (min-width: 50em) {
  .title {
    ...;
  }
}
```

## Responsive column

Web designer Brad Frost has compiled a list of responsive patterns that you can browse at https://bradfrost.github.io/this-is-responsive/patterns.html. You can arrange your columns for responsive design as a wide column and a narrow column, equalwidth columns, or a two-column or three-column layout, to name a few examples.

Ultimately, these arrangements come down to a variation of our approach here, with a combination of columns or column widths.

Sometimes, you won’t even need the media queries, as natural line wrapping will take care of that for you. Flexbox layouts using `flex-wrap: wrap` and a reasonable `flex-basis` is an excellent way to do this. Similarly, a `grid` layout with `auto-fit` or `auto-fill` grid columns will determine how many items will fit in a row before wrapping to a new one. You could also use `inline-block` elements, though in that case, they
won’t grow to fill the width of the container.

## Fluid Layout

Fluid layout (sometimes called liquid layout) refers to the use of containers that grow and shrink according to the width of the viewport.

In a fluid layout, the main page container typically doesn’t have an explicit `width`, or it has one defined using a `percentage`. It may, however, have left and right `padding`, or `auto` left and right `margins` to add breathing room between its edges and the edges of the viewport. This means it can be slightly narrower than the viewport, but never wider.

Inside the main container(s), any columns are defined using a percentage (for instance, a main column of 70% width and a sidebar of 30% width). This way, no matter what the screen width is, the containers fit within. A `flexbox` layout works as well, assuming the flex items have `flex-grow` and, more importantly, `flex-shrink` values that allow the items to fit regardless of the screen width.

## Responsive images

The best practice is to create a few copies of an image, each at a different resolution. If you know, based on media queries, that the screen is a certain size, there’s no sense sending an extremely large image; the browser will have to downscale it to make it fit.

```css
.hero {
  padding: 2em 1em;
  text-align: center;
  background-image: url(coffee-beans-small.jpg); /* uses small image on mobile */
  background-size: 100%;
  color: #fff;
  text-shadow: 0.1em 0.1em 0.3em #000;
}
@media (min-width: 35em) {
  .hero {
    padding: 5em 3em;
    font-size: 1.2rem;
    background-image: url(coffee-beans-medium.jpg);
  }
}
@media (min-width: 50em) {
  .hero {
    padding: 7em 6em;
    background-image: url(coffee-beans.jpg);
  }
}
```

### srcset

This attribute is a newer addition to HTML. It allows you to specify multiple image URLs for one `<img>` tag, specifying the resolution of each. The browser will then figure out which image it needs and download that one.

```html
<img
  alt="A white coffee mug on a bed of coffee beans"
  src="coffee-beans-small.jpg"
  srcset="
    coffee-beans-small.jpg   560w,
    coffee-beans-medium.jpg  800w,
    coffee-beans.jpg        1280w
  "
/>
```

`src` : Supplies a normal src for browsers that don’t support `srcset` (for example, IE and Opera Mini)

`srcset`: URL of each image and its width

Tip: As part of a fluid layout, you should always ensure images don’t overflow their container’s width. Do yourself a favor and always add this rule to your stylesheet to prevent that from happening:

```css{
img {
  max-width: 100%;
}
```

# Modular CSS

## base style

```css
:root {
  box-sizing: border-box;
}
*,
*::before,
*::after {
  box-sizing: inherit;
}
body {
  font-family: Helvetica, Arial, sans-serif;
}
```

## Module

```css
.message {
  /* Base */
  padding: 0.8em 1.2em;
  border-radius: 0.2em;
  border: 1px solid #265559;
  color: #265559;
  background-color: #e0f0f2;
}
.message--success {
  /* Modifiers or variants */
  color: #2f5926;
  border-color: #2f5926;
  background-color: #cfe8c9;
}
.message--warning {
  color: #594826;
  border-color: #594826;
  background-color: #e8dec9;
}
.message--error {
  color: #59262f;
  border-color: #59262f;
  background-color: #e8c9cf;
}
```

```html
<div class="message message--error">Invalid password</div>
```

### Modules with multiple elements

```css
.media {
  padding: 1.5em;
  background-color: #eee;
  border-radius: 0.5em;
}
.media::after {
  content: '';
  display: block;
  clear: both;
}
.media__image {
  float: left;
  margin-right: 1.5em;
}
.media__body {
  overflow: auto;
  margin-top: 0;
}
.media__body > h4 {
  margin-top: 0;
}
```

```html
<div class="media">
  <img class="media__image" src="runner.png" />
  <div class="media__body">
    <h4>Strength</h4>
    <p>
      Strength training is an important part of injury prevention. Focus on your
      core&mdash; especially your abs and glutes.
    </p>
  </div>
</div>
```

This is a versatile module. It works inside containers of multiple sizes, growing naturally to fill their width. You can put multiple paragraphs inside the body. You can use it with larger or smaller images (though you might want to consider adding a max-width to the image so it doesn’t crowd out the body).

## Modules at scale

Dropdown example

```html
<div class="dropdown">
  <button class="dropdown__toggle">Main Menu</button>
  <div class="dropdown__drawer">
    <ul class="menu">
      <li><a href="/">Home</a></li>
      <li><a href="/coffees">Coffees</a></li>
      <li><a href="/brewers">Brewers</a></li>
      <li><a href="/specials">Specials</a></li>
      <li><a href="/about">About us</a></li>
    </ul>
  </div>
</div>
<script type="text/javascript">
  (function () {
    var toggle = document.querySelector('.dropdown__toggle');
    toggle.addEventListener('click', function (event) {
      event.preventDefault();
      var dropdown = event.target.parentNode;
      dropdown.classList.toggle('is-open');
    });
  })();
</script>
```

Use double-underscore notation here to indicate that toggle and drawer are sub-elements of the Dropdown module.

```css
.dropdown {
  display: inline-block;
  position: relative;
}
.dropdown__toggle {
  padding: 0.5em 2em 0.5em 1.5em;
  border: 1px solid #ccc;
  font-size: 1rem;
  background-color: #eee;
}
.dropdown__toggle::after {
  content: '';
  position: absolute;
  right: 1em;
  top: 1em;
  border: 0.3em solid;
  border-color: black transparent transparent;
}
.dropdown__drawer {
  display: none;
  position: absolute;
  left: 0;
  top: 2.1em;
  min-width: 100%;
  background-color: #eee;
}
.dropdown.is-open .dropdown__toggle::after {
  /* Use state class*/
  top: 0.7em;
  border-color: transparent transparent black;
}
.dropdown.is-open .dropdown__drawer {
  display: block;
}
```

This module is the first one you’ve built that uses positioning. Notice that it establishes its own containing block (`position: relative` on the main element). The absolutely positioned elements (the drawer and the `::after` pseudo-element) are based on positions that are defined within the same module

## BEM methodology

https://en.bem.info/methodology/

# background. shadow, blend mode

## gradient

```css
.fade {
  height: 200px;
  width: 400px;
  background-image: linear-gradient(to right, white, blue);
}
```

OR

```css
.fade {
  height: 200px;
  width: 400px;
  background-image: linear-gradient(
    90deg,
    red 40%,
    white 40%,
    white 60%,
    blue 60%
  );
}
```

other gradient function: `radial-gradiant`, `repeating-linear-gradient`, `repeating-radial-gradient`

## Shadow

```css
.button {
  padding: 1em;
  border: 0;
  font-size: 0.8rem;
  color: white;
  border-radius: 0.5em;
  background-image: linear-gradient(to bottom, #57b, #148);
  box-shadow: 0.1em 0.1em 0.5em #124;
}
.button:active {
  box-shadow: inset 0 0 0.5em #124, inset 0 0.5em 1em rgba(0, 0, 0, 0.4);
}
```

`inset`: I’ve added the inset keyword. This makes the shadow appear inside the border of the element, rather than outside.

Modern button

```css
.button {
  padding: 0.8em;
  border: 0;
  font-size: 1rem;
  color: white;
  border-radius: 0.5em;
  background-color: #57b;
  box-shadow: 0 0.4em #148;
  text-shadow: 1px 1px #148;
}
.button:active {
  background-color: #456ab5;
  transform: translateY(0.1em);
  box-shadow: 0 0.3em #148;
}
```

## Blend Mode

```css
.blend {
  min-height: 400px;
  background-image: url(images/bear.jpg), url(images/bear.jpg);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: -30vw, 30vw;
  background-blend-mode: multiply;
}
```

# Typography

```css
body {
  font-family: Roboto, sans-serif;
}
```

## @font-face

```css
/* latin */
@font-face {
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 300;
  src: local('Roboto Light'), local('Roboto-Light'),
    url(https://fonts.gstatic.com/s/roboto/v15 Hgo13ktfSpn0qi1SFdUfZBw1xU1rKptJj_0jans920.woff2)
      format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02C6, U+02DA, U+02DC,
    U+2000-206F, U+2074, U+20AC, U+2212, U+2215;
}
```

The @font-face ruleset defines the fonts for the browser to use in your page’s CSS. The first ruleset here effectively says, “If the page needs to render Latin characters with a Roboto font-family using a normal font style (not italic) and a weight of 300, use this font file.”

# Transitions

```css
button {
  background-color: hsl(180, 50%, 50%);
  border: 0;
  color: white;
  font-size: 1rem;
  padding: 0.3em 1em;
  transition: border-radius 0.3s linear, background-color 0.6s ease;
}
button:hover {
  background-color: hsl(0, 50%, 50%);
  border-radius: 1em;
}
```

## Selecting a Transition function

Whether you use keyword timing functions or custom Bézier curves, it’s helpful to know when to use which. Each site or application should have a decelerating curve (ease-out), an accelerating curve (ease-in), as well as the linear keyword. It’s best to re-use the same few curves to provide a more consistent user experience.

You can use each of the three functions in the following scenarios:

- Linear—Color changes and fade in/out effects.
- Decelerating—User-initiated changes. When the user clicks a button or hovers over an element, use ease-out or something similar. This way, the user will see a fast, instant response to their input, easing out as the element comes to a stop.
- Accelerating—System-initiated changes. When content finishes loading or a timeout event triggers, use ease-in or something similar. This way, the element will ease in at first to draw the user’s attention before the element speeds up and completes its motion.

These are soft rules. They provide a good starting place, but don’t be afraid to break them if something doesn’t “feel” right. Occasionally, you’ll also want a fourth curve for larger or more playful motions: use either an ease-in-out (accelerate then decelerate) or a bounce effect.

## Fading in/out

```css
.dropdown__drawer {
  position: absolute;
  background-color: white;
  width: 10em;
  opacity: 0;
  transition: opacity 0.2s linear;
}
.dropdown.is-open .dropdown__drawer {
  opacity: 1;
}
```

Applying `visibility: hidden` to an element removes it from the visible page, but it doesn’t remove it from the document flow; meaning it’ll still take up space.

# transform

`transform: rotate(90deg);`

- `rotate`: Spins the element a certain number of degrees around an axis
- `translate`: Moves the element left, right, up, or down (similar to relative positioning)
- `scale`: Shrinks or expands the element
- `skew`: Distorts the shape of the element, sliding its top edge in one direction and its bottom edge in the opposite direction

Can apply multiple transform

```css
.card {
  padding: 0.5em;
  margin: 0 auto;
  background-color: white;
  max-width: 300px;
  transform: rotate(15deg) translate(20px, 0);
}
```

## transform + transition

```css
.nav-links__icon {
  transition: transform 0.2s ease-out;
}
.nav-links a:hover > .nav-links__icon,
.nav-links a:focus > .nav-links__icon {
  transform: scale(1.3);
}
```

## create an flyin effect

```css
.nav-links:hover .nav-links__label,
.nav-links a:focus > .nav-links__label {
  opacity: 1;
  transform: translate(0);
}
.nav-links > li:nth-child(2) .nav-links__label {
  transition-delay: 0.1s;
}
.nav-links > li:nth-child(3) .nav-links__label {
  transition-delay: 0.2s;
}
.nav-links > li:nth-child(4) .nav-links__label {
  transition-delay: 0.3s;
}
.nav-links > li:nth-child(5) .nav-links__label {
  transition-delay: 0.4s;
}
```

# Animations

```css
.box {
  width: 100px;
  height: 100px;
  background-color: green;
  animation: over-and-back 1.5s linear 3;
}

@keyframes over-and-back {
  0% {
    background-color: hsl(0, 50%, 50%);
    transform: translate(0);
  }
  50% {
    transform: translate(50px);
  }
  100% {
    background-color: hsl(270, 50%, 90%);
    transform: translate(0);
  }
}
```

## Example: Spinner in a button

```css
button.is-loading {
  position: relative;
  color: transparent;
}
button.is-loading::after {
  position: absolute;
  content: '';
  display: block;
  width: 1.4em;
  height: 1.4em;
  top: 50%;
  left: 50%;
  margin-left: -0.7em;
  margin-top: -0.7em;
  border-top: 2px solid white;
  border-radius: 50%;
  animation: spin 0.5s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
```

# Selector Reference

## Basic selector

- `tagname`: Type selector or tag selector. This selector matches the tag name of the elements to target. Has a specificity of 0,0,1. Examples: p; h1; strong.
- `.class`: Class selector. Targets elements that have the specified class name as part of their class attribute. Has a specificity of 0,1,0. Examples: .media; .nav-menu.
- `#id`: ID selector. Targets the element with the specified ID attribute. Has a specificity of 1,0,0. Example: #sidebar.
- `*`: Universal selector. Targets all elements. Has a specificity of 0,0,0

## Combinator

- Child combinator `>`:Targets elements that are a direct descendant of another element. Example: .parent > .child.
- Adjacent sibling combinator `+`: Targets elements that immediately follow another. Example: p + h2.
- General sibling combinator `~`: Targets all sibling elements following a specified element. Note this doesn’t target siblings that appear before the indicated element. Example: li.active ~ li.

## Pseudo-class selectors

- `:first-child`:Targets elements that are the first element within their parent element.
- `:last-child`: Targets elements that are the last element within their parent element.
- `:only-child`: Targets elements that are the only element within their parent element (no siblings).
- `:nth-child(an+b)`: Targets elements based upon their position among their siblings. The formula an+b, where a and b are integers, indicates which elements to target. To know exactly how a formula works, imagine solving it for all integer values of n, beginning with zero. The results of this equation indicate which children are targeted.
- `:nth-last-child(an+b)`: Similar to :nth-child(), but instead of counting forward from the first element, this selector counts backward from the last element. The formula in the parentheses follows the same pattern as in :nthchild().
- `:first-of-type`: Similar in nature to :first-child, except instead of considering the position among all children, it considers an element’s numerical position only among other children with the same tag name.
- `:last-of-type`: Targets the last child element of each type.
- `:only-of-type`: Targets elements that are the only child of their type.
- `:nth-of-type(an+b)` Targets elements of their type based on their numerical order and the specified formula; similar to :nth-child.
- `:nth-last-of-type(an+b)`: Targets elements of their type based on a specified formula, counting from the last of those elements backward; similar to :nth-last-child.
- `:not(<selector>)`: Targets elements that don’t match the selector within the parentheses. The selector inside the parentheses must be simple: it can only refer to the element itself; you can’t use this selector to exclude ancestors. It also mustn’t contain another negation selector.
- `:empty`: Targets elements that have no children. Beware that this doesn’t target an element that contains whitespace as the whitespace is represented in the DOM as a text node child. At the time of writing, the W3C is considering a :blank pseudo-class that behaves similarly but also selects elements that contain only whitespace; :blank is not yet supported in any browser.
- `:focus`:Targets elements that have received focus, whether from a mouse click, screen tap, or Tab key navigation.
- `:hover`: Targets elements while the mouse cursor hovers over them.
- `:root`: Targets the root element of the document. In HTML, this is the <html> element. But CSS can be applied to other XML or XML-like documents, such as SVG; in which case, this selector works more generically.

Several other pseudo-classes relate to form fields. Some of these were introduced or refined with the selectors level 4 specification, so they don’t work in IE10 and some other browsers. Check caniuse.com for support.

- `:disabled`: Targets disabled elements, including inputs, selects, and buttons.
- `:enabled`: Targets enabled elements, meaning they can be activated or accept focus.
- `:checked`: Targets selected checkboxes, radio buttons, or select box options.
- `:invalid`: Targets elements with invalid input values, as defined by the input type. For example, an <input type="email">, whose value is not a valid email address. (Level 4).
- `:valid`: Targets elements with valid values (Level 4).
- `:required`: Targets elements with a required attribute set (Level 4).
- `:optional`: Targets elements that do not have a required attribute set (Level 4).

This list of pseudo-classes isn’t exhaustive. See the MDN documentation at https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes for a complete list.

## Pseudo-element selectors

Pseudo-elements are similar to pseudo-classes, but instead of selecting elements with a special state, they target a certain part of the document that doesn’t directly correspond to a particular element in the HTML. They may target only portions of an element or even inject content into the page where the markup defines none.

These selectors begin with a double-colon (::), though most browsers also support a single-colon syntax for backward-compatibility reasons.

- `::before`: Creates a pseudo-element that becomes the first child of the matched element. This element is inline by default. Can be used to insert text, images, or other shapes. The content property must be specified to make this element appear. Example: .menu::before.
- `::after`: Creates a pseudo-element that becomes the last child of the matched element. This element is inline by default. Can be used to insert text, images, or other shapes. The content property must be specified to make this element appear. Example .menu::after.
- `::first-letter`: Lets you specify styles for only the first text character inside the matched element. Example: h2::first-letter.
- `::first-line`: Lets you specify styles for the first line of text inside the matched element.
- `::selection`: Lets you specify styles for any text the user has highlighted with their cursor. This is often used to change the background-color of selected text. Only a handful of properties can be used; those include color, background color, cursor, and text decoration.

## Attribute selectors

Attribute selectors can be used to target elements based on their HTML attributes.

- `[attr]`:Targets elements that have the specified attribute attr, regardless of its value. Example: input[disabled].
- `[attr="value"]`: Targets elements that have the specified attribute attr, and its value matches the specified string value. Example: input[type="radio"].
- `[attr^="value"]`: “Starts with” attribute selector. Targets by attribute and value, where the value begins with the specified string value. Example: `a[href^="https"]`.
- `[attr$="value"]`: “Ends with” attribute selector. Targets by attribute and value, where the value ends with the specified string value. Example: `a[href$=".pdf"]`.
- `[attr*="value"]`: “Contains” attribute selector. Targets by attribute and value, where the attribute value contains the specified string value. Example: [class*="sprite-"].
- `[attr~="value"]`: “Space-separated list” attribute selector. Targets by attribute and value, where the attribute value is a space-separated list of values, one of which matches the specified string value. Example: a[rel="author"].
- `[attr|="value"]`: Targets by attribute and value, where the value either matches the specified string value, or begins with it and is immediately followed by a hyphen (–). Useful for the language attribute, which may or may not specify a language subcode (for example, Mexican Spanish, es-MX, or Spanish in general, es). Example: [lang|="es"].
