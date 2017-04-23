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
?>
```

### Integer
An integer data type is a non-decimal number between -2,147,483,648 and 2,147,483,647. An integer must have at least one digit. An integer must not have a decimal point. An integer can be either positive or negative. Integers can be specified in three formats: decimal (10-based), hexadecimal (16-based - prefixed with 0x) or octal (8-based - prefixed with 0)

```php
<?php 
$x = 5985;
var_dump($x);
// var_dump() function returns the data type and value
?>
```

## Using server-side includes
```php
<?php include 'includes/external.php';  ?>
```
PHP has 4 include functions `include`, `include_once`, `require` and `require_once`. Redefining a function or class, generates a fatal error in PHP. So the once versions are intended to prevent that from happening in a complex programming environment.
Require has a sense of being mandatory. If the file can't be found the script goes no further. But if your using include the rest of the script is executed even if the include file can't be found.

PHP uses a relative path to find the include. If the document is in the same folder, use `./`.
