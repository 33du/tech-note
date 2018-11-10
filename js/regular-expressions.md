Create a regular expression:  
- let re = new **RegExp**("abc");
- let re = /abc/; (must use backslash to escape some characters with special meaning if they are meant to represent the character itself)

**test**: checks whether the parameter contains a match of the pattern in the expression 
```
/abc/.test("abcde"); → true  
/abc/.test("abxde"); → false
```

### Syntax
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
