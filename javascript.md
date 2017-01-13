# javascript-notes
JavaScript is the de facto language of the internet. It's a tool that, once mastered, will allow you to interact with your users via your app in fun and interesting ways. It is a dynamically typed, prototypical, functional programming language. 

To make a variable called `greeting` and set it equal to the string `"hello world"` in Ruby, we could just write:

```ruby
greeting = "hello world"
```

In JavaScript, to declare a local variable (which is almost always what you want to do), you add the keyword `var` before the variable name, like so:

```javascript
var greeting = "hello world";
```
Notice that semicolons are used after variable declaration and after printing information to the console.


As in Ruby, tabs and whitespace enhance readability, but have no special meaning.

```javascript
var tvShows = {
    "ABC": [
        "The Bachelorette",
        "Grey's Anatomy"
    ],
    "FOX": [
        "Bones",
        "Empire"
    ]
};
```

Semicolons *must* be used in some cases, *should* be used in others, and other times not at all.

**Necessary**

Semicolons must be used when two statements are on the same line (this is called statement chaining). In Ruby, you probably didn't see too much statement chaining so here's an example:

```ruby
fox = "XOF NWORB KCIUQ EHT"
fox.reverse!; fox.downcase!
# => "the quick brown fox"
```

Notice how that semicolon separates the reversal from the down-casing? JavaScript, like Ruby, also requires semicolons when executing two statements on the same line:

```javascript
var fox = "XOF NWORB KCIUQ EHT";
fox = fox.split("").reverse().join(""); fox = fox.toLowerCase();
// => "the quick brown fox"
```
Of course, there are exceptions. For instance, when we cover loops, you may notice that the `for` loop abides by this rule for the first two statements in its argument but not for the last.

**Ideal**

