# Preparation

What do you like abut your job?
what sort of things are you looking for in the next job?

## Pro Tips

important to network
have a webpage
have a linkedin

# Phone screen - recruiter

What do you do currently
what are some project you are working on
What are the challenge you have to solve

# Prescreen questions

- Difference const, let, var?

  > `const` is immutable in a way that we cannot point to another value (change the pointer)
  > `let` we can change the pointer but not hoisted so will return `reference error` if access before declared
  > `var` hoisted to the top so will return `undefined` if access before adding a value to it

- Explain prototypal inheritance

  > When you create an Object from another Object the new Object will come bake with all the propreties and function from the other Object.

- What `this` mean in JS?

  > It's the current context/scope you are in and if there is no context it will be by default the global scope

- What is the data structure of the DOM

  > Tree

- What is a Stack and Queue? How would you create them in JS

  > Data structure, FIFO, LIFO, you create them with an array (push, pop, shift)

- How can you tell an image is loaded on a page

  > Use `onload` callback on Image object

- What is `call()` and `apply()`

  > Ways on changing the scope of a caller function. call use series of arg and apply use an array of arg

- What is event delegation and what are the performance tradeoffs

  > If we have event handlers in html, you can apply to every single element you want to have. Or if you want to have 1 event listeners at the top every child element that triggers event will be bubling up to the parent with the listeners.
  > It's important to know because event listeners are expensive on a page so better to have one event handler vs 60 for performance reason.

- What is a worker? When would you use one?
  > Use in a browser to offload expensive computation in order to not block UI which is single threaded.

# Code test

