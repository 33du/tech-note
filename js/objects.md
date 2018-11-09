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
**Object.create(null)**: no prototype, without property like toString

**Object.keys** returns only an object’s own keys, not those in the prototype.  
**in** also checks prototype properties !-> **hasOwnProperty**

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

### Modern class notation
```
class Rabbit {  
  constructor(type) {  
    this.type = type;  
  }  
  speak(line) {  
    console.log(`The ${this.type} rabbit says '${line}'`);  
  }  
}  

let killerRabbit = new Rabbit("killer");
```

Or:
```
let object = new class { getWord() { return "hello"; } };
```

### Symbol as property
Symbol cannot be created twice:  
let sym = Symbol("name");  
sym != Symbol("name"));

```
const toStringSymbol = Symbol("toString");  
Array.prototype[toStringSymbol] = function() {  
  return `${this.length} cm of blue yarn`;  
};

console.log([1, 2].toString());  
// → 1,2  
console.log([1, 2][toStringSymbol]());  
// → 2 cm of blue yarn
```

Or:
```
let stringObject = {  
  [toStringSymbol]() { return "a jute rope"; }  
};
```

### Iterator (for/of loop)
use **Symbol.iterator** to get the iterator of an object:   
```
let okIterator = "OK"[Symbol.iterator]();  
console.log(okIterator.next());  
// → {value: "O", done: false}  
console.log(okIterator.next());  
// → {value: "K", done: false}  
console.log(okIterator.next());  
// → {value: undefined, done: true}
```

### Getter, setter and static
```
class Temperature {  
  constructor(celsius) {  
    this.celsius = celsius;  
  }  
  get fahrenheit() {  
    return this.celsius * 1.8 + 32;  
  }  
  set fahrenheit(value) {  
    this.celsius = (value - 32) / 1.8;  
  }  

  static fromFahrenheit(value) {  
    return new Temperature((value - 32) / 1.8);  
  }  
}

let temp = new Temperature(22);  
console.log(temp.fahrenheit); // → 71.6  
temp.fahrenheit = 86;  
console.log(temp.celsius); // → 30

Temperature.fromFahrenheit(100);
```

### Inheritance
```
class SymmetricMatrix extends Matrix {  
  constructor(size, element = (x, y) => undefined) {  
    super(size, size, (x, y) => {  
      if (x < y) return element(y, x);  
      else return element(x, y);  
    });  
  }  
}
```

new SymmetricMatrix(2) **instanceof** Matrix
