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

### Array and associative arrays
An array stores multiple values in one single variable.

```php
<?php 
$cars = array("Volvo","BMW","Toyota");
var_dump($cars);
// array(3) { [0]=> string(5) "Volvo" [1]=> string(3) "BMW" [2]=> string(6) "Toyota" }
?>

<?php
$cars = array("Volvo", "BMW", "Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
?>

<?php
$cars = array("Volvo", "BMW", "Toyota");
echo count($cars);
?>

<?php
$cars = array("Volvo", "BMW", "Toyota");
$arrlength = count($cars);

for($x = 0; $x < $arrlength; $x++) {
    echo $cars[$x];
    echo "<br>";
}
?>
```

Associative arrays
```php
<?php
$age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");
echo "Peter is " . $age['Peter'] . " years old.";
?>

<?php
$age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");

foreach($age as $x => $x_value) {
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
}
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

### Sorting arrays
PHP has the following array sort functions:

- sort() - sort arrays in ascending order
- rsort() - sort arrays in descending order
- asort() - sort associative arrays in ascending order, according to the value
- ksort() - sort associative arrays in ascending order, according to the key
- arsort() - sort associative arrays in descending order, according to the value
- krsort() - sort associative arrays in descending order, according to the key

```php
<?php
$cars = array("Volvo", "BMW", "Toyota");
rsort($cars);
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

## OOP
### Magic constants
There are nine magical constants that change depending on where they are used.
All these "magical" constants are resolved at compile time, unlike regular constants thats resolved at runtime. These special constants are case-insensitive and are as follows:

Name | Description
--- | ---
__LINE__	| The current line number of the file.
__FILE__	| The full path and filename of the file with symlinks resolved. If used inside an include, the name of the included file is returned.
__DIR__	| The directory of the file. If used inside an include, the directory of the included file is returned. This is equivalent to dirname(__FILE__). This directory name does not have a trailing slash unless it is the root directory.
__FUNCTION__	| The function name.
__CLASS__	| The class name. The class name includes the namespace it was declared in (e.g. Foo\Bar). Note that as of PHP 5.4 __CLASS__ | works also in traits. When used in a trait method, __CLASS__ is the name of the class the trait is used in.
__TRAIT__	| The trait name. The trait name includes the namespace it was declared in (e.g. Foo\Bar).
__METHOD__	| The class method name.
__NAMESPACE__	| The name of the current namespace.
ClassName::class	| The fully qualified class name.

```php
<?php
 
class MyClass
{
  public $prop1 = "I'm a class property!";
 
  public function __construct()
  {
      echo 'The class "', __CLASS__, '" was initiated!<br />';
  }
 
  public function __destruct()
  {
      echo 'The class "', __CLASS__, '" was destroyed.<br />';
  }
 
  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }
 
  public function getProperty()
  {
      return $this->prop1 . "<br />";
  }
}
 
// Create a new object
$obj = new MyClass;
 
// Get the value of $prop1
echo $obj->getProperty();
 
// Output a message at the end of the file
echo "End of file.<br />";

# The class "MyClass" was initiated!
# I'm a class property!
# End of file.
# The class "MyClass" was destroyed.
?>
```
When the end of a file is reached, PHP automatically releases all resources. To explicitly trigger the destructor, you can destroy the object using the function `unset($obj);`.

### Methods visibility
For added control over objects, methods and properties are assigned visibility. This controls how and from where properties and methods can be accessed. There are three visibility keywords: `public`, `protected`, and `private`. In addition to its visibility, a method or property can be declared as `static`, which allows them to be accessed without an instantiation of the class.

#### Protected
When a property or method is declared `protected`, it can only be accessed **within the class itself or in descendant classes** (classes that extend the class containing the protected method).

