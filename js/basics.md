console.log(output);
let number = Number(prompt("Pick a number")); -> open a dialog

### Data types
**typeof** can return: string, number, boolean, undefined(~null), function, object  
array -> object

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
- const square = function(x) {};  
- function square(x) {}
- const square = (x) => {return x * x;};  
  if only one parameter and single expression: const square = x => x * x;