From [Codecademy's blog on semicolons](https://www.codecademy.com/blog/78):

> As we saw above, the semicolon in JavaScript is used to separate statements.
> However, it can be omitted if the statement is followed by a line break (or
> there’s only one statement in a {block}). A statement is a piece of code that
> tells the computer to do something. Here are the most common types of
> statements:

```javascript
var i;                        // variable declaration
i = 5;                        // value assignment
i = i + 1;                    // value assignment
i++;                          // same as above
var x = 9;                    // declaration & assignment
console.log("hi");            // function call
```

> All of these statements can end with a `;` but none of them must. However, it
> is a good habit to terminate each statement with a `;`.

**Avoid**

Semicolons don't follow the closing curly brackets of `if`, `for`, `while`, and
`switch` blocks. For instance, this if statement has no ending semicolon:

```javascript
var lunchtime = true;

if (lunchtime) {
  console.log("Go get lunch!");
}
```

The above-mentioned cases are pretty easy to remember. It's a bit trickier to remember that semicolons follow **function expressions** but not **function declarations**.

``` javascript
// function declaration (no semicolon)
function foo() {
    return 'foo';
}

// function expression (semicolon)
var bar = function() {
    return 'bar';
};
```

Also avoid putting semicolons between an ending parenthesis and a beginning curly bracket. In other words, you shouldn't write `) ; {`.

**Hints**

Should you get stuck with your semicolon usage (and don't worry, even professional JavaScript developers occasionally forget what goes where), paste your code into a JSLinter, such as [JSLint](http://www.jslint.com/) or even [JSFiddle](http://jsfiddle.net/). If you click `Lint`, you'll see a line-by-line evaluation of your semicolon usage and other syntax mistakes.

**A slightly more heretical take on semicolons**

Semicolons don't really matter in JavaScript. If we're working on a large JavaScript application, chances are we're at least using a [minifier](https://developers.google.com/speed/docs/insights/MinifyResources) that will insert semicolons as appropriate when it processes our code. And lately, it's becoming more popular to compile ECMAScript 6 to ES5 (the JavaScript that most browsers work in) using a tool like [Babel](https://babeljs.io/). [TypeScript](https://www.typescriptlang.org/) is also growing in popularity.

All of this is to say that the long-running semicolon debate in JavaScript is largely overblown at this point. Remember to use semicolons to separate statements that occur on a single line; otherwise, follow the conventions of the team.

## Truthy and Falsey Values

We make a lot of assumptions when we write computer programs. In order to make the right assumptions, we need to understand what values in our program evaluate to `truthy` values and which evaluate to `falsey` values.

Truthy values:

```javascript
'all non-empty strings';

'all number strings'; // for example '0', '5', '0.2', '-3.14'

[]; // an empty array

{}; // an empty object

1; // any non-zero number
```

Falsey values:

```javascript
0; // the number zero

'';  // an empty string

NaN;

null;

undefined;
```

## Printing to the Console

One of the first methods you probably learned in Ruby was `puts`. JavaScript has an equivalent function called `console.log()`. Let's take the Ruby code below and turn it into JavaScript code:

```ruby
puts "JavaScript is also known as ECMAScript"
```

Well let's change that `puts` into a `console.log`:

```javascript
console.log "JavaScript is also known as ECMAScript"
```
Now we'll wrap the string in parentheses:

```javascript
console.log("JavaScript is also known as ECMAScript")
```

And the icing on the top of this printing-string cake (yum!) is to add a semicolon at the end:

```javascript
console.log("JavaScript is also known as ECMAScript");
```

Will the code execute and work without the semicolon? Yes. Is the semicolon best practice? Yes. Should you use semicolons? Also yes.

Well, this code is all fine and dandy but how do you run it? The best, possibly most powerful thing about JavaScript is that all modern browsers know how to run it. All you need to do is make sure you're reading this on either Chrome or Firefox (trust us, the developer tools of these browsers are way better than on Safari). Then open up your browser's console. That's right, just like your computer knows about running a Ruby sandbox when you type `irb`, your browser runs a JavaScript sandbox when you open up the console.

To open up your browser's console from Chrome, type `command` + `option` + `J`. From Firefox, type `command` + `option` + `K`. (If you love writing directly into your terminal, go ahead and install [Node](http://blog.teamtreehouse.com/install-node-js-npm-mac). Then type `node` in your terminal and you'll be in a JavaScript sandbox.)

From your console (or Node terminal), simply type the code above into the prompt and hit `return`. There, you've run your first JavaScript code!

Open up Chrome and on any tab (even a blank one) right click and select `Inspect` from the dropdown. From there, select the `console` tab:

![console](https://s3.amazonaws.com/learn-verified/console.png)

The white space below is your console, a sandbox where you can enter and execute any JavaScript code you want.

![Executing JavaScript in Console](https://s3.amazonaws.com/learn-verified/exectuing-js-in-console.png)

## JavaScript Variables
As in Ruby, variables in JavaScript are used to store data that will be used in our program.  A variable can point to almost any type of value including numbers, strings, arrays, and hashes.

Just like Ruby, variables are assigned values using the `=` operator. Variable names are typically all lower case, and in the case of multiple words, the words are joined together using [lowerCamelCase](http://c2.com/cgi/wiki?LowerCamelCase).

Lets say I have the variable `word`. In Ruby, to assign a value to this variable, we would simple do

```ruby
word = "hey"
```

Ruby would understand automatically that we're creating a new variable and assigning it a value. If this variable was created in a method, it would only exist in the scope of the method. If it was created in a block, it would only exist in the scope of that block. Later in the method or block when you used that variable, Ruby wouldn't think to look outside the block for the variable definition.

JavaScript variables operates a little differently, and scope in JavaScript operates a lot differently. JavaScript variables must be declared before they can be assigned a value. If you don't declare your variable, JavaScript will bubble up through layers of scope (up out of the function you defined your variable in), till it finds a declared variable with that name. This means you could end up using different values than you thought you were.

### Value declaration
Declaring a variable without defining a value looks like this:

```js
var word;
```

Now try entering `word` in console. You should see `Undefined` because we never defined a value for this variable. Now I can assign `word` a value:
```js
word = "hey";
```

When you enter `word` you should see `hey`. This works, but feels pretty tedious. Thankfully we can declare and define a variable all on one line:

```js
var word = "hey";
```

### Multiple values assignment
```javascript
var a = 5;
var b = 2;
var c = 3;
var d = {};
var e = [];
```

The above is equivalent to:

```javascript
var a = 5,
    b = 2,
    c = 3,
    d = {},
    e = [];
```

which can be converted to:

```javascript
var a = 5, b = 2, c = 3, d = {}, e = [];
```

### Reassigning Variable Value

Changing the value of a variable in JavaScript works just in the same way as it does in Ruby:

```js
var word = "hey";
word;
// returns "hey"
word = "javascript";
word;
// returns "javascript";
```

### Local vs. Global Variables

Just like Ruby, JavaScript also has local and global variables. In Ruby, a program is written within the scope `Main`. The JavaScript equivalent is `window` (the browser window).

We'll dive much deeper into scope in JavaScript, but for all intents and purposes a global variable is any variable defined within the `window`. You can also think of global variables as any variable existing outside of a function (or method).

```javascript
var firstNum = 10; // is defined in the window and is thus accessible by the entire program
window.firstNum;
// returns 10
```

But if we declare a variable inside a function:

```js
function myFunk(){
  var funky = true;
}

myFunk();
window.funky;
// returns undefined because funky is variable defined inside a function and is thus a variable local to that function.
```
These consequences are due to JavaScript's approach to scoping. In Ruby, we didn't have to worry too much about scoping because all variables assigned within a method are scoped to just that method. For instance:

```ruby
def make_variable
  cute_animal = "sugar glider"
  return cute_animal
end

make_variable
# => "sugar glider"

cute_animal
# => NameError: undefined local variable or method `cute_animal' for main:Object
```

Unlike Ruby, JavaScript will make a variable have local scope only if you use the keyword `var`. For instance:

```javascript
function makeVariable() {
  cuteAnimal = "sugar glider";
  return cuteAnimal;
}

makeVariable();
// "sugar glider"

cuteAnimal;
// "sugar glider"
```

JavaScript does not treat variables as local by default; you must use the keyword `var`. Without this keyword, variables have a global scope. JavaScript knew about the variable `cuteAnimal` because we accidentally gave it a global scope. To make it local in scope (always what you want; don't pollute that global namespace with variables!), you'd have to add that `var` keyword, like so:

```javascript
function makeVariable() {
  var cuteAnimal = "sugar glider";
  return cuteAnimal;
}

makeVariable();
// "sugar glider"

cuteAnimal;
// ReferenceError: cuteAnimal is not defined
```

It's been mentioned already, but again, it is best to use the key word `var` before declaring a variable. This ensures that the variable is set to the current scope. If `var` is not used in defining a new variable it becomes global and is accessible throughout the program.

Keep in mind too that the same variable name used in different scopes is effectively a different variable.
We sometimes refer to repeating a variable name in an inner scope as "shadowing" — it's best to avoid,
as you'll quickly see how confusing it can be:

``` javascript
var cuteAnimal = 'quokka';

function makeVariable() {
  var cuteAnimal = 'sugar glider';
  return cuteAnimal;
}

makeVariable();
// 'sugar glider'

cuteAnimal;
// 'quokka'
```

### Changing Variable Values

Local variable assignment can overwrite global variable assignment:

```javascript
volume = 10; //declares a global variable called volume and sets it to 10

function returnEleven () {
  var volume = 11;  //declares a local variable called volume and sets it to 11
  return volume;
}

returnEleven(); // returns 11
volume; // the global variable is still 10

function goToEleven(){
  volume = 11;  //changes the global variable to 11
  return volume;
}

goToEleven(); // returns 11
volume; // the global variable volume has been changed to 11
```

However, global variable assignment can't overwrite local variable assignment, rather it simply reassigns the value of the local variable:

```javascript
function sayHello() {
  var greeting = "hola";
  greeting = "hello";
  return greeting;
}

sayHello()
// Returns "hello",
// This demonstrates that the variable greeting is now pointing to the string "hello" instead of "hola"

greeting
// ReferenceError: greeting is not defined
// This demonstrates that the variable greeting is still local instead of global
```

## Datatypes in JS
At the machine level, all data on a computer is bits — 1s and 0s. Humans, it turns out, prefer not to work so close to the metal, so we have _data types_ for describing different bits of information. Data types give us a quick way of understanding how we can operate on a given bit of data.

How do we know what types we're dealing with? JavaScript gives us the handy dandy `typeof`. We use it like so

``` javascript
typeof 2 // "number"
typeof "2" // "string"
typeof '2' // "string" — strings can be enclosed in single (') or double (") quotes
```

In addition to numbers and strings, JavaScript has the following _primitive_ types:
- Boolean
- Undefined
- Null

Strings are very straightforward in JavaScript. They are collections of characters. Plus signs are used to concatenate strings.

In their most basic form, strings look like

``` javascript
"I'm a string"
'I\'m also a string'
```

Notice that we added a `\` before the `'` in the second string above. Because we use quotation marks to tell JavaScript, "Hey, this is a string!", we have to _escape_ quotation marks when they're inside of a string so that JavaScript knows to treat them as part of the string.

When we wrap a string in double quotation marks, we don't need to escape single quotation marks (the apostrophe, in this example) that appear inside.

``` javascript
"I'm a string"
```

Similarly, we don't need to escape double quotes when we use them in a singly-quoted string:

``` javascript
'I said, "Strings are pretty nifty."'
```

We can also insert strings into other strings — this is called _interpolation_. JavaScript supports string interpolation with [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals).

Template literals look and behave like strings, except instead of being wrapped in a single or double quote, they're wrapped in backticks (at the top left of your keyboard, under the escape key — it looks like `).

```javascript
`High ${3 + 2}!` // 'High 5!'
```

## JavaScript Primitive Data Types and Objects
While JavaScript shares a lot of the same data types with Ruby, it does handle them a little bit differently.

In Ruby, every data type is an object. A string (whether it's made with the literal constructor or `String.new`) is considered an instance of the String class. Also in Ruby, every object is mutable, which means you can change the object with methods like `upcase` and `swapcase`.

In JavaScript, data types fall into two categories: **primitive data types** and **objects** and they behave very differently.

All primitive types are immutable (values incapable of being changed), while objects are mutable. Primitive values are important for application performance. They are way faster for an application to process than objects.

### Primitive Data Types

JavaScript has six primitive data types: 

+ Number
+ String
+ Boolean
+ Undefined
+ Null
+ Symbol

Primitive values are created using the literal constructor for each data type. 

```js
var name = "joe";
typeof name;
// returns "string"

var cents = 99;
typeof cents;
// returns "number"

var truth = true;
typeof truth;
// returns "boolean"
```

### Objects

In JavaScript, objects can be seen as a collection of properties. You can think of them as most similar to Ruby hashes but with the superpower of storing functions as values in addition to the standard strings, numbers, etc.

Objects are created using the New constructor:

```js
var word = new String("hello");
typeof word;
//returns "object"

word;
//returns String {0: "h", 1: "e", 2: "l", 3: "l", 4: "o", length: 5, [[PrimitiveValue]]: "hello"}

var num = new Number(10);
typeof num;
//returns "object"
num;
//returns Number {[[PrimitiveValue]]: 10}
```
Because objects are mutable, you can use convenience methods to mutate these values. We'll get more into String convenience methods later, but just know they exist and can be done to any string object.

### Mutating Numbers and Strings

So we know we can modify objects but not primitive data types. But that's not entirely true. Let's take a string created with the literal constructor as an example:

```js
var word = "hello";
typeof word;
//returns "string"


word.toUpperCase();
// returns "HELLO"
```

But wait, I thought primitive values, like the one we just created, were immutable? That's because JavaScript gives you the ability to treat primitive values like objects. In the example above, behind the scenes, JavaScript turns the primitive value into an object, mutates the object, and turns it back into a primitive value.

Given the following code:

```js
  var greeting = "hello";
  var word = new String("hello");
  word === greeting; 
  // returns false
```

The `===` is a type comparison operator. This means that it checks not just value, but data type. If the value and the data type don't match, it returns false. Using the type comparison operator, `greeting` and `word` are not the same type (one is a primitive value and the other an object) so we wouldn't expect them to be equal. But you can use convenience methods to manipulate both in the same way.

Because of this, you want to use the literal constructor as much as possible. For all intents and purposes, you can use the literal constructor and the new constructor in same way, but your code will process much more quickly with the literal constructor.

## Javascript Strings
Just like in Ruby, JavaScript has a handful of convenience methods built into the language that allow us to easily manipulate and modify strings. You can read all about Strings in JS, and even more methods [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String).

### Concatenation
Unlike Ruby, JavaScript doesn't come with a prior knowledge of how to interpolate a variable (remember, interpolation in Ruby looks like this: `"Hi, my name is #{name}"`).

Instead, when you want to combine several variables into one long string, you must concatenate.

```js
var name = "cricky";
console.log("Hi my name is " + name);
//this will print "Hi my name is cricky"
```
JavaScript does something unusual when you add a string with a number:

```javascript
var hours = 10;
var species = "sloths";

// the line below prints "sloths sleep 10 hours a day"
console.log(species + " sleep " + hours + " hours a day");
```

You'll notice in the example above, the variable `hours` is storing a number, while the rest of the sentence is a string. When adding a number and a string, JavaScript automatically transforms the number into a string.

### Length
JavaScript strings have a `length` property that will return the number of characters in that string.  Note that this is not a function call.

```javascript
var species = "mantis shrimp";

// the line below will print 13
console.log(species.length);
```

### Changing Case

The two common functions to change the case of a string are `toUpperCase()`  and `toLowerCase()`. It's important to note that when you call a function that doesn't require a parameter, like `toUpperCase()` or `toLowerCase()`, the function still needs to be invoked with `()`.

```javascript
"I'm not shouting!!!".toUpperCase();
// => "I'M NOT SHOUTING!!!"

"I'M A CONSCIENTIOUS LIBRARIAN.".toLowerCase();
// => "i'm a conscientious librarian."
```
You may notice that there is no pre-built `capitalize()` method. To capitalize just the first letter of a string, you'll have to make your own custom method.

### Replacing Characters
To replace characters with new characters, you can use the `replace()` method. It works similarly to Ruby's `gsub` method in that the first parameter is the set of characters you would like to remove and the second is the string you would like to add instead.

```javascript
var sentence = "pandas have two compound eyes and a proboscis".replace("pandas", "butterflies");
// => "butterflies have two compound eyes and a proboscis"
sentence.replace("two compound eyes", "two antennae");
// => "butterflies have two antennae and a proboscis"
```

Notice that the `replace()` will replace only the first occurrence:

```javascript
var quote = "xylophone phone home";
quote.replace("o", "*");

// quote is now "xyl*phone phone home"
```

The string class's `replace()` function can also take a regex for the first parameter:

```javascript
var phoneNumber = "5556768799";

// the line below will return "(555) 676 - 8799"
phoneNumber.replace(/(\d{3})(\d{3})(\d{4})/, '($1) $2 - $3');
```

### Turn String to Number

Two different functions to turn a string into a number are `parseInt(`) and `Number()`. The `Number` function creates a new number, while the `parseInt` function parses the string. Check out [this stack overflow post](http://stackoverflow.com/questions/4090518/what-is-the-difference-between-parseint-and-number) for more information. 

```javascript
Number("78");
// => 78
parseInt("78");
// => 78
Number("20px");
// => NaN
parseInt("20px");
// => 20
```

### Slice

The `slice()` method extracts a section of a string and returns a new string. It accepts one required parameter, the index to start on, and one optional parameter, the index to end on.

```javascript
var sentence = "They misunderestimated me.";
var word = sentence.slice(5, 22);

// the line below prints "misunderestimated"
console.log(word); 
```

Like in Ruby, you can also use negative indices with strings:

```javascript
var bushism = "They misunderestimated me.";
var word = bushism.slice(5, -4);

// the line below prints "misunderestimated"
console.log(word); 
```

### Split

The `split()` method splits a String object into an array of strings by separating the string into substrings.

```javascript
var longString = "Jan,Feb,Mar,Apr,May,Jun";
var months = longString.split(",");

// months is now ["Jan","Feb","Mar","Apr","May","Jun"]
```

### Resources

* [Codecademy -  Strings](http://www.codecademy.com/glossary/javascript/strings)
* [StackOverflow - What's the fastest way to convert String to Number in JavaScript?](http://stackoverflow.com/a/12862627/2890716)
* [StackOverflow - Capitalize the first letter of string](http://stackoverflow.com/a/1026087/2890716)
* [StackOverflow - Replacing all occurrences of a string in JavaScript](http://stackoverflow.com/a/1144788/2890716)

## Javascript Math
In Ruby, solving for the square root of a number involves using the `.sqrt` class method on the `Math` class.

While JavaScript supports a lot of the same mathematical operations as Ruby, there is one big difference. `Math` in JavaScript is actually a single built-in object that you can perform operations on. This `Math` object does have plenty of convenience functions that allow you to do math simply and easily. Think of these functions as the equivalent of instance methods in Ruby. You can read more about math in JavaScript [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math).

### Rounding

If you'd like to round to the nearest whole number, use the `.round()` method built out for you by JavaScript's Math class.

```javascript
// Returns the value 20
Math.round(20.49);

// Returns the value 21
Math.round(20.5);

// Returns the value -20
Math.round(-20.5);

// Returns the value -21
Math.round(-20.51);
```

### Floor

If you'd like to round to a whole number less than or equal to a number, use `Math.floor()`.

```javascript
// Returns 45
Math.floor(  45.95 );

// Returns -46
Math.floor( -45.95 );
```

### Absolute Value

The `Math.abs()` function returns the absolute value of a number.

```javascript
// Returns the value 1
Math.abs('-1');

// Returns the value 1.67
Math.abs('1.67');

// Returns the value 2.5
Math.abs(-2.5);
```

### Square Root

To take the square root of a number, pass the number to the Math class' `sqrt()` method.

```javascript
// Returns the value 3
Math.sqrt(9); // 3

// Returns the value 1.414213562373095
Math.sqrt(2);

// Returns the value 1
Math.sqrt(1);

// Returns the value 0
Math.sqrt(0);
```

### Order of Operations

Just like in Ruby, parentheses indicate precedence. For instance, the equation to convert Fahrenheit to Celsius is to first subtract thirty-two then multiply by five-ninths. To prioritize this subtraction before the multiplication happens, we wrap it in parentheses.

```js
temp_in_fahrenheit = 104
var celsius = (temp_in_fahrenheit - 32) * 5/9;
var rounded = Math.round(celsius);
rounded + "°C";

// Returns the value "40°C"
```

If we forgot to wrap the subtraction in parentheses, the code would multiply thirty-two by five-ninths then subtract this product from the value for Fahrenheit, leading to a return value of "86°C" instead (for reference, 86°C is 187°F).

### Adding Strings and Numbers

Something to be aware of as a Rubyist learning JavaScript is that JavaScript will concatenate numbers. In Ruby, when you add the strings "1", "2", and "3", you get the return string "123". However, if you were to add the strings "1" and "2" plus the number 3, you would get a type error:

Ruby:
```ruby
# Ruby

"1" + "2" + "3"
# => "123"

"1" + "2" + 3
# => TypeError: no implicit conversion of Fixnum into String
```

Unlike Ruby, JavaScript will convert this final number into a string and tack that string onto the end.

Javascript:
```javascript
// JavaScript

"1" + "2" + "3"
// => "123"

"1" + "2" + 3
// => "123"
```

Here are some more examples of JavaScript adding strings with one number:

```javascript
"1" + "5"
// => "15"

"1" + 5
// => "15"

5 + "1"
// => "51"
```

Note that JavaScript evaluates from left-to-right. In the following scenario, the two numbers are first summed, and then the result, `10`, is converted into a string and concatenated:

```javascript
5 + 5 + "5"
// => "105"
```

However, if a string starts off the statement, JavaScript will treat the following numbers as strings and not perform math on them:

```javascript
"5" + 5 + 5
// => "555"
```

For more info, see the [docs for the addition operator](http://es5.github.io/#x11.6.1).

### Resources

* [MDN - Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)
* [Addition operator](http://es5.github.io/#x11.6.1)
* [Arithmetic Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators)

## Functions
You're already very familiar with the idea of wrapping our code as methods in Ruby in order to make them reusable. In JavaScript, we call them functions and the syntax is slightly different, but the general idea is the same.

### Function Without Parameters

Here's the basic syntax for a function that doesn't take any parameters (you
know them as arguments):

```javascript
function nameOfFunction() {
  // code goes here
  return valueToReturn;
}
```

Notice the `def` keyword has been replaced with the `function` keyword. The name of the function is always followed by `()` and then curly braces that begin and end the function.

Further, the name of the function is not snake_cased, but rather lowerCamelCased. Snakecase is not used in JavaScript.

One last important thing to note is that JavaScript functions will always return
`undefined` unless you use the `return` keyword. In Ruby, writing `return` is
optional because Ruby always returns the value of the last line of code
evaluated. However, JavaScript has no implicit-return-value concept, so you must
write `return` before the value you want to return.

**NOTE**: This isn't exactly true in [ECMAScript 6](http://es6-features.org/).
For [arrow function](http://es6-features.org/#ExpressionBodies) expression
bodies that are not wrapped in curly brackets, you can omit the return:

``` javascript
[1, 2, 3, 4].filter(i => i % 2 === 0) // [2, 4]
```

But if you wrap the function body in curly brackets, you must explicitly
return your desired value:

``` javascript
[1, 2, 3, 4].filter(i => { return i % 2 === 0 }) // [2, 4]

// alternatively

[1, 2, 3, 4].filter(i => {
    return i % 2 === 0
})
```

Here's a simple example of a function that will greet us good morning:

```javascript
function greet() {
  return "Good morning you!";
}
```

To call a function, you type the function's name followed by parentheses and a semicolon:

```javascript
// Returns "Good morning you!"
greet();
```

If you try to call on a function using just its name, as you can in Ruby, the entire function will get returned back to you instead of the function's return value, so be careful!

```javascript
// Returns [Function: greet]
greet;
```

It's important to note that JavaScript does not run the code during a function definition. All it does is read that there is a function called `greet` and store it in memory. It doesn't actually create any of the variables inside the function (if there are any) until the method is called.

### Function Return Values

Unless you use the `return` keyword or call a function that explicitly returns a value, the implicit return value in JavaScript is  `undefined`.

> Undefined represents the absence of a primitive value

Other special return values in JavaScript are `null`

> Null represents the absence of an object

and `NaN`

> NaN represents an error from the improper use of a math operator.

### Functions "declaration" with parameters
The above is function is fine, but it could be better. For instance, what if you wanted your program to say the person's name instead of just "you" (ex. "Good morning Adam!" or "Good morning Steph!"). Well, just like in Ruby, you'd want to pass this function an argument. In JavaScript, arguments are called parameters.

```javascript
function greet(name) {
  return "Good morning " + name + "!";
}

// Returns "Good morning Adam!"
greet("Adam");

// Returns "Good morning Steph!"
greet("Steph");
```

What if we also wanted it to greet us based on the time of day, for instance "Good afternoon Harold!" or "Good night Jasmine!"? Well, then we'd have to pass our function a second parameter, the time of day:

```javascript
function greet(name, timeOfDay) {
  return "Good "+ timeOfDay + " "+ name + "!";
}

// Returns "Good afternoon Harold!"
greet("Harold", "afternoon");

// Returns "Good night Jasmine!"
greet("Jasmine", "night");
```

### Function "Expression"

There are two different ways to write functions in JavaScript. You can write
them as a function **declaration**, which is how we've been writing them, or as a
function **expression**.

A function expression looks something like this:

```js
var greet = function(name, timeOfDay){
  return "Good "+ timeOfDay + " "+ name + "!";
};
```

We can still call this function in the same way we would a function written with a function declaration:

```js
// Returns "Good afternoon Grover!"
greet("Grover", "afternoon");
```

**Function declarations are hoisted to the top of the current scope; function
expressions are not.**

```javascript
// this works, even though we're calling
// the function before the declaration —
// the `sayGoodbye` function is *hoisted*
// to the top of the current scope when
// the script is evaluated
sayGoodbye();

function sayGoodbye() {
    return 'Goodbye!';
}

// this does not work, as the function
// is not hoisted in a function expression

sayHello(); // TypeError: sayHello is not a function

var sayHello = function() {
    return 'Hello!';
};
```

### Default parameters

Ruby allows you to pass optional arguments into a method with default
argument values. To create a default argument, you add an equal sign where you
define the variable name of your argument and set it equal to the value you want
it to default to:

```ruby
def greet(name="you")
  "Good morning #{name}!"
end
```

This way, you can call on the method with the argument and it will set `name` to
be the string you passed.

Ruby:

```ruby
greet("Jasmine")
# => "Good morning Jasmine!"
```

**or** you can call on the method without the argument and `name` will default
to the string `"you"`:

```ruby
greet
# => "Good morning you!"
```

In JavaScript, you can make functions behave as though they have default
arguments. Here again is the simple greeting function:

```javascript
function greet(name) {
  return "Good morning " + name + "!";
}
```

To give the variable `name` a default value, you check to see if `name` has been defined on the first line of your function. To do this, you call JavaScript's `typeof` operator, which is a lot like calling `.class` in Ruby. If the function wasn't passed a parameter, then the variable will be `"undefined"`.

```javascript
function greet(name) {
  if (typeof name === "undefined") {
    // add code
  }
  return "Good morning " + name + "!";
}
```

Inside the `if` statement, go ahead and set the default value you desire:

```javascript
function greet(name) {
  if (typeof name === "undefined") {
    name = "you";
  }
  return "Good morning " + name + "!";
}
```

Let's now see our function in action:

```javascript
// Returns "Good morning Blake!"
greet("Blake");

// Returns "Good morning you!"
greet();
```

Unlike Ruby, JavaScript doesn't force you to call a function with a set number of parameters. If a JavaScript function it defined to accept two parameters, and you call the function with only one parameter, the function will still run, placing `undefined` in the spot of the missing paramater:

```js
function greet(name, timeOfDay) {
  return "Good "+ timeOfDay + " "+ name + "!";
}

greet("joe");
// returns "Good undefined joe!"
```

### **ES6 Note**
ECMAScript 6 introduces [default parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
in JavaScript. Their syntax should look familiar:

``` javascript
function greet(name="Sue", timeOfDay="morning") {
    return "Good " + timeOfDay + " " name + "!";
}

// 'Good morning, Sue!'
greet();

// 'Good morning, George!'
greet('George');

// 'Good afternoon, Dory!'
greet('Dory', 'afternoon');
```

### Example

If you'd like more practice with JavaScript functions, this section covers how to rewrite a Ruby method that converts feet to meters in JavaScript. Here's that Ruby method:

```ruby
def convert_to_meters(feet)
  meters = feet / 3.2808
  rounded = meters.round(2)
  "#{rounded} meters"
end
```

Now let's make it a JavaScript function!

The first step is to change the `def` keyword into a `function` keyword. The next step is to change the method's name, which is in snake case, into lower camelCase. The third step will be to change the `end` keyword into an ending curly brace and add a beginning curly brace after the argument, like so:

```javascript
function convertToMeters(feet) {
  // add code
  // add code
  // add code
}
```

The next step is to take the variable, `meters` and preempt it with the keyword `var` so that it's limited in its scope. We'll then end that line of code with a semicolon:

```javascript
function convertToMeters(feet) {
  var meters = feet / 3.2808;
  // add code
  // add code
}
```

Just like we changed the `meters` variable from having a global scope to having a local scope, we'll do the same for the `rounded` variable:

```javascript
function convertToMeters(feet) {
  var meters = feet / 3.2808;
  var rounded = // add code
  // add code
}
```

To round a float in JavaScript, you call `.toFixed()` on the number. If you're operating on the literal number, remember to wrap it in parentheses, but since we're storing it in the variable `num`, we don't have to worry about that. You then pass `.toFixed()` a parameter, the number 2, meaning you would like two digits after the decimal:

```javascript
var num = 3.14159;

// Returns the number 3.14
num.toFixed(2);
```

Let's incorporate the `.toFixed()` function on the second line of our new JavaScript function:

```javascript
function convertToMeters(feet) {
  var meters = feet / 3.2808;
  var rounded = meters.toFixed(2);
  // add code
}
```

Finally, let's concatenate our rounded meter value with the string " meters":

```javascript
function convertToMeters(feet) {
  var meters = feet / 3.2808;
  var rounded = meters.toFixed(2);
  rounded + " meters";
}
```

This looks about right. To see if this function works, we'll call on the function by name, just like in Ruby:

```javascript
// Returns undefined
convertToMeters(16)
```

Alright, so instead of getting our desired result of `"4.88 meters"`, we got `undefined`. As mentioned above, this is because JavaScript functions, unlike Ruby methods, will always return `undefined` unless you use the `return` keyword. Let's add that keyword to the left of the function's last line:

```javascript
function convertToMeters(feet) {
  var meters = feet / 3.2808;
  var rounded = meters.toFixed(2);
  return rounded + " meters";
}
```

And if we try to run our function again, we can see if that did the trick:

```
// Returns "4.88 meters"
convertToMeters(16)
```

### Resources

* [Eloquent JavaScript - Functions](http://eloquentjavascript.net/03_functions.html)
* [MDN - Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)
* [MDN - Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
* [MDN - Default Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)

## Debugging with Mocha
Testing is important no matter what language we're working with. There is always the chance that our code won't behave as expected. Tests and debugging skills help us make sure that our code always works appropriately. JavaScript provides tons of testing libraries; we're going to use [Mocha](http://mochajs.org/). JavaScript comes with a built in debugging tool, called `debugger`.

Note that our test file has the same name as our application file, just with a `'-test'` suffix. This is a convention that we'll see (in various forms) a lot — and it's a good one to follow, as it makes it clear which tests cover which parts of the application.

Take a look at the `index-test.html` file. Near the bottom you'll see this:

```js
<script src="mocha.js"></script>
<script src="https://unpkg.com/expect/umd/expect.min.js"></script>
<script>mocha.setup('bdd');</script>
<script src="../index.js"></script>
<script src="intro-test.js"></script>
<script>
  mocha.run();
</script>
```

What's happening here is that we're loading up all the Javascript files needed to run the test and the we're calling `mocha.run()`. This function will run our tests in the browser for us!

<iframe width="960" height="720" src="https://www.youtube.com/embed/orxiGMn0yCg?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

Let's take a minute to think about how we'd organize our tests. We'd naturally have the tests themselves - but bunching them all together would get real messy real quick. Instead, we can group them together in so-called _suites_. These suites can contain both other suites as well as tests, allowing us to organize things rather neatly.

Mocha exposes a couple of methods to write our tests with. The ones you're going to use most often are `describe()` and `it()`. `describe()` allows us to define a suite. The first argument of `describe()` is the name of our suite. This keeps things grouped together in our testing output.

The `it()` function is an actual test; it takes a string as the first argument about what it is testing and a function that contains an assertion (we're using the [`expect` library](https://github.com/mjackson/expect) here) that compares your code to the expected outcome.

If we need to run certain code before or after our tests run, we can use the `before()` and `after()` functions. Similarly, if we want to do something before or after _every_ test, we can make use of `beforeEach()` and `afterEach()`.

For example, in our test file, we're writing **browser-based tests**. We need to simulate a browser in our testing environment. To do so, we use `jsdom`, which mocks out objects and behaviors as if we were in a browser without actually forcing us to render anything in a browser window. This setup makes our tests portable — that is, it makes it so that they can run just about anywhere.

We set up our fake "browser" in a call to `before()`:

```javascript
global.expect = require('expect');
const jsdom = require('jsdom');
const path = require('path');
// this is setup - require is built into our test environment (running in Node.js; it will not work in your browser).

// The before(), after(), beforeEach() and afterEach(), as well as our test functions, can have an optional first argument. 
// This argument is a function, and is usually called done by convention. This signals to Mocha that what we're doing inside of before() runs asynchronously
// and tells it to wait until we call done() to start running the tests. 
// Notice where we call done() inside of the callback that gets err, window as its arguments.
before(function(done) {
    // we assign the location of the code we want to test to the variable src
    // path.resolve is a part of the Node.js path library for determining the path of something. In this case, it's figuring out where code.js lives.
  const src = path.resolve(__dirname, '..', 'index.js');
  // we compile our code, this helps make our code run easily in multiple environments
  const babelResult = require('babel-core').transformFileSync(src, {
    presets: ['es2015']
  });
  const html = path.resolve(__dirname, '..', 'index.html');

  // jsdom.env function receives four arguments
    // html string sets up the DOM
    // array of paths to source files
    // callback function, in typical Node.js fashion, receives an error first
  jsdom.env(html, [], { src: babelResult.code }, (err, window) => {
  
    // if `err` is defined, we call done(err) to tell Mocha to stop and show us what went wrong
    if (err) {
      return done(err);
    }

    // if everything's fine, we take all of the things defined on window (including, in this lab, the functions we've written) 
    // and add them to global so that we can call them in our tests
    Object.keys(window).forEach(key => {
      global[key] = window[key];
    });

    // we call done() with no arguments to tell Mocha that we're finished with this before(). 
    // The tests start running here.
    return done();
  });
});
```

Let's look at a test more closely:
```javascript
describe('favoriteIceCream', () => {
  it('should return the correct sentence when passed an icecream flavor', () => {
    const result = favoriteIceCream('mint chocolate chip');
    const expectedResult = 'I love mint chocolate chip';
    expect(result).toBe(expectedResult);
  });
});

// could also be written
describe('favoriteIceCream()', function () {
  it('should return the correct sentence when passed an icecream flavor', function () {
    expect(favoriteIceCream('mint chocolate chip')).toEqual('I love mint chocolate chip');
  });
});
```

To run our tests more quickly, we can also simply run `npm test`. And if we'd like to pass options to Mocha — say, for instance, that we'd like to stop running tests after the first failure — we can pass an option like `npm test -- --bail`. The `--` is necessary so that the `npm` passes the `--bail` option to the right process (in this case, to `mocha` (which is in the `test` script) and not `npm` itself). If we'd like to avoid passing arguments at the command line, we can also add them to a `mocha.opts` file in the `test/` directory. For example, `--bail` in `mocha.opts` will use the bail option.

### Resources
- [MDN - Debugger]
- [MDN - Debugging JavaScript]

[MDN - Debugger]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger
[MDN - Debugging JavaScript]: https://developer.mozilla.org/en-US/docs/Mozilla/Debugging/Debugging_JavaScript

## Arrays
JavaScript arrays can contain any types of values and they can be of mixed types, just like in Ruby. You can create arrays in two different ways, the most common of which is to list values in a pair of square brackets. These are called array literals.

Syntax:

```javascript
var arrayName = [element0, element1, ..., elementN];

// examples 
var primeNumbers = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37];

var tvShows = ["game of thrones", "true detective", "the good wife", "empire"];

var weirdGreeting = [ "he", 110, "w", 0, "r", {"1":"d"} ];

var empty = [];
```

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
Ruby has two ways to add elements to the end of the array, `push` and `<<`. You're probably more familiar with the latter which is also known as the shovel method. JavaScript doesn't have the shovel method, but like Ruby, it does feature a push method.
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

### Adding at a specific index
To add elements to an array at specific indexes, you use the bracket equals notation. This notation will also work for replacing values.
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
To remove an element in JavaScript, you can call on the `splice()` function. This function takes two required arguments and some optional arguments:

1. index to start at (required)
2. number of elements to remove (required)
3. an element to add to the array (optional)
4. etc.

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
JavaScript's `forEach` function will help you to iterate through an array. The forEach method executes a provided function once per array element. The first argument that this provided function needs is the variable name for currentValue. In Ruby, this value is typically seen inside of the double pipes (ex. `letters.each do |letter|`).

```javascript
var letters = ["z", "y", "x", "w", "v", "u", "t", "s"];

letters.forEach(function(letter) {
  console.log("♫ " + letter + " ♬");
});
// this will print the following to the console:
// ♫ z ♬
// ♫ y ♬
// ♫ x ♬
// ♫ w ♬
// ♫ v ♬
// ♫ u ♬
// ♫ t ♬
// ♫ s ♬
```

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

Like Ruby's `.each_with_index` method, you can also instruct JavaScript to keep track of the index number of the element it is currently on. To do this, pass the provided function a second argument:

```javascript
var letters = ["z", "y", "x", "w", "v", "u", "t", "s"];

letters.forEach(function(letter, index) {
  var number = index + 1;
  console.log(number + ". " + letter);
});
// this will print the following to the console:
// 1. z
// 2. y
// 3. x
// 4. w
// 5. v
// 6. u
// 7. t
// 8. s
```

We can pass a function as an argument.
That function, in turn, accepts up to three arguments: the current element in the array, the index of that element in the array, and the array itself.
When a language allows functions to be used as arguments, we say that the language treats functions as first-class objects.

```javascript
function doToElementsInArray(array, callback) {
  array.forEach(callback)
}
```

## Hashes in Javascript
In JavaScript, all objects are effectively key-value pairs. We're not going to talk about objects in relation to Object Orientation, but objects as hashes. We will eventually get to objects and properties of objects, but right now we're just focused on how to create, manipulate and delete key-value pairs from a "hash".

JavaScript Objects behave sort of like a cross between Classes and Hashes in Ruby. For now, we're just going to focus on how to use them like a hash. From here on out, we'll be referring to a hash as an object. Every time you see the word "object" think "hash".

You can create an object in two different ways, with the literal syntax and with the new Object constructor.

Literal Syntax:
```js
var meals = {};
```

Object Constructor:
```js
var meals = new Object();
```

You can also create an object with key-value pairs:
```js
var meals = {breakfast: "oatmeal"};
```

Note that JavaScript does not have `=>` syntax. You mark a key with `:` and set the value directly after.

Now that we have an empty object, it's time to start adding key-value pairs:
```js
var meals = {}
meals["breakfast"] = "oatmeal"
meals["lunch"] = "turkey sandwich"
meals["dinner"] = "steak and potatoes"
```

Just like in Ruby, we access the value of an object from its key:
```js
var meals = {breakfast: "oatmeal", lunch: "turkey sandwich", dinner: "steak and potatoes"}
meals["breakfast"] //returns "oatmeal"
meals["lunch"] // returns "turkey sandwich"
meals["dinner"] // returns "steak and potatoes"
```

**Delete a value:** Let's say it's only 5pm and we haven't actually eaten dinner yet, so we want to delete the dinner key-value pair:

```js
var meals = {breakfast: "oatmeal", lunch: "turkey sandwich", dinner: "steak and potatoes"};
delete meals["dinner"];
meals;
//returns {breakfast: "oatmeal", lunch: "turkey sandwich"}
```

**Change a value:** Let's say we actually ate oatmeal and a banana for breakfast, and we want to update the value the `breakfast` key is storing:
```js
var meals = {breakfast: "oatmeal", lunch: "turkey sandwich", dinner: "steak and potatoes"};
meals["breakfast"] = "oatmeal and banana";
meals;
//returns {breakfast: "oatmeal and banana", lunch: "turkey sandwich", dinner: "steak and potatoes"}
```

**Check Empty Object** Unlike Ruby, JavaScript does not have a handy `.empty?` convenience method. But, you can check to see if your object is empty by using `Object.keys(yourObject)` which returns an array of all the keys in your object.

```js
var meals = {breakfast: "oatmeal", lunch: "turkey sandwich", dinner: "steak and potatoes"};
Object.keys(meals);
//returns ["breakfast", "lunch", "dinner"]
```

You can also count the number of key-value pairs by doing something like this:

```js
var meals = {breakfast: "oatmeal", lunch: "turkey sandwich", dinner: "steak and potatoes"};
Object.keys(meals).length;
//returns 3
```

**Iterating over an object:** In order to iterate over an object, we need to use a new loop, the for in loop. We'll stick with the meals object for this example. The for in loop looks something like this:

```js
for (variable in object) {
  // code to be executed goes here
}
```
In this case, we want to iterate over every key value pair, so our variable is `key` and our object is `meals`. JavaScript will automagically take every key in the hash in turn and print out in the console, `"for breakfast I ate oatmeal"`, `"for lunch I ate turkey sandwich"`, `"for dinner I ate steak and potatoes"`.

```js
var meals = {breakfast: "oatmeal", lunch: "turkey sandwich", dinner: "steak and potatoes"};
for (var key in meals) {
  console.log("for " + key + " I ate " + meals[key]);
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

## Scope in Javascript
Scope is the current context of your code, and can be locally or globally defined. Understanding scope is the key to understanding how your variables interact with each other in your code. Without a solid understanding of scope, your variables could be storing entirely different values than you think they are.

By deault here we mean variable scope — what's available in the context of your currently executing code. In Ruby, there are four levels of variable scope — global, class, instance, and local — determined by prefixes on the variable name:

- `$` for global variables
- `@@` for class variables
- `@` for instance variables
- no prefix for local variables

We'll focus on **local variables** for now. In Ruby, a variable's locality is determined by the function context it appears in. So in this example

```ruby
def locality
  here = true
end
```

`here` is local to the method `locality`. Similarly,

```ruby
["bread", "wine", "cheese"].each do |food|
  puts food
end
```

in the above example, `food` is local to the `each` block.

In Ruby, you can only access local variables that have been declared before the currently executing line in the current scope:

```ruby
x = 1
$x = 3
@@x = 4
@x = 5

def my_method
  y = 2
  puts $x
  puts @@x
  puts @x
  puts x
end

my_method

puts y

puts x
```

The method call `my_method` returns the unknown local variable or method error, because `x` is not defined in the scope of that method. However, it successfully prints `$x`, `@@x`, and `@x` because of their broader scopes. (You'll probably see `warning: class variable access from toplevel` for `@@x` — this is expected, and you shouldn't use class variables in this way. We're bending the rules here to highlight how local scope works.)

`puts y` returns the exact same error as `my_method` because `y` was only defined inside the method, not in the global scope. `puts x` does work, because we're printing out `x` in the same scope in which it was created.

**But translate that same code to JavaScript and you get a very different result**:

```js

var x = 1;

function myFunction(){
  var y = 2;
  console.log(x);
}


myFunction();
// prints out 1

console.log(y)
// error - y is not defined

console.log(x)
// prints out 1
```

The function call `myFunction();` actually does work perfectly, even `console.log(x);` This is because the variable `x` is defined in the global scope. Any variable defined in an outer scope is available to inner scopes. (So _everything_ defined in the global scope is available _everywhere_.)

However, anything defined in a function is not accessible outside of it, which is why the `console.log(y);` does not work. `y` was only defined inside the `myFunction` function, making it inaccessible anywhere else.

And again `console.log(x);` works because we're printing it out in the scope in which it was defined.

### The Key To Scope

To determine a variable's scope in JavaScript, ask yourself two questions:

    1. Is it declared inside a function?
    2. Is it declared with the `var` keyword?

If a variable is declared in the outermost scope of a project, it's going to be global no matter what – that is, whether you write `var foo = 'bar'` or `foo = 'bar'`. Remember, saying that a variable is "global" in JavaScript just means that it's a property of the top-most object. (In the browser, that object is `window`; in other environments (like Node.js), it could be `global`.)

Inside a function, variables declared with `var` are only available within that function's scope. Variable's declared without `var` attach themselves to the global object.

Contrast this to Ruby's variable scoping mechanisms: Where Ruby uses prefixes to determine a variable's scope, JavaScript determines scope primarily based on where a variable is declared (and only when declared within a function does any sort of keyword prefix matter).

In our `myFunction` function, the variable `y` was defined with the `var` keyword. If instead, we had done:

```js
function myFunction(){
  y = 2;
  console.log(x);
}
```

We would be able to call `console.log(y)` outside the function as well.

Let's take our whole code example:

```js
var x = 1;

function myFunction(){
  y = 2;
  console.log(x);
}

console.log(y);

console.log(x);
```


If you copy and paste this into the JS Console, you'll get an error with `console.log(y);` At this point in time, even though `y` is a global variable, the variable hasn't been defined. JavaScript has just stored a function called `myFunction`.

If we were to do this:

```js
var x = 1;

function myFunction(){
  y = 2;
  console.log(x);
}

myFunction();

console.log(y);

console.log(x);
```
Now, because of the `myFunction` call, the global variable `y` exists, and `console.log(y)` will work!

### Closure
One of the most powerful things about scope in JavaScript is how easily we can hide variables from the global scope but still make them available in inner scopes.

```js
function outerFunction() {
  var innerVariable = "I'm sort of a secret.";

  return function innerScope() {
    var inaccessible = "Nothing can touch me.";

    return innerVariable;
  }
}
```

JavaScript has first-class functions, meaning that we can pass them around with ease. When we call `outerFunction()`, the returned value is another function.

Let's give it a try:

```js
var myScope = outerFunction();

// the stringified version of `innerScope()`
myScope;
```

Cool! What happens if we try to call `innerScope()` directly?

```js
innerScope();
```

This will throw the error `undefined is not a function` (very helpful, JavaScript). But if we call `myScope` (the variable to which we assigned the output of `outerFunction()`):

```js
myScope();
```

We should see `"I'm sort of a secret."` returned! What happened?

Inside `outerFunction()`, we made the variable `innerVariable` available to `innerScope()` in what we call a **closure**. In this example, `innerScope()` _remembers_ the environment it was created in, and it maintains references to variables declared in that environment in a closure. 

As we noted above, **JavaScript functions have access to their entire outer scope**, so the `innerScope()` function has access to `outerFunction()`'s environment and to the global (`window`) environment.

Note, though, that `outerFunction()` doesn't know anything about what's in `innerScope()` — the variable `inaccessible` is aptly named, because we can't get at its value except inside `innerScope()`.

### Let and Const (ES6)

We've over-simplified the case for JavaScript for the moment. ECMAScript 6 (ES6) introduces two new declarations, `let` and `const`. Scope-wise, the difference is that `let` and `const` have block-level scope whereas `var` only has function-level scope. This means that declaring with `let` and `const` inside, for example, an `if` block will restrict the use of a variable to that `if` block. You can try out the following at https://babeljs.io/repl

```js
if (true) {
  const blockVariable = "block";
  var localVariable = "local";
}

// when you run the following lines, you should see:
// "local"
// blockVariable is not defined
// on the right-hand side of the lower middle part of your screen.
console.log(localVariable);
console.log(blockVariable);
```

Try changing `const` to `let` — you should get the same error output.

## Hoisting
JavaScript has a well-intentioned trick that can often lead to trouble (and nowadays, even when the trick is used well, it can make code less readable). Here's the gist:

``` javascript
// When we declare and assign a variable with `var`

var myVariable = 1;

// JavaScript actually treats it as a separate declaration and assignment:

var myVariable;
myVariable = 1;

// Here's the thing: JavaScript *hoists* variables like this to the _top_ of the file.
// So if we have a long file like this

var doSomethingComplicated = function() {
  // complicated stuff happening in here...
  // takes
  // a
  // lot
  // of
  // lines
  // of
  // code
}

var myVariable = 1;


// JavaScript actually interprets the file like

var doSomethingComplicated, myVariable;

doSomethingComplicated = function() {
  // ...
}

myVariable = 1
```

We call this process **hoisting**. For all intents and purposes, hoisting applies primarily to variables declared with `var` and to function declarations. Variables declared with `let` and `const` are _technically_ hoisted, but they cannot be referenced until they're assigned. MDN offers a great explanation:

> In ECMAScript 2015, `let` [and `const`] will hoist the variable to the top of the block. However, referencing the variable in the block before the variable declaration results in a ReferenceError. The variable is in a "temporal dead zone" from the start of the block until the declaration is processed.
> [MDN - let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)

### Variable Hoisting
While talking about hoisting, it's important to remember that JavaScript has function-level scope, which means that functions have access to variables that are defined outside of it, but any variables defined inside of a function are not accessible to the outside.

```js
function myFunction(){
  console.log(word)
  var word = "cake";
}
```

Before reading on, what do you think the above will print? What did we say happens to variables in the previous section?

Ready?
It would print out `undefined`. That's because variable hoisting just raises the **variable declaration**. JavaScript reads the above code like this:

```js
function myFunction(){
  var word;
  console.log(word);
  word = "cake";
}
```

We see `undefined` printed in the console because JavaScript reads the variable as declared, but not defined.

To put this another way, we can **declare** the variable `word` simply by writing `var word;`. Its initial value is `undefined`. We can then **assign** a value to the variable by writing `word = "cake";`. In this way, JavaScript let's us split _declaration_ and _assignment_ into two different steps:

```js
var word;
word = "cake";
```

Or we can perform them both on the same line:

```js
var word = "cake";
```

Under the hood, JavaScript is always hoisting the _declaration_ to the top of the current scope, and then performing the _assignment_ at the appropriate line.

Let's start with an example of JavaScript accessing a variable inside a function that was defined outside:

```js
var snack = "granola bar";
function eat(){
  console.log(snack);
}
eat();
// prints out "granola bar" and returns undefined
```

Let's modify the function a little bit:
```js
var snack = "granola bar";
function eat(){
  console.log(snack);
  var snack = "snickers";
}
eat();
// prints out undefined, returns undefined
console.log(snack);
// prints out "granola bar"
```

In the example above, we first define the variable `snack` to store `"granola bar"`. Then we define a function `eat`. Inside the function, we `console.log(snack)` (which we would expect to print out `"granola bar"` because of the variable definition outside the function). The following line, we define `snack` to store `"snickers"`.

JavaScript reads our function like this:

```js
function eat(){
  var snack;
  console.log(snack);
  var snack = "snickers";
}
```

So our function will print out to the console `undefined`. Our function returns `undefined` because we have not explicitly given it a return value.

Outside the function, we call `console.log(snack)` which prints out `"granola bar"` because` variables defined inside a function are scoped to that function.

Variable hoisting is important to remember because you want to define all variables at the top of the scope in which you need them. Define all variables at the top of your functions.

### Function Hoisting
#### Function Declarations

Let's take the following example:

```js
eat();

function eat(){
  console.log("time to eat cake!")
}
```

The above JavaScript executes correctly. The function prints out `"time to eat cake!"`. That's because JavaScript hoists the **entire function** to the top of its scope, which in this case is the main scope of the program. JavaScript translate the above code to the following:

```js
function eat(){
  console.log("time to eat cake!")
}
eat();
```

#### Function Expressions
Function expressions are handled differently than function declarations. Let's take an example:

```js
eat();

var eat = function(){
  console.log("time to eat cake!");
}
```

The above code errors with `Uncaught TypeError: eat is not a function`. This is because the variable declaration for `eat` is hoisted, but not its assignment. JavaScript reads the above code like this:

```js
var eat;
eat();
eat = function(){
  console.log("time to eat cake!");
}
```

### Resources

+ [Jon Grover's Hoisting Video](https://www.youtube.com/watch?v=nmlJIOnahBA&list=PLj148bJp5wiwI0291sD0vdXiwn1u6aQkc&index=3)
+ [Adequately Good](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html)
+ [Tuts Plus](http://code.tutsplus.com/tutorials/javascript-hoisting-explained--net-15092)

## JavaScript Comparison Operators
Recall that Comparison operators allow you to test the relationship (equality and identity) between objects.

By now you know about the following comparison operators:

| Operator      | Description   | Example |
| ------------- | ------------- |-------- |
| ==            | checks if value of two operands are equal or not. If yes then returns true  | (10 == 20) is not true; hence returns false |
| !=            |checks if the value of 2 operands are equal. If values are not equal, returns true| (a != b) is true; hence returns true |
| >             | checks if value of left operand is greater than value of right operand. If left operand is greater than right operand, returns true  | (10 > 20) is not true hence returns false         |
| <  | checks if value of left operand is less than value of right operand. If left operand is less than right operand, returns true | (10 < 20) is true        |
| >=   | checks if value of left operand is greater than or equal to value of right operand. If it is, returns true; else returns false  | (10 >= 20 ) is false         |
| <=  | checks if value of left operand is less than or equal to right operand. If left operand is less than or equal to right operand, returns true; else returns false  |  (10 <= 20) is true       |


In JavaScript, in addition to the regular comparison operators, we have **strict comparison operators**; which are strict equals  (`===`) and strict not-equals (`!==`).

**Loose comparison operators** ( `==` and `!=` ) are quite liberal - they convert type to match one another. This means that values may be considered equal even if they are different types, since the operator will force coercion of one or both operators into a single type (usually a number) before performing a comparison.

On the other hand, **strict comparison operators** (`===` and `!==` ) compare both type and value equality.

**YOU ALMOST ALWAYS WANT TO USE `===` or `!==`.**

Relying on JavaScript's implicit type-casting is inviting subtle and hard to track bugs into your code. Use `===` and `!==`. The only exception is when you're dealing with a value that might be either `null` or `undefined` but can also be `0` or `false`. In this case, a simple `if (variable) { ... }` won't work, because all of the above are false-y. But we can check `if (variable == null) { ... }` or `if (variable != null) { ... }` to catch `null` and `undefined` but not `0` and `false`.

* [MDN - Comparison Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)

## Flow control in JS
Flow Control allows the execution of code only under certain conditions. In Ruby, we used if statements, if/else statements, if/elsif/else statements, ternary operators, and case statements to control what code runs when. 

JavaScript has similar methods to control what blocks of code to execute: if statements, if/else statements, if/else if/else statements, ternary operators, and switch statements.

### If Statements

An `if` statement evaluates the code wrapped in parentheses to either `true` or `false`. If `true`, the code block is executed. If `false`, nothing is executed.

JS Syntax:

```javascript
if (conditionToTestIsTrue) {
  // code to be executed here
}
```

### If/Else Statements

You will often see an `if` statement used in combination with an  `else` clause. An `else` clause is a fallback to an `if` statement and will only get executed if the previous `if` statement is false.

```javascript
if (conditionToTestIsTrue) {
  // condition is false hence code is not executed
} else {
  // code to be executed because previous condition is false
}
```

### If/Else If Statements

`if` statements can also be combined with an `else if` clause. This is like an else statement, but with its own condition. It will only run if its condition is true, and the previous statement's condition was false. 

Note: An important thing to keep in mind when transitioning to JavaScript from Ruby is that the `elsif` becomes two full words: `else if`.

```javascript
if (conditionToTestIsTrue){
    // condition is false hence code is not executed
} else if (thisConditionIsTrue) {
  // execute this code if previous statement is false
} else {
  // execute this code if the 2 conditions above are false
}
```

### Ternary Operator

The ternary operator is used as a shortcut for the `if-else` statement. You've probably seen it before in Ruby looking something like this:

```ruby
cart = ["graphic t-shirt", "aluminum water bottle"]

puts cart.empty? ? "Please add something to your cart." : "You're ready to check out." 

# Above prints:
# You're ready to check out.
```

This operator tests a condition; if the condition is true, it returns a certain value, otherwise it returns a different value:

Syntax:

```javascript
conditionToTest ? valueToBeReturnedIfTrue : valueToBeReturnedIfFalse
```

### Switch Statements

Switch statements acts like a big if/else if/else chain. The switch expression is evaluated once and the value of the expression is compared with the values of each case. If there is a match, the associated block of code is executed.

```javascript
switch (expression) {
  case n:
      // code to be executed if case n is true
      break; // break out of switch statement once code executed
  case m:
      // code to be executed if case m is true
      break; // break out of switch statement once code executed
  default:  // all other cases
      // code to be executed if case n and case m false
}
```

Example:
```js
var mood = "hungry"
switch(mood){
  case "happy":
    console.log("Dance to Pharrell's Happy");
    break;
  case "sad":
    console.log("You should eat a pint of ice cream");
    break;
  case "anxious":
    console.log("Take some deep breaths");
    break;
  case "hungry":
    console.log("You should eat a big chocolate cake");
    break;
  default: 
    console.log("That's not a mood we support");
}
```

## Exaple of JS code for shopping cart
```javascript
var cart = [];

function setCart(newCart) {
  cart = newCart;
}

function total() {
  let t = 0
  for (var i = 0, l = cart.length; i < l; i++) {
    for (var item in cart[i]) {
      t += cart[i][item]
    }
  }
  return t
}

function getCart() {
  return cart
}

// add the item and the price as an object ({{item: price}) to the cart array.
function addToCart(item) {
  var price = Math.floor(Math.random() * 100);
  // should I use ${} for pushing the object?
  cart.push({[item]: price});
  console.log(`${item} has been added to your cart.`);
  return cart
}

function viewCart() {
  var list = "";
  if (cart.length === 0) {
    console.log("Your shopping cart is empty.")
  } else {
    let i = 0;

    while ( i < cart.length ) {
      for (var item in cart[i]) {
        list +=  `${item} at $${cart[i][item]}, `;
      }
      i += 1
    }
    list = list.slice(0, -2)
    console.log(`In your cart, you have ${list}.`)
  }
}

function removeFromCart(item) {
  if (cart.length === 0) {
    console.log("That item is not in your cart.")
  } else {
    for (var i = 0; i < cart.length; i++) {
      if (cart[i].hasOwnProperty(item)) {
        cart.splice(i, 1)
        return cart
      } else {
        console.log("That item is not in your cart.")
      }
    }
  }
}

function placeOrder(cardNbr) {
  if (cardNbr !== undefined) {
    console.log(`Your total cost is $${total()}, which will be charged to the card ${cardNbr}.`);
    cart = [];
  } else {
    console.log("We don't have a credit card on file for you to place your order.")
  }
}
```

