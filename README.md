# Regular Expressions

A __regular expression__ (sometimes called a _rational expression_) is a sequence of characters that define a search pattern, mainly for use in pattern matching with strings, or string matching, i.e. `find and replace` like operations. The concept arose in the 1950s, when the American mathematician Stephen Kleene formalized the description of a regular language, and came into common use with the Unix text processing utilities ed, an editor, and grep, a filter.

## Online editors

[Regex101](https://regex101.com/)
[RegExr](http://regexr.com/)


## Chars that must be escaped

```
^  $  (  )  <  [  {  \  |  >  .  *  +  ?
```

## Anchors

```
\   Escape
^   Start of subject
$   End of subject
|   Start of alternative branch
```

## Ranges

```
.      Any character (except newline)
()     Group, sub pattern
(?:)   Passive group (non-capturing)
       These are groups that are ignored for the purposes of replacement.
       Useful when you want to match something that requires an "or" section, but don't want it in the replacement.
[]     Char class definition
       This allow you to specify the range of valid characters to compare. If any of them exists, the condition is true.
^      negates (if is first char)
-      indicates char range
```

### Examples

```
(a|b)   a or b
[abc]   Range (a or b or c)
[^abc]  Not a or b or c
[a-q]   Letter between a and q
[A-Q]   Upper case letter between A and Q
[0-7]   Digit between 0 and 7
\n      nth group/subpattern
```

## Quantifiers

```
?      0 or 1
*      0 or more
+      1 or more
{}     min/max
*      is equal to {0,}
+      is equal to {1,}
?      is equal to {0,1}
{3}    Exactly 3
{3,}   3 or more
{3,5}  3, 4 or 5
??     Ungreedy (lazy) 1 or 0
*?     Ungreedy (lazy) more or 0
+?     Ungreedy (lazy) more or 1

```

### Examples

```
{3,5}   greedy will try the matches trying longest first (5 then 4 and then 3).
{3,5}?  ungreedy will try the matches trying shortest first (3 then 4 and then 5).
```

## Character classes

```
\c     Control character
\d     Any decimal digit
\D     Any char not a decimal digit
\s     Any whitespace char
\S     Any not whitespace char
\w     Any word character (letter, number, underscore)
\W     Any "non-word" character
\xhh   Hexadecimal char
\Oxxx  Octal character xxx
       \d is equal to [0-9]
       \D is equal to [^0-9]
       \s is equal to [\t\n\r]
       \S is equal to [^\t\n\r]
       \w is equal to [0-9A-Za-z]
       \W is equal to [^0-9A-Za-z]
```

## Assertions

```
?=   Lookahead assertion
?!   Negative lookahead
?<=  Lookbehind assertion
?!=  Negative lookbehind
?<!  Negative lookbehind
?>   Once-only Subexp­ression
?()  Condition [if then]
?()| Condition [if then else]
?#   Comment
```

## Pattern modifiers

```
g   Global match
i   Case-insensitive
m   Multiple lines
s   Treat string as single line
x   Allow comments and white space in pattern
e   Evaluate replacement
U   Ungreedy pattern
```

## Strings extracts

```
$n   nth non-passive group
$2   "xyz" in /^(abc(xyz))$/
$1   "xyz" in /^(?:abc)(xyz)$/
$`   Before matched string
$'   After matched string
$+   Last matched string
$&   Entire matched string
$_   Entire input string
$$   Literal "$"
```

## Backreference

If you want to reference a previous group in a regular expression, you would use `\n`, where "n" is the number of the group. You might need a pattern to match `aaa` or `bbb`, followed by numbers, followed by the same 3 letters, and this would be done with groups, like so:

```
(aaa|bbb)[0-9]+\1
```

The above matches `aaa or bbb`, and groups the match with the brackets. This is followed by a pattern for one or more numbers `("[0-9]+")`, then finally `\1`. The `\1` backreferences the first group, and looks for the same thing. It will match the matched text from the string, not the pattern, so `aaa123bbb` will not match the above pattern, as the `\1` will be looking for `aaa` to follow the numbers.

```
(aaa|bbb)(cc)[0-9]+\2
```

This will match strings like:

```
aaacc125cc
bbbcc89cc
```

## Greediness

Greediness affects the order in which the engine performs the search and the contents of the captures if there are capturing groups. By default, the regular expression repetition operators such as `*`, `+` and friends are greedy. Hence, they will try to match as much as possible of the string as they can. If you wish them to match as little as possible, append a question mark (`?`) after them.

```
{3,5}     Greedy. Will try the matches trying longest first (5 then 4 and then 3).
{3,5}?    Ungreedy. Will try the matches trying shortest first (3 then 4 and then 5).
(a{3,5})(\w*)   for "aaaaabc" will capture aaaaa and bc
(a{3,5}?)(\w*)  for "aaaaabc" will capture aaa and aabc
```

Quantifier are `greedy` by default. So the quantifier `+`, which means `one or more`, will match as many items as possible. This can be a problem on occasion, so you can tell a quantifier to not be greedy (to be lazy), using a modifier.

The pattern `".*"` will match text contained in quotation marks. However, you may have a string like this:

```
<a href="helloworld.htm" title="Hello World">Hello World</a>
```

In this string the pattern above will match the following:

```
"helloworld.htm" title="Hello World"
```

It has been too greedy, matching as much text as it could.

The pattern `".*?"` will also match any characters contained in quotation marks. The non-greedy version (note the "?" modifier) will match as little as possible of the string, so will match each item in quotation marks separately:

```
"helloworld.htm"
"Hello World"
```
