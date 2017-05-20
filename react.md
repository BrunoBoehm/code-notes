# ReactJS

React is a JavaScript *library* (not a framework - as opposed to Angular) that's used for building user interfaces. It's an open source project, that started at Facebook, and is maintained by developers at Facebook and Instagram, and also a large community of contributors. One of the benefits and goals of the React project is to make developing a large scale, single page application easier.
React is also included in the larger movement in the world of JavaScript, the functional programming movement. You may have heard about functional JavaScript, a programming paradigm that emphasizes function composition over object orientation. React also aims to follow the principles of immutable data. So we create copies of objects and replace them, instead of mutating the originals. React is commonly used in enterprise applications, by companies including PayPal, AirBnB, Apple, Uber, and, of course, Facebook.

React encourages us to write small, laser-focused components that compose together in an elegant manner.

React has a virtual DOM that writes to the browser DOM only when it needs to. It only interacts with the virtual DOM, that JavaScript object in between the JavaScript logic and the actual DOM. When we call the `render` function, React will update the virtual DOM. That will push only the necessary changes all the way to the real DOM.

Virtual DOM is a technique employed by several front-end libraries and frameworks, most notably React. In a nutshell, virtual DOM builds a virtual representation of what our document should look like. When we're ready to render things to the screen, the virtual DOM will take a look at the existing DOM and change only what needs to be changed (in more technical terms, it's diffing and re-rendering the changes).

The performance gains here are not to be underestimated. Virtual DOM is created by a bunch of super smart people using extremely efficient algorithms. Most virtual DOMs also batch their updates to the DOM, ensuring that the real DOM gets modified as little as possible.

This means that we don't need to write imperative code to update every tiny bit of our application, *we just declare what the end result should look like, and the virtual DOM will do the rest*! The ability to simply declare what our component should look like and how it should behave is a huge win. That means we can think more about how our UI fits together, rather than how we're supposed to be updating it!

