#JQuery

## JS Libraries
A JavaScript library is prewritten JavaScript code that helps you streamline your own code base.

There are many JavaScript libraries designed for different purposes. [Jasmine](http://jasmine.github.io/) is a library for writing tests. [D3.js](https://d3js.org/) is a library for data visualization. [Bootstrap](http://getbootstrap.com/) has a JavaScript library for adding animations and effects to our sites.

One of the most widely used JavaScript libraries is [**jQuery**](https://jquery.org/).

### The problem with Javascript
As you've learned from CSS, not all browsers are created equal. There are properties specific to each browser, and code you have to write so that designs and features are cross-browser compatible. Even though things have improved drastically from the earlier versions of browsers (Internet Explorer 6, etc.), we still have to pay attention to browser differences.

Let's say we want to use JavaScript to add a CSS property to an HTML element.

We'll be working with the following HTML (which is also in `html/index.html` for you to test yourself with the console) for this example.

```html
<!doctype html>
<head>
  <title> JavaScript Madness</title>
</head>
<body>
  <p id="hey">HEY HEY HEY HEY HEY</p>
</body>
```

This HTML renders as a white page with black text displaying `"HEY HEY HEY HEY HEY"` in the top left corner of the page.

Let's say we want to change the font color to blue and the background color of the `p` tag to gray using JavaScript. We could do that with the following JavaScript:

```js
document.getElementById("hey").style.color = "blue";
document.getElementById("hey").style.backgroundColor = "#ccc";
```

`document` is the JavaScript object that represents the HTML document that gets loaded to render the page. We can use JavaScript functions to select and manipulate different HTML elements via `document`.

The functions `getElementById()` selects an HTML element by an ID. In this case we're passing in the ID `"hey"` which will select the `p` tag on our page. We're then using the JavaScript `style.property = "value"` function which allows us to change the CSS of an element.

Now that we're familiar with using JavaScript to change the CSS, let's try to float the text to the right side of the page.

For earlier versions of Firefox and Chrome, you could achieve that with the following JS:

```js
document.getElementById("header").style.cssFloat = "left";
```

But this code wouldn't work with earlier versions of Internet Explorer. Instead, you would have to write:

```js
document.getElementById("header").style.styleFloat = "left";
```

This need for different JavaScript code for different browsers can get very messy very quickly. Suddenly you need crazy if-statements and browser detection to trigger different lines of code for each browser, all just for a simple CSS change.

### jQuery To The Rescue

jQuery was created to solve this problem of needing different lines of code for different browsers. If you include the jQuery library in your code, you can write one single line of code that handles each browser on its own. AMAZING!

jQuery provides us with immense convenience methods that make our jobs as developers much easier, especially when dealing with AJAX and API calls. We can write significantly fewer lines of code and still achieve the same effect. jQuery is also an open source library and has a tremendous community around it. At the time of this writing, over [78% of the top million sites on the web use jQuery](http://trends.builtwith.com/javascript/jQuery) — crazy! There are many, many jQuery plugins that provide additional functionality and that allow us to build complex applications very quickly.

### Modifying HTML with JQUERY
In order to start writing jQuery, we need to include the library in our HTML. One way to do this would be to download a copy of the jQuery library and include it with our project. 

We can also link to the library hosted by a content delivery network, or CDN. For this example, we'll be loading jQuery from Google's CDN. Copy the code below and paste at the very bottom of the HTML `body` (right above the `</body>` close tag). This `script` tag links our HTML file to the jQuery library.

```html
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.3/jquery.min.js"></script>
```

The `$` is just a function — it's equivalent to `jQuery` (which is also a function that you can call). You might think of it as a fancy alias (with a few tricks up its sleeve) to `document.querySelectorAll`. The `$` is a shortcut for `jQuery`, and provides an interface to the library. Every time you see `$`, think `jQuery`.
```js
$("#yo").append("hey hey hey hey!!!!!");
```
`"#yo"` is our jQuery selector -- we're selecting the HTML element with the ID `yo`. We're then using the jQuery `append` function, which adds text to an HTML element, and we're passing in `"hey hey hey hey!!!!!"` which is the text we want to add.

### Document Ready
We don't ever want to write our JavaScript and jQuery inside our HTML files. For the same reasons that we want to separate out our CSS from our HTML, we want to separate out our JavaScript from our HTML, too.

But so far we've written our JavaScript code at the bottom of our HTML `<body>` so that the code would run once the page loads. How can we run our code when it's in a totally different file? We need to guarantee that the HTML document is loaded before our other files are triggered.

The first thing we need to do is load `script.js` in `index.html`. In the olden days (the 1990s and 2000s), we used to import our scripts in the `<head>` of our HTML documents. As our applications grew more interactive, our JavaScript files grew larger and our pages took longer to load. This was because the browser loads _everything_ in between the `<head>` tags before it attempts to render the page. Once the browser gets to `<body>`, it starts to load things in order (synchronously).

When that's just painting tags with the appropriate styles, the browser simply chugs along; but when it encounters a `<script>` tag, it either needs to evaluate the script or else make a request to the location specified in the `<script>` tag's `src` attribute. These requests take time, so nowadays, we put all of our `<script>` tags at the bottom of `<body>`, below all of the static HTML content. Go ahead and add a `<script>` tag for "script.js" at the bottom of `<body>`:

```html
<script src="script.js"></script>
```

Thankfully, the browser has a built-in way to determine when a page is loaded. In `script.js`, we need to set up a document ready in order to detect when our HTML page has loaded, and the document is ready to be manipulated:

```js
$(document).ready(function() {
  // code to be executed goes here
});
```

+ [Learn jQuery](http://learn.jquery.com/using-jquery-core/document-ready/)
+ [jQuery Docs](https://api.jquery.com/ready/)



## The DOM 
Let's think about Gmail for a second. When you're looking at your inbox, there are a million actions you can take. You favorite an email and the star turns yellow without a new page loading. You add a tag to an email and the tag also appears without the page reloading. Even opening an email just replaces the inbox content with the email content without loading a new page. All of this is done using JavaScript. But how was the code able to manipulate the HTML? By using, traversing and manipulating the DOM.

The DOM is the parsed version of your HTML. The DOM is a fully object-oriented representation of the web page, and it can be modified with a scripting language such as JavaScript.

JavaScript can update just about any aspect of the DOM, including adding and removing elements, modifying interactions, and even changing the entire page.
The DOM gets parsed from top to bottom, so it is best to write all of our JavaScript at the bottom of the page.

![Dom Structure Tree](https://s3.amazonaws.com/learn-verified/dom-tree.gif)

### **Window**
  + The highest level of the DOM tree is the `window` object. Think of the window as the browser window. The `window` contains the entire DOM document. All components of your HTML file will be accessible from within the window.
  + The `window` object has a large number of properties that return information about the object. Below are a few examples

```js
window.document;
//returns the entire HTML document

window.innerWidth;
// returns the inner width of the browser window. Open a console in the browser and enter this. Then shrink the browser window and run it again. You should get a different value.

window.innerHeight;
// returns the inner height of the browser window.
```

### **Document**
  + The `document` object represents any web page loaded in the browser. The `document` contains all the nodes that represent the HTML elements of the page. We use the `document` object to traverse through the HTML and manipulate elements.
  + There are many `document` properties that allow us to obtain information about the `document` programmatically.

```js
document.all;
// returns all the nodes inside the document object

document.contentType;
// returns the type of content contained. Most web pages should return "text/html"

document.URL;
// returns the URL of the document object
```

Below the document are all the nodes representing the HTML or XML elements on the page.

### Altering The DOM
We can alter the DOM through several different ways:

1. Add/remove HTML elements in the page.
  + You can add elements with functions like `appendChild`.
  + You can remove elements with the similarly named `removeChild`.
  + Both of these functions can be called on any node in the DOM tree.
2. Add/remove/change HTML attributes.
  + If you have a DOM node called `element`, `element.attributes` gives you access to its attributes.
  + You can remove attributes with `removeAttribute`.
3. Add/remove/change CSS styles.
  + You can modify any DOM node's `style` property.
4. Listen for key presses or mouse events on Elements.
  + You can add a listener directly using `addEventListener`. Elements emit an [extensive array](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) of events.
5. Create events in the page.

### Selecting elements
We can select HTML elements in the document with JavaScript and jQuery:

```javascript
document.getElementById('greeting')
// selects the element with id="greeting"

document.getElementsByClassName('banner')
// selects the element with class="banner"

document.getElementsByTagName('div')
// selects all elements corresponding to the tag "div"

document.getElementsByTagName('div')[0]
// returns the first div element

document.querySelector('div')
// would return the first match for div

document.querySelectorAll('div')
// it returns a NodeList (which, remember, is not an Array) of matching elements
```

We could write the above example in jQuery as:
```js
$("p");
```

Because jQuery was created as a way to make DOM manipulation quick and painless, a big part of that is correctly selecting the DOM node you want to work with. Thankfully, the browser provides a seamless way to select the correct HTML, similar to CSS selectors.


To set up a jQuery selector, you start with the `$`. Every line of jQuery code should start with the `$`.

```js
$('selector-goes-here')
```

The element selector returns all HTML elements with that specific tag:
```js
$('img')
```

Just like in CSS, we use a `.` to denote a class. This code is selecting any HTML elements with the class `pics`, which in this case is two different divs.
```js
$('.pics')
```

An ID selector works in much the same way as a class selector, you just replace the `.` with a `#`:
```js
$('#baby-ninja')
```

Let's say we want to select all list items inside the `ul` tag. The `li` tags are descendants of the `ul` tag. We can use a descendant selector like this:
```js
$('ul li')
```

Let's say we want to select the first `img` tag inside each `div`. We could do that with the first child selector. You set up this selector:
```js
$('parent-tag child-tag:first-child')
```

Because we want to select the first image inside a `div`, we put `div` first. Next comes the element we want to select, which is `img`, followed by `:first-child`:
```js
$('div img:first-child')
```

Let's say we want to select an image that has a specific alt text. The second image on our page (or the beatles) has the alt text `"the beatles making faces"`. We can use that text to find the image:
```js
$("img[alt='the beatles making faces']")

$('input[type="checkbox"]');
```

Let's say you want to select the last div on the page. You could use the last selector to do that
```js
$('div:last')
```

There are a tremendous number of other DOM selectors, including but not limited to last-child, nth-type-of,  next sibling, input selector, and the list goes on. Take a look at the [MDN docs](https://api.jquery.com/category/selectors/) for a full list.

## jQuery methods
Now that we know about jQuery selectors, we know how to select HTML elements to be able to manipulate them. But how do you actually start doing the manipulating? Enter jQuery methods, a quick and easy way to apply effects to DOM elements.

### Attribute Methods
#### addClass()
The `addClass()` function is called on a specific HTML element and adds a class (passed in as a parameter) to that element.

Given the following HTML:

```html
<p> This is my paragraph</p>
```

And this CSS:
```css
.myBorder {
  border-color: blue;
  border-width: 5px;
  border-style: dotted;
}
```

In our HTML, we never applied the class `myBorder` to our `p` tag. But let's say we wanted to do that programmatically with jQuery:

```js
$("p").addClass("myBorder");
```

We use `$("p")` as our selector to select the `p` tag, and then we call the `addClass` function, and pass it the paramater `"myBorder"`.

Our resulting HTML will look like this:

```html
<p class="myBorder"> This is my paragraph</p>
```

#### .html and .text
```js
$('#result').html(num1 - num2);

$("#result").text('Sorry, one of those is not a valid number!');
```

#### Val
Let's say you're building a form for an online order. Typically, you give your user an option to check if their shipping address is different than their billing address. If they click the box that they are different addresses, then they would expect an additional form to fill out for shipping address. In order to achieve this, you would want to check the `value` of the checkbox, and trigger the form to be filled out if it is checked with the `val` function.
The `val()` function returns the `value` attribute for an HTML element. 

Given the following HTML:

```html
<form>
  <input type="checkbox" value="choice a">Choice A
  <input type="checkbox" value="choice b">Choice B
</form>
```

We can select the value of an input with:
```js
$('input').val();
```

It's important to note that this function returns the very first match, so it would find the first input field and return `"choice a"`. 

We could change our selector to look for the selected checkbox:

```js
$('input:checked').val();
```

If you wanted to change the value of the input to "choice Z" instead of `"choice a"`, you would just pass a parameter to the `val` function:

```js
$('input').val("choice Z");
```

## CSS Methods

This group of jQuery methods deals with retrieving and setting CSS properties of HTML elements. These methods include `addClass`, `hasClass`, `removeClass`, `width`, `scrollLeft`, and many more.

### hasClass

If you wanted to trigger an event based on a class already set on a property, you could use `hasClass` to check if that class exists. If it does, it will return `true`.

Given the following HTML

```html
<ul class="favList">
  <li>Chocolate</li>
  <li>Puppies</li>
  <li>Gold</li>
</ul>
```

We could check if this list has the class `favList` by passing it as an argument to `hasClass`:

```js
$('ul').hasClass('favList')
```

## Effects
We can use jQuery to add special effects (like animations) to our web page.

### animate

The animate function allows you to create a custom animation on any HTML element. The function accepts a CSS object as a parameter.

Let's say we have an image of a bird on our website that we want to get bigger, we could achieve that with the animate function:

HTML:
```html
<img src="images/bird.png">
```

jQuery:
```js
$('img').animate({
  height: '500px'})
```

#### fadeOut

The `fadeOut` function changes the opacity of the selected elements.

Let's take the bird image again:

```html
<img src="images/bird.png">
```

We could make the bird disappear by using fadeOut. This function accepts a parameter of the amount of time in milliseconds for the item to fade:

```js
$('img').fadeOut(4000);
```

#### fadeIn

Once an item is opaque, you can make it  come back to full visibility with `fadeIn`.

HTML:
```html
<img src="images/bird.png">
```

```js
$('img').fadeOut(4000); //takes 4 seconds to fade image out
$('img').fadeIn(2000); //image back to full visibility in 2 seconds
```
#### hide

The `hide` function hides all matched elements.

Let's say we have a paragraph that we want to hide:

```html
<p> paragraph.</p>
```

We easily hide it:

```js
$('p').hide(); // hides the paragraph immediately
$('p').hide(4000); //gradually shrinks and hides the paragraph over 4 seconds
```

#### show

The show function makes hidden elements reappear:

```html
<p> paragraph.</p>
```

We easily hide it:

```js
$('p').hide(); // hides the paragraph immediately
$('p').show(); // shows the paragraph immediately

$('p').hide();
$('p').show(4000); // gradually grows and shows the paragraph
```

## DOM insertion: Creating and inserting nodes
The append method adds specific content to the end of the element. 

HTML:
```html
<p> Hey there. This is a short paragraph.</p>
```

To add text to the paragraph, we can use the `append` function and pass it a parameter of the content we want to add to the end of the p tag:

```js
$('p').append("Now the paragraph is a little bit longer because I am adding text to it with jQuery");
```

Now our HTML looks something like this:

```html
<p> Hey there. This is a short paragraph.Now the paragraph is a little bit longer because I am adding text to it with jQuery</p>
```

+ [jQuery Attribute Methods](https://api.jquery.com/category/attributes/)
+ [jQuery CSS Methods](https://api.jquery.com/category/css/)
+ [jQuery Effects Methods](https://api.jquery.com/category/effects/)
+ [jQuery DOM Insertion Methods](http://api.jquery.com/category/manipulation/dom-insertion-inside/)

```javascript
var element = document.createElement(tagName)
// tagName is the string representation of any valid HTML tag (e.g., 'p', 'div', 'span', etc.)

element.innerHTML = "Hello";
element.style.backgroundColor = '#f9f9f9';
element.style.textAlign = 'center';

document.body.appendChild(element)


// lets now create a list
var ul = document.createElement('ul')

for (let i = 0; i < 3; i++) {
	let li = document.createElement('li')
	li.innerHTML = (i + 1).toString()
	ul.appendChild(li)
	// Node.appendChild() method adds a node to the end of the list of children of a specified parent node
}

element.appendChild(ul)


// lets remove one of the li with element.removeChild(child); where child is the child node to be removed from the DOM
ul.removeChild(ul.querySelector('li:nth-child(2)'))

// lets remove the whole list
ul.remove()
```

## jQuery even listeners: listening to nodes
Click events probably make up a majority of events listened to, but other common events are change, 'keydown', 'keyup', 'load', 'mouseover', 'mouseout'
Full event list one [MDN](https://developer.mozilla.org/en-US/docs/Web/Events)

```javascript
// lets use target.addEventListener(type, listener[, options]);

const main = document.getElementById("main")
main.addEventListener('click', function(event) {
	alert('I was clicked')
})


const input = document.querySelector('input')
input.addEventListener('keydown', function(e) {
	console.log(e.which)
})

// and if we want to prevent the default behavior for one of the letters
input.addEventListener('keydown', function(e) {
	// if we strike keyboard key number 71
	if (e.which === 71) {
		return e.preventDefault()
		// preventDefault is a function that, when called, will prevent the, well, default event from taking place
	}
})
```

Like preventDefault, stopPropagation is a function that, when called, interrupts the event normal behavior. In this case, it stops the event from triggering other nodes in the DOM that might be listening for the same event.
Indeed one action can trigger multiple events.
By default, events nowadays all bubble.

DOM events propagate by DEFAULT (!!) by bubbling (they start at the target note and move up the DOM tree to the root)

DOM events propage also (not by default) by capturing (starting from the target node parent element and propagating down the tree to reach target)
In order to capture, we need to set the third argument to addEventListener to true

The basic problem is very simple. Suppose you have a element inside an element and both have an onClick event handler. If the user clicks on element2 he causes a click event in both element1 and element2. But which event fires first? 
More info on [Quirksmode.org](http://www.quirksmode.org/js/events_order.html)

```javascript
const divs = document.querySelectorAll('div')

function bubble() {
	console.log(this.firstChild.nodeValue.trim() + ' bubbled')
}

for (let i=0; i < divs.length; i++) {
	divs[i].addEventListener('click', bubble, false)
	// same as divs[i].addEventListener('click', bubble)
}

// trim() Remove whitespace from both sides of a string
// nodeValue() returns the value of the indicated node
// element.firstChild() returns the node inside of the target element
```

Now to capture

```javaScript
const divs = document.querySelectorAll('div')

function capture() {
	console.log(this.firstChild.nodeValue.trim() + ' captured')
}

for (let i = 0, l = divs.length; i<l; i++) {
	// dont forget to set the third argument to true
	divs[i].addEventListener('click', capture, true)
}
```


Sometimes when we want the event not to trigger any propagation, we can use ```stopPropagation```

```javascript
const divs = document.querySelectorAll('div')
 
function bubble() {
  // stop! that! propagation!
  e.stopPropagation()
 
  console.log(this.firstChild.nodeValue.trim() + ' bubbled')
}
 
for (let i = 0, l = divs.length; i < l; i++) {
  divs[i].addEventListener('click', bubble)
}
```

Ever used a website where an action you took triggered something to happen on the page? You clicked part of a form and suddenly more to fill out appeared. You moused over part of the page and a modal window appeared. On Facebook, you click to see more comments and the rest appear. The list goes on and on and on. When these actions happen, code is responding to an event taken by a user, and responding with an action.

In JavaScript, these things that happen are called **DOM events** and the code written to trigger the action is called an **event listener** or **event handler**

In order to trigger events, we need to set up a handler that will respond when we can run our code.

Let's take a basic example:

```html
<h1> This is some text to click</h1>
```

Given the above HTML, let's set up an event handler to pay attention to this `h1` tag:

```js
$('h1').on("eventname", function(){
    //action you want taken
});
```

We use `on` to bind an event to the HTML element we selected (in this case `h1`). The `on` function takes an argument of the name of the event you want the user to take. Whenever that event happens, the code inside the function will run.

### load

The load event handler triggers an action to happen when the element it's called on is loaded. An image is a good example for when this would be used because images typically load slower than text.

```html
  <img src="https://s3.amazonaws.com/after-school-assets/minion-tongue.jpg">
```

JS:
```js
$('img').on('load', function(){
  //actions you want to happen
});
```

The `keypress` event keeps track of every time a key on the keyboard is pressed (excluding ALT, CTRL, SHIFT, ESC). It's important to note that `keypress` isn't covered by browser specifications, so it's behavior isn't guaranteed.

`keydown` tracks every time a key is pressed down on the keyboard, and the `keyup` checks every time a key press is released.

keypress:

```js
$(document).on('keypress', function(key) {
  if (key.which == 13){
    alert('enter was pressed');
  }
});
```

keyup:
```js
$(document).on('keyup', function(key) {
  if (key.which == 82){
    alert('r was pressed');
  }
});
```

keydown:
```js
$(document).on('keydown', function(key) {
  if(key.which == 83){
      alert('s was pressed');
  }
});
```

The three examples above use `document` as the jQuery selector, so any time a key is pressed anywhere on the screen, the alert will appear. If you want to check if the cursor is in a particular area (like an input field) then you would just need to change the selector.

Let's walk through how `keydown` works. We use `document` as our selector, and bind the `keydown` event. We pass `theKey` to the function as an argument. Inside the function, we set up an `if` statement that checks if the `which` of the pressed key matches. `83` is the ASCII code for `s`. If the `s` key was pressed, the alert `"s was pressed"` will appear.

### submit

The `submit` event submits a form. Let's set up our submit event so that it only submits the form if a certain value has been entered in the text field:

```js
$("form").on("submit", function() {
  if ($( "input:first" ).val() === "correct") {
    alert('your form is going to be submitted now');
    return;
  }
  alert("you entered the wrong value");
  return;
});
```

In the example above, we are using the `submit` event. We use the HTML `form` as our selector to bind the event on. We then set up an if-statement that checks to see if the value of the first input is `"correct"`. If the user typed `"correct"` then the alert `'your form is going to be submitted now'` will appear, and the function will `return` so that no other lines of code will be executed. If they didn't enter `"correct"` the alert `"you entered the wrong value"` will appear. As of right now, the form is still submitting even with the wrong value, but later we'll learn how to prevent that from happening.

+ [Browser Events](https://api.jquery.com/category/events/browser-events/)
+ [ASCII Key Values](http://keycode.info/)

## This
It's often important in programming to know what the "owner" of a function is so that we can operate on some specific object or data safely.

For instance, consider an event handler that fires when someone clicks on a link in the DOM. When that event handling function is invoked, we might want to know *which specific* link was clicked so that we can manipulate it in some way.

That's where the `this` keyword comes into play. Every function is automatically assigned a `this` value when called, and that represents "who" called the function. This value can be an object, an event, or even another function.

The value of `this` is determined by the *execution context*, or scope, of the function call.

### Global Scope

Code running outside of a function is in the **Global Scope**. Every JavaScript runtime has a default global object that will be the value of `this` when a function is called in global scope. In the browser, if we're referencing `this` in the global scope, we're referencing the `window` object.

Try adding the following code to `script.js` and then run `index.html` in your browser with the JavaScript console open:

```js
console.log(this === window);
// returns true
```

Here, we called `this` in the global scope, so the value of `this` was set to the default global object, `window`, when the function was invoked.

### Function (Local) Scope

Inside of a function, the value of `this` will be set based on how the function is called.

#### Simple Function Call

Add the following to `script.js` and refresh the page with the console open:

```js
function checkThis(){
  console.log(this);
}
checkThis();
// outputs window object
```

This will also output the `window` object because this simple function call doesn't set the `this` value. Because we aren't in *strict mode*, the value of `this` must be an object, so the default global object `window` is used.

**Advanced:** Strict mode is a setting that enables better error-checking in your code by prohibiting the use of implicitly declared variables, duplicate parameter names, and other potentially bug-causing behavior. Strict mode also converts some silent execution errors into not-so-silent ones. For more information, check out the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode).

Let's put our function in strict mode:

```js
function checkThis(){
  'use strict';
  console.log(this);
}
checkThis();
// outputs "undefined"
```

In strict mode, `this` remains at whatever it's set to when entering the execution context. If it's not defined, it remains undefined rather than being assigned the default `window` object.

#### Constructor Function Call

When we use the constructor function pattern to create objects, we make use of `this` to assign property values to the instance of the object being constructed.

```js
function Chair(style, color) {
  console.log(this);
  this.style = style;
  this.color = color;
}
var sofa = new Chair("sofa", "green");
```

Here, because we're calling `Chair()` with the `new` keyword, `this` refers to the new `Chair` object. So when you see `this.style = style;` you can think of it as saying "set the style property of THIS current instance of a Chair."

In your console, `this` will be an empty Chair object because we haven't assigned the property values yet when we output `this`, but try moving that `console.log(this)` to the end of the function and see what you get!

#### Object Function Call

Objects can have functions as well as properties. When a function is an attribute of an object, `this` is set to the object instance that contains the function, similar to how it works with a constructor function.


```js
var couch = {
  color: 'green',
  f: function() {
    return this;
  }
};

console.log(couch.f());
// outputs object
```

**Top-tip:** Notice that this only outputs an `Object` and not something called `couch`? That's because `this` is just an instance of the base `Object` type here, and `couch` is just the name of the variable holding that object instance. The object itself is the owner of the function and has no idea what you named it.

#### DOM Events

We can also make use of `this` when handling events on the DOM. If our `index.html` has three image elements with the same CSS class, we might need to know which specific `img` was clicked.

```html
<img class="pix" src="http://i.giphy.com/S1phUc5mmaZqM.gif">
<img class="pix" src="http://i.giphy.com/eGe59ekUJEll6.gif">
<img class="pix" src="http://i.giphy.com/l41lNT5u8hCI92nQc.gif">
<script type="text/javascript" charset="utf-8">
    var els = document.getElementsByClassName("pix");
    function handleClick(e) {
      console.log(this);
    }
    for(var i=0 ; i < els.length ; i++){
      els[i].addEventListener("click", handleClick, false);
    }
</script>
```

Here, when we click a given image, we see that `this` refers to that specific DOM element, so, if we wanted to do something like hide it or apply some other class to it, we could safely do so on just that one element.


+ [MDN This](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
+ [JavaScript is Sexy](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/)
+ [StackOverFlow This Quiz](http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work)

## JS anonymous functions
We know functions (like methods in Ruby) are used to bundle our code to be able to reuse again and again and again. So why take the time to define and store a function we never plan on reusing? It takes up memory, requires extra typing, and completely goes against the purpose of a function. Thankfully, JavaScript allows us to easily solve this problem by using an **anonymous function**. An anonymous function is one that is never defined and is planned on being used once. They allow us to execute a block of code once. One and done.

One of the most common usages for anonymous functions is a callback function (a function passed as a parameter to another function).

### Event Handler Callbacks

We know that functions can accept functions as parameters. We've seen this time and time again when setting up jQuery event handlers. The `on` function accepts a string of the event you would like to bind and a function that is executed when the event is triggered. But the function we pass as a parameter was never defined anywhere previously. That function does not have a name.

```js
$('#submit').on('click', function(){
  alert("form submitted!");
});
```
The anonymous function (the callback function) in the example above simply creates an alert in the browser with the text `"form submitted!"`. This function is never executed in any other part of the code. We only want it to fire when the submit button is clicked.

#### setTimeout
The `setTimeout` function executes a code block after a specific amount of time has passed. This function accepts two parameters, a function and time in milliseconds.

```js
setTimeout(function(){ console.log("I waited 5 seconds to execute");}, 5000)
```

The `setTimeout` function accepts the anonymous function as the first parameter. This anonymous function prints `"I waited 5 seconds to execute"` to the console.

```js
function(){ console.log("I waited 5 seconds to execute");}
```

#### Function Expression

We've seen function expressions already, but they can also be considered anonymous functions:

```js
var numberz = function() {
    return 6+3;
}

//anonymous function because the function the variable is storing doesn't have a name

var cats = function kitties() {
    return "meow meow pow pow";
}
// not an anonymous function because the function has the name kitties
```

Remember that function declarations get hoisted to the top of their scope, but function expressions do not. In the above example, `var numberz;` would get hoisted to the top of the scope; the anonymous function it's storing would not. Therefore, at the top of the scope, the variable `numberz` is storing `undefined` and thus not taking up memory for the entire duration of the program.

You might be thinking that you've seen something like these anonymous functions before — and you'd be right. Anonymous functions are _sort of_ like Ruby blocks in that they provide a handy way to process incoming data but can't be called outside of their immediate context.

+ [JavaScript WE Blog](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/)
+ [Thoughtbot Blog](https://robots.thoughtbot.com/back-to-basics-anonymous-functions-and-closures)

## jQuery Iterators
Just like Ruby and JavaScript, jQuery also has iterators. They work in much the same way as the iterators we've already studied, but jQuery's let us manipulate the DOM. Feel free to copy and paste the examples below into `index.html` and `script.js` to try them out!

### `$.each`

Let's say we have an array of names, and we want to iterate over them to say hello:

```js
var names = ["Carleton", "Avi", "Azat", "Ann"]
```

jQuery has a built in iterator that lets us iterate over **plain JavaScript arrays**:

```js
$.each(names, function(index, name){
  console.log("hey " + name);
});
```

`$.each` accepts two parameters, the array to iterate over and a function that contains the actions to be taken with each item in the array. This anonymous function accepts two parameters: the index of the array and a variable placeholder to represent each item individually.

The first time iterating over the `names` array, `index` is storing 0 and `name` is storing `"Carleton"`.

### `.each`

`.each` is only different from `$.each` because it works on a collection of jQuery objects instead of a plain JavaScript array.

Let's say we have three divs on a page, and we want to print the div number inside each div using jQuery.

HTML:
```html
<div>
</div>
<div>
</div>
<div>
</div>
<div>
</div>
```
We can use the jQuery selector `$('div')` which will return us an array of jQuery objects.

We can iterate over this array using `.each` and use it to append the div number to the body of the div:

```js
$('div').each(function(index, div){
  $(this).append("this is div number " + (index + 1));
});
```

The `.each` function accepts a function as a parameter. The function accepts two parameters, a variable to represent the index of the array and a variable to represent each individual jQuery object in the array. Once inside the body of the  function, `this` represents the element of the array.

### `$.map`

Just like `$.each`, `$.map` only works on plain JavaScript arrays. Just like in Ruby, `map` in JavaScript creates a new array with the results of calling a provided function.

Let's take our first example of an array of names:

```js
var names = ["Carleton", "Avi", "Azat", "Ann"];
```

Using `$.map`, we can iterate over the names to add their programming language of choice:

```js
$.map(names, function(name, index){
   return name + " loves JavaScript"; 
});
```

You'll notice the order of the parameters in the anonymous functions are reversed. This is done to match the order of parameters in the plain JavaScript `map` function.

### `.map`

Let's say we have a list on a page, and we want to grab all the text snippets from the list to manipulate them:

```html
<ol>
  <li>Chocolate</li>
  <li>Fried Chicken</li>
  <li>Dumplings</li>
  <li>Pizza</li>
</ol>
```

We can use `.map` to iterate over the list and return an array of the text:

```js
function listIterate(){
  return $('li').map(function(item, index){
      return item.innerHTML;
  });
}
``` 

We defined a `listIterate` function. The body of the function calls `.map` on `$("li")`, which returns an array of jQuery objects representing all the list items. The important thing to note is that we have `return` in this method twice. The first `return` sets the new array as the return value of the `listIterate` function. The second `return` tells `.map` to add the text of the list item to the new array and move on to the next element in the array.









