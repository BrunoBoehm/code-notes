## The DOM 
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