- [Why did we build React?](https://facebook.github.io/react/blog/2013/06/05/why-react.html)
- [The difference between Virtual DOM and DOM](http://reactkungfu.com/2015/10/the-difference-between-virtual-dom-and-dom/)
- [React (Virtual) DOM Terminology](https://facebook.github.io/react/docs/glossary.html)

## Declarative Programming
Let's say we have some kind of search component that allows us to filter a list of results. When the input updates, we filter the list data and then display the updated results in our component. 

In React, we just tell it what we want the component to look like (i.e. display these items that have been filtered), whereas in a non-declarative application, we'd have to slog through manually updating our DOM — removing the results that are not relevant anymore, and adding the new ones to the list.

[Declarative programming](http://stackoverflow.com/questions/33655534/difference-between-declarative-and-imperative-in-react-js) allows us to focus on what our application should look like — as opposed to being concerned with manually updating DOM, adding and removing classes, and so on. That stuff is all done for us in React: we just tell React what the end result should be. It'll do the heavy lifting for us.

## ES6
[ES6](https://nodejs.org/en/docs/es6/) is the next specification for JavaScript, and it's finally started to appear in a major way in browsers and on servers thanks to Node.js 5.

We'll use `"use strict";` - generally, it turns silent failures into thrown errors, and it helps prevent variables sneaking into the global scope. You can use it at the top of the current script — strict mode then applies to the entire script. Alternatively, you can apply strict mode to individual functions.

Strict mode does just as its name implies: it enforces stricter rules on the execution of your code. Note that some transpilers (like babel) can set strict mode for you.
Strict mode also enables some newer features that ES6 developers wanted to make sure users explicitly opted in to.

There are a lot of new features, like

* [`const` and `let`](#block-scoping)
* [Classes](#classes-use-strict)
* [Arrow Functions](#arrow-functions)
* [Promises](#promises)
* [Object Literal Extensions](#object-literal-extensions)
* [Spread Operator](#spread-operator)
* [Template Strings](#template-strings)
* [Destructuring](#destructuring)

### Block Scoping

#### `let` ("use strict")

The keyword [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) is a new way of declaring local variables. How does it differ from good ol' `var`? Variables declared with `let` have block-level scope:

```javascript
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Scoping_rules
function letTest() {
  let x = 31;
  if (true) {
    let x = 71;  // different variable
    console.log(x);  // 71
  }
  console.log(x);  // 31
}
```

Notice how `x` declared outside of the `if` block differs from the `x` declared inside the block. Block-level scope means that the variable is available only in the block (`if`, `for`, `while`, etc.) in which it is defined; it differs from JavaScript's normal function-level scope, which restricts the variable to the function in which it is defined (or `global`/`window` if it's a global variable).

#### `const`

The keyword [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) does not require strict mode. Like `let`, it declares a variable with block-level scope; additionally, it prevents its variable identifier from being reassigned.

That means that the following will throw an error:

```javascript
const myVariable = 1;

myVariable = 2; // syntax error
```

However, this does not mean that the variable's value is immutable — the value can still change.

```javascript
const myObject = {};

// this works
myObject.myProperty = 1;

// 1
console.log(myObject.myProperty)
```

### Classes ("use strict")

```javascript
class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }

  // whaaaaat -- getters!
  get area() {
    return this.calcArea();
  }

  calcArea() {
    return this.height * this.width;
  }
}

const rectangle = new Polygon(10, 5);

console.log(rectangle.area);
```

Let's extend it:

```javascript
class Square extends Polygon {
  constructor(sideLength) {
    super(sideLength, sideLength)
  }
}

const mySquare = new Square(5);

// Square { height: 5, width: 5 }
mySquare;

// [Function: Square]
mySquare.constructor;

// 25
mySquare.area;
```

### Arrow functions

[Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) provide not only a terser way to define a function but also _lexically bind the current `this` value_. This ain't your grandpa's JS.

```javascript
const greet = (greeting, person) => {
  return greeting + ', ' + person + '!';
};

// 'Hello, Marv'
greet('Hello', 'Marv');

var a = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryl­lium'
];

// compare this implementation...
var a2 = a.map(function(s){ return s.length });

// ... to this implementation with the fat arrow
var a3 = a.map(s => s.length);
```

Fat arrows also have implicit returns — the following are equivalent:

```javascript
var a3 = a.map(s => s.length);
var a4 = a.map(s => {
  return s.length;
});
```

If the function only accepts one argument, parentheses are optional:

```javascript
// this...
var a3 = a.map(s => s.length);

// ... is the same as this
var a3 = a.map((s) => s.length);
```

If there are zero or two or more arguments, though, you must use parens:

```javascript
var evens = [1, 2, 3, 4].reduce((memo, i) => {
  if (i % 2 === 0) {
    memo.push(i)
  }

  return memo;
}, []);
```

### Promises

[Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) offer a new way of handling asynchronicity.

```javascript
const promise = new Promise((resolve, reject) => {
  return someIntenseTask().then(result => {
    if (result.success) {
      return resolve(result)
    }

    return reject(result.error)
  })
})

promise.then(result => {
  return doSomething(result);
}).catch(error => handleError(error))
```

### Object literal extensions

ES6 gives us a number of handy [new ways to deal with objects](https://github.com/lukehoban/es6features#enhanced-object-literals). They're features that you either wish JavaScript had, or ones you didn't know you needed.

```javascript
const prop = function() {
  return "I'm a prop!";
}

const myObj = {
  // computed (dynamic) property names
  ['foo' + 'bar']: 'something',

  // methods
  shout() {
    return 'AH!'
  },

  // short for `prop: prop`
  prop
}

// 'something'
myObj.foobar

// "I'm a prop!"
myObj.prop()

// 'AH!'
myObj.shout()
```

### Spread operator

The [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator) — `...` — is unassuming but incredibly powerful.

We can use it for arrays:

```javascript
const a = [1, 2, 3]
const b = [0, ...a, 4, 5]

// [0, 1, 2, 3, 4, 5]
b
```

functions:

```javascript
function printArgs() {
  // recall that every function gets an `arguments`
  // object
  console.log(arguments);
}

// using `a` from above
// { '0': 1, '1': 2, '2': 3 }
printArgs(...a);
```

### Template Strings

[Template strings](https://nodejs.org/en/docs/es6/) in ES6 are most commonly used for string interpolation. Instead of writing:

```javascript
var foo = 'bar';
var sentence = 'I went to the ' + foo + ' after working in ES5 for too long.';
```

we can now write:

```javascript
var foo = 'bar';
var sentence = `I went to the ${foo} after working in ES5 for too long.`;
```

You can also use _tagged template literals_ to perform more advanced manipulation:

A _tag_ is simply a function whose first argument is an array of strings and whose subsequent arguments are the values of the substitution expressions (the things in `${}`).

Here's the example from [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Tagged_template_literals):

```javascript
var a = 5;
var b = 10;

function tag(strings, ...values) {
  console.log(strings[0]); // "Hello "
  console.log(strings[1]); // " world "
  console.log(values[0]);  // 15
  console.log(values[1]);  // 50

  return "Bazinga!";
}

tag`Hello ${ a + b } world ${ a * b }`;
// "Bazinga!"
```

### Destructuring

Destructuring makes it easier than ever to pull values out of objects and arrays and store them in variables. We destructure an array by putting our new variable names at the corresponding index and an object by giving our variable the same name as the key we are interested in.

```js
const [a, b] = [1, 2];
// a === 1 && b === 2

const { a, b } = { a: 1, b: 2 }
// a === 1 && b === 2
```

To see what other amazing things we can to with destructuring, check out the [docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).


## Making requests using XHR, $.ajax and fetch()
Getting remote data in JavaScript has classically required a fair amount of plumbing to make things happen.
It's not that it's *hard* to get data out of `XMLHttpRequest`, but it does take quite a bit of setup. 
Let's make a simple request of the Github repository commits API.

```js
let xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.github.com/repos/jquery/jquery/commits');
xhr.responseType = 'json';

xhr.onload = function() {
  console.log(xhr.response);
};

xhr.onerror = function() {
  console.log('Booo');
};

xhr.send();
```

Sure, it works, but that's a lot of setup to just say "give me some JSON from this URL".
Things get a little better with jQuery's `$.ajax`, except then we have to tightly couple ourselves to jQuery, and ultimately, it's just syntactic sugar over `XMLHttpRequest`.

### GET

The `fetch()` function is a new API for fetching resources. It's a global function, which means no creating new XHR objects, and it vastly streamlines simple resource requests. Let's try that call to the Github commits API again.

```js
fetch('https://api.github.com/repos/jquery/jquery/commits')
  .then(res => res.json())
  .then(json => console.log(json));
```

In JavaScript, a `Promise` object represents a value that may not be available yet, but will be resolved at some point in the future. Essentially, the promise is an object that represents the result of an operation, whenever it occurs. This allows us to write more flexible asynchronous code than simply passing callback functions to asynchronous functions.

All promises implement a `then` function that is called when the promise is *fulfilled*, or completed successfully. So this is very similar to the idea of a callback on success that we'd use with `XMLHttpRequest` or `$.ajax`.

The interesting thing about this `fetch` code is that it highlights a powerful feature of a `thenable` object — we can chain each `then` call, and the next one receives the result of the previous one as its argument.

```js
fetch('https://api.github.com/repos/jquery/jquery/commits')
  .then(res => res.json())
  .then(json => console.log(json));
```

the line `then(res => res.json())` is getting the response `res` from `fetch` and using the `json` method (more on this in a bit) to turn it into JSON. Then it's passing the JSON to the next line, `then(json => console.log(json))`, to be handled by that function.

The `fetch` API includes a *mixin*, or additional code, called [Body](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#Body), which has functions that specialize in transforming the `body` of a request or response.

In an `XMLHttpRequest`, you would potentially have to `JSON.parse` the `responseText` data to turn it into JSON. The `Body` mixin takes that a step further, giving us handy ways to parse many formats, including JSON, plain text, and even blobs for binary or image data.

So calling `res.json()` in our `fetch` is just a nice shorthand for saying "give me the `body` of the response parsed as JSON".

GitHub's API uses [OAuth2](https://developer.github.com/v3/oauth/) for authorization. In a production setting, you would register an application with GitHub and receive an application ID and secret. This allows GitHub to track and monitor API access, and ensure that its users are protected by only granting authorization through registered apps.

```js
const token = 'YOUR_TOKEN_HERE'
fetch('https://api.github.com/user/repos', {
  headers: {
    Authorization: `token ${token}`
  }
}).then(res => res.json()).then(json => console.log(json));
```

We just pass the desired headers as part of a second options argument to `fetch` and we are in business. Easy as that!


**Top-Tip:** Don't ever give out your access token or store it in a publicly accessible place or a shared GitHub repository. We're just using these for learning purposes. In a production setting, user's access tokens would be stored securely in a database and not exposed to other people.

As you can see, `fetch` provides us with such a clean, low-maintenance way to fetch and work with resources. You may be wondering why you'd ever use XHR again.

Keep in mind that, while it is increasing, [browser support](http://caniuse.com/#feat=fetch) for `fetch` is still limited primarily to current versions of Chrome, Firefox, and Opera. So if you're supporting older browsers, don't let XHR and jQuery Ajax go just yet. They're still powerful and useful tools for creating dynamic applications.

### POST

While `GET` operations are straightforward, when we're building out full applications, we often need to use other HTTP verbs, such as `POST`, to write data as well as read it. Luckily, it's very easy to `POST` with `fetch` as well.

Let's look at an example of posting a new comment to a commit with the GitHub API. Replace the commit with a commit from one of your repositories, and use your token if you want to try this out.

```js
const token = 'YOUR_TOKEN_HERE';
const postData = {
  body: 'Great stuff'
};

fetch('https://api.github.com/repos/:your_ghname/:your_repo/commits/:sha/comments', {
  method: 'POST',
  body: JSON.stringify(postData),
  headers: {
    Authorization: `token ${token}`
  }
}).then(res => console.log(res));
```

Here we created an object called `postData` that we will pass as a JSON string using `JSON.stringify` in the request `body`. We're also setting the method to `'POST'`, and finally using our `Authorization` header like we did before, since any write action is going to require authorization.

All of these additional settings go in that `options` argument, which is just an object that we can pass as the second argument to `fetch`.

Finally, we can examine the response in our `then` function just the same as we did with a `GET` request.

**Top-tip:** Make sure you read the API documentation carefully! They will often specify which fields are required and which are optional, as well as the format of the request body. GitHub expects JSON data in the body, but another API might want form data (which you can create with `new FormData()` or XML or something else. Always read the docs!

- [`fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [GitHub API](https://developer.github.com/v3/)
- [Handlebars](http://handlebarsjs.com)

## Hello World
From the react website let's take the dev source files
Here's an example on [codepen](http://codepen.io/gaearon/pen/ZpvBNJ?editors=0010)
```html
<!DOCTYPE html>
<html>
    <head>
        <script src="https://fb.me/react-15.2.1.js"></script>
        <script src="https://fb.me/react-dom-15.2.1.js"></script>
        <title>My First React File</title>
    </head>
    <body>
        <div id='react-container'></div>
        <script>
            ReactDOM.render(
                React.createElement('div', null, 'Hello World'),
                document.getElementById('react-container'))
        </script>

    </body>
</html>
```

## React.createElement()
We'll run out mini-React application using Python Simple Server. From the directory of the project, in your terminal, *simply* run:

```
$ python -m SimpleHTTPServer
```

To learn more about Python Simple Server, check out [this blog post](http://www.linuxjournal.com/content/tech-tip-really-simple-http-server-python). 

We're going to use the React (and ReactDOM) libraries, so we should include those in our code. Run `npm install` to install dependencies. Now let's add the libraries by loading the right scripts in our `index.html` file:

```html
<script src="node_modules/react/dist/react.js"></script>
<script src="node_modules/react-dom/dist/react-dom.js"></script>
```

These should go in the body tag, after any content, but _before_ the `index.js` script. That file will contain our own code, so it expects React to already be loaded by that point!

Let's create a really basic page title in React using `React.createElement()` in the `index.js` file:

```js
const title = React.createElement('h1', {}, 'My First React Code');
```

Let's briefly talk about the arguments of `React.createElement()`.

- The first one is type of element we're creating, in this case an `<h1>` tag. This could also be another React component. If we're creating an HTML element, we pass in the name as a string, just like we did above. If we're creating a React component, we pass in the variable that the component is assigned to.
- The second argument is an object containing properties ('props' in React terms) that get passed to the component. Since we're just getting started with React, we won't use these just yet — but be aware that the second options serves this purpose.
- The third argument is the children of that component. This can be a quoted string like shown above, in which case the content will be interpreted as text. However, we can also pass in a reference to another component, allowing us to nest elements and components within each other.

Now that we have our element, it's time to render it to the page. We do this using `ReactDOM.render()`.
```js
ReactDOM.render(
  title,
  document.getElementById('main')
);
```
This takes two arguments: 

- the first one being the thing we want to render (our `title` element)
- the second one is a target DOM node to render things into.

### Nesting with const
Now we're going to add an element as a child, so we pass it by reference instead of using a string:
```js
const title = React.createElement('h1', {}, 'My First React Code');
const container = React.createElement('div', {}, title);

ReactDOM.render(
  container,
  document.getElementById('main')
);
```

Our `title` seems a little lonely though... Let's add a sibling! We'll create a `paragraph` element and then pass it as another child to our `container` children.

```js
const title = React.createElement('h1', {}, 'My First React Code');
const paragraph = React.createElement('p', {}, 'Writing some more HTML. Cool stuff!');
const container = React.createElement('div', {}, [title, paragraph]);

ReactDOM.render(
  container,
  document.getElementById('main')
);
```

Note how if we want to add multiple children, we use an _array_!

### Nesting inline
We can nest children as much as we want. We also don't need to store our elements in variables before using them, we can declare them inline as well (though the downside of this is less readable code):

```js
const list =
  React.createElement('div', {},
    React.createElement('h1', {}, 'My favorite ice cream flavors'),
    React.createElement('ul', {},
      [
        React.createElement('li', {}, 'Chocolate'),
        React.createElement('li', {}, 'Vanilla'),
        React.createElement('li', {}, 'Banana')
      ]
    ));

ReactDOM.render(list, document.getElementById('main'));
```
### Adding attributes
As mentioned before, we pass properties to an element using the second argument (an object, which we've left empty for now). Suppose we wanted to add some classes to make our ice cream flavors stand out.

The prop is called `className` because `class` is a _reserved keyword_ in JavaScript. Using reserved keywords as keys in an object is something that you should never do, since this can result in unexpected behavior. Instead, React expects the `className` prop instead, if we want to add a class to our element.

```js
const list =
  React.createElement('div', {},
    React.createElement('h1', {}, 'My favorite ice cream flavors'),
    React.createElement('ul', {},
      [
        React.createElement('li', { className: 'brown' }, 'Chocolate'),
        React.createElement('li', { className: 'white' }, 'Vanilla'),
        React.createElement('li', { className: 'yellow' }, 'Banana')
      ]
    ));

ReactDOM.render(list, document.getElementById('main'));
```

We can also add any other HTML attributes here, like `disabled`, `id`, and so on. These props are also used to pass in custom data to our components!

- [Raw React](http://jamesknelson.com/learn-raw-react-no-jsx-flux-es6-webpack/)

## Introducing JSX
JSX, or JavaScript as XML. So instead of creating all these createElement function calls, we're going to use tags right here in our JavaScript.
[Babel](https://babeljs.io) will take the JSX and turn it into createElement calls, so the browser can read it easily. Babel is a transpiler that will transpile JavaScript code. It works for JSX, and it also works for ES6 and beyond. So, instead of having to wait around to use syntax that's brand new, we can use it right away with the help of Babel.

Babel turns JSX code like
```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>
```
into compiled javascript like
```js
React.createElement(
  "ul",
  null,
  React.createElement(
    "li",
    null,
    "Item 1"
  ),
  React.createElement(
    "li",
    null,
    "Item 2"
  ),
  React.createElement(
    "li",
    null,
    "Item 3"
  )
);
```

We're using version 5.8. Version six, which is the newest version of Babel, isn't going to work as an in-browser transpiler. Let's copy this scr URI and paste it into another script tag. The process of in-browser transpiling means that all of our code that isn't ready for the browser is going to be transpiled at runtime. This is a slow process, so this isn't recommended for production, we're going to use webpack to include Babel as a module.

Note we add a `type="text/babel"` to our script tag.
```html
<!DOCTYPE html>
<html>
    <head>
        <script src="https://fb.me/react-15.2.1.js"></script>
        <script src="https://fb.me/react-dom-15.2.1.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser.js"></script>
        <title>My First React File</title>
    </head>
    <body>
        <div id='react-container'></div>
        <script type="text/babel">
            ReactDOM.render(<ul>
                    <li>item 1</li>
                    <li>item 2</li>
                    <li>item 3</li>
               </ul>,
                document.getElementById('react-container'))
        </script>

    </body>
</html>
```

Looking at this code, there are some important things to note. First of all, JSX is not a string — it's not in between quotes. Think of it as another type in JavaScript. Secondly, the parentheses containing the JSX are entirely optional, but recommended by convention.

It's important to note that using JSX is entirely optional. If writing HTML in your JavaScript feels like committing a mortal sin, it's okay to use React.createElement(). You'll be less productive, but at least you'll feel good about it!

**JSX always has one, and only one element** (that optionally has children, grandchildren, and so on): returning two values at once in JavaScript is conceptually impossible.

Another thing to note is that since we're still writing JS code, we need to avoid using keywords in our code. You might have noticed it already: we're setting HTML classes using the `className` attribute (or prop, in React terms), instead of `class`. This is because class is a reserved keyword in JavaScript! The same thing is true for the for label, which is another keyword in JS. If you want to use the HTML `for` attribute, you'd use `htmlFor` instead.

Babel also allows us to use new JS features before they are standardised and implemented in the browser, allowing us to write the most modern code we can, without worrying about browser support. Babel transpiles everything back down to JavaScript that _all_ browsers can understand. Neat!

[Browserify](http://browserify.org/) lets us require modules using [Node's version of the CommonJS module system](https://github.com/substack/browserify-handbook#node-packaged-modules). This means that we can include modules in our file (both local files as well as `node_modules` installed with `npm`). When compiling a file with Browserify, it'll check every file for stuff that it needs to import, and also include that code. In more technical terms, it's traversing the dependency tree and inlining those dependencies in our script. What we'll end up with is one big JS file that includes _all_ of our code, including any dependencies (like `React`, or our own components) in that file too. That way, we also only need to have one script reference in our HTML: the bundled version!

### Browserify

One of the benefits of Browserify is that it wraps every module in an [IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE), ensuring that no variable is global (unless you force it by setting something on `window`). This allows for pretty powerful modularization — we only export what we want other modules to use, and the rest is 'private' by default.

Let's see if we can get this to work! First of all, we'll need to install the `browserify` module:

```
$ npm install --save-dev browserify
```

Once that's done, we'll compile our code using the `browserify` command. We can do so by running the following:

```
$ browserify index.js -o bundle.js
```

The first argument is the path to our main JS file (that is responsible for importing all other files). Next, we tell it where we want the output bundle to go. In this case, that's the same directory, in a file called `bundle.js`. When you run the command above, chances are you'll run into an error that says `browserify` is not found. That's totally fine! It's not globally installed, but we don't need it to be, because we'll build our JS using an `npm` script. Taking a look at the `package.json` file, we'll find an object called `scripts` that contains various scripts we can run in our project. We'll add another script now that builds our JS:

```json
"scripts": {
  ... (other scripts)
  "bundle": "browserify index.js -o bundle.js",
}
```

Now, we'll run the script using `npm run bundle`. Wait, now it does work? Why's that? Well, `npm` knows `browserify` is a module that has an executable (or a binary), and links it in `node_modules/.bin/`. When `npm run` runs a script, it first checks if any command there can be found in that `.bin` folder, before using the system-wide commands. Handy!

Our `index.js` file is still empty at this point though. Let's practice writing modular code by creating a new file in `components/foo.js` (you'll also need to create the `components/` directory). In that file, we'll add this content:

```js
module.exports = 'I am a component!';
```

We'll get to what the `module.exports` stuff is in just a second. We can import this component in our `index.js` by using `require()` and assigning the result to a variable:

```js
const component = require('./components/foo');
```

Note that files are always referred to using a relative path (even if they are in the same directory). This way Node knows whether to look for a local module or one found in `node_modules`, or in the global modules. Adding the `.js`  extension is not required.

#### Named exports

Named exports allow us to export several things at once. This is useful for utility modules or libraries. Exporting several things at once is done by exporting an object when setting the value of `module.exports`.

```js
// In a file called `fruits.js`
module.exports = {
  apple: 'red',
  banana: 'yellow',
};

// In a file in the same directory
const fruits = require('./fruits');
console.log(fruits.apple); // prints 'red'

// In another file, also in the same directory
const apple = require('./fruits').apple;
console.log(apple); // prints 'red'
```

When using named exports, we can choose to either import the entire thing and then reference the keys on the exported object, or we can import one specific key. The last example can also be rewritten using ES2015 destructuring syntax:

```js
const { apple } = require('./fruits');
console.log(apple); // prints 'red'
```

#### Default Exports
A default export means we're exporting just one thing. This is useful for exporting components in their own file, since there's only one thing there: the component itself. Exporting one thing only is done by exporting a reference to what we want to export when setting the value of `module.exports`. You can also inline the value of what you want to export.

```js
// In a file called `Tweet.js`
const React = require('react');

class Tweet extends React.Component {
  render() {
    return (
      <div className="tweet">
        <img src="http://twitter.com/some-avatar.png" className="tweet__avatar" />
        <div className="tweet__body">
            <p>We're writing this tweet in JSX. Holy moly!</p>  
        </div>
      </div>
    );
  }
}

module.exports = Tweet;

// In a file in the same directory
const Tweet = require('./Tweet');

ReactDOM.render(
  <Tweet />,
  document.getElementById('main')
);
```

You'll mostly be using this method. It's important to correctly export your components, otherwise the tests can't access the code you've written, causing them to fail!

### Transforming JSX with Babel and Browserify

Now that we know how we can compile our bundle using Browserify, it's time to transform our JSX using Babel. Without the right transformer, Browserify won't know how to handle the JSX in our code and will fail, thinking the code we wrote has syntax errors in it.

To use a transformer, we pass the transformer name to the `browserify` command. First, we'll have to install some extra stuff:

```
$ npm install --save-dev babel-core babel-preset-es2015 babel-preset-react babelify
```

Let's quickly go through the modules we just installed. `babel-core` is Babel itself. Just by itself, Babel does absolutely _nothing_ to your files. You need to explicitly tell it what plugins to use. Luckily, there are transform _presets_ that form a collection of these separate plugins. That's what the `babel-preset-es2015` and `babel-preset-react` modules are for: they respectively transform our ES2015 and JSX code into JS that is understandable by browsers _today_. Lastly, we install `babelify` which is the transformer for Browserify.

We still need to let Babel know what presets we'd like to use. Babel reads its configuration from a `.babelrc` file in the project's root. This is a JSON file with Babel's options:

```json
{
  "presets": ["es2015", "react"]
}
```

Now all that's left to do is let Browserify know which transformer to use. In `package.json`, we'll update our `bundle` script to use the transformer:

```json
"scripts": {
  ... (other scripts)
  "bundle": "browserify index.js -t babelify -o bundle.js",
}
```

That should do the trick. Running `npm run bundle` should now correctly transpile our ES2015 and JSX code into one file called `bundle.js`. Success!

**Important: whenever writing JSX, _always_ import React (`const React = require('react');`). This is because Babel uses `React.createElement()` to compile our JSX down to JS. If `React` isn't found in that file, it'll throw an error!**

In the past, we've always loaded our source code in the HTML:

```html
<script src="index.js"></script>
```

Instead, now we'll reference our compiled (or bundled) code:

```html
<script src="bundle.js"></script>
```

That's the only script we need to add to our HTML file to make everything work. This bundle includes our source code as well as anything else we've imported in our code, like libraries (e.g. `React`, `ReactDOM` and others).

It's very important to know how this stuff works on a high level, because most of the React code nowadays is being compiled in one way or another — be it using Browserify, Webpack or something else. However, we don't want to create unnecessary busywork for you. Every lab from now on already has the bundling stuff set up for you. You just need to run `npm run bundle` any time you want to compile your changes and view them in the browser.

- [Browserify](https://github.com/substack/node-browserify)
- [Babel](http://babeljs.io/)
- [Babelify](https://github.com/babel/babelify)
- [JSX](https://facebook.github.io/react/docs/jsx-in-depth.html)

Babel is used to transform our ES2015 (and even newer) code to ES5 — the previous version of JavaScript that all browsers know and understand. Most of the ES2015 features are already present in browsers, but it's best to transpile your code using Babel anyway.

Plugins are small, composable dependencies that transform parts of our code. These plugins get applied to the code when compiling it with Babel, each doing their own little job and changing our code. For example, the `transform-es2015-destructuring` allows us to use ES2015 destructuring in our code:

```js
// Source code
const { foo, bar } = myLib;

// Gets transformed by the plugin to:
var _myLib = myLib;
var foo = _myLib.foo;
var bar = _myLib.bar;
```

Having small, separate [plugins](https://babeljs.io/docs/plugins/) like this allows us to tweak our configuration to our heart's desire. However, installing every single plugin just to write ES2015 and React code seems like such a hassle... Luckily, there's a thing in Babel called plugin presets! These dependencies are basically a collection of plugins that are grouped together. For example, to transform the code we're writing in this course, we use `babel-preset-es2015` and `babel-preset-react`.

We install them using `npm`, and then we use a file called `.babelrc` in the root of our project to configure Babel:

```json
{
  "presets": ["es2015", "react"],
  "plugins": ["an-example-plugin", "another-example-plugin"]
}
```

## Components
Components are small user interface elements that display data as it changes over time. These components can display data, handle state, and be nested inside of one another. React can be used for an entire site or just for smaller features, like a form, or an image carousel.

While HTML elements are the basic building blocks of an application (for example, a `<div>`), a React application usually consists of several React _components_ combined together. Unlike the simple HTML elements, React components are smarter and bigger. They allow you to add event handlers, store internal state, communicate with parent components, and so on.

Components names are always capitalized, it's a best practice.
Render is always required when we create a component, because it tells react what we want to render to the DOM.

### Component with `.createClass` (old school)
While this method of creating React components is outdated (in a sense), it's important to know what its syntax looks like. Most React tutorials and guides still use this syntax, so if you see it in the wild, now you know what it is!

```js
const Button = React.createClass({
  render() {
    return React.createElement('button', {}, 'Click me!');
  }
});
```

`React.createClass()` takes one argument: an object that is basically the specification of your component. The _only_ requirement for this specification is that your object has a `render()` method — everything else is optional. The reason for `render()` being required is, of course, that React needs to know _what_ it should show on our screen! More specifically, `render()` needs to return a _single_ child (that optionally has children of its own).

```html
<!DOCTYPE html>
<html>
    <head>
        <script src="https://fb.me/react-15.2.1.js"></script>
        <script src="https://fb.me/react-dom-15.2.1.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser.js"></script>
        <title>React Components</title>
    </head>
    <body>
        <div id='react-container'></div>
        <script type="text/babel">
            var MyComponent = React.createClass({
                render() {
                    return <div>
                        <h1>Hello World</h1>
                        <p>This is my first React component!</p>
                    </div>
                }
            })

            ReactDOM.render(<MyComponent />,
                document.getElementById('react-container'))

        </script>
    </body>
</html>
```

Another example: a component with some children without using JSX

```js
const ShoppingList = React.createClass({
  render() {
    return React.createElement('ul', {}, [
      React.createElement('li', {}, 'Bananas'),
      React.createElement('li', {}, 'Vanilla ice cream'),
      React.createElement('li', {}, 'Chocolate'),
    ]);
  }
});
```

Instead of passing in a string, we can also pass in a React component! For example, to create a `<div>` with two `Button` components, we would do the following:

```js
const nestedButtons = React.createElement('div', {}, [
  React.createElement(Button),
  React.createElement(Button),
]);
```

When React was first introduced, create class was the only way to create a component. Since then, two other techniques have emerged.

### Component with a ES6 class (new school)
With the introduction and widespread adoption of ES2015, we can create React component classes. We're extending the base `React.Component` class, and adding in any methods for our component specification. In this case, that's just the `render()` method.

```js
class Button extends React.Component {
  render() {
    return React.createElement('button', {}, 'Click me!');
  }
}
```

Using JSX we can do something like:

```js
class MyComponent extends React.Component {
    render() {
        return <div>
            <h1>Hello World</h1>
            <p>This is my first React component!</p>
        </div>
    }
}

ReactDOM.render(<MyComponent />,
    document.getElementById('react-container')) 
```    


Since createElement is just plain JavaScript, you can mix in loops, if statements, and anything else JavaScript allows.
You can also easily substitute in data stored in JSON.

```js
var contacts = [
  {key: 1, name: "James K Nelson", email: "james@jamesknelson.com", description: "Front-end Unicorn"},
  {key: 2, name: "Jim", email: "jim@example.com"},
  {key: 3, name: "Joe"},
]

var ContactItem = React.createClass({
  propTypes: {
    name: React.PropTypes.string.isRequired,
    email: React.PropTypes.string.isRequired,
    description: React.PropTypes.string,
  },

  render: function() {
    // I wrap mult-line return statements in parentheses to avoid the
    // inevitable bugs caused by forgetting that JavaScript will throw away
    // the final lines when possible. The parentheses are not strictly
    // necessary.
    return (
      React.createElement('li', {},
        React.createElement('h2', {}, this.props.name),
        React.createElement('a', {href: 'mailto:'+this.props.email}, this.props.email),
        React.createElement('div', {}, this.props.description)
      )
    )
  },
})

var contactItemElements = contacts
  .filter(function(contact) { return contact.email })
  .map(function(contact) { return React.createElement(ContactItem, contact) })

var rootElement =
  React.createElement('div', {}, 
    React.createElement('h1', {}, "Contacts"),
    React.createElement('ul', {}, contactItemElements)
  )

ReactDOM.render(rootElement, document.getElementById('react-app'))
```


### Component as a stateless functional component
A stateless functional component is just a simple function that returns react elements.

```js
const MyComponent = () => {
    return <div>
            <h1>Hello World</h1>
            <p>This is my first React component!</p>
        </div>
}

ReactDOM.render(<MyComponent />,
    document.getElementById('react-container'))
```


## Adding Properties
We can make our components more dynamic by adding properties. Sending properties to a component is very similar to adding attributes to HTML.

We’re going to wrap any JSX expression in `{curly braces}` and then we’re going to reference this on the React Props object.

```js
var MyComponent = React.createClass({
    render() {
        return <div>
            <h1>{this.props.text}</h1>
            <p>{this.props.children}</p>
        </div>
    }
})

ReactDOM.render(<div>
    <MyComponent text="Hello World">
    This is message 1
    </MyComponent>
    <MyComponent text="I am a Component">
    This is message 2
    </MyComponent>
    <MyComponent text="I have been reused!">
    This is message 3
    </MyComponent>
    </div>,
    document.getElementById('react-container'))
```

Props make it very very easy to display dynamic content which is what React is all about. It’s about displaying dynamic content and being able to reuse components.

## Handling Events
Inside of the render method it's always a best practice to wrap the JSX code into `()` to make sure we're free with line breaks.
In JSX we don't use the word `class` because class is a JS reserved word. Instead we use the JavaScript style `className` and use the Camel case.

```js
var Note = React.createClass({
    edit() {
        alert("Editing Note")
    },
    remove() {
        alert("Removing Note")
    },
    render() {
        return ( 
            <div className="note">
                <p>{this.props.children}</p>
                <span>
                  <button onClick={this.edit}>EDIT</button>
                  <button onClick={this.remove}>X</button>
                </span>
            </div>
            )
    }
})

ReactDOM.render(<Note>Hello World</Note>, 
    document.getElementById('react-container'))
```    

## State
When a component's state data changes, the render function will be called again to re-render the change in state.
`getInitialState()` is a React method enabling us to set an object with our initial values.
`onChange()` enables us to call a function on state change due to an action on our checkbox.

```js
var Checkbox = React.createClass({
    getInitialState() {
        return {checked: true}
    },
    handleCheck() {
        this.setState({checked: !this.state.checked})
    },
    render() {
        var msg
        if(this.state.checked) {
            msg = "checked"
        } else {
            msg = "unchecked"
        }
        return ( <div>
                <input type="checkbox" 
                       onChange={this.handleCheck}
                       defaultChecked={this.state.checked}/>
                <p>This box is {msg}</p>
            </div>)
    }
})
ReactDOM.render(<Checkbox/>, 
    document.getElementById('react-container'))
```    

Let's improve our note system
```js
var Note = React.createClass({
    getInitialState() {
        return {editing: false}
    },
    edit() {
        this.setState({editing: true})
    },
    save() {
        this.setState({editing: false})
    },
    remove() {
        alert("Removing Note")
    },
    renderForm() {
        return (
            <div className="note">
              <textarea></textarea>
              <button onClick={this.save}>SAVE</button>
            </div>
        )
    },
    renderDisplay() {
        return ( 
            <div className="note">
                <p>{this.props.children}</p>
                <span>
                  <button onClick={this.edit}>EDIT</button>
                  <button onClick={this.remove}>X</button>
                </span>
            </div>
            )
    },
    render() {
      return (this.state.editing) ? this.renderForm()
                                  : this.renderDisplay()

    }
})

ReactDOM.render(<Note>Hello World</Note>, 
    document.getElementById('react-container'))
```

## Using Refs
To grab data from our app we need to use a ref tag. Anytime you need to reach out to a form element or something where you can't access the value via Props and State, a Reference might be useful.

On our `renderForm()` method we add the ref tag
```js
renderForm() {
    return (
        <div className="note">
          <textarea ref="newText"></textarea>
          <button onClick={this.save}>SAVE</button>
        </div>
    )
}
```

Let's update our `save()` method
```js
save() {
    var val = this.refs.newText.value
    alert('Later we will save this value: ' + val)
    this.setState({editing: false})
},
```

## Props
Prop types are an optional feature, and serve as documentation about how we wish components to work, and what values you expect for them.

Let's create a new Board component and validate its props

```js
var Board = React.createClass({
    propTypes: {
        count: function(props, propName) {
            if(typeof props[propName] !== "number") {
                return new Error("the count must be a number")
            } 

            if(props[propName] > 100) {
                return new Error('Creating ' + props[propName] + ' notes is ridiculous')
            }
        }
    },
    render() {
        return (<div className='board'>
            {this.props.count}
            </div>)
    }
})

ReactDOM.render(<Board count={5000}/>, 
    document.getElementById('react-container'))
```

Let's update this code to put some dummy notes

```js
var Board = React.createClass({
    propTypes: {
        count: function(props, propName) {
            if(typeof props[propName] !== "number") {
                return new Error("the count must be a number")
            } 

            if(props[propName] > 100) {
                return new Error('Creating ' + props[propName] + ' notes is ridiculous')
            }
        }
    },
    getInitialState() {
        return {
            notes: [
              'Call Bob',
              'Email Sarah',
              'Eat lunch',
              'Finish proposal'
            ]
        }
    },

    render() {
        return (<div className='board'>
            {this.state.notes.map((note, i) => {
                return <Note key={i}>{note}</Note>
            })}
            </div>)
    }
})

ReactDOM.render(<Board count={10}/>, 
    document.getElementById('react-container'))
```  

## Update and Delete
The final code looks like

```js
var Note = React.createClass({
    getInitialState() {
        return {editing: false}
    },
    edit() {
        this.setState({editing: true})
    },
    save() {
        this.props.onChange(this.refs.newText.value, this.props.id)
        this.setState({editing: false})
    },
    remove() {
        this.props.onRemove(this.props.id)
    },
    renderForm() {
        return (
            <div className="note">
              <textarea ref="newText"></textarea>
              <button onClick={this.save}>SAVE</button>
            </div>
        )
    },
    renderDisplay() {
        return ( 
            <div className="note">
                <p>{this.props.children}</p>
                <span>
                  <button onClick={this.edit}>EDIT</button>
                  <button onClick={this.remove}>X</button>
                </span>
            </div>
            )
    },
    render() {
      return (this.state.editing) ? this.renderForm()
                                  : this.renderDisplay()

    }
})

var Board = React.createClass({
    propTypes: {
        count: function(props, propName) {
            if(typeof props[propName] !== "number") {
                return new Error("the count must be a number")
            } 

            if(props[propName] > 100) {
                return new Error('Creating ' + props[propName] + ' notes is ridiculous')
            }
        }
    },
    getInitialState() {
        return {
            notes: [
              {id: 0, note: 'Call Bob'},
              {id: 1, note: 'Email Sarah'},
              {id: 2, note: 'Eat Lunch'},
              {id: 3, note: 'Finish proposal'}
            ]
        }
    },
    update(newText, id) {
        var notes = this.state.notes.map(
            note => (note.id !== id) ?
               note : 
                {
                    ...note, 
                    note: newText
                }
            )
        this.setState({notes})
    },
    remove(id) {
        var notes = this.state.notes.filter(note => note.id !== id)
        this.setState({notes})
    },
    eachNote(note) {
        return (<Note key={note.id}
                      id={note.id}
                      onChange={this.update}
                      onRemove={this.remove}>
                  {note.note}
                </Note>)
    },
    render() {
        return (<div className='board'>
                   {this.state.notes.map(this.eachNote)}
                </div>)
    }
})

ReactDOM.render(<Board count={10}/>, 
    document.getElementById('react-container'))
```

We're passing property data up and down the tree from parent to child, and child to parent.
Note we have a `key` property: each child in an array, or iterator should have a unique key property. This is to ensure that the state and the identity of our components is maintained through multiple renders.

## Create 
We use the spread operator from ES6, that takes whatever is in the state of notes and makes those the first items in the array. Then we're adding a new item in the array: an object with an ID and note text.

Inside of our Board component let's add a button to add a note

```js
render() {
    return (<div className='board'>
               {this.state.notes.map(this.eachNote)}
               <button onClick={() => this.add('New Note')}>+</button>
            </div>)
}
```

In our Board component let's now make sure we have the method to handle the `onClick`: the `.add()` function.
We also make sure to create an Id for each note by creating a `nextId()` function.

```js
...
getInitialState() {
    return {
        notes: []
    }
},
nextId() {
    this.uniqueId = this.uniqueId || 0
    return this.uniqueId++
},
add(text) {
    var notes = [
        ...this.state.notes,
        {
            id: this.nextId(),
            note: text
        }
    ]
    this.setState({notes})
}
```

## Using Props
Props allow us to pass values into our components. These values can be anything: a string, an array, functions, and so on. They give us the opportunity to make our components more dynamic, and a **lot more** reusable. For example, say we have a `<MovieCard />` component. A movie has a title, a poster image, and many other attributes (or **prop**erties!). Our component would kind of look like this, with _hardcoded_ data:

```js
import React from 'react';
import ReactDOM from 'react-dom';

class MovieCard extends React.Component {
  render() {
    return (
      <div className="movie-card">
        <img src="http://image.tmdb.org/t/p/w342/kqjL17yufvn9OVLyXYpvtyrFfak.jpg" alt="Mad Max: Fury Road" />
        <h2>Mad Max: Fury Road</h2>
        <small>Genres: Action, Adventure, Science Fiction, Thriller</small>
      </div>
    );
  }
}

ReactDOM.render(
  <MovieCard />,
  document.getElementById('main')
);
```

To pass props to a component, you add them as attributes when you render them:

```js
<MyComponent propName={propValue} />
```

The value of a prop is passed in through curly braces, like above.

Armed with that knowledge, let's update our `ReactDOM.render()` call to include the data for the Mad Max movie in our props:

```js
ReactDOM.render(
  <MovieCard
    title="Mad Max: Fury Road"
    poster="http://image.tmdb.org/t/p/w342/kqjL17yufvn9OVLyXYpvtyrFfak.jpg"
    genres={['Action', 'Adventure', 'Science Fiction', 'Thriller']} 
  />,
  document.getElementById('main')
);
```

Notice how we passed in the genres as an inline array? We could also pass in variables instead, like this:

```js
const madMaxGenres = ['Action', 'Adventure', 'Science Fiction', 'Thriller'];

ReactDOM.render(
  <MovieCard
    title="Mad Max: Fury Road"
    poster="http://image.tmdb.org/t/p/w342/kqjL17yufvn9OVLyXYpvtyrFfak.jpg"
    genres={madMaxGenres} 
  />,
  document.getElementById('main')
);
```

Now that we've passed in our props, let's change our hardcoded data in the `render()` method to make use of the props we pass in instead. Props in a component can be accessed through `this.props` in the `render()` method (and most other component methods):

```js
class MovieCard extends React.Component {
  render() {
    return (
      <div className="movie-card">
        <img src={this.props.poster} alt={this.props.title} />
        <h2>{this.props.title}</h2>
        <small>Genres: {this.props.genres.join(', ')}</small>
      </div>
    );
  }
}
```

What if we didn't have a poster image for a movie? Ideally, we'd have a default poster image for that instead. Instead of passing in that default poster image in case we don't have one, we can tell update our `MovieCard` component to use a default value instead, if the `poster` prop was not provided. To do that, we add the `propTypes` property to our `MovieCard` class:

```js
class MovieCard extends React.Component {
  render() {
    // ... The render stuff from before
  }
}

MovieCard.defaultProps = {
  poster: 'http://i.imgur.com/bJw8ndW.png'
};
```

Now, whenever we omit the `poster` prop, or if it's undefined, the `MovieCard` component will use this default prop instead.

If we were still writing our components using `React.createClass()` instead of the ES2015 way, this is how we would add default props to that component:

```js
const MovieCard = React.createClass({
  getDefaultProps() {
    return {
      poster: 'http://i.imgur.com/bJw8ndW.png'
    };
  },
  render() {
    // ... The render stuff from before
  }
})
```

Note that the order of the method definitions does _not_ matter: `getDefaultProps()` could be added below the `render()` method too. Generally, though, it's best to keep your `render()` method last when declaring methods, and keeping the code for the initial state and props for the component all the way up top.

- [React Default Prop Values](https://facebook.github.io/react/docs/reusable-components.html#default-prop-values)
- [Babel: transform-class-properties](http://babeljs.io/docs/plugins/transform-class-properties/)

### PropTypes
Let's pretend we're running a super modern ice cream store where orders are done through the computer and shown in the browser using React. This means that we'd need some kind of `<Order />` component to represent all the delicious items that people have ordered.

We'll take a moment to stop and think about how we want to represent our order. Which data (i.e.) props do we need? What are the options? Are some of them required? Let's list them:

- `cone` — a boolean indicating if the ice cream should be in a cone, defaults to true
- `size` — a string to indicate the size of the order, defaults to `'regular'`
- `scoops` — an array of ice cream flavors
- `orderInfo` — an object containing data about the ice cream order

Our `<Order />` component would roughly look like this:

```js
class Order extends React.Component {
  render() {
    return (
      <div className="order">
        <ul>
          <li>{this.props.cone ? 'Cone' : 'Cup'}</li>
          <li>{this.props.size}</li>
          <li>{this.props.scoops.length} scoops: {this.props.scoops.join(', ')}</li>
          <li>Ordered by {this.props.orderInfo.customerName} at {this.props.orderInfo.orderedAt}.</li>
        </ul>
      </div>
    );
  }
}
```

Now that we know what our component will look like, let's add our default props (see the props list above). Afterwards, we'll start adding PropTypes to validate all the props being passed in. We do so by setting the `propTypes` property (which is an object) on the `Order` class:

```js
class Order extends React.Component {
  render() {
    // ...
  }
}

Order.defaultProps = {
  cone: true,
  size: 'regular'
};

Order.propTypes = {
  cone: React.PropTypes.bool,
  size: React.PropTypes.string,
  scoops: React.PropTypes.arrayOf(React.PropTypes.string).isRequired,
  orderInfo: React.PropTypes.object.isRequired
};
```

### Defining "shape" for object PropTypes
We told the `orderInfo` prop to expect an object, but can we be more specific? We don't just need any object, we want an object with the properties (`customerName` and `orderedAt`) that we care about!

Good news: we can! Using `React.PropTypes.shape`, we can tell our component to expect the prop to have a certain _shape_:

```js
Order.propTypes = {
  cone: React.PropTypes.bool,
  size: React.PropTypes.string,
  scoops: React.PropTypes.arrayOf(React.PropTypes.string).isRequired,
  orderInfo: React.PropTypes.shape({
    customerName: React.PropTypes.string.isRequired,
    orderedAt: React.PropTypes.number.isRequired // We're using UNIX timestamps here
  }).isRequired
};
```

- [PropTypes reference](https://facebook.github.io/react/docs/reusable-components.html#prop-validation)

Example: 
```js
// const React = require('react');
import React from 'react';

class Product extends React.Component {
  render(){
    return (
      <div className="product">
        <p>Name: {this.props.name}</p>
        {this.props.producer ? <small>{this.props.producer}</small> : null}
        <p>{this.props.hasWatermark ? 'Watermarked' : 'Not watermarked'}</p>
        <p>Weight: {this.props.weight}</p>
      </div>
    )
  }
};

Product.defaultProps = {
  hasWatermark: false
};

Product.propTypes = {
  name: React.PropTypes.string.isRequired,
  producer: React.PropTypes.string,
  hasWatermark: React.PropTypes.bool,
  color: React.PropTypes.oneOf(['white', 'eggshell-white', 'salmon']).isRequired,
  weight: (props, propName) => {
    const weight = props[propName];

    if (weight === undefined) {
      return new Error('The `weight` prop is required.');
    }

    if (isNaN(weight)) {
      return new Error('The `weight` prop is not a number.');
    }

    const isValidWeight = weight > 80 && weight < 300;

    if (!isValidWeight) {
      return new Error('The `weight` prop should range between 80 and 300.');
    }
  },

  // name: a string — required
  // producer: a string — optional
  // hasWatermark: a boolean — optional, defaults to false
  // color: a string — required, can only be 'white', 'eggshell-white' or 'salmon'
  // weight: a number — required, ranges between 80 and 300
};

module.exports = Product;
```

## React this.props.children
In React, a component can have one, many or no children. Consider the following code:

```js
<VideoPlayer>
  <VideoHeader>
    <h1 className="video-title">The Simpsons</h1>
  </VideoHeader>
  <VideoControls />
</VideoPlayer>
```

In this example, the `VideoPlayer` has two children: `VideoHeader` and `VideoControls`. `VideoHeader`, in turn, has one child: the `h1` with the title content. `VideoControls`, on the other hand, has no children.

Why is this important? As you can see above, we can use children to compose our interface. For a more concrete example, let's say we're creating a `<Panel>` component that allows us to add content to it. Using a panel might look a little like this:

```js
<Panel title="Browse for movies">
  <div>Movie stuff...</div>
  <div>Movie stuff...</div>
  <div>Movie stuff...</div>
  <div>Movie stuff...</div>
</Panel>
```

As you can see, we're adding content *inside* of the `<Panel>` tags. Now, how do we render that content in our component? We access it through **`this.props.children`** — a special prop that is passed to components automatically.

```js
export default class Panel extends React.Component {
  render() {
    return (
      <div className="panel">
        <div className="panel-header">{this.props.title}</div>
        <div className="panel-body">{this.props.children}</div>
      </div>
    );
  }
}
```

Since `this.props.children` can have one element, multiple elements, or none at all, its value is respectively `undefined`, a single child node, or an array of child nodes. Sometimes, we want to transform our children before rendering them — for example, to add additional props to every child. If we wanted to do that, we'd have to take the possible types of `this.props.children` into account. For example, if there is only one child, we can't map it.

### React.Children
Luckily, React provides us with a clean API to handle of looping children. If there is only one child (or none at all), it won't throw a fuss — it'll handle things for us nicely in the background.

Let's say we have a list of `Movie` components that are nested inside of a `MovieBrowser` component:

```js
<MovieBrowser>
  <Movie title="Mad Max: Fury Road" />
  <Movie title="Harry Potter & The Goblet Of Fire" />
</MovieBrowser>
```

Now, let's assume for some reason that we need to pass down an extra prop to our children — the props would like to know if they are being played or not. Our `MovieBrowser` component would look something like this, before we added the prop:

```js
export default class MovieBrowser extends React.Component {
  render() {
    const currentPlayingTitle = 'Mad Max: Fury Road';
    
    return (
      <div className="movie-browser">
        {this.props.children}
      </div>      
    );
  }
}
```

Now let's add in our `isPlaying` prop to the children of `MovieBrowser`:

```js
export default class MovieBrowser extends React.Component {
  render() {
    const currentPlayingTitle = 'Mad Max: Fury Road';
    const childrenWithExtraProp = React.Children.map(this.props.children, child => {
      return React.cloneElement(child, {
        isPlaying: child.props.title === currentPlayingTitle
      });
    });
    
    return (
      <div className="movie-browser">
        {childrenWithExtraProp}
      </div>      
    );
  }
}
```

`React.Children.map` has two parameters: the first one is the children themselves, and the second one is a function that transforms the value of the child. In this case, we're adding an extra prop. We do that using `React.cloneElement`. As the first argument we pass in the child component itself, and as the second argument, we pass in any additional props. Those additional props get merged with the child's existing props, overwriting any props with the same key.

As another example, let's say we want to wrap our components in an extra `div` with a special class. We also want to display the total amount of children.

```js
export default class SomeComponent extends React.Component {
  render() {
    const childrenWithWrapperDiv = React.Children.map(this.props.children, child => {
      return (
        <div className="some-component-special-class">{child}</div> 
      );
    });
    
    return (
      <div className="some-component">
        <p>This component has {React.Children.count(this.props.children)} children.</p>
        {childrenWithWrapperDiv}        
      </div>      
    );
  }
}
```

- [Explanation on Children](https://facebook.github.io/react/docs/multiple-components.html#children)
- [React.Children API](https://facebook.github.io/react/docs/top-level-api.html#react.children)

## State
Let's quickly talk about what _state_ is in React. State is basically data that is mutated in your component. Like with any state, this state can also **change**. That's an important part: while a component can't change its own props, it _can_ change its state.

State is used to handle several things in your component:

- Interactivity (e.g. changing data when a user clicks a button)
- Fetching remote data (remote data is, by definition, not available right away when the component is mounted — state gives us a way of updating the component once that data arrives)
- Reacting to the passing of time (i.e. setting an interval or timeout)

The best way to figure out if data should go in props or state is to ask ourselves _'Will this data ever change?'_. If not, it's a prop. If it will, it should go in state! Keep in mind that whenever props _and/or_ state change, the component will run its `render()` method again.

### Setting Initial State
Let's say we have a `<ToggleButton />` component. A toggle button has an on and off state. Using props for that value wouldn't work, since we can't actually change our props! Let's use state for this instead. We'll assume that the default state of this component is to be in the _off_ state. Let's call that state property `isEnabled`. Setting the initial state is done in the `constructor` of our ES2015 class component:

```js
class ToggleButton extends React.Component {
  constructor() {
    super();
    
    this.state = {
      isEnabled: false
    };
  }

  render() {
    return (
      <div className="toggle-button">I am toggled {this.state.isEnabled ? 'on' : 'off'}.</div>
    );
  }
}
```

If, for some reason, we were still using `React.createClass()`, we would add a method called `getInitialState()` that returns our state object:

```js
const ToggleButton = React.createClass({
  getInitialState() {
    return {
      isEnabled: false
    };
  },

  render() {
    return (
      <div className="toggle-button">I am toggled {this.state.isEnabled ? 'on' : 'off'}.</div>
    );
  }
});
```

Our component would show 'I am toggled off.' on the screen, since the initial state has set the `isEnabled` property to `false`. Not very exciting yet, but we'll get there!

### Keep state slim!
It's important to try and keep your state **as small as possible**. You should strive for a minimal amount of data in your state and compute the rest. For example, let's say we have a component `<Address />` that takes in two props: the `street` and the `city`. We'll add those two together to show the user a complete address. An example of having computed data in your state would be this:

```js
class Address extends React.Component {
  constructor(props) {
    super();
    
    this.state = {
      fullAddress: `${props.street}, ${props.city}`
    }
  }

  render() {
    return (
      <div className="address">{this.state.fullAddress}</div>
    );
  }
}
```

While this is all perfectly valid React code, storing computed values in your state (in this case, `fullAddress`) should be avoided. There's no good reason for the full address to go into our state, since we're just using props to 'compute' the full address. Instead, we should use the component's props directly:


```js
class Address extends React.Component {
  render() {
    return (
      <div className="address">{this.props.street}, {this.props.city}</div>
    );
  }
}
```

While component state is a very powerful feature, it should be used as sparingly as possible. State is hard to manage and can be very easy to lose sight of.

- [Official React docs on state](https://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html#components-are-just-state-machines)
- [Props vs. state](https://github.com/uberVU/react-guide/blob/master/props-vs-state.md)
- [Props in getInitialState Is an Anti-Pattern](https://facebook.github.io/react/tips/props-in-getInitialState-as-anti-pattern.html)

Another example

```js
import React from 'react';

class Bomb extends React.Component {
  constructor(props) {
    super();

    this.state = {
      secondsLeft: props.initialCount
    }
  };

  render() {
    const message = this.state.secondsLeft === 0 ? 'Boom!' : `${this.state.secondsLeft} seconds left before I go boom!`;

    return(
      <div>{message}</div>
    )
  };
}

module.exports = Bomb;
```

## React Event System
React has its own event system with special event handlers called `SyntheticEvent`. The reason for having a specific event system instead of using native events is cross-browser compatibility.

It's important to keep in mind that they are the  _exact same events_, just implemented in a consistent way! That means these events also have methods like `preventDefault()`, `stopPropagation()`, and so on.

We attach event handlers to an element much like how we'd add a prop. The handler name is always comprised of `on`, and the event name itself — for example `click`. These are joined together and camel-cased, so if we wanted to add a click handler, we'd call the prop `onClick`. This prop takes a function as a value — it can either be a reference to a method on the class (like our `tickle()` method), or an inline function. Most of time, we'll use a function reference. It looks like this:

```js
class Tickler extends React.Component {
  constructor() {
    super();
    
    this.tickle = this.tickle.bind(this);
  }
  
  tickle() {
    console.log('Tee hee!');
  }

  render() {
    return (
      <button onClick={this.tickle}>Tickle me!</button>
    );
  }
}
```

The important bit here is the `constructor()`, where we're binding our `tickle()` method. Note that this is _not_ required in this example (since we're not accessing the component's `this`). Realistically, all methods in a React component class will almost always use `this` in one way or another, so it's a good idea to get the binding out of the way, even if you don't explicitly need it yet.

There are a lot of event handlers we can add to an element, for example `onKeyUp`, `onMouseDown`, `onFocus`, `onSubmit`, and many more. Check out the [complete list of supported events](https://facebook.github.io/react/docs/events.html#supported-events) to see what else you can play around with!

- [React Synthetic Events](https://facebook.github.io/react/docs/events.html)
- [Supported-events](https://facebook.github.io/react/docs/events.html#supported-events)

### Accessing event data
Let's take a deeper look at the actual event being passed through. A `SyntheticEvent` event has all of its usual properties and methods. These includes its type, target, mouse coordinates, and so on. As a reminder, we add an event handler to a component, and then we can use the event's data like this:

```js
export default class Clicker extends React.Component {
  constructor() {
    super();
    
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick(event) {
    console.log(event.type); // prints 'click'
  }

  render() {
    return (
      <button onClick={this.handleClick}>Click me!</button>
    );
  }
}
```

For example, if we wanted to get the target of an event, we'd use `event.target`. If we want to prevent a default action whenever an event happens, we call `event.preventDefault()`. This is all super similar to regular browser events and should feel very familiar!

### Event Pooling
Event pooling means that whenever an event fires, its event data (an object) is sent to the callback. The object is then immediately cleaned up for later use. This is what we mean by 'pooling': the event object is in effect being sent back to the pool for use in a later event. It's something that trips up a lot of people, and you might have run into it yourself when inspecting `SyntheticEvent` in the browser.

If we click the button of our `Clicker` component and then inspect the logged out object in our console, we notice that all properties are `null` again. By the time we inspect the object in our browser, the event object will have already been returned to the pool. This means that we can't access event data in an asynchronous manner by saving it in the state, or running a timeout and _then_ accessing the event again.

You usually don't need to access your event data in an asynchronous manner like described above, but if you do, there are two options: you either store the data you need in a variable (e.g. `const target = event.target`), _or_ we can make the event persistent by calling that method: `event.persist()`.

```js
import React from 'react';

class DelayedButton extends React.Component {
  constructor(){
    super();

    this.handleClick = this.handleClick.bind(this);
  }

  handleClick(event){
    event.persist();
    setTimeout(() => {
      this.props.onDelayedClick(event);
    }, this.props.delay);
  }

  render(){
    return (
      <button onClick={this.handleClick}></button>
    )
  }
}

module.exports = DelayedButton;
```

## Updating State
While a React component can have initial state, the real power is in updating its state — after all, if we didn't need to update the state, the component shouldn't _have_ any state. State is only reserved for data that _changes_ in our component and is visible in the UI.

Instead of directly modifying the state using `this.state`, we use `this.setState()`. This is a function available to all React components, and allows us to let React know that the component state has changed. This way the components knows it should re-render, because its state has changed and its UI will most likely also change. 

Using a setter function like this is very performant. While other frameworks like Angular.js use "dirty checking" (continuously checking for changes in an object) to see if a property has changed, React _already knows_ because we use a built-in function to let it know what changes we'd like to make!

For example, let's say we have a component with a button, and a bit of text to indicate whether that button has been pressed yet. To update our state, we use `this.setState()` and pass in an object. This object will get merged with the current state. When the state has been updated, our component re-renders automatically.

```js
class ClickityClick extends React.Component {
  constructor() {
    super();
    
    // Define the initial state:
    this.state = {
      hasBeenClicked: false,
    };
    
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    this.setState({
      hasBeenClicked: true,
    });
  }

  render() {
    return (
      <div>
        <p>I have {this.state.hasBeenClicked ? 'not' : null} been clicked yet!</p>
        <button onClick={this.handleClick}>Click me!</button>
      </div>
    );
  }
}
```

### How State gets merged
When updating state, we don't have to pass in the entire state, just the property we want to update. For example, consider the following state for our component:

```
{
  hasBeenClicked: false,
  currentTheme: 'blue',
}
```

If we updated the `hasBeenClicked` using `this.setState()` like we did above, it would _merge_ the new state with the existing state, resulting in this new state:

```
{
  hasBeenClicked: true,
  currentTheme: 'blue',
}
```

One super important thing to note is that it only merges things on the first level.

A deep merge means that the merge will happen recursively, leaving any unchanges properties intact. For example, consider the following code sample:

```js
const house = {
  kitchen: {
    cabinets: 'white',
    table: {
      legs: 4
    }
  }
};

// Note: `deepMerge()` isn't actually a built-in function
const updatedHouse = deepMerge(house, {
  kitchen: {
    table: {
      legs: 8
    }
  }
});
```

Deeply merging like this would only update the `legs` property with a value of `8`, but the rest of the `kitchen` and `house` objects' structure will remain intact.

We can also use `Object.assign()` by merging the `addressInfo` object with the new data ourselves:

```
{
  theme: 'blue',
  addressInfo: {
    street: null,
    number: null,
    city: null,
    country: null
  },
}
```

We can update the city like so:

```js
this.setState({
  addressInfo: Object.assign({}, this.state.addressInfo, {
    city: 'New York City',
  }),
});
```

Another example
```js
const React = require('react');

class YouTubeDebugger extends React.Component {
  constructor() {
    super();

    this.state = {
      errors: [],
      user: null,
      settings: {
        bitrate: 8,
        video: {
          resolution: '1080p'
        }
      }
    };

    this.handleChangeBitrate = this.handleChangeBitrate.bind(this);
    this.handleChangeResolution = this.handleChangeResolution.bind(this);
  }

  handleChangeBitrate() {
    this.setState({
      settings: Object.assign({}, this.state.settings, {
        bitrate: 12
      }),
    });
  }

  handleChangeResolution() {
    this.setState({
      settings: Object.assign({}, this.state.settings, {
        video: Object.assign({}, this.state.settings.video, {
          resolution: '720p'
        })
      }),
    });
  }

  render() {
    return (
      <div>
        <button className="bitrate" onClick={this.handleChangeBitrate}>Change bitrate</button>
        <button className="resolution" onClick={this.handleChangeResolution}>Change resolution</button>
      </div>
    );
  }
}

module.exports = YouTubeDebugger;
```

### Setting state is not synchronous
One thing to keep in mind is that setting state is _not_ synchronous. For all intents and purposes, it might seem that way, since our components update right away. State updates, however, are _batched_ internally and then executed simultaneously whenever React feels it's appropriate. This might result in some unexpected behavior. Going back to our `ClickityClick` component above, let's log the state after we've set it using `this.setState()`:

```js
handleClick() {
  this.setState({
    hasBeenClicked: true,
  });
  console.log(this.state.hasBeenClicked); // prints false
}
```

The console output says `false`... but we just set it to `true`! What is this madness?

State changes, however instant they might appear, happen _asynchronously_. If we want to access our new state after it has been updated, we can optionally add a callback as a second argument to the `this.setState()` function. This callback will fire once the state has been updated, ensuring that `this.state` is now the new, shiny updated state. In code:

```js
handleClick() {
  this.setState(
    { hasBeenClicked: true },
    function () {
      console.log(this.state.hasBeenClicked); // prints true
    }
  );
}
```

### State changes vs. prop changes
It's important to note the difference between changes in state and changes in props. Changes in state and/or props will both trigger a re-render of our React component. However, changes in state can only happen _internally_ due to components changing their own state. Changes in props can only happen _externally_, due to changes in prop values being passed in.

- [Transferring props](https://facebook.github.io/react/docs/transferring-props.html)
- [Component API](https://facebook.github.io/react/docs/component-api.html)

## Components lifecycle
The component lifecycle provides hooks for creation, lifetime, and teardown of components. These methods allow you to do things like add libraries, load data, and more at very specific times.

- `getInitialState()` is going to be called once and will set the default for a state
- `componentWillMount()` is called right before the render, and it's the last chance to effect state prior to the render
- `render()`, that is the only required method
- `componentDidMount()` is going to fire right after a successful render

The component lifecycle also provides methods for updating

- `componentWillReceiveProps()` gives the opportunity to change the object and effect state
- `shouldComponentUpdate()` and `componentWillUpdate()` are invoked right before rendering and are used for optimization
- `render()` method again as it is part of the updating lifecycle as well
- `componentDidUpdate()` fires right after everything in the DOM has been updated

`componentWillUnmount()` is called right before the component is unmounted, to clean up DOM elements and invalidate timers. When it is called on the parent, all of the children are unmounted as well.

```js
var Box = React.createClass({
    componentWillMount() {
        alert('Component is about to mount')
    },
    componentDidMount() {
        alert('Component just mounted')
    },
    render() {
        return <div id='myDiv'></div>
    }
})
ReactDOM.render(<Box />, 
    document.getElementById('react-container'))

var getRidOfBox = document.getElementById('myDiv')
getRidOfBox.onclick = function() {
    ReactDOM.unmountComponentAtNode(
        document.getElementById('react-container'))
        alert('component is unmounted')
}
```

Another great example is `getDefaultProps()`, that provides us a life cycle method for initializing default properties for our components.

```js
var Box = React.createClass({
    getDefaultProps() {
        return {
            backgroundColor: 'purple',
            height: 200,
            width: 200
        }
    },
    render() {
        return (<div id='myDiv'>
                    <div style={this.props}></div>
                    <section style={this.props}></section>
                </div>)
    }
})
```

## Forms
Forms in React are fairly straight-forward and similar to regular HTML elements, albeit with a few changes that we need to be aware of.

Form elements include `<input>`, `<textarea>`, `<select>`, and `<form>` itself. When we talk about inputs in this lesson, we broadly mean the form elements (`<input>`, `<textarea>`, `<select>`) and not always specifically just `<input>`.

To control the value of these inputs, we use a prop specific to that type of input:

- For `<input>` and `<textarea>`, we use `value`
- For `<input type="checkbox">` and `<input type="radio">`, we use `checked`
- For `<option>`, we use `selected`

To listen for changes to the value of an input, we simply pass in the `onChange` prop with a callback function. Default values are set using either the regular value prop (`value`, `checked`, ...) _or_ the `defaultValue`/`defaultChecked` prop. This depends on if we're using an uncontrolled or controlled component. 

React provides us with two ways of setting and getting values in form elements. These two methods are called _uncontrolled_ and _controlled_ components. The quickest way to check if a component is controlled or uncontrolled is to check for `value` or `defaultValue`. If the component has a `value` prop, it is controlled (the state of the component is being controlled by React). If it doesn't have a `value` prop, it's an uncontrolled component. Uncontrolled components can optionally have a `defaultValue` prop to set its initial value. These two props (`value` and `defaultValue`) are _mutually exclusive_: a component is either controlled or uncontrolled, but it cannot be both.

### Uncontrolled component
In uncontrolled components, the state of the component's value is kept in the DOM itself — in other words, the form element in question (e.g. an `<input>`) has its _own internal state_. To retrieve that value, we would need direct access to the DOM component that holds the value, _or_ we'd have to add an `onChange` handler.

To set an initial value for the element, we'd use the `defaultValue` prop.

We can't use the `value` prop for this: we're not using state to explicitly store its value, so the component would never update its value anymore (since we're rendering the same thing).

### Controlled component
In controlled components, we explicitly set the value of a component, and update that value in response to any changes the user makes to the value of that component. That might sound a little wonky, but when you see the code, it'll become much clearer:

```js
class ControlledInput extends React.Component {
  constructor() {
    super();
    
    this.handleChange = this.handleChange.bind(this);
    
    this.state = {
      value: '',
    };
  }
  
  handleChange(ev) {
    this.setState({
      value: ev.target.value,
    });
  }

  render() {
    return (
      <input type="text" value={this.state.value} onChange={this.handleChange} />
    );
  }
}
```

As you can see, we can easily define the initial value by setting the initial `value` property on the state to whatever we want. 

Using a controlled component is the preferred way to do things in React — it allows us to keep _all_ component state in the React state, instead of relying on the DOM to retrieve the element's value through its internal state. Whenever our state\ changes, the component re-renders, rendering the input with the new updated value. If we don't update the state, our input wouldn't update when the user would type. In other words, we need to update our input's state _programatically_.

It might seem a little counterintuitive that we need to be so verbose, but this actually opens the door to additional functionality. For example, let's say we want to write an input that only takes in a number (let's pretend there is no `<input type="number">`). We can now validate the data the user enters _before_ we set it on the state, allowing us to block any invalid values. If the input is invalid, we simply avoid updating the state, preventing the input from updating. We could optionally set another state property (for example, `isInvalidNumber`). Using that state property, we can show an error in our component to indicate that the user tried to enter an invalid value.

If we tried to do this using an uncontrolled component, the input would be entered regardless, since we don't have control over the internal state of the input. In our `onChange` handler, we'd have to roll the input back to its previous value, which is pretty tedious!

- [React Forms](https://facebook.github.io/react/docs/forms.html)

```js
const React = require('react');

class TwitterMessage extends React.Component {
  constructor() {
    super();

    this.handleChange = this.handleChange.bind(this);

    this.state = {
      tweet: ''
    };
  }

  handleChange(event) {
    this.setState({
      tweet: event.target.value
    })
  }

  render() {
    return (
      <div>
        <strong>Your message:</strong>
        <input type="text" value={this.state.tweet} onChange={this.handleChange} />
        <p>{this.props.maxChars - this.state.tweet.length} characters remanining.</p>
      </div>
    );
  }
}

module.exports = TwitterMessage;
```

Another example
```js
const React = require('react');

class LoginForm extends React.Component {
  constructor() {
    super();

    this.handleUsernameChange = this.handleUsernameChange.bind(this);
    this.handlePasswordChange = this.handlePasswordChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);

    this.state = {
      username: '',
      password: ''
    };
  }

  handleUsernameChange(event){
    this.setState({
      username: event.target.value
    })
  }

  handlePasswordChange(event){
    this.setState({
      password: event.target.value
    })
  }

  handleSubmit(event){
    const {username, password} = this.state;
    //=> const username = this.state.username
    //=> const password = this.state.password
    event.preventDefault();

    if (!username || !password){
      console.log('Cannot log in with incomplete info');
      return;
      // if not valid gets out of the function without proceeding
    }
    // could be an else statement
    this.props.onSubmit({ username, password })
    // <LoginForm onSubmit={login} /> with login() a function that console.log()
    // function login({ username, password }) {
    //   console.log(`Logging in ${username} with password ${password}`);
    // }
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <div>
          <label>
            Username
            <input id="test-username" type="text" value={this.state.username} onChange={this.handleUsernameChange} />
          </label>
        </div>
        <div>
          <label>
            Password
            <input id="test-password" type="password" value={this.state.password} onChange={this.handlePasswordChange} />
          </label>
        </div>
        <div>
          <button type="submit">Log in</button>
        </div>
      </form>
    );
  }
}

module.exports = LoginForm;
```

Another example
```js
const React = require('react');

function countWords(line){
  console.log(line.split(' '));
  return line.split(' ').filter(function(word) { return word }).length;
  // return line.split(' ').filter(word => word).length;
}

function validatePoem(poem){
  const lines = poem.split('\n').filter(line => line);
  // returns an array of lines without empty lines

  const correctLineCount = lines.length === 3;
  // validates that there are 3 lines

  const correctWordCount = countWords(lines[0]) === 5 && countWords(lines[1]) === 3 && countWords(lines[2]) === 5;
  // validates length of each line

  return correctLineCount && correctWordCount;
  // returns true or false
}

class PoemWriter extends React.Component {
  constructor() {
    super();

    this.handleChange = this.handleChange.bind(this);

    this.state = {
      poem: '',
      isValid: false
    };
  }

  handleChange(event){
    const content = event.target.value
    if (content) {
      this.setState({
        poem: content,
        isValid: validatePoem(content)
      })
    }
  }

  render() {
    return (
      <div>
        <textarea rows="3" cols="60" value={this.state.value} onChange={this.handleChange} />
        {!this.state.isValid ? <div id="poem-validation-error" style={{color: 'red'}}>This poem is not written in the right structure!</div> : null}
      </div>
    );
  }
}

module.exports = PoemWriter;
```

