# DOM

Document Object Model

## `document` object

interface between html and javascript

## Terminology

```html
<a href="index.html">Home</a>
```

this is a `HTML Element` 👆

- `a` is the `tag`
- `href` is the `attribute`
- index.html is the attribute value
- Home is the text.

### getElementById

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Learning the DOM</title>
  </head>

  <body>
    <h1>Document Object Model</h1>
    <a id="nav" href="index.html">Home</a>
  </body>
</html>
```

`document.getElementById('nav');` returns a HTML Element

```js
let navLink = document.getElementById('nav');
navLink.href = 'https://www.wikipedia.org';
navLink.textContent = 'Navigate to Wikipedia';
console.log(navLink); // <a id="nav" href="https://www.wikipedia.org/">Navigate to Wikipedia</a>
```

## Modify DOM with Events

An event in JavaScript is an action the user has taken. When the user hovers their mouse over an element, or clicks on an element, or presses a specific key on the keyboard, these are all types of events.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Learning the DOM</title>
  </head>

  <body>
    <h1>Document Object Model</h1>
    <button id="changeBackground">Change Background Color</button>

    <script src="scripts.js"></script>
  </body>
</html>
```

```js
const button = document.getElementById('changeBackground');

button.addListener('click', () => {
  document.body.style.backgroundColor = 'fuchsia';
});
```

## How to access Elements in the DOM

| Gets             | Selector Syntax | Method                |
| ---------------- | --------------- | --------------------- |
| ID               | #demo           | getElementById        |
| Class            | .demo           | getElementByClassName |
| Tag              | demo            | getElementByTagName   |
| Selector(single) |                 | querySelector         |
| Selector(all)    |                 | querySelectorAll      |

In the case of a selector with multiple elements, such as a class or a tag, querySelector() will return the first element that matches the query. We can use the querySelectorAll() method to collect all the elements that match a specific query.

Note: The methods `getElementsByClassName()` and `getElementsByTagName()` will return HTML collections which do not have access to the forEach() method that querySelectorAll() has. In these cases, you will need to use a standard for loop to iterate through the collection.

```js
const demoQuery = document.querySelector('#demo-query'); // access selector for an id attribute <div id="demo-query">Access me by query</div>
```

# How to traverse the DOM

The `document` object is the root of every node in the DOM. This object is actually a property of the `window` object, which is the global, top-level object representing a tab in the browser. The window object has access to such information as the toolbar, height and width of the window, prompts, and alerts. The document consists of what is inside of the inner window.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Learning About Nodes</title>

    <style>
      * {
        border: 2px solid #dedede;
        padding: 15px;
        margin: 15px;
      }
      html {
        margin: 0;
        padding: 0;
      }
      body {
        max-width: 600px;
        font-family: sans-serif;
        color: #333;
      }
    </style>
  </head>

  <body>
    <h1>Shark World</h1>
    <p>
      The world's leading source on <strong>shark</strong> related information.
    </p>
    <h2>Types of Sharks</h2>
    <ul>
      <li>Hammerhead</li>
      <li>Tiger</li>
      <li>Great White</li>
    </ul>
  </body>

  <script>
    const h1 = document.getElementsByTagName('h1')[0];
    const p = document.getElementsByTagName('p')[0];
    const ul = document.getElementsByTagName('ul')[0];
  </script>
