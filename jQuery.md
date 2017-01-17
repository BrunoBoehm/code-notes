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

## Prevent Default
What happens when you submit a form? Think about when you sign up for an online service, such as an Amazon account, or log into your Facebook or email accounts. What happens in the browser when you submit those forms? As soon as the form is submitted, the page in the browser refreshes. This happens every single time. This is known as the default browser behavior.

While this is obviously the behavior we would want to have most of the time, there are times when you maybe don't want a page refresh. Maybe you have client-side validations that check to make sure the form input is correct and the user doesn't enter valid input. Maybe you're building a single page application (like the calculator we built, or a to-do list), and refreshing the page would clear the data from the page.

Both of those circumstances involve stopping jQuery from performing the default behavior. We can do that by using the `preventDefault` function.

### Default Behavior

Let's say we have the following form with a div below it:

```html
<form>
  <input type="text" id="name">
  <input type="submit" value="submit">
</form>
<div id="hello">
</div>
```

And the following `submit` event that says hello to the user based on the name they entered. The greeting is added to the `div` with the ID `hello`. 

```js
$('form').on('submit', function(event){
  var name = $('#name').val();
  $("#hello").text("Hello, " + name);
});
```

When you actually enter and submit the form, instead of seeing the greeting, you'll see the page refresh.

### jQuery Event Object
So how do we use `preventDefault`? We need to refactor our code slightly:

```js
$('form').on('submit', function(event){
  var name = $('#name').val();
  $("#hello").text("Hello, " + name);
  event.preventDefault();
});
```

In the above code, we had to pass an `event` to the anonymous function. This `event` is the jQuery event object. Every time an event is bound to an element, this jQuery event object is created to represent that event.

In `js/script.js` go ahead and comment out all the code except for the code directly below the comment `//examine event object`. That code should have `debugger`. Refresh `index.html` in the browser and fire your click event. When you're dropped in the debugger console, go ahead and take a look at `event`.

You should see something like this:

```js
event
> n.Event {originalEvent: Event, type: "submit", timeStamp: 1453912261129, jQuery211012266199523583055: true, which: undefined…}
```

`event.currentTarget;` will return the HTML `form` because that is the element the submit event is bound to. `event.type;` returns `"submit"`.

```js
$(document).ready(function(){
  
  // browser refreshes on submit
  // $('form').on('submit', function(){
  //   var name = $('#name').val();
  //   $("#hello").text("Hello, " + name);
  // });

  //examine event object
  // $('form').on('submit', function(event){
  //   var name = $('#name').val();
  //   $("#hello").text("Hello, " + name);
  //   debugger;
  //   event.preventDefault();
  // });

  // stop page refresh
  $('form').on('submit', function(event){
    var name = $('#name').val();
    $("#hello").text("Hello, " + name);
    event.preventDefault();
  });

});
```

Now that we know what `event` represents, let's talk about what we do with that object. We have to pass `event` as an argument to the anonymous callback function.
```js
$('form').on('submit', function(event){
 //code
});
```

Now that we have `event` accessible inside the function, we can use it to call `preventDefault`:

```js
event.preventDefault();
```

This stops the event from performing its default behavior. Go into `js/script.js` and comment out all the code except the lines directly below the `// stop page refresh` comment. Now refresh `index.html` in the browser and submit the form. You should see your greeting appear!