- make your code as readble as possible (comment, don't complicate)
- Don't import too many libraries
- Add unit tests
- Ask questions!

# First interview

```html
<html>
  <body>
    <div>
      <div class="foods">Best foods</div>
    </div>
  </body>
</html>
```

```js
/*
  Hi there! Thanks for taking on this code test. The requirements are listed below:
  
  1. Create a "Foods" class or constructor that will take two arguments: a root element and a data object (foodData).
  2. Render all of the items in the data object into the DOM with the root element as the parent
  3. If the user clicks a food item, it should be removed from the list
  
  Rules:
  - Only vanilla JS
  - Feel free to use Google, Bing, DuckDuckGo to look things up
  - Time limit: 30 minutes
*/

const rootElement = document.querySelector('.foods');

const foodData = [
  {
    id: 1,
    image: '🌮',
    name: 'taco',
  },
  {
    id: 2,
    image: '🍔',
    name: 'burger',
  },
  {
    id: 3,
    image: '🍆',
    name: 'eggplant',
  },
  {
    id: 4,
    image: '🍎',
    name: 'apple',
  },
  {
    id: 5,
    image: '🥞',
    name: 'pancakes',
  },
];

// CODE HERE
class Foods {
  constructor(root, foodData) {
    this.foodData = foodData;
    this.root = root;
  }

  // Can improve by using Fragment instead of appendChild each time and Event delegation on the top root element
  render() {
    this.foodData.forEach((food) => {
      const newDiv = document.createElement('div');
      newDiv.id = food.id;
      newDiv.addEventListener('click', () => {
        this.foodData = this.foodData.filter((f) => f.id !== food.id);
        const currentChild = document.getElementById(food.id);
        this.root.removeChild(currentChild);
      });
      // OR
      newDiv.addEventListener('click', (event) => {
        event.srcElement.parentElement.removeChild(event.srcElement);
      });

      const newContent = document.createTextNode(`${food.image} ${food.name}`);
      newDiv.appendChild(newContent);
      this.root.appendChild(newDiv);
    });
  }
}

// OTHER SOLUTION
class Foods {
  constructor(el, foodData) {
    this._root = el;
    this._data = foodData;
  }

  renderList() {
    this._root.addEventListener('click', (event) => {
      const { target } = event;
      target.remove();
    });

    const fragment = document.createDocumentFragment();

    this._data.forEach((i) => {
      fragment.appendChild(this.createFoodItem(i));
    });

    this._root.appendChild(fragment);
  }

  createFoodItem(item) {
    const itemEl = document.createElement('div');
    itemEl.innerText = item.image;
    itemEl.classList.add(item.name);

    return itemEl;
  }
}

const foods = new Foods(rootElement, foodData);
foods.render();
```

# On-site

## String

Primitive type, Immutable
.split()
.toLowerCase()
.substring()
.startsWith()

## Array

Object.entries()
Array.from()
[...item]

.isArray()
.filter()
.reduce()
.concat()
.join('')
.pop()
.push()
.shift()
.map()

## Scope

How to change scope:
.call()
.apply()
.bind()

```js
// implement bind prototype
function foo{
    console.log(this.bar)
}

const baz = foo.bind({bar: 'huhu'});

baz() // huhu

Function.prototype.bind(context) {
    const fn = this; // because it's a prototype and will be called like: foo.bind({bar: 'huhu'})
    return function(){
        return fn.call(context)
    }
}

```

## Timing

setInterval()
setTimeout()

```js
// DEBOUNCE
// In the debouncing technique, no matter how many times the user fires the event, the attached function will be executed only after the specified time once the user stops firing the event.
function debounce(fn, time) {
  let timeoutId;
  return function () {
    if (timeoutId) {
      clearTimeout(timeoutId);
    }
    timeoutId = setTimeout(() => {
      fn.apply(this, arguments);
      timeoutId = null;
    }, time);
  };
}

// THROTTLE
// Throttling is a technique in which, no matter how many times the user fires the event, the attached function will be executed only once in a given time interval.
function throttle(fn, time) {
  let timeoutId;
  return function () {
    if (timeoutId) {
      return; // DIFF HERE
    }
    timeoutId = setTimeout(() => {
      fn.apply(this, arguments);
      timeoutId = null;
    }, time);
  };
}
```

## Tree

### DOM Tree

```js
// We have two identical DOM trees, A and B. For DOM tree A, we have
// the location of an element. Create a function to find that same element
// in tree B.
function backwardsPath(element, root) {
  const path = [];
  let current = element;

  while (current.parentNode) {
    const index = [...current.parentNode.children].indexOf(current);
    path.push(index);
    current = current.parentNode;
  }

  current = root;
  while (path.length) {
    current = current.children[path.pop()];
  }

  return current;
}
```

## Rendering

how to move a element smootly : use `requestAnimationFrame()`. Available in window object to move element every 16ms and doesn't trigger a rerender so it is perfomant.

```js
// Create a function to move an element. The function arguments are,
// distance, duration, and the element to move.

/*
    function moveElement(duration, distance, element) {}
*/
function moveElement(duration, distance, element) {
  const start = performance.now();

  function move(currentTime) {
    const elapsedTime = currentTime - start;
    const progress = elapsedTime / duration;

    const amountToMove = progress * distance;
    element.style.transform = `translateX(${amountToMove}px)`;

    if (progress < 1) {
      requestAnimationFrame(move);
    }
  }

  move(performance.now());
}
```

## Promises

```js
// Create a sleep function that takes one parameter (time) and
// will wait "time" ms

/*
    async function run() {
        await sleep(500);
        console.log('hello');
        await sleep(500);
        console.log('world');
    }
*/
const sleep = (time) =>
  new Promise((resolve) => {
    setTimeout(() => {
      resolve();
    }, time);
  });
```

```js
// Create a function to turn any function into a "promisfied" function.
// Any function to be promisified will always have a callback as the last argument.
// The callback will always have this signature:
//   function(result){}

/*
    const exampleFn = function (x, y, callback) {};
    const promisedFn = promisify(exampleFn);
    promisedFn().then(...).then(...)
*/
function promisify(fn) {
  return function (...args) {
    return new Promise(function (resolve, reject) {
      function cb(result) {
        resolve(result);
      }

      fn.apply(this, args.concat(cb));
    });
  };
}
```
