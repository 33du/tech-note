Create a regular expression:  
- let re = new **RegExp**("abc");
- let re = /abc/; (must use backslash to escape some characters with special meaning if they are meant to represent the character itself)

### Syntax
#### Single character
\[0123456789\] or \[0-9\] or **\d**: any digit character  
**\w**	An alphanumeric character (“word character”)  
**\s**	Any whitespace character (space, tab, newline, and similar)  
**\D**	A character that is not a digit  
**\W**	A nonalphanumeric character  
**\S**	A nonwhitespace character  
**.**	Any character except for newline

```
let dateTime = /\d\d-\d\d-\d\d\d\d \d\d:\d\d/;  
dateTime.test("01-30-2003 15:20"); // → true
```

\[\d.\]: any digit or a period character (period character loses here its special meaning)

**Invert**: caret(^) after \[  
\[^01\]: any character except 0 or 1

#### A sequence of characters
**+** after an element: it may be repeated more than once  
**\***: repeated 0 to many times  
**?**: 0 or 1 time
```
/'\d+'/.test("'123'"); → true  
/'\d+'/.test("''"); → false  
/'\d*'/.test("''"); → true  
/neighbou?r/.test("neighbor"); → true
```

**Indicate repeat times after an element**:  
{4}: 4 times  
{2,4}: 2 <= 4  
{3,}: >= 3

**Use operator on more elements**: parenthese  
(boo)+

**case insensitive**: /.../**i**

**caret(^) and dollar($)**: matches start and end of the input string instead of part of the string  
/^\d+$/ matches a string consisting entirely of one or more digits, /^!/ matches any string that starts with an exclamation mark

**\b**: enforces word boundary
```
/cat/.test("concatenate"); // → true  
/\bcat\b/.test("concatenate"); // → false
```

**pipe character (|)**: or  
(pig|cow|chicken)

### Methods
#### test
checks whether the parameter contains a match of the pattern in the expression 
```
/abc/.test("abcde"); → true  
/abc/.test("abxde"); → false
```

#### exec
return null if no match was found and return an object with information about the match otherwise
```
let match = /\d+/.exec("one two 100");  
console.log(match); // → ["100"]  
console.log(match.index); // → 8
```

First whole match, then match the group in parentheses:
```
let quotedText = /'([^']*)'/;
quotedText.exec("she said 'hello'"); // → ["'hello'", "hello"]
```

```
function getDate(string) {  
  let [_, month, day, year] =  
    /(\d{1,2})-(\d{1,2})-(\d{4})/.exec(string);  
  return new Date(year, month - 1, day);  
}  
console.log(getDate("1-30-2003"));  
// → Thu Jan 30 2003 00:00:00 GMT+0100 (CET)
```

#### replace
string.replace(first, second): replace first with second, first can also be regular expression, replace the first match in string -> /../**g**: global, replace all matches in string
