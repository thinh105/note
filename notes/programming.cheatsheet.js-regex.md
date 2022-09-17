---
id: a7ky3unbi7k8yuf507t2a7j
title: JS Regex
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
latitude: 10.8142
longitude: 106.6438
altitude: 0
title_imported: JS Regex
updated_imported: '2020-04-30T04:52:35.000Z'
created_imported: '2020-02-19T11:29:14.000Z'
---

[TO C]

# Methods
## `RegEx.test(testString)` Method =>  True/False

Return `True`/`False` when the string contain the RegEx or not.

```js
let testStr = "freeCodeCamp";
let testRegex = /Code/;
testRegex.test(testStr); // Returns true

let testStr = "Hello, my name is Kevin.";
let testRegex = /Kevin/;
testRegex.test(testStr); // Returns true

let wrongRegex = /kevin/;
wrongRegex.test(testStr); // Returns false
```

## `match()` method.

```js
"Hello, World!".match(/Hello/); // Returns ["Hello"]

let ourStr = "Regular expressions";
let ourRegex = /expressions/;

ourStr.match(ourRegex); // Returns ["expressions"]
```

Note that the `.match` syntax is the "opposite" of the `.test` method

```js
'string'.match(/regex/);
/regex/.test('string');
```



# Syntax 
## Regular Expression Basics
```js
.	// Any character except newline

a|b 	// a or b

let petString = "James has a pet cat.";
let petRegex = /dog|cat|bird|fish/;
let result = petRegex.test(petString); // True


a*	// 0 or more a's

[ab-d1-5]	// One character of: a, b, c, d,1,2,3,4,5
[^ab-d]     // One character except: a, b, c, d

^	// Start of string
$	// End of string

\w	// One alpha-numeric character | equal to [A-Za-z0-9_]
\W	// One non-alpha-numeric character | equal to [^A-Za-z0-9_]

\d	// One digit
\D	// One non-digit

\s	// One whitespace
\S	// One non-whitespace

\'number'	// Match the Y'th captured group | \1 \2 \3 ...
$'number'  // Insert Y'th captured group | $1 $2 $3 ...
```

## Regular Expression Flags
```js
g	// Global Match


i	// Ignore case

/ignorecase/i. => "ignorecase", "igNoreCase", and "IgnoreCase".

let myString = "freeCodeCamp";
let fccRegex = /freecodecamp/i;
let result = fccRegex.test(myString); // true



m	// ^ and $ match start and end of line
```

## Regular Expression Quantifiers
```
*	// 0 or more
+	// 1 or more
?	// 0 or 1

{2}	// Exactly 2
{2, 5}	// Between 2 and 5
{2,}	// 2 or more
```

Default is greedy matching - finds the longest possible part.

Append `?` for reluctant or lazy matching - finds the smallest possible part.

ex: `.*?` meaning the smallest possible part

## Regular Expression Groups
```
(...)	// Capturing group
(?:...)	// Non-capturing group
```

## Regular Expression Character Classes

```
[\b]	// Backspace character

```
## Regular Expression Assertions
```
\b	// Word boundary
\B	// Non-word boundary
(?=...)	// Positive lookahead
(?!...)	// Negative lookahead
```

## Regular Expression Special Characters
```
\n	// Newline
\r	// Carriage return
\t	// Tab
\0	// Null character
\YYY	// Octal character YYY
\xYY	// Hexadecimal character YY
\uYYYY	// Hexadecimal character YYYY
\cY	// Control character Y
```
## Regular Expression Replacement
```
$$	// Inserts $
$&	// nsert entire match
$`	// Insert preceding string
$'	// Insert following string

```


# Example

You need to check all the usernames in a database. Here are some simple rules that users have to follow when creating their username.

1) Usernames can only use alpha-numeric characters.

2) The only numbers in the username have to be at the end. There can be zero or more of them at the end. Username cannot start with the number.

3) Username letters can be lowercase and uppercase.

4) Usernames have to be at least two characters long. A two-character username can only use alphabet letters as characters.

```
Test case:
Your regex should match JACK
Your regex should not match J
Your regex should match Jo
Your regex should match Oceans11
Your regex should match RegexGuru
Your regex should not match 007
Your regex should not match 9
Your regex should not match A1
Your regex should not match BadUs3rnam3
Your regex should match Z97
Your regex should not match c57bT3
```

```js
let username = "JackOfAllTrades";

let userCheck = /^[a-z](\d\d+|[a-z]+\d*)$/i; 

let result = userCheck.test(username);
```