```php
<?php
 
class MyClass
{
  public $prop1 = "I'm a class property!";
 
  public function __construct()
  {
      echo 'The class "', __CLASS__, '" was initiated!<br />';
  }
 
  public function __destruct()
  {
      echo 'The class "', __CLASS__, '" was destroyed.<br />';
  }
 
  public function __toString()
  {
      echo "Using the toString method: ";
      return $this->getProperty();
  }
 
  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }
 
  protected function getProperty()
  {
      return $this->prop1 . "<br />";
  }
}
 
class MyOtherClass extends MyClass
{
  public function __construct()
  {
      parent::__construct();
echo "A new constructor in " . __CLASS__ . ".<br />";
  }
 
  public function newMethod()
  {
      echo "From a new method in " . __CLASS__ . ".<br />";
  }
}
 
// Create a new object
$newobj = new MyOtherClass;
 
// Attempt to call a protected method
echo $newobj->getProperty();
 
 
# The class "MyClass" was initiated!
# A new constructor in MyOtherClass. 
# Fatal error: Call to protected method MyClass::getProperty() from context '' in /Applications/XAMPP/xamppfiles/htdocs/testing/test.php on line 55 
?>
```
Now let's call it from inside the class

```php
<?php
 
class MyClass
{
  public $prop1 = "I'm a class property!";
 
  public function __construct()
  {
      echo 'The class "', __CLASS__, '" was initiated!<br />';
  }
 
  public function __destruct()
  {
      echo 'The class "', __CLASS__, '" was destroyed.<br />';
  }
 
  public function __toString()
  {
      echo "Using the toString method: ";
      return $this->getProperty();
  }
 
  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }
 
  protected function getProperty()
  {
      return $this->prop1 . "<br />";
  }
}
 
class MyOtherClass extends MyClass
{
  public function __construct()
  {
      parent::__construct();
echo "A new constructor in " . __CLASS__ . ".<br />";
  }
 
  public function newMethod()
  {
      echo "From a new method in " . __CLASS__ . ".<br />";
  }
 
  public function callProtected()
  {
      return $this->getProperty();
  }
}
 
// Create a new object
$newobj = new MyOtherClass;
 
// Call the protected method from within a public method
echo $newobj->callProtected();
 
?>
```

#### Private
A property or method declared `private` is accessible **only from within the class that defines it**. This means that even if a new class extends the class that defines a private property, that property or method will not be available at all within the child class.

#### Static
A method or property declared `static` can be accessed without first instantiating the class; you simply supply the class name, scope resolution operator, and the property or method name.

```php
<?php
 
class MyClass
{
  public $prop1 = "I'm a class property!";
 
  public static $count = 0;
 
  public function __construct()
  {
      echo 'The class "', __CLASS__, '" was initiated!<br />';
  }
 
  public function __destruct()
  {
      echo 'The class "', __CLASS__, '" was destroyed.<br />';
  }
 
  public function __toString()
  {
      echo "Using the toString method: ";
      return $this->getProperty();
  }
 
  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }
 
  private function getProperty()
  {
      return $this->prop1 . "<br />";
  }
 
  public static function plusOne()
  {
      return "The count is " . ++self::$count . ".<br />";
  }
}
 
class MyOtherClass extends MyClass
{
  public function __construct()
  {
      parent::__construct();
      echo "A new constructor in " . __CLASS__ . ".<br />";
  }
 
  public function newMethod()
  {
      echo "From a new method in " . __CLASS__ . ".<br />";
  }
 
  public function callProtected()
  {
      return $this->getProperty();
  }
}
 
do
{
  // Call plusOne without instantiating MyClass
  echo MyClass::plusOne();
} while ( MyClass::$count < 10 );
 
?>
```
When accessing static properties, the dollar sign ($) comes after the scope resolution operator.

## Comparing Procedural and OOP in PHP
### Procedural
```php
<?php
 
function changeJob($person, $newjob)
{
  $person['job'] = $newjob; // Change the person's job
  return $person;
}
 
function happyBirthday($person)
{
  ++$person['age']; // Add 1 to the person's age
  return $person;
}
 
$person1 = array(
  'name' => 'Tom',
  'job' => 'Button-Pusher',
  'age' => 34
);
 
$person2 = array(
  'name' => 'John',
  'job' => 'Lever-Puller',
  'age' => 41
);
 
// Output the starting values for the people
echo "<pre>Person 1: ", print_r($person1, TRUE), "</pre>";
echo "<pre>Person 2: ", print_r($person2, TRUE), "</pre>";
 
// Tom got a promotion and had a birthday
$person1 = changeJob($person1, 'Box-Mover');
$person1 = happyBirthday($person1);
 
// John just had a birthday
$person2 = happyBirthday($person2);
 
// Output the new values for the people
echo "<pre>Person 1: ", print_r($person1, TRUE), "</pre>";
echo "<pre>Person 2: ", print_r($person2, TRUE), "</pre>";
 
?>
```