</html>
```

### Parent

- `html` is the parent of `head`, `body`, and `script`.
- `body` is the parent of h1, h2, p and ul, but not li, since li is two levels down from body.

```js
p.parentNode; // <body>...</body>
p.parentNode.parentNode; // <html>...</html>
```

### Children

| Property          | Gets                             |
| ----------------- | -------------------------------- |
| childNodes        | Child Nodes (Node/Text/Comment)  |
| firstChild        | First Child Node                 |
| lastChild         | Last Child Node                  |
| children          | Element Child Nodes (only Nodes) |
| firstElementChild | First Child Element Node         |
| lastElementChild  | Last Child Element Node          |

```js
for (let element of ul.children) {
  element.style.background = 'yellow';
}
```

### Siblings

| Property               | Gets                          |
| ---------------------- | ----------------------------- |
| previousSibling        | Previous Sibling Node         |
| nextSibling            | Next Sibling Node             |
| previousElementSibling | Previous Sibling Element Node |
| nextElementSibling     | Next Sibling Element Node     |

## How to make changes to the DOM

| Property/method  | Description                                                        |
| ---------------- | ------------------------------------------------------------------ |
| createElement()  | Create a new element node                                          |
| createTextNode() | Create a new text node                                             |
| node.textContent | Get or set the text content of an element node                     |
| node.innerHTML   | Get or set the HTML content of an element - careful of XSS attacks |

```js
const paragraph = document.createElement('p');
paragraph.textContent = "I'm a brand new paragraph."; // A combination of createElement() and textContent creates a complete element node.
```

### Inserting Node

| Property/method                         | Description                                                           |
| --------------------------------------- | --------------------------------------------------------------------- |
| node.appendChild(node)                  | Add a node as the last child of a parent element                      |
| node.insertBefore(newNode, nextSibling) | Insert a node into the parent element before a specified sibling node |
| node.replaceChild(newNode, oldNode)     | Replace an existing node with a new node                              |

```js
// To-do list ul element
const todoList = document.querySelector('ul');

// Create new to-do
const newTodo = document.createElement('li');
newTodo.textContent = 'Do homework';
// Now that we have a complete element for our new to-do, we can add it to the end of the list with appendChild().

// Add new todo to the end of the list
todoList.appendChild(newTodo);
```

### Removing Node

| Property/method    | Description       |
| ------------------ | ----------------- |
| node.removeChild() | Remove child node |
| node.remove()      | Remove node       |

```js
const todoList = document.querySelector('ul');
todoList.removeChild(todoList.lastElementChild);
```

## Modify Attributes, Classes and Styles

### Modifying Attributes

| Property/method      | Description                                        | Example                                     |
| -------------------- | -------------------------------------------------- | ------------------------------------------- |
| hasAttribute()       | Returns true or false                              | element.hasAttribute('href');               |
| getAttribute()       | Returns the value of a specified attribute or null | element.getAttribute('href');               |
| setAttribute()       | Adds or updates value of a specified attribute     | element.setAttribute('href', 'index.html'); |
| removeAttribute()    | Removes an attribute from an element               | element.removeAttribute('href');            |
| element.[attrbiutes] | Adds or updates value of a specified attribute     | element.href = 'index.html';                |

The `hasAttribute()` and `getAttribute()` methods are usually used with conditional statements, and the `setAttribute()` and `removeAttribute()` methods are used to directly modify the DOM.

### Modifying Classes

| Property/method      | Description                                            | Example                                  |
| -------------------- | ------------------------------------------------------ | ---------------------------------------- |
| className            | Gets or sets class value                               | element.className;                       |
| classList.add()      | Adds one or more class values                          | element.classList.add('active');         |
| classList.toggle()   | Toggles a class on or off                              | element.classList.toggle('active');      |
| classList.contains() | Checks if class value exists                           | element.classList.contains('active');    |
| classList.replace()  | Replace an existing class value with a new class value | element.classList.replace('old', 'new'); |
| classList.remove()   | Remove a class value                                   | element.classList.remove('active');      |

### Modifying Styles

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div
      style="height: 100px;
                width: 100px;
                border: 2px solid black;"
    >
      Div
    </div>
  </body>
</html>
```

```js
// it is better to use the style attribute directly
// instead of div.setAttribute('style', 'text-align: center');
// because this will remove all existing inline styles from the element
div.style.height = '100px';
div.style.width = '100px';
div.style.border = '2px solid black';
```

## Events in JS

Events are actions that take place in the browser that can be initiated by either the user or the browser itself. Below are a few examples of common events that can happen on a website:

- The page finishes loading
- The user clicks a button
- The user hovers over a dropdown
- The user submits a form
- The user presses a key on their keyboard

### Event Handlers and Event Listeners

When a user clicks a button or presses a key, an event is fired. These are called a click event or a keypress event, respectively.

