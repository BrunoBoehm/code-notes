# PHP


## Displaying values
Use echo and print or the shorthand method (if supported by your server - php 5.4).

```
<?php
  $variable = "booh";
  $name = "John";
  $surname = "Machiato";
?>

/* in the HTML */

<?php echo $variable; ?>
<?php print $variable; ?>

<?= $variable; ?>

<?= $name . " " . $surname; ?>
<?= "$name $surname"; ?>
/* this will not work with single quotes ' '  */
```
