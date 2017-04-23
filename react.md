# ReactJS

React is a JavaScript library that's used for building user interfaces. It's an open source project, that started at Facebook, and is maintained by developers at Facebook and Instagram, and also a large community of contributors. One of the benefits and goals of the React project is to make developing a large scale, single page application easier.
React is also included in the larger movement in the world of JavaScript, the functional programming movement. You may have heard about functional JavaScript, a programming paradigm that emphasizes function composition over object orientation. React also aims to follow the principles of immutable data. So we create copies of objects and replace them, instead of mutating the originals. React is commonly used in enterprise applications, by companies including PayPal, AirBnB, Apple, Uber, and, of course, Facebook.

React has a virtual DOM that writes to the browser DOM only when it needs to. It only interacts with the virtual DOM, that JavaScript object in between the JavaScript logic and the actual DOM. When we call the `render` function, React will update the virtual DOM. That will push only the necessary changes all the way to the real DOM.

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

### Introducing JSX
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