An `event handler` is a JavaScript function that runs when an event fires.

An `event listener` attaches a responsive interface to an element, which allows that particular element to wait and “listen” for the given event to fire.

There are three ways to assign events to elements:

- Inline event handlers (Don't use these):

```html
<button onclick="changeText()">Click me</button>
```

```js
const changeText = () => {...}
```

- Event handler properties

```html
<button>Click me</button>
```

```js
const changeText = () => {...}
const button = document.querySelector('button');
button.onclick = changeText; // pass a reference, don't execute
```

Note: here onclick execute the function
Note: Event handlers do not follow the camelCase convention that most JavaScript code adheres to. Notice that the code is onclick, not onClick.

- Event listeners (Best) - addEventListener()

```html
<button>Click me</button>
```

```js
const changeText = () => {...}

// Listen for click event
const button = document.querySelector('button');
button.addEventListener('click', changeText); // reference as click not onclick
```

can set multiple event listeners on same element

```js
button.removeEventListener('click', alertText); // remove listener
```

### Common Events

#### Mouse events

click, dblclick, mouseenter, mouseleave, mousemove

#### Form events

submit, focus, blur

JavaScript is often used to submit forms and send the values through to a backend language. The advantage of using JavaScript to send forms is that it does not require a page reload to submit the form, and JavaScript can be used to validate required input fields.

#### Keyboard events

keydown, keypress, keyup

```js
document.addEventListener('keydown', (event) => {
  console.log('key: ' + event.keyCode); // key id (ex: 65) // deprecated
  console.log('key: ' + event.key); // represent the character (ex: 'a')
  console.log('code: ' + event.code); // represents the physical key being pressed (ex: KeyA)
});
```

### Event Object

The `Event` object consists of properties and methods that all events can access. In addition to the generic Event object, each type of event has its own extensions, such as `KeyboardEvent` and `MouseEvent`.

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <title>Events</title>
  </head>
  <body>
    <section>
      <div id="one">One</div>
      <div id="two">Two</div>
      <div id="three">Three</div>
    </section>
  </body>
</html>
```

```js
const section = document.querySelector('section');

// Print the selected target
// we can place one event listener on the outer section element and get the most deeply nested element.
// Clicking on any one of those elements will return output of the relevant specific element to the Console using event.target
section.addEventListener('click', (event) => {
  console.log(event.target);
});
```

### Prevent default - e.preventDefault();

Sometimes, you'll come across a situation where you want to prevent an event from doing what it does by default. The most common example is that of a web form, for example, a custom registration form. When you fill in the details and select the submit button, the natural behavior is for the data to be submitted to a specified page on the server for processing, and the browser to be redirected to a "success message" page of some kind (or the same page, if another is not specified.)

### Event Bubbling & Capturing - e.stopPropagation();

When an event is fired on an element that has parent elements (in this case, the <video> has the <div> as a parent), modern browsers run two different phases — the capturing phase and the bubbling phase.

In the capturing phase:

- The browser checks to see if the element's outer-most ancestor (<html>) has an onclick event handler registered on it for the capturing phase, and runs it if so.
- Then it moves on to the next element inside <html> and does the same thing, then the next one, and so on until it reaches the element that was actually selected.

In the bubbling phase, the exact opposite occurs:

-The browser checks to see if the element selected has an onclick event handler registered on it for the bubbling phase, and runs it if so.

- Then it moves on to the next immediate ancestor element and does the same thing, then the next one, and so on until it reaches the <html> element.

In modern browsers, by default, all event handlers are registered for the bubbling phase.

This is a very annoying behavior, but there is a way to fix it! The standard Event object has a function available on it called stopPropagation() which, when invoked on a handler's event object, makes it so that first handler is run but the event doesn't bubble any further up the chain, so no more handlers will be run.

### Event Delegation

One of the hot methodologies in the JavaScript world is event delegation, and for good reason. Event delegation allows you to avoid adding event listeners to specific nodes; instead, the event listener is added to one parent.

```html
<ul id="parent-list">
  <li id="post-1">Item 1</li>
  <li id="post-2">Item 2</li>
  <li id="post-3">Item 3</li>
  <li id="post-4">Item 4</li>
  <li id="post-5">Item 5</li>
  <li id="post-6">Item 6</li>
</ul>
```

Let's also say that something needs to happen when each child element is clicked. You could add a separate event listener to each individual LI element, but what if LI elements are frequently added and removed from the list? Adding and removing event listeners would be a nightmare, especially if addition and removal code is in different places within your app. The better solution is to add an event listener to the parent UL element. But if you add the event listener to the parent, how will you know which element was clicked?

Simple: when the event bubbles up to the UL element, you check the event object's target property to gain a reference to the actual clicked node. Here's a very basic JavaScript snippet which illustrates event delegation:

```js
// Get the element, add a click listener...
document.getElementById('parent-list').addEventListener('click', function (e) {
  // e.target is the clicked element!
  // If it was a list item
  if (e.target && e.target.nodeName == 'LI') {
    // List item found!  Output the ID!
    console.log(
      'List item ',
      e.target.id.replace('post-', ''),
      ' was clicked!'
    );
  }
});
```

## Document Fragment

The `DocumentFragment` interface represents a minimal document object that has no parent.

The key difference is due to the fact that the document fragment isn't part of the active document tree structure. Changes made to the fragment don't affect the document (even on reflow) or incur any performance impact when changes are made.

A common use for DocumentFragment is to create one, assemble a DOM subtree within it, then append or insert the fragment into the DOM using Node interface methods such as appendChild() or insertBefore(). Doing this moves the fragment's nodes into the DOM, leaving behind an empty DocumentFragment. Because all of the nodes are inserted into the document at once, only one reflow and render is triggered instead of potentially one for each node inserted if they were inserted separately.

```html
<ul id="list"></ul>
```

```js
var list = document.querySelector('#list');
var fruits = ['Apple', 'Orange', 'Banana', 'Melon'];

var fragment = new DocumentFragment();

fruits.forEach(function (fruit) {
  var li = document.createElement('li');
  li.innerHTML = fruit;
  fragment.appendChild(li);
});

list.appendChild(fragment);
```

## Fetch API

### Legacy XHR - XMLHttpRequest

```js
// Just getting XHR is a mess!
if (window.XMLHttpRequest) {
  // Mozilla, Safari, ...
  request = new XMLHttpRequest();
} else if (window.ActiveXObject) {
  // IE
  try {
    request = new ActiveXObject('Msxml2.XMLHTTP');
  } catch (e) {
    try {
      request = new ActiveXObject('Microsoft.XMLHTTP');
    } catch (e) {}
  }
}

// Open, send.
request.open('GET', 'https://davidwalsh.name/ajax-endpoint', true);
request.send(null);
```

### Fetch API

```js
// url (required), options (optional)
fetch('https://davidwalsh.name/some/url', {
  method: 'get',
})
  .then(function (response) {})
  .catch(function (err) {
    // Error :(
  });
```

### Request & Request Headers

```js
// Create an empty Headers instance
var headers = new Headers();

// Add a few headers
headers.append('Content-Type', 'text/plain');
headers.append('X-My-Custom-Header', 'CustomValue');

// Check, get, and set header values
headers.has('Content-Type'); // true
headers.get('Content-Type'); // "text/plain"
headers.set('Content-Type', 'application/json');

// Delete a header
headers.delete('X-My-Custom-Header');

// Add initial values
var headers = new Headers({
  'Content-Type': 'text/plain',
  'X-My-Custom-Header': 'CustomValue',
});

var request = new Request('https://davidwalsh.name/some-url', {
  method: 'POST',
  mode: 'cors',
  redirect: 'follow',
  headers,
});

fetch(request).then(function () {
  /* handle response */
});
```

### JSON

```js
const response = await fetch('https://davidwalsh.name/demo/arsenal.json');
const jsonResponse = await response.json();
```

### POsting FormData

```js
fetch('https://davidwalsh.name/submit', {
  method: 'post',
  body: new FormData(document.getElementById('comment-form')),
});
```
