### Process global binding
**process.exit(0)**

**process.stdout.write()**: writable stream  
similar to console.log, but without new line

**process.argv**
```
//showargv.js
console.log(process.argv);

$ node showargv.js one --and two
["node", "/tmp/showargv.js", "one", "--and", "two"]
```

## Modules -> CommonJS

### NPM
install packages in **/node_modules** local or global  
show the current position with 
```
$ npm list [-g]
```

**package.json**: list all dependencies  
```
$ npm install // -> install all packages in package.json
```

### File system module (fs)
```
let {readFile} = require("fs");
readFile("file.txt", "utf8", (error, text) => {
  if (error) throw error;
  console.log("The file contains:", text);
});
```
without **encoding** argument: returns a **Buffer** object instead of string

```
const {writeFile} = require("fs");
writeFile("graffiti.txt", "Node was here", err => {
  if (err) console.log(`Failed to write file: ${err}`);
  else console.log("File written.");
});
```

use **promise**:
```
const {readFile} = require("fs").promises;
readFile("file.txt", "utf8")
  .then(text => console.log("The file contains:", text));
```

**synchronous** version:
```
const {readFileSync} = require("fs");
console.log("The file contains:",
            readFileSync("file.txt", "utf8"));
```

### HTTP module
**server**:
```
const {createServer} = require("http");
let server = createServer((request, response) => {
  response.writeHead(200, {"Content-Type": "text/html"});
  response.write(`
    <h1>Hello!</h1>
    <p>You asked for <code>${request.url}</code></p>`);
  response.end();
});
server.listen(8000);
```
-> http://localhost:8000/

**client**:
```
const {request} = require("http");
let requestStream = request({
  hostname: "eloquentjavascript.net",
  path: "/20_node.html",
  method: "GET",
  headers: {Accept: "text/html"}
}, response => {
  console.log("Server responded with status code",
              response.statusCode);
});
// requestStream.write(...);
requestStream.end();
```

**reading from stream**(request, response): with event handler (**on** ~ addEventListener)  
events: "**data**" -> fires every time data comes in, "**end**"

```
const {createServer} = require("http");
createServer((request, response) => {
  response.writeHead(200, {"Content-Type": "text/plain"});
  request.on("data", chunk =>
    response.write(chunk.toString().toUpperCase()));
  request.on("end", () => response.end());
}).listen(8000);
```
chunk is buffer object and can be converted with toString()

```
const {request} = require("http");
request({
  hostname: "localhost",
  port: 8000,
  method: "POST"
}, response => {
  response.on("data", chunk =>
    process.stdout.write(chunk.toString()));
}).end("Hello server");
// → HELLO SERVER
```
