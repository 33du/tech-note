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
