# PHP


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
```

