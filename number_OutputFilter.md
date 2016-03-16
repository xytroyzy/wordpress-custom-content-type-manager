

Added in 0.9.7.1

# Description #

The **number** Output Filter formats numbers into a nicely readable formats, e.g. for currency.  It uses the PHP [number\_format()](http://php.net/manual/en/function.number-format.php)


# Usage #

Generic invocation of any Output Filter occurs via the [get\_custom\_field()](TemplateFunctions#get_custom_field.md) function:

_mixed_ **get\_custom\_field**( string _$input_ `[`, mixed _$options_ `]` )

  * **$input**: _mixed_: a string, or an array of strings, each representing an amount of money.
  * **$options**: _string_.  an optional pipe-separated string containing the number of decimals (0), the thousands separator (,), and the decimal separator (.).  The default is `0|,|.` -- other common formats may include `2|.|,` for European currency, or dollar-amounts with with 2 decimals: `2|,|.`
  * **OUTPUT**: _string_.  The generated number (money).

## Example 1 ##

```
<?php
print_custom_field('amount:number');
// e.g. 2,000
// or 
print '$'. get_custom_field('amount:number');
// e.g. $2,000
?>
```

## Example 2 ##

Show an amount with 2 decimal places, in a tpl

```
My formatting template includes a placeholder for [+amount:number==2|,|.+]
// e.g. 2,000.00
?>
```

## Example 3 ##

In Euros, 2 decimal places, using a period (.) as the thousands-separator, and a comma (,) as the decimal separator.

```
<?php
print '&euro;'. get_custom_field('amount:money', '2|.|,');
// e.g. €2.000,00
?>
```