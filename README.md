# javascript-notes

## Creating an empty array

```javascript
var array = []
```
or
```javascript
var array = new Array();
```

## Adding an element to an array
### To the end of the array
- destructive (mutate the underlying array)

```javascript
array.push("blabla");
```
- non destructive method

```javascript
["blabla", ...array]
```

### To the beginning of the array

- destructive (mutate the underlying array)

```javascript
array.unshift("blabla");
```

- non destructive method

```javascript
[...array, "blabla"]
```

## Adding at a specific index
```javascript
array[2] = 4

array // [undefined, undefined, 4]
```

## Removing an element to an array
### At the beginning of the array

- destructive (mutate the underlying array)

```javascript
array.shift()
```

- non destructive method

```javascript
var array = ["Milo", "Garfield", "Otis"]
array.slide(1) // ["Garfield", "Otis"]
array // ["Milo", "Garfield", "Otis"]
```

### At the end of an array

- destructive (mutate the underlying array)

```javascript
var iceCreams = ["chocolate", "vanilla", "raspberry"]
 
iceCreams.pop() // returns the removed element, in this case "raspberry"
 
iceCreams // ["chocolate", "vanilla"]
```

- non destructive method

```javascript
var cats = ["Milo", "Garfield", "Otis"]
 
// get the last 2 cats
cats.slice(-2) // ["Garfield", "Otis"]
 
// get the last 1 cat
cats.slice(-1) // ["Otis"]
```

Or for pop() but in a non destructive way

```javascript
var iceCreams = ["chocolate", "vanilla", "raspberry"]
 
iceCreams.slice(0, iceCreams.length - 1) // ["chocolate", "vanilla"]
 
iceCreams // ["chocolate", "vanilla", "raspberry"]
```

### removing an element at the middle of an array
removing 1 item

- destructive (mutate the underlying array)

```javascript
items = [1, 2, 3, 4]
 
 // "at index 1, remove 1 item"
 // it returns the removed item(s): [2]
items.splice(1, 1)
 
items // [1, 3, 4]
```

- non destructive method
 
```javascript
var items = [1, 2, 3, 4, 5]
 
// let's remove the third element
 
// a slice from the start up to but not including index 2 (the third element)
// and a slice from index 3 to the end
[...items.slice(0, 2), ...items.slice(3)] // [1, 2, 4, 5]
```

***

## Javascript callbacks

In JavaScript, we have the ability to pass functions as arguments.

```javascript
function doTo5(anything) {
  return anything(5)
}

function divide10ByN(n) {
  return 10 / n
}
 
doTo5(divide10ByN) // 2


function appendToHello(s) {
  return `Hello, ${s}!`
}
 
doTo5(appendToHello) // 'Hello, 5!'

We do not call divide10ByN or appendToHello when we pass them as functions to doTo5() — that is, we pass just their name without any parentheses after it. That is because the calling actually happens inside doTo5(), when doTo5() calls back to the "outside world." Because of this process of calling back, we often call functions that we pass to other functions callbacks.

```

## Arrow functions

ES6 is the newest version of JavaScript to be released.
You are familiar by now with the standard 

```javascript
function foo() { return 'bar' } 
```
style of functions in JavaScript. What if I told you that ES6 introduced a new way of writing functions that is terser and more readable?

```javascript
var arrowFunction = () => {
  console.log('I was called!')
}
 
var regularFunction = function() {
  console.log('I was called, too!')
}
```


## Array.prototype.forEach()

```javascript
var evens = [0, 2, 4, 6, 8, 10]
 
events.forEach(even => {
	console.log(`${even} is not a loop`)
})

evens.forEach((even, index, array) => {
  debugger
  console.log(`${even} is not odd!`)
})
```

We can pass a function as an argument.
That function, in turn, accepts up to three arguments: the current element in the array, the index of that element in the array, and the array itself.
When a language allows functions to be used as arguments, we say that the language treats functions as first-class objects.

```javascript
function doToElementsInArray(array, callback) {
  array.forEach(callback)
}
```

## Return, Break, Continue

This will break as soon as encounters the condition
```javascript
const array = ["dog", 1, "cat"]
 
for (let i = 0, l = array.length; i < l; i++) {
  if (typeof array[i] !== 'string') {
    break
  }
 
  doSomethingToString(array[i])
}
```

In many instances, we can rewrite that final break example by encapsulating it in a function and using return to break out of the loop.
```javascript
function find(array, criteriaFunc) {
  for (let i = 0, l = array.length; i < l; i++) {
    if (criteriaFunc(array[i])) {
      return array[i]
    }
  }
}

Return exits the entire function if there are sevelal in the function, the first to be triggered exits the whole function.

// Find the first even number in an array
find([1, 1, 3, 1, 5, 6, 8, 10], (n) => n % 2 === 0) // 6
 
// function(n) { n % 2 === 0 }


// Find the first string whose length is > 3
find(["dog", "cat", "horse", "mouse", "ant", "cow", "antelope"], (s) => s.length > 3) // "horse"

```


To perform a certain operation on all of the elements of an array, but we want to skip elements that dont need to change
```javascript
const scores = [3, 4, 10, 5, 11, 6]
 
// increment all scores < 10
for (let i = 0, l < scores.length; i < l; i++) {
  if (scores[i] >= 10) {
    continue
  }
 
  scores[i]++
}
 
scores // [4, 5, 10, 6, 11, 7]
```

## Traversing Nested Objects

The values in an object can also be other objects.

```javascript
const person = {
  name: "Awesome Name",
  occupation: {
    title: "Senior Manager of Awesome",
    yearsHeld: 2
  },
  pets: [{
    kind: "dog",
    name: "Fiona"
  }, {
    kind: "cat",
    name: "Ralph"
  }]
}

person.occupation.yearsHeld // 2
person.pets[0].kind // "dog"
```

To access a nested array

```javascript
const collections = [1, [2, [4, [5, [6]], 3]]]

collections[1][1][1][1][0] // 6
```

If we need to loop through the nested array a function like this is needed

```javascript
function find(array, criteriaFunc) {
	let current = array
	let next = []
	debugger
	while (current) {
		// while current is truthy 
		// current will be falsey when we'll try to shift() `undefined`  (when next will be empty)
		if (criteriaFunc(current)) {
			return current
			// returns the current element that satisfies the search function
			// return will exit the whole function
		}
		if (Array.isArray(current)) {
			// if current is an array we want to loop through and list whats next to test into the `next` array
			for (let i =0, l = current.length; i<l; i++) {
				next.push(current[i])
			}
		}
		// take the first element of `next` array and push it to `current` testing, erase it (shift()) from the `next` array
		current = next.shift()
	}
	return null
}
```
BFS: bredth-first search is a search technique layer after layer.

## the DOM 

The DOM is the parsed version of your HTML. The DOM is a fully object-oriented representation of the web page, and it can be modified with a scripting language such as JavaScript.
JavaScript can update just about any aspect of the DOM, including adding and removing elements, modifying interactions, and even changing the entire page.
The DOM gets parsed from top to bottom, so it is best to write all of our JavaScript at the bottom of the page

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

## Creating and inserting nodes

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

## listening to nodes

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