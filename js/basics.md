console.log(output);  
let number = Number(prompt("Pick a number")); -> open a dialog

### Data types
**typeof** can return: string, number, boolean, undefined(~null), function, object  
array -> object

**string**: \ to escape  
"half of 100 is ${100 / 2}": ${} is a template literal, its result will be computed, converted to a string, and included at that position

undefined == null  
undefined !== null  
NaN != NaN

**==**: automatic type conversion

**||**: return left value when that can be converted to true, return right value otherwise  
null || "user" -> "user"  
**&&**: return left value when false

### Variable definition
**var**: global/local scope, variable hoisted to the top (only declarations are hoisted: var x, not initializations: x = 5)  
**let**, **const**: block scope (inside {}), not hoisted

### Function
- const square = function(x) {}; //square is function name  
- function square(x) {}
- const square = (x) => {return x * x;};  
  if only one parameter and single expression: const square = x => x * x;
  
```
function multiplier(factor) {  
  return number => number * factor;  
}

let twice = multiplier(2);  
console.log(twice(5));  
```

pass too many arguments to a function: extra ones ignored  
pass too few: missing arguments get assigened undefined
**Optional argument**: function power(base, exponent = 2) {}

### Object 
access a property:  
obj.prop  
obj\[prop\]: prop here can be a variable, number, or strings like "aaa bbb"

**string properties**: length, toUpperCase  
**array methods**: push, pop, shift(pop from end), unshift(push to begin)
