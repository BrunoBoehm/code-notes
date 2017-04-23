# PHP

## Hello World
```html
<!DOCTYPE html>
<html>
<body>

<h1>My first PHP page</h1>

<?php
echo "Hello World!";
?>

<?php
// This is a single-line comment

# This is also a single-line comment

/*
This is a multiple-lines comment block
that spans over multiple
lines
*/

// You can also use comments to leave out parts of a code line
$x = 5 /* + 15 */ + 5;
echo $x;
?>

</body>
</html>
```

## Setting Variables
```php
<?php
$txt = "Hello world!";
$x = 5;
$y = 10.5;
?>
```

## Displaying values
Use echo and print or the shorthand method (if supported by your server - php 5.4).

```php
<?php
  $variable = "booh";
  $name = "John";
  $surname = "Machiato";
?>

/* in the HTML */

<?php echo strtoupper($variable); // string to upper function ?>
<?php print $variable; ?>

<?= $variable; # just outputting a value ?>

<?= $name . " " . $surname; ?>
<?= "$name $surname";
/* this will not work 
with single quotes ' '  */
?>

<?php
$txt1 = "Learn PHP";
$txt2 = "W3Schools.com";
$x = 5;
$y = 4;

echo "<h2>$txt1</h2>";
echo "Study PHP at $txt2<br>";
echo $x + $y;
?>
```

