console.log(output);  
let number = Number(prompt("Pick a number")); -> open a dialog

### Data types
**typeof** can return: string, number, boolean, undefined(~null), function, object  
array -> object

**string**: \ to escape  
\`half of 100 is ${100 / 2}\`: ${} is a template literal enclosed by the back-tick (\` \`), its result will be computed, converted to a string, and included at that position

string.**charCodeAt**(index) -> code of a unit  
string.**codePointAt**(index) -> code of a full unicode character (one or two unit)

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

Any number of arguments:  
function max(...numbers) {} //put numbers in an array  
-> call the function:  
max(4, 1, 9, -2) or  
let numbers = \[5, 1, 7\]; max(...numbers));

function max(\[num1, num2, num3\]) {}

**function inside a function**:  
```
function buildGraph(edges) {  
  let graph = Object.create(null);  
  function addEdge(from, to) { }  
  for (let [from, to] of edges.map(r => r.split("-"))) {  
    addEdge(from, to);  
    addEdge(to, from);  
  }  
  return graph;  
}
```

### Object 
(Array is object)

```
let day1 = {  
  squirrel: false,  
  events: ["work", "touched tree", "pizza", "running"],  
  "touched tree": "Touched a tree"  
};
```

access a property:  
obj.prop  
obj\[prop\]: prop here can be a variable, number, or strings like "aaa bbb"

pass an object:  
{events} if only "events" needed  
or {squirrel, events}

**delete** obj.prop;  
"prop" **in** obj -> false  
**Object.keys**(obj) -> array of property names

**const** object: must point to same object, but contents of the object can change  
**Object.freeze**(object): make object immutable

**string methods**: length, toUpperCase, slice, indexOf(can search for a string), trim(remove whitespace from start and end), padStart, split, join, repeat  
**array methods**: push, pop, shift(pop from end), unshift(push to begin), includes, indexOf(only search for an element), lastIndexOf, slice(get part of the array)

**Iterate over array**: for (let element of array)

**Array.from()**: make an array out of an array-like object  
string.map(l => \[...l\]): turn a string to array  
**...**: spread operator, seperating elements

### JSON
Serialize data

JSON.stringify(object) -> string  
JSON.parse(string) -> object

### Math
0 <= Math.random() < 1  
Math.floor: round down  
Math.ceil, Math.round
