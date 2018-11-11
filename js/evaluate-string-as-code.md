### eval
```
const x = 1;  
function evalAndReturnX(code) {  
  eval(code);  
  return x;  
}

console.log(evalAndReturnX("var x = 2"));  
// → 2  
console.log(x);  
// → 1
```

### Function constructor
parameters: a string containing a comma-separated list of argument names and a string containing the function body
```
let plusOne = Function("n", "return n + 1;");  
console.log(plusOne(4));  
// → 5
```
