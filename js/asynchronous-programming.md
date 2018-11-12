### Callback function
an extra argument passed to a slow action, when the action finishes, the callback function is called with its result
```
setTimeout(() => console.log("Tick"), 500);
```

### Promises
A promise is an asynchronous action that may complete at some point and produce a value. It is able to notify anyone who is interested when its value is available.

```
var promise = new Promise(function(resolve, reject) {  
  // do a thing, possibly async, then…

  if (/* everything turned out fine */) {  
    resolve("Stuff worked!");  
  }  
  else {  
    reject(Error("It broke"));  
  }  
});
```

```
promise.then(function(result) {  
  console.log(result); // "Stuff worked!"  
  // return ...  
}, function(reason) {  
  console.log(reason); // Error: "It broke"  
  // return ...
});
```

Promise constructor: execute immediately the function(**executor**) passed to it  
if an error is thrown in executor, the promise is rejected

resolve, reject: callback functions  

then: takes two optional callbacks, one for success and one for failure (handler for rejection can also be passed to **catch**)
  -> returns a new promise, can again attach a then() and "result" is return value from last then()  
  
"then" always run after the current script finishes, even if the promise is already resolved:
```
Promise.resolve("Done").then(console.log);  
console.log("Me first!");  
// → Me first!  
// → Done
```
  
**Promise.resolve**(value):  
return a promise resolved with given value  
value can also be a **thenable** object: { then: function(onFulfill, onReject) { onFulfill('fulfilled!'); }}

**Promise.all**(iterable):  
returns a promise that either fulfills when all of the promises in the iterable argument have fulfilled or rejects as soon as one of the promises in the iterable argument rejects  
  result: array of all values from fulfilled promises or reason from first rejected promise
  
### Async function
a more synchronous way to do asynchronous computation
```
function resolveAfter2Seconds() {  
  return new Promise(resolve => {  
    setTimeout(() => {  
      resolve('resolved');  
    }, 2000);  
  });  
}

async function asyncCall() {  
  var result = await resolveAfter2Seconds();  
  console.log(result);  
  // expected output: 'resolved'  
}

asyncCall();
```

Not the same with promise.then():
```
function resolveAfter2Seconds() {}  
function resolveAfter1Second() {}

var sequentialStart = async function() {  
  // If the value of the expression following the await operator is not a Promise, it's converted to a resolved Promise.  
  const slow = await resolveAfter2Seconds();  
  const fast = await resolveAfter1Second();  
  
  console.log(slow);  
  console.log(fast);  
}

var concurrentStart = async function() {  
  const slow = resolveAfter2Seconds();  
  const fast = resolveAfter1Second();

  console.log(await slow);  
  console.log(await fast); // waits for slow to finish, even though fast is already done!  
}

var parallel = function() {  
  resolveAfter2Seconds().then((message)=>console.log(message));  
  resolveAfter1Second().then((message)=>console.log(message)); // comes first
}