## Variable types
### Strings
A string is a sequence of characters, like "Hello world!". A string can be any text inside quotes. You can use single or double quotes:
```php
<?php 
$x = "Hello world!";
$y = 'Hello world!';

echo $x;
echo "<br>"; 
echo $y;

echo str_word_count("Hello world!"); // outputs 2
echo strlen("Hello world!"); // outputs 12
echo strrev("Hello world!"); // outputs !dlrow olleH
echo strpos("Hello world!", "world"); // outputs 6
echo str_replace("world", "Dolly", "Hello world!"); // outputs Hello Dolly!
?>
```
More functions on [W3C string reference](https://www.w3schools.com/php/php_ref_string.asp).

### Integer
An integer data type is a non-decimal number between -2,147,483,648 and 2,147,483,647. An integer must have at least one digit. An integer must not have a decimal point. An integer can be either positive or negative. Integers can be specified in three formats: decimal (10-based), hexadecimal (16-based - prefixed with 0x) or octal (8-based - prefixed with 0)

```php
<?php 
$x = 5985;
var_dump($x);
// var_dump() function returns the data type and value
?>
```

### Float
A float (floating point number) is a number with a decimal point or a number in exponential form.

```php
<?php 
$x = 10.365;
var_dump($x);
?>
```

### Boolean
A Boolean represents two possible states: TRUE or FALSE.

```php
$x = true;
$y = false;
```

### Array
An array stores multiple values in one single variable.

```php
<?php 
$cars = array("Volvo","BMW","Toyota");
var_dump($cars);
// array(3) { [0]=> string(5) "Volvo" [1]=> string(3) "BMW" [2]=> string(6) "Toyota" }
?>
```

### Object
An object is a data type which stores data and information on how to process that data. In PHP, an object must be explicitly declared. First we must declare a class of object. For this, we use the class keyword. A class is a structure that can contain properties and methods:

```php
<?php
class Car {
    function Car() {
        $this->model = "VW";
    }
}

// create an object
$herbie = new Car();

// show object properties
echo $herbie->model;
?>
```

### NULL
Null is a special data type which can have only one value: NULL. A variable of data type NULL is a variable that has no value assigned to it. If a variable is created without a value, it is automatically assigned a value of NULL. Variables can also be emptied by setting the value to NULL:

```php
<?php
$x = "Hello world!";
$x = null;
var_dump($x);
// NULL
?>
```

### Constants
A constant is an identifier (name) for a simple value. The value cannot be changed during the script. A valid constant name starts with a letter or underscore (no $ sign before the constant name). Unlike variables, constants are automatically global across the entire script.

```php
<?php
// define(name, value, case-insensitive)
define("GREETING", "Welcome to W3Schools.com!");
echo GREETING;

define("GREETING", "Welcome to W3Schools.com!", true);
echo greeting;
?>

<?php
// constants are global
define("GREETING", "Welcome to W3Schools.com!");

function myTest() {
    echo GREETING;
}
 
myTest();
?>
```

## Manipulating data
### If Else
In PHP we have the following conditional statements:

- if statement - executes some code if one condition is true
- if...else statement - executes some code if a condition is true and another code if that condition is false
- if...elseif....else statement - executes different codes for more than two conditions
- switch statement - selects one of many blocks of code to be executed

```php
<?php
$t = date("H");

if ($t < "20") {
    echo "Have a good day!";
}
?>

<?php
$t = date("H");

if ($t < "20") {
    echo "Have a good day!";
} else {
    echo "Have a good night!";
}
?>

<?php
$t = date("H");

if ($t < "10") {
    echo "Have a good morning!";
} elseif ($t < "20") {
    echo "Have a good day!";
} else {
    echo "Have a good night!";
}
?>
```

### Switch
Use the switch statement to select one of many blocks of code to be executed.

```php
<?php
$favcolor = "red";

switch ($favcolor) {
    case "red":
        echo "Your favorite color is red!";
        break;
    case "blue":
        echo "Your favorite color is blue!";
        break;
    case "green":
        echo "Your favorite color is green!";
        break;
    default:
        echo "Your favorite color is neither red, blue, nor green!";
}
?>
```

### Loops
In PHP, we have the following looping statements:

- while - loops through a block of code as long as the specified condition is true
- do...while - loops through a block of code once, and then repeats the loop as long as the specified condition is true
- for - loops through a block of code a specified number of times
- foreach - loops through a block of code for each element in an array

```php
<?php 
$x = 1; 

while($x <= 5) {
    echo "The number is: $x <br>";
    $x++;
} 
# The number is: 1 
# The number is: 2 
# The number is: 3 
# The number is: 4 
# The number is: 5 
?>

<?php 
$x = 1; 

do {
    echo "The number is: $x <br>";
    $x++;
} while ($x <= 5);
# The number is: 6
?>

<?php 
for ($x = 0; $x <= 10; $x++) {
    echo "The number is: $x <br>";
} 
?>

<?php 
$colors = array("red", "green", "blue", "yellow"); 

foreach ($colors as $value) {
    echo "$value <br>";
}
?>
```

## Functions
Besides the built-in PHP functions, we can create our own functions. A function is a block of statements that can be used repeatedly in a program. A function will not execute immediately when a page loads. A function will be executed by a call to the function.

```php
<?php
function writeMsg() {
    echo "Hello world!";
}

writeMsg(); // call the function
?>

<?php
function familyName($fname) {
    echo "$fname Refsnes.<br>";
}

familyName("Jani");
familyName("Hege");
familyName("Stale");
familyName("Kai Jim");
familyName("Borge");
?>

<?php
function familyName($fname, $year) {
    echo "$fname Refsnes. Born in $year <br>";
}

familyName("Hege", "1975");
familyName("Stale", "1978");
familyName("Kai Jim", "1983");
?>

<?php
function setHeight($minheight = 50) {
    echo "The height is : $minheight <br>";
}

setHeight(350);
setHeight(); // will use the default value of 50
setHeight(135);
setHeight(80);
?>

<?php
function sum($x, $y) {
    $z = $x + $y;
    return $z;
}

echo "5 + 10 = " . sum(5, 10) . "<br>";
echo "7 + 13 = " . sum(7, 13) . "<br>";
echo "2 + 4 = " . sum(2, 4);
?>
```

## Using server-side includes
```php
<?php include 'includes/external.php';  ?>
```
PHP has 4 include functions `include`, `include_once`, `require` and `require_once`. Redefining a function or class, generates a fatal error in PHP. So the once versions are intended to prevent that from happening in a complex programming environment.
Require has a sense of being mandatory. If the file can't be found the script goes no further. But if your using include the rest of the script is executed even if the include file can't be found.

PHP uses a relative path to find the include. If the document is in the same folder, use `./`.