+ [jQuery Event Object](https://api.jquery.com/category/events/event-object/).

To make a small todo list app
```html
<!doctype html>
<head>
  <script src="js/jquery-2.1.1.min.js"></script>
  <script src="js/todo.js"></script>
</head>
<body>
  <h1> TO DO LIST</h1>

  <h3> Your List</h3>
  <div id="list">
    <ol>
    </ol>
  </div>
  <h3> Add Items To Your List</h3>
  <form>
    <label form="item" name="item"><input type="text" id="item">
    <input type="submit" value="submit">
  </form>

</body>
```

And in the `js/todo.js` script file:
```js
$(document).ready(function(){
  // call functions here
  addItem();
});

// define functions here
function addItem() {
	$("form").on("submit", function(e) {
		// debugger;
		var item = $('#item').val();
		$('#list ol').append('<li>' + item + '</li>');
		e.preventDefault();
	});
}
```

## Stop Propagation
Let's say you're building a course registration site for Flatiron. You need to build a list of available courses and include some details about each course.

You need to build it so that when you click a course the details about the course disappear and reappear. Each course would need to have a click event that toggles the details. Flatiron also wants to be able to remove courses if they're not offered that semester, so there will need to be a button that removes that course (an `x`).

We'd end up with some HTML like this:

```html
<ul class="courses">
  <li class="course">
    Ruby
      <span class='delete'>x</span>
    <div class="detail">
      Learn Ruby. It loves you. Be happy.
    </div>
  </li>
  <li class="course">
    JavaScript
      <span class='delete'>x</span>
    <div class="detail">
      Learn JavaScript to build powerful full stack web apps.
    </div>
  </li>
  <li class="course">
    iOS
      <span class='delete'>x</span>
    <div class="detail">
      Everyone loves a good iPhone app.
    </div>
  </li>
</ul>
```

And let's say we create those click events:

```js
$('.course').on('click', function(){
  $(this).find('.detail').slideToggle();
});

$('.course .delete').on('click', function(){
  alert("about to delete");
});
```

The first click event toggles the details on and off the screen. The second click event just alerts `"about to delete"`. Go ahead and first click `Ruby` in the browser to hide the details. Then click the `x` to "delete" the item. You should see the alert appear, as we expected. But wait, why do the details toggle on the screen? How did that event happen?

In jQuery, all click events "bubble up" the DOM. The `document` object knows about every event that is triggered on a page. This means that in our example above, the `span` with the `x` is a child of the `li` with the class `course`. When we click the `x`, that click event bubbles up the DOM and the parent element, the `li`, knows a click event has been triggered, which in turn triggers the `li`'s click event.

What in the world? Why is that behavior we would want? In most cases you wouldn't at all. Imagine if you had a large series of nested elements all with click events. Firing the click event of the innermost child would trigger the click events of every single parent.

So how the heck do we stop that from happening? With `stopPropagation()`. Let's go ahead and refactor our jQuery:

```js
$('.course').on('click', function(){
  $(this).find('.detail').slideToggle();
});

$('.course .delete').on('click', function(event){
  alert("about to delete");
  event.stopPropagation();
});
```

You'll notice we didn't change anything to the click event on the class `course`. But we did make some changes to the delete button. We passed `event` to the anonymous function and then called the `stopPropagation()` function on the event object. This function stops the click event from bubbling up the DOM.

Go ahead and comment out all the code in `js/script.js` except the code under the comment `//stop propagation`. Refresh the page in the browser. Click a course name to hide the details, and then click the `x`. The alert should still appear, but this time the details should remain hidden.

+ [jQuery Docs](https://api.jquery.com/event.stoppropagation/)
+ [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation)

## AJAX and callbacks

### APIs
In JavaScript, we can use `XMLHttpRequest` directly to access remote data or web APIs, but it requires us to do a lot of lower-level wiring to get everything working. Now we're going to take a look at the Ajax capabilities of jQuery, which abstracts the XHR code into a nice, higher-level package.

We interact with web APIs through a set of URLs. Each URL defines a resource that we request or take action on.

Rather than just talk about it, let's get our hands dirty and explore an API. To do this, we're going to use a tool called Postman. Postman is an easy to use Chrome extension that lets us make different web requests. It easily allows us to interact with web APIs. For our exercise, we are going to work with the GitHub API to retrieve information about the jQuery GitHub repository. To get started we need to setup Postman.

Great!  Now you're all set up to make your own API requests. As you know, jQuery is a large open source project with many contributors. Those contributors make a hefty amount of commits. The GitHub API allows us to retrieve the list of commits made to the jQuery repository.

At this point Postman should be loaded and ready to go:

* Enter **https://api.github.com/repos/jquery/jquery/commits** in the URL textbox.
* Click the **Send** button.

Once the request finishes, Postman will display the results. Does this format look familiar?  If you said JSON (JavaScript Object Notation), then give yourself a pat on the back. What we're looking at is a JSON list of all the commits made to the jQuery repository. Each hash in the array has an author key. Do you recognize any of the committers?  If not, let's try to narrow the results returned from the GitHub API.

GitHub exposes a way for us to do this using HTML parameters. By changing the URL slightly to include the `author` parameter, we can ask the GitHub API to return only the commits made by John Resig (bonus points if you know who this is).

For the purpose of this lesson, we are mostly concerned with web APIs. But the term API actually has a more broad meaning.

> In computer programming, an application programming interface (API) is a set of routines, protocols, and tools for building software and applications. - Wikipedia

In its simplest form, an API in relation to object-oriented programming is a class and the list of methods we define for that class. When creating a class, we are defining a guidebook on how to interact with other parts of the code. We get to decide which methods and variables are public or private, essentially controlling how to interact with the class.

When we apply this concept to the web, we get web APIs like the GitHub and Twitter API. From our Postman experiment, we saw how GitHub provides a way for us to interact with the data on their system. Just like how a class provides a set of public methods to interact with, web APIs provide us with URLs.

The list of URLs that GitHub provides on https://developer.github.com/v3 act as the public methods into their system. The developers that created the API control which resources they want to share and who has access to them. In the end it's all just the same data available on GitHub.com. The big difference is that the GitHub API forgoes the HTML/CSS/JavaScript that makes up GitHub.com and serves up pure data, which is the only thing our applications need.

### AJAX
Wouldn't it be nice if page refreshes didn't exist? What if we could do multiple things at once from a single web page? In a perfect world we could type into a search textbox and have searches performed in the background as we type. That world is here, and it's called Ajax! Asynchronous JavaScript and XML (Ajax) is a technique that is used in web applications. It provides a way to retrieve content from a server and display it without having to refresh the entire page.

Modern dynamic applications provide a better user experience by allowing users to manipulate data on the server and see the results without having to refresh the page. This is the power of Ajax in action. In the background, requests are made to a web API using JavaScript. As developers we can then choose to alter the displayed HTML based on the responses from the web API.

When you've used `XMLHttpRequest` directly to query an API like GitHub to dynamically update your page, you've been using Ajax. With jQuery, we have a set of Ajax-related functions to make that even easier.

Let's try an example. You have an `index.html` file with a basic structure. Let's add a reference to jQuery so we can use it.

```html
<body>
  <p id="sentences">
    Loading our page...
  </p>
  <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>
  <script src="script.js"></script>
</body>
```

There's also a `sentence.html` file. Let's add some data to it.

```html
<p>Ajax is a brand of cleaning products, introduced by Colgate-Palmolive in 1947 for a powdered household and industrial cleaner.</p>
<p>It was one of the company's first major brands.</p>
```

Now in our `script.js`, let's use the jQuery `get()` function to make a `GET` request to our `sentence.html` data.

**Note:** Remote data can come from anywhere — even your own server! Remote just means it isn't directly included in the current page.

```js
// We should wait for the page to load before running our Ajax request
$(document).ready(function(){
  // Now we start the Ajax GET request. The first parameter is the URL with the data.
  // The second parameter is a function that handles the response.
  $.get("sentence.html", function(response) {
    // Here we are getting the element on the page with the id of sentences and
    // inserting the response
    $("#sentences").html(response);
  });
});
```

Let's load up `index.html` and see what happens!
Okay. Not a whole lot.

We actually need to serve this page from an HTTP server rather than load
it directly in our browser. At the console, run the following command:

```bash
$ python -m SimpleHTTPServer
```

This starts a simple server that will serve our files over HTTP. You need to start a server instead of just opening up `index.html` in the browser because of the browser enforced same-origin policy. To prevent security risks, the browser enforces a same origin policy. A different origin can be interpreted as a different domain, different protocol, and a different port.

When you open up  `index.html` by right clicking on the file, the site opens with `file://`. Same-origin policy only allows HTTP, data, chrome, chrome-extension, HTTPS, chrome-extension-resource protocols. Ajax uses HTTP requests and thus must interact with HTTP protocol in the browser.

Browse to [http://localhost:8000](http://localhost:8000/) and watch as the Ajax request is made and the new data is added to our web page. Pretty cool! This might all happen too quick to really notice anything, so you may want to have your terminal window side by side with the browser window. This way you can see the request hit our server.

We used the power of Ajax to load data from `sentence.html`. This same idea can be applied to calling the GitHub API or another remote resource.

### Callbacks
If we look at our last example, the Ajax request completed very quickly, but this won't always be the case. If we request data from a slow server over a slow internet connection, it might take multiple seconds to get a response. Using a callback allows our code to make our request and continue doing other things until the request completes.

Ajax follows an asynchronous pattern, which makes Ajax requests *non-blocking*. This means we don't sit around and wait for the request to finish before running the rest of our code. We set callbacks and then fire and forget. When the request is complete, the callbacks are responsible for deciding what to do with the returned data.

To make this concept stick, let's look at a real world example. When you put food into a microwave, you don't stand there and wait for the food to finish cooking. Okay, well, I do. Just in case.

But even if you stand there, you can do other things. You probably pick up your phone, look at Instagram, read some text messages, and, of course, work on [Learn](https://learn.co). Basically, you are doing other things while the microwave takes care of cooking your food.

When the food is cooked, the microwave beeps, and you remove the food and eat it. This final step of removing the food and eating it is exactly how our callbacks work. One thing to note: as we wait for our food, we don't check if it's done every 5 seconds (again, I do because I'm very hungry, but I don't *have* to). We wait until the beep tells us it's done. Checking every 5 seconds is called *polling*, and it's a lot less efficient than waiting for the beep, which is our *callback*.

So far, we have been dealing with successful API requests. But things don't always go according to plan. What happens if the API we are using doesn't respond? Or if we attempt to retrieve a resource that doesn't exist?

This can happen when API requests are based on user input. Let's go back to the GitHub API where we are retrieving commits. Imagine we want to retrieve a specific commit using a SHA.

Postman:
`https://api.github.com/repos/jquery/jquery/commits?sha=8f447576c918e3004ea479c278c11677920dc41a`
> Returns success.

Postman error:
`https://api.github.com/repos/jquery/jquery/commits?sha=fake-SHA`
> Returns a 404 not found.

A good developer will make sure to handle these unexpected events gracefully when using Ajax. We can provide multiple callbacks when using jQuery: one to handle a successful response and one to handle when an error occurs.

Let's add this inside our document ready function. Then, open the inspector, and reload the page.

```js
$(document).ready(function() {
	$.get("sentence.html", function(response) {
		$("#sentences").html(response);
	});

	$.get("this_doesnt_exist.html", function(data) {
		doSomethingGood();
	}).fail(function(error) {
  		console.log('Something went wrong: ' + error);
	});
});
```

We chained an additional call to `fail` on the end of our request. We passed a callback function to the method that will run only if an error occurs. In our example, we logged the error to the console, but in a real world situation you might want to try to fix the issue or inform the user.

Note that it doesn't matter what you call `data` and `error` in the above examples — the only thing that matters is the order of the arguments. In the callback to `get()`, the first argument is going to be the data that the server sent back, so it makes sense to call it `data` — but we could just as well call it `response`. Similarly, the first argument to `fail()`'s callback is an error object, so we should probably give it a descriptive name like `error` (but we don't have to).

This is another example of how we could use jQuery to perform an Ajax request.

```js
var url = "https://api.github.com/repos/rails/rails/commits?sha=82885325e04d78fb7ec608a4670164d842d23078";

$.get(url)
  .done(function(data) {
    console.log("Done");
    console.log(data);
  });
```

Note: The callback that gets passed into `.done`  gets `data` as an argument. `data` represents the response returned from the API. jQuery handles passing in that `data` object to the callbacks. This is essential to our fire and forget technique. We don't have to sit around and wait for the API to give us a response. Instead, we tell jQuery that when it receives a response to please pass it along to our callbacks so they can handle it accordingly.

* [Application programming interface](http://en.wikipedia.org/wiki/Application_programming_interface)
* [jQuery.get()](http://api.jquery.com/jquery.get/)

### Example with Handlebars
```js
// index.js

function handlebarsSetup() {
  //put any handlebars setup in here
  Handlebars.registerPartial("userDetails", $("#user-details-partial").html())
}

$(document).ready(function (){
  handlebarsSetup();
  searchRepositories();
});


function searchRepositories() {
	$("form a").on("click", function(event) {
		var searchTerm = $("input").val();

		$.get('https://api.github.com/search/repositories?q=' + searchTerm, function(data) {
			var source = $("#results-template").html()
			var template = Handlebars.compile(source);
			displaySuccess(searchTerm);
			$('#results').html(template(data))
		}).fail(function(error) {
			displayError();
		});
	});
}


function showCommits(repo) {
	// reads the data-attributes of the link
	var owner = repo.dataset.owner
	var repository = repo.dataset.repository
	$.get(`https://api.github.com/repos/${owner}/${repository}/commits`, function(data) {
		var source = $('#commits-template').html();
		var template = Handlebars.compile(source);
		$('#details').html(template(data));
	}).fail(function(error) {
		displayError();
	});
}

function displaySuccess(term) {
	// $("#errors").text("Here is what we found for " + term + "!");
	$("#errors").text(`Here is what we found for ${term}!`);
}

function displayError() {
	$("#errors").text("I'm sorry, there's been an error. Please try again.");
}
```
and for HTML
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Ajax Lab</title>

    <style>
      .flexbox-container {
        display: -ms-flex;
        display: -webkit-flex;
        display: flex;
      }
      .flexbox-container > div {
        width: 50%;
        padding: 10px;
        background-color: grey;
      }
      .flexbox-container > div:first-child {
        margin-right: 20px;
      }
    </style>
  </head>
  <body>
    <form>
      <input type="text" id="searchTerms"> <a href="#">Search Repositories</a>
    </form> 
    <hr>
    <main id="main">
      <div id="errors"></div>
    </main>
    <div class="flexbox-container">
      <div>
        <h3>Repositories</h3>
        <div id="results"></div>
      </div>
      <div>
        <h3>Details</h3>
        <div id="details"></div>
      </div>
    </div>
    <script src="jquery-3.1.0.min.js"></script>
    <script src="handlebars.js"></script>
    <script src="index.js"></script>

    <script id="results-template" type="text/x-handlebars-template">
      {{#each items }}
        <div class="entry">
          <h2>{{name}}</h2>
          <div class="body">
              <p>{{ description }}</p>
              <p><a href="{{html_url}}">Learn more about {{name}}</a></p>
              <p><a href="#" data-repository="{{ name }}" data-owner="{{ owner.login }}" onclick="showCommits(this)">Show commits</a></p>
          </div>
        </div>
      {{/each}} 
    </script>

    <script id="commits-template" type="text/x-handlebars-template">
      {{#each this }}
        <div class="entry">
          <h3>Commit {{ sha }}</h3>
          <div class="body">
              <p>{{ commit.message }}</p>
              <p><a href="{{ commit.url }}">Go to commit</a></p>
              <div>{{> userDetails author }}</div>
          </div>
        </div>
      {{/each}}
    </script>

    <!-- Needs to be registered in index.js -->
    <script id="user-details-partial" type="text/x-handlebars-template">
      <h4><a href="{{ url }}">{{ login }}</a></h4>
      <img width="32px" height="32px" src="{{ avatar_url }}">
      <hr>
    </script>    

  </body>
</html>
```
More about [Handlebars](http://handlebarsjs.com/).

## JS order of operations
Knowing when a line of code will run is easy if those lines are written like this:

```javascript
console.log(1);
console.log(2);
console.log(3);
```

We start from line #1 and execute each statement in order. If we run this code we'll see the following in our console:

```
1
2
3
```

Nothing special right? Each console statement is executed one after another. When we introduce callbacks, the order of operation changes. If we were to take our previous example and place the `console.log(2)` inside of the callback of an Ajax request:

```javascript
console.log(1);
$.getJSON("https://api.github.com/repos/rails/rails/commits", function(response){
  console.log(2);
});
console.log(3);
```

The console output would change to this:

```
1
3 // The 3 is printed before the 2!!
2
```

What happened, why are things out of order now?! The answer is simple, `console.log(2)` won't run until the `$.getJSON` completes. Our code executes in order except when we reach the Ajax request. The order now looks like this:

- `console.log(1)` executes.
- We create an Ajax request with `$.getJSON`.
  + Our callback contains `console.log(2)` but `console.log(2)` isn't executed just yet.
- `console.log(3)` executes.
- Our Ajax request completes and `console.log(2)` executes.

As we learned in previous lessons, not all Ajax requests complete successfully. Sometimes things go wrong and errors happen. When we add both success and error callbacks, order of operations is a little more confusing. In this next example, we run `console.log("Success")` only if the Ajax request completes successfully but if it fails, we only run `console.log("Fail")`.

```js
console.log(1);
$.getJSON("https://api.github.com/repos/rails/rails/commits", function(response){
  // This is our success callback which is called automatically
  console.log("Success");
}).fail(function(response){
  // We have to explicitly configure the fail/error callback for getJSON to handle it
  console.log("Fail");
});
console.log(2);
```
In our new code, both the success and error callbacks will only be executed after `console.log(1)` and `console.log(2)`. Nothing new there but which callback is executed is dependent on how the Ajax request completes. If the Ajax request fails, the callback provided for error is executed. If the Ajax request is successful, the callback provided for success will be executed. Let's take a look at how our new code runs:

- The `console.log(1)` executes.
- We make an Ajax request with `$.getJSON`.
  + Our success callback contains `console.log("Success")`.
  + Our error callback contains `console.log("Fail")`.
- `console.log(2)` executes.
- Our Ajax request finishes.
  + If our request succeeded then `console.log("Success")` is executed.
  + If our request failed then `console.log("Fail")` is executed.

## Lazy Loading
[Lazy Loading](http://en.wikipedia.org/wiki/Lazy_loading) is a way that web pages increase the speed at which their pages load by only loading some of the content that they want. They then add more content as the client scrolls down.

To see this in action, take a look at [Boxed.com](https://www.boxed.com/products/category/6/household). Scroll to the bottom of the page and notice that the site adds four items to the DOM each time you scroll down.

Lazy loading is a smart way to increase load times of pages. Let's say you work for a funny t-shirt company that has two-thousand designs that clients can choose from. If a client visits your site and clicks  "View All Designs", their browser takes a *while* to render the page because the browser has to render two-thousand images to their page, one for each design. This is not so good.

You could fix this by loading only the first thirty designs when the user clicks "View All". Then, as they scroll towards the bottom of the page, you could add thirty new shirts to the page. You would then have the JavaScript repeat this process until all the designs have been loaded.

## jQuery's `ajax` function

jQuery's [ajax](http://api.jquery.com/jquery.ajax/) function makes an asynchronous HTTP request. 

For instance, if I wanted to see the Netflix's share price, I could use the MarkIt API. The url for fetching a stock quote via MarkIt is `http://dev.markitondemand.com/Api/v2/Quote/jsonp?symbol=< stock symbol here>`. Netflix has a symbol of `NFLX`. Therefore, to get its quote info, you would visit [http://dev.markitondemand.com/Api/v2/Quote/jsonp?symbol=NFLX](http://dev.markitondemand.com/Api/v2/Quote/jsonp?symbol=NFLX). 

Here's an example AJAX request that adds Netflix's last stock price to the div `#netflix-price`.

```html
<html>
  <head>
    <title>Netflix</title>
    <script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
  </head>
  <body>
    <h1>Netflix Stock Price</h1>
    <div id="netflix-price"><i class="fa fa-spinner fa-spin"></i> Fetching Data...</div>
    <script>
    
      var url = "http://dev.markitondemand.com/Api/v2/Quote/jsonp?symbol=NFLX";

      $.ajax({
        url: url,
        contentType: 'application/json',
        dataType: 'jsonp',
        success: function(data) {
          var price = data["LastPrice"];
          $("#netflix-price").html("$" + price);
        }
      });

    </script>
  </body>
</html>
```

To see the above code in action, run `rackup` and visit [http://localhost:9292/example](http://localhost:9292/example).

### Example with easy loader for cars
In a ruby app, with `index.erb`
```erb
<html>
  <head>
    <title>Cars</title>
    <link rel="stylesheet" href="/css/bootstrap.min.css">
    <script src="/js/jquery.min.js"></script>
  </head>
  <body>
    <div class="jumbotron">
      <div class="container">
        <h1>Car Model Lazy Load</h1>
      </div>
    </div>
    <div class="container">
      <div id="cars">

        <div class="row">
          <div class="col-md-4 car">
            <h2>Chevrolet</h2>
            <p><strong>Model:</strong> Tahoe</p>
            <p><strong>Year:</strong> 2012</p>
          </div>
          <div class="col-md-4 car">
            <h2>Toyota</h2>
            <p><strong>Model:</strong> Camry</p>
            <p><strong>Year:</strong> 2002</p>
          </div>
          <div class="col-md-4 car">
            <h2>Mercedes-Benz</h2>
            <p><strong>Model:</strong> E-Class</p>
            <p><strong>Year:</strong> 1998</p>
          </div>
        </div>

        <div class="row">
          <div class="col-md-4 car">
            <h2>GMC</h2>
            <p><strong>Model:</strong> Acadia</p>
            <p><strong>Year:</strong> 2013</p>
          </div>
          <div class="col-md-4 car">
            <h2>Ford</h2>
            <p><strong>Model:</strong> Fusion</p>
            <p><strong>Year:</strong> 2011</p>
          </div>
          <div class="col-md-4 car">
            <h2>Chrysler</h2>
            <p><strong>Model:</strong> 300</p>
            <p><strong>Year:</strong> 2005</p>
          </div>
        </div><!-- row -->

        <!-- more cars should get added here -->
        <!-- even more cars should get added here -->
        <!-- et cetera -->

      </div><!-- car div -->
      <br>
      <div class="row">
        <div class="col-md-4 col-md-offset-4 center">
          <button class="btn btn-success" id="load-cars">Load more cars</button>
        </div>
      </div><!-- row -->
      <br>
      <br>
    </div><!-- container -->
    <script src="js/cars.js"></script>
    <script src="js/on-click.js"></script>
  </body>
</html>
```

This calls and `on-click.js` file
```js
"use strict";

$(document).ready(function() {
  // add click listener here
  // it should call on fetchJSON()
  $("#load-cars").on("click", function() {
  	fetchJSON();
  });
});
```

That in turn calls `cars.js`
```js
"use strict";

// this is the base API url
var baseUrl = "http://mimeocarlisting.azurewebsites.net/api/cars/";
var currentPage = 3;

function formatCars(carsJSON) {
  // this function shold return a string of properly formatted html
  // refer to app/views/index.erb lines 16 - 22 for an example of how
  // to format three cars, each in a div with a class "col-md-4", in a 
  // div with a class "row"
  var html = '<div class="row">';
  $.each(carsJSON, function(index, car) {
  	html += '<div class="col-md-4 car">';
  	html += 	'<h2>' + car.Make + '</h2>';
  	html += 	'<p><strong>Model: </strong>' + car.Model + '</p>';
  	html += 	'<p><strong>Year: </strong>' + car.Year + '</p>';
  	html += '</div>';
  });
  html += '</div>';
  return html;
}

function addCarsToDOM(carsJSON) {
  // this function should pass carsJSON to formatCars() and then 
  // add the resulting HTML to the div with an id of "cars"
  debugger;
  var html = formatCars(carsJSON);
  $("#cars").append(html);
}

function fetchJSON() {
  // this function will make the ajax call
  // on success of the ajax call, it will pass the returned data
  // to addCarsToDOM()
  var url = baseUrl + currentPage + "/3";
  currentPage += 1;
  $.ajax({
  	url: url,
  	contentType: 'application/json',
  	dataType: 'jsonp',
  	success: function(carsJSON) {
  		addCarsToDOM(carsJSON);
  	}, 
  	error: function(error) {
  		$('body').text("Sorry, there was an error with the request. Please refresh the page.")
  	}
  });
}
```

## APIs and JSON
Passing data back and forth from our server is easy when that data is simple. Once we have to represent complex data it gets hard really quick. Think about it, how would you represent a model class in a way that both Ruby and JavaScript understand. JSON provides a simple, light weight format that will do just that.

Data is a core part of programming. It can be represented in many different shapes and forms. We can create arrays and hashes to store the data. This is great for data that exists only in our application but what about data outside our application? We need some sort of standard, light weight way to represent our data.

JSON stands for JavaScript Object Notation. In a nutshell, it's a way to represent data in a format that closely resembles JavaScript objects. It's very light weight and easy to read. This is great because not only will it save bandwidth, we can read it with a plain old text editor. On top of all this, JavaScript can parse this format with one super easy line of code, `JSON.parse`.

What does JSON even look like? Let's take a look.

```javascript
{
  "artist_name": "Hozier",
  "track_name" : "Take Me to Church",
  "album_name" : "Take Me to Church EP"
}
```
Similar to how JavaScript objects are defined with `{}`, the same goes for JSON. Inside of the `{}` we have our key/value pairs. The artist name for this chunk of data is **Hozier** and the track name is **Take me to church**. We might even guess this represents a song. Let's put JSON to work with our Ajax requests.

### Getting JSON With Ajax
If we already know that the API we are using responds with JSON, we can use the jQuery function `$.getJSON`. This function performs an Ajax request and parses the response as JSON automatically. This means the callback receives a JavaScript object with the contents of the response data ready for us to use.

This next example shows an Ajax request to the Spotify API for information about a specific song.

```javascript
var url = 'https://api.spotify.com/v1/tracks/1zHlj4dQ8ZAtrayhuDDmkY';
var success_callback = function (songFromAPIRequest){
  // The response has already been parsed for us.
  alert("Song Name: " + songFromAPIRequest.name);
};

$.getJSON(url, success_callback);
```
We made a request to the API url and in our callback we created an alert dialog with the name of the song we got back. jQuery handled the request plus the parsing of the result so all we need to do is to figure out what to do with the data.

### Changing Our Request With Parameters
At some point, we will need to alter our request in order to change the data we receive. How we do this depends on the API we are calling but there is a good chance we will need to use url parameters. jQuery provides an easy way to do this by passing our parameters as a JavaScript object. Let's use the Spotify API to search for a song. We will specify 3 different criteria, `q` will be the search string, `type` will be the type of the thing we want to find and `limit` will be how many results we want.

```javascript
var url = 'https://api.spotify.com/v1/search';

// These parameters will be made into url parameters by jQuery
var url_params = {
  q: "Take Me to Church",
  type: "track",
  limit: 10
};

var success_callback = function (searchResultsFromAPIRequest){
  var tracks = searchResultsFromAPIRequest.tracks.items;
  $.each(tracks, function(index, track) {
    console.log("Song Found: " + track.name);
  });
};

// The second parameter we are passing is the url parameters to use in the request
$.getJSON(url, url_params, success_callback);
```
The result of the request will be the name of all the songs we found printed to the console. Under the covers, jQuery took our parameters and constructed a url to make the request with.

```
https://api.spotify.com/v1/search?qTake+Me+to+Church&type=track&limit10
```

Its also possible to create the url ourselves.

```javascript
// Here we manually create a url with all the url parameters
var url = 'https://api.spotify.com/v1/search?q=Take+Me+to+Church&type=track&limit=10';

var success_callback = function (searchResultsFromAPIRequest){
  var tracks = searchResultsFromAPIRequest.tracks.items;
  $.each(tracks, function(index, track) {
    console.log("Song Found: " + track.name);
  });
};

// No url parameters being passed since we already included them in the url
$.getJSON(url, success_callback);
```
In the end, we get the same results but we have the option to choose our approach based on the situation. One final thing to consider is that our success callback won't actually be called if the JSON that gets return from the API is invalid. We will need to have a error callback to handle these types of problems.

### Example with Github API
Let's write some code together to show how this all works. We will be
creating a simple markdown parser using the GitHub API.

Copy the following code into `js/api_client.js`.
```javascript
var printStargazers = function(users) {
  $.each(users, function(index, user) {
    console.log(user.login + ' starred the Rails Repository');
  });
};
```

This function will print an array of users to the console. Next, copy the following code into `js/api_client.js`.

```javascript
  $.ajax({
    url: 'https://api.github.com/repos/rails/rails/stargazers',
    type: 'GET',
  }).done(function(users) {
    printStargazers(users);
  });
```
This code makes a request to the GitHub API for all the users that starred the Ruby on Rails repository. The callback calls the `printStargazers` function. Let's try out our code so far. In your terminal run `python -m SimpleHTTPServer`. Browse to http://localhost:8000 and open Chrome developer tools. You should see a number of logs like this `dhh starred the Rails Repository`.

Now let's try a POST request. Copy the following code into `js/api_client.js`.

```javascript
  var addHTML = function (html){
    $('#search_results').html(html);
  };

  var bindCreateButton = function (){
    $('#convert').click(function(event) {
      var markdown = $('#markdown').val();
      $.ajax({
        url: 'https://api.github.com/markdown',
        type: 'POST',
        data: JSON.stringify({ text: markdown, mode: "markdown" })
      }).done(function(response) {
        addHTML(response);
      });
    });
  };

  $(document).ready(function(){
    bindCreateButton();
  });
```
Here we are sending markdown to the GitHub API to render into HTML. Once we get a response it adds the HTML to `<div id="search_results">`.

# Object-Oriented Javascript
You've already worked with objects in JavaScript, but so far we've only treated them like hashes. Now it's time to change frame of mind and start to view them as objects with properties and values, just like the objects we make in Ruby using classes.

This frame of reference from Ruby is going to make dealing with objects in JavaScript that much easier. You already know what an object is, so now it's time to learn how JavaScript handles them.

Let's create a user class and object in Ruby. The user will have a `name` and `email` attribute:

```ruby
class User
  attr_accessor :name, :email

  def initialize(name, email)
    @name = name
    @email = email
  end
end

kevin = User.new("kevin", "kevin@aol.com")
```

Now let's recreate the same thing in JS:

```js
var kevin = {
  name: "kevin",
  email: "kevin@aol.com"
}
```

How come when we create our `kevin` JavaScript object, it looks just like a hash but in Ruby our `kevin` object looks more like what we typically think of as an object?

Objects in Javascript are a lot more versatile than they are in Ruby.  Essentially hashes in Javascript are objects, but they can also have functions attached to them (methods).  In Ruby we think of Objects and Hashes as fundamentally different ideas (technically Hashes in Ruby are objects).  However, in Javascript, we can use objects like Hashes, just as a set of key value pairs, or we can attach functions to them and use them more like we traditionally think of using objects in Ruby.  In Ruby objects contain both data and behavior and we'll soon learn to use our Javascript objects in the same manner.

## Creating Objects in JS
Simple variables are great for holding primitive data types, like strings and integers, but we often need a way to represent more complex data, associating many values to a single idea. In JavaScript, the `Object` is the basic associative data structure, and it works just like a dictionary. We can use these objects to associate data *values* with unique *keys*, giving us a human-readable representation of a logical collection of data.

We can construct objects in JavaScript using the literal constructor:
`{}` and giving it some properties. Let's make a few sandwiches:

```js
var blt = {
  bread: "white",
  crust: false,
  meat: "bacon",
  condiments: "mayo",
  veggies: ["lettuce", "tomato"],
  cheese: "none"
}

var turkeyClub = {
  breadType: "sourdough",
  crust: true,
  meat: ["turkey", "bacon"],
  condiments: "mayo",
  veggies: ["lettuce", "tomato"],
  cheese: "cheddar"
}

var grilledCheese = {
  breadType: "white",
  crust: false,
  meat: "none",
  condiments: "none",
  veggies: "none",
  cheese: "cheddar"
}

```

Great. Three sandwiches. Plenty to share so we don't get into a... situation. You probably felt like this got tedious, however, and we only made three sandwiches. We're *[repeating ourselves](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)* a lot, copying and pasting (or, even worse, re-typing!) all of the same keys for our objects.

When we talk about objects in terms of data structures it's simple. The language provides us with an `Object` type that helps us encapsulate data into key/value pairs.

What we want to do now though is talk about objects in terms of *object-oriented programming*, which goes beyond simple data structures.

In object-oriented programming, we use objects to represent logical and often physical concepts, such as students, books, and even delicious sandwiches. In object-oriented programming (OOP), our objects should not only allow us to *encapsulate* data (i.e. gather and store values that are attributes of the object, such as `meat` and `condiments` in a `sandwich`), but also allow us to *reuse* the data structure without constantly redefining it.

In other words, we should only have to define the properties of a sandwich one time and then be able to create as many different sandwiches as we want without repeating ourselves. Is there a way to use JavaScript to create a template for a sandwich object that we can use to construct many different sandwiches?

### Constructor Function
Of course there is! It's called a *constructor function*, and its job, as you might guess from the very on-the-nose name, is to construct new objects. We use the constructor function pattern to essentially build a *prototype* for what an object will look like, including all the properties.

**Advanced:** We call the constructor function a *pattern* because it's not a concept that's built-in to the JavaScript language, but rather a design pattern that has evolved in to common usage as an accepted standard way to instantiate an object. Patterns can be small, task-oriented recipes, such as this constructor function, or they can be big, architecture-oriented guidelines, such as the MVC pattern at the root of a framework like Ruby on Rails or AngularJS. You can read more about design patterns [here](http://www.oodesign.com/).

Let's build a constructor function for our sandwich objects:

```js
function Sandwich(bread, crust, meat, condiments, veggies, cheese) {
  this.breadType = bread;
  this.crust = crust;
  this.meat = meat;
  this.condiments = condiments;
  this.veggies = veggies;
  this.cheese = cheese;
}
```

You'll notice the name of the constructor function `Sandwich` starts with a capital letter. This is important. While the capitalization of a function does not affect how it behaves, it serves as an important signal to our fellow programmers that this function should ONLY be used as a constructor. Adhering to this *convention* is a good way to communicate intent to other developers who may have to maintain this code.

Next, we define the function to accept a whole bunch of parameters. When we create objects with this constructor function, we'll pass in the value of the properties we want our object to have.

**Top-tip:** You'll notice inside the body of the constructor function we're using `this` in front of each of the property names. In this case, `this` will refer to the current object being created, and it's how we differentiate between the *property* `crust` and the local variable `crust` that we got from the function arguments.

### Creating an Instance From a Constructor Function
Now that we have a constructor function, let's create our sandwiches:

```js
var blt = new Sandwich("white", false, "bacon", "mayo", ["lettuce", "tomato"], "none");

var turkeyClub = new Sandwich("sourdough", true, ["turkey", "bacon"], "mayo", ["lettuce", "tomato"], "cheddar");

var grilledCheese = new Sandwich("white", false, "none", "none", "none", "cheddar");
```

Notice that when we call these functions, we always call them with the `new` keyword. JavaScript needs us to use the `new` keyword to instantiate a new instance of an object. Without it, we're just invoking the function and setting it to the value of a variable, and since the function doesn't return anything, our variable will be `undefined`.

All functions in JavaScript can be invoked with the `new` keyword, but we only want to do it with functions that are intended to be used as constructor functions. The way we let ourselves and others know when to use the `new` keyword is by making constructor functions start with capital letters! If we forget the `new` keyword we'll run into all sorts of problems.

How do we know that these are objects and that they were all created using the same constructor function?  We can look at the `constructor` property, which gets set automatically during the initialization of the object.

```js
blt.constructor;
// returns the Sandwich constructor function
turkeyClub.constructor;
//returns the Sandwich constructor function
grilledCheese.constructor;
//returns the Sandwich constructor function
```

### Reading Property Values
So now that we used the constructor function, how do we read the properties of an object?

You can access the properties just like you did when we were treating objects as hashes:

```js
blt["breadType"];
//returns white
turkeyClub["meat"]
// returns ["turkey", "bacon"]
grilledCheese["crust"]
//returns false
```

Or, you can use the dot-notation you're familiar with from Ruby:

```js
blt.breadType;
//returns white
turkeyClub.meat;
// returns ["turkey", "bacon"]
grilledCheese.crust;
//returns false
```

### Reassigning Property Values
Let's say you actually like to eat your grilled cheese with a slice of bacon and tomato, we would need to change the values of the `meat` and `veggies` properties:

```js
grilledCheese["meat"] = "bacon";
grilledCheese.veggies = "tomato";
```

## Object Methods and Classes
Objects have both data and behavior. Data comes in the form of properties that store information, such as the `length` of an array, or the `name` of a Person. But objects also can have properties that store behavior, or functions, such as the `slice()` method of an array. When a function is a property of an object, it is known as a *method* of that object.

### Adding Methods to an Object
Let's create a constructor function for some `User` objects.

```js
function User (name, email){
  this.name = name;
  this.email = email;
}
```

How do we give our JavaScript user objects the ability to say hello?

We already know how to create functions.  Now we need to attach a function to an object as a property.

```js
function User (name, email){
  this.name = name;
  this.email = email;
  this.sayHello = function(){
    console.log("Hello, my name is " + this.name);
  }
}
```

We've now added the `sayHello` method to our `User` constructor function. Because a method is just a function that is attached to an object via a property, `sayHello` is a method. We call `User` a function, and not a method, because it's a standalone function and not a property of any object.

It's a semantic distinction. All methods are also functions. We just use "method" as a convention when we communicate that lets other people know that we mean a function that is part of an object.

It's important to note that we use `this` twice in relation to the `sayHello` method. We use it once: `this.sayHello`, where `this` is referencing the object we'll create (as long as we invoke the function with the `new` keyword).  The `this` keyword is probably the most confusing concept in JS so for now let's just assume it works like Ruby's `self` and refers to the instance of the object we're refering to.

Let's make a few users:
```js
carl = new User("Carl", "sparkles@aol.com");

betsy = new User("Betsy", "betsy@flatironschool.com")

george = new User("George", "george@me.com")
```

We can have the users greet us too:
```js
carl.sayHello();
// prints "Hello, my name is Carl" to the console
betsy.sayHello();
// prints "Hello, my name is Betsy" to the console
george.sayHello();
// prints "Hello, my name is George" to the console
```

But there's a problem here. When we build the method directly into the constructor function like this, we're using a lot of space in memory. Every single time a `User` object is created and stored in memory, the `sayHello` function is created and stored in memory with it. What if you're Facebook and have 1.19 billion active users a month? If you were to instantiate all those users at once, you'd be recreating that function in memory 1.19 billion times! (incidentally this is how Ruby does it)

### Add Method to Prototype
Javascript objects have something called a Prototype.  For now, we won't get into an extremely detailed discussion of what Prototypes are, but we will use them as a place to keep our "instance" methods.  In Javascript, when you call a property, the interpreter will look on the instance of the object for a property, and when it finds none, it will look at the Object's Prototype for that property.  If we've attached a function as the property of that name it will call that function in a similar way that Ruby's method lookup chain works.

```js
function User (name, email){
  this.name = name;
  this.email = email;
}

User.prototype.sayHello = function(){
  console.log("Hello, my name is "+ this.name);
}

var sarah = new User("sarah", "sarah@aol.com");

sarah.sayHello();
```

For all intents and purposes, we've created a JS class following a common pattern that combines the use of constructor functions with extending behavior via the object's prototype. This works, but is incredibly verbose, and you always run the risk of forgetting to add methods to the prototype instead of directly to the constructor.

It would be nice if there were an approach that allowed us to construct true *classes* while still taking advantage of the prototypal nature of JavaScript. 

## ES6 Classes
ECMAScript 6 introduces the concept of a `class` to JavaScript that
provides a handy shortcut for organizing our objects.

It's important to note that the `class` keyword doesn't actually turn
JavaScript into a class-based object-oriented paradigm. It's just
*syntactical sugar*, or a nice abstraction, over the prototypal
object creation we've been doing.

Let's convert our user to a class.
```js
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  sayHello() {
    console.log("Hello, my name is "+ this.name);
  }
}

var sarah = new User("Sarah", "sarah@aol.com");
sarah.sayHello();
```

Instead of our `User` constructor function, we now have a `class User`. Within the body of the class, we can define a special function named `constructor` to be our constructor function. In the end, we still instantiate a `new User` the same way.

We also define our `sayHello` function directly in the body of the class. However, unlike defining it in the constructor function, we can verify that `sayHello` is defined on the User prototype by examining `User.prototype`.

### ES6 Class Inheritance With extends
We can also easily inherit from ES6 classes without having to go through the trouble of assigning `prototype` via `Object.create`.

Say we want to create a `Teacher` class for our school system that inherits from `User`. We can just define a new class and use the `extends` keyword.

```js
class Teacher extends User {
    sayHello() {
      super.sayHello()
      console.log("I am a teacher");
    }
}

var t = new Teacher("Tom", "tom@geocities.edu")
t.sayHello()
```

Here, we've *extended*, or inherited from `User` when creating the new `Teacher` class. We also created an *override* to the `sayHello` method so that it would reflect our teacher object better.

If you look at the line `super.sayHello()`, what we're doing there is calling the `sayHello` method of the *superclass*, or class our class inherits from. We wanted to preserve the behavior that was already there and then add to it, so rather than repeat the code, the `super` object gives us access to it programmatically.

+ [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
+ [MDN: Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)








