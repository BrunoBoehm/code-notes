# javascript-notes

#3 Creating an empty array

```javascript
var array = []
```
or
```javascript
var array = new Array();
```

#3 Adding an element to an array
#4 To the end of the array
- destructive (mutate the underlying array)

```javascript
array.push("blabla");
```
- non destructive method

```javascript
["blabla", ...array]
```

#4 To the beginning of the array

- destructive (mutate the underlying array)

```javascript
array.unshift("blabla");
```

- non destructive method

```javascript
[...array, "blabla"]
```

#4 Adding at a specific index
```javascript
array[2] = 4

array // [undefined, undefined, 4]
```

#3 Removing an element to an array
#4 At the beginning of the array

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

#4 At the end of an array

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

#4 removing an element at the middle of an array
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
