### Function as argument
```
function repeat(n, action) {  
  for (let i = 0; i < n; i++) {  
    action(i);  
  }  
}

repeat(3, console.log);
```

or:

```
let labels = [];  
repeat(5, i => {  
  labels.push(`Unit ${i + 1}`);  
});
```

### Function as return value
```
function greaterThan(n) {  
  return m => m > n;  
}  
let greaterThan10 = greaterThan(10);  
greaterThan10(11); // → true
```

```
function noisy(f) {  
  return (...args) => {  
    let result = f(...args);  
    return result;  
  };  
}  
noisy(Math.min)(3, 2, 1); // -> f = Math.min, return 1
```

### Array method for higher-order function
\["A", "B"\].**forEach**(l => console.log(l));  

array.**filter**(test); -> return a new array containing elements where test(element) == true  
array.filter(s => s.direction == "ttb")

array.**map**(s => s.name); -> apply to every element and return a new array

**reduce/fold**:  
array.reduce(combine, start = array\[0\])  
\[1, 2, 3, 4\].reduce((a, b) => a + b) -> 10

array.**some**(test) -> true if test(element) = true for any element  
array.**every**(test)

array.**findIndex**(c => c.name == name); -> -1 if no such element