### OOP
There's a little bit more setup involved to make the approach object oriented, but after the class is defined, creating and modifying people is a breeze; a person's information does not need to be passed or returned from methods, and only absolutely essential information is passed to each method.

```php
<?php
 
class Person
{
  private $_name;
  private $_job;
  private $_age;
 
  public function __construct($name, $job, $age)
  {
      $this->_name = $name;
      $this->_job = $job;
      $this->_age = $age;
  }
 
  public function changeJob($newjob)
  {
      $this->_job = $newjob;
  }
 
  public function happyBirthday()
  {
      ++$this->_age;
  }
}
 
// Create two new people
$person1 = new Person("Tom", "Button-Pusher", 34);
$person2 = new Person("John", "Lever Puller", 41);
 
// Output their starting point
echo "<pre>Person 1: ", print_r($person1, TRUE), "</pre>";
echo "<pre>Person 2: ", print_r($person2, TRUE), "</pre>";
 
// Give Tom a promotion and a birthday
$person1->changeJob("Box-Mover");
$person1->happyBirthday();
 
// John just gets a year older
$person2->happyBirthday();
 
// Output the ending values
echo "<pre>Person 1: ", print_r($person1, TRUE), "</pre>";
echo "<pre>Person 2: ", print_r($person2, TRUE), "</pre>";
 
?>
```

## Commenting with DocBlocks
While not an official part of OOP, the DocBlock commenting style is a widely accepted method of documenting classes. Aside from providing a standard for developers to use when writing code, it has also been adopted by many of the most popular software development kits (SDKs), such as Eclipse and NetBeans, and will be used to generate code hints.

The real power of DocBlocks comes with the ability to use tags, which start with an at symbol (@) immediately followed by the tag name and the value of the tag. DocBlock tags allow developers to define authors of a file, the license for a class, the property or method information, and other useful information.

The most common tags used follow:

- @author: The author of the current element (which might be a class, file, method, or any bit of code) are listed using this tag. Multiple author tags can be used in the same DocBlock if more than one author is credited. The format for the author name is John Doe <john.doe@email.com>.
- @copyright: This signifies the copyright year and name of the copyright holder for the current element. The format is 2010 Copyright Holder.
- @license: This links to the license for the current element. The format for the license information is http://www.example.com/path/to/license.txt License Name.
- @var: This holds the type and description of a variable or class property. The format is type element description.
- @param: This tag shows the type and description of a function or method parameter. The format is type $element_name element description.
- @return: The type and description of the return value of a function or method are provided in this tag. The format is type return element description.

## Variables Scope
The [scope](http://php.net/manual/en/language.variables.scope.php) of a variable is the context within which it is defined. For the most part all PHP variables only have a single scope. **Any variable used inside a function is by default limited to the local function scope.**

```php
<?php
$a = 1; /* global scope */ 

function test()
{ 
    echo $a; /* reference to local scope variable */ 
} 

test();
?>
```

This script will not produce any output because the echo statement refers to a local version of the $a variable, and it has not been assigned a value within this scope. You may notice that this is a little bit different from the C language in that global variables in C are automatically available to functions unless specifically overridden by a local definition. This can cause some problems in that people may inadvertently change a global variable. **In PHP global variables must be declared global inside a function if they are going to be used in that function.**

```php
<?php
$a = 1;
$b = 2;

function Sum()
{
    global $a, $b;

    $b = $a + $b;
} 

Sum();
echo $b;
?>
```
By declaring $a and $b global within the function, all references to either variable will refer to the global version.

A second way to access variables from the global scope is to use the special PHP-defined `$GLOBALS` array. The previous example can be rewritten as:
```php
<?php
$a = 1;
$b = 2;

function Sum()
{
    $GLOBALS['b'] = $GLOBALS['a'] + $GLOBALS['b'];
} 

Sum();
echo $b;
?>
```
