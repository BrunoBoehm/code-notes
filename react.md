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

## Components
Components are small user interface elements that display data as it changes over time. These components can display data, handle state, and be nested inside of one another. React can be used for an entire site or just for smaller features, like a form, or an image carousel.

Components names are always capitalized, it's a best practice.
Render is always required when we create a component, because it tells react what we want to render to the DOM.

### Component with `.createClass`
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

When React was first introduced, create class was the only way to create a component. Since then, two other techniques have emerged.

### Component with a ES6 class
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
