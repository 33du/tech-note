**private**: _privateProperty

### Methods
~ properties holding function values
```
let rabbit = {};  
rabbit.speak = function(line) {  
  console.log(`The ${this.type} rabbit says '${line}'`);  
};
```

Arrow function can use **this** of the outer scope, function defined with "function" cannot.

### Prototype ~ class
Object.getPrototypeOf({}) == Object.prototype   
Object.getPrototypeOf(Object.prototype) == null  
Object.getPrototypeOf(Math.max) == Function.prototype  
Object.getPrototypeOf(\[\]) == Array.prototype

create an object(instance) with a specific prototype: **Object.create**
```
let protoRabbit = {  
  speak(line) {  
    console.log(`The ${this.type} rabbit says '${line}'`);  
  }  
};  
let killerRabbit = Object.create(protoRabbit);
```

**Constructor**:
```
function makeRabbit(type) {  
  let rabbit = Object.create(protoRabbit);  
  rabbit.type = type;  
  return rabbit;  
}
```

Or use **new**:
```
function Rabbit(type) {  
  this.type = type;  
}  
Rabbit.prototype.speak = function(line) {  
  console.log(`The ${this.type} rabbit says '${line}'`);  
};  

let weirdRabbit = new Rabbit("weird");
```
Rabbit.prototype: prototype of the instance created through it, not prototype of the function itself
