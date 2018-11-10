### Strict mode
put **"use strict"** top of a file or a function body

-> report error when:  
- use a variable/object without declaration
- delete a variable/object/function
- duplicate a parameter name
- write to read-only property or getter
- 'this' holding the value undefined in functions that are not called as methods
```
"use strict";  
function Person(name) { this.name = name; }  
let ferdinand = Person("Ferdinand"); // forgot new  
// → TypeError: Cannot set property 'name' of undefined
```
(constructors created with the class notation will always complain if they are called without new)

### Testing
```
function test(label, body) {  
  if (!body()) console.log(`Failed: ${label}`);  
}  

test("convert Latin text to uppercase", () => {  
  return "hello".toUpperCase() == "HELLO";  
});
```

### Breakpoint
- set breakpoint in browser's debugger tool
- include **debugger;** statement and open debugger tool

### Exception
```
function promptDirection(question) {  
  let result = prompt(question);  
  if (result.toLowerCase() == "left") return "L";  
  if (result.toLowerCase() == "right") return "R";  
  throw new Error("Invalid direction: " + result);  
}

try {  
  console.log("You turn", promptDirection());  
} catch (error) {  
  console.log("Something went wrong: " + error);  
} finally {  
}
```

#### Define new error type
```
class InputError extends Error {}

function promptDirection(question) {  
  throw new InputError("Invalid direction: " + result);  
}

try {  
} catch (e) {  
  if (e instanceof InputError) {  
    console.log("Not a valid direction. Try again.");  
  } else {  
    throw e;  
  }  
}
```
