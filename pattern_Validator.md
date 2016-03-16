

# Description #

The **Pattern** validator is used when you want to restrict input to a pattern of letters or numbers, e.g. for phone numbers, SKUs, IP addresses and so forth.  A simple format is provided, or developer users can use [regular expressions](http://php.net/manual/en/function.preg-match.php).

# Options #

  * **Pattern**: For simple patterns, specify symbols for each character, e.g. `#` for numbers, `*` for letters (see below).
  * **Use  preg\_match**: if checked, you must supply a valid regular expression pattern.
  * **Message**: Write a custom message here that shows when a field fails validation.  Because your pattern may be testing for vastly different things, it's necessary to offer a way to customize the error message.  You may use "%s" as a placeholder for the field name.

If you are using simple patterns (preg\_match _not_ checked), then the following characters can define your pattern:

  * `#` : Matches any number (0-9)
  * `*` : Matches any letter (A-Z).  This is not case sensitive.  This does not match UTF-8 characters.
  * `?` : Matches any single character.

The purpose of this validator is not to reinvent regular expression syntax: for anything more complex than a character-for-character mapping, you must use a valid regex pattern.

# Examples #

## Example 1: Phone Number (simple) ##

  * **Pattern:** `###-###-####`
  * **preg\_match**: unchecked.

Note that this pattern would not allow for extensions or any other variation. `1-555-555-1212` or `(555)-555-1212` would both be considered invalid.

## Example 2: SKU (simple) ##

Some businesses use alphanumeric codes to identify products.  As long as they are always the same length, you may use a simple pattern to identify it.

## Example 3: Message (simple) ##

Sometimes, you may want users to type in a message verbatim, e.g. giving their consent for something.  Here's how you could require a user to type in the phrase "I agree":

  * **Pattern:** `I agree`
  * **preg\_match**: unchecked.
  * **Message:** `You must type "I agree" before continuing.`

## Example 4: Phone Number (advanced) ##

If you want to allow different formats for phone numbers, you must use a regular expression.  Let's say you want to accept the following formats:

  * `1-888-555-1212`
  * `888.555.1212`
  * `888-555-1212x123`

  * **Pattern:** `/^([1][\-\.])?[0-9]{3}[\-\.][0-9]{3}[\-\.][0-9]{4}$/i`
  * **preg\_match**: Checked.

Here's how you could modify the pattern to accept an optional 1-4 digit extension:

  * **Pattern:** `/^([1][\-\.])?[0-9]{3}[\-\.][0-9]{3}[\-\.][0-9]{4}([x][0-9]{1,4})?$/i`


## Example 5: IP Address (advanced) ##

Since IP addresses are not always represented with the same length of characters, it's necessary to use a regular expression .

  * **Pattern:** `/^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$/`
  * **preg\_match**: Checked.

Note this regex isn't perfect: `888.888.888.888` would match the pattern, but it is not a valid IPv4 address.