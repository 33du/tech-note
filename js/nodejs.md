### Process global binding
process.exit(0)

process.argv
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
start a server:
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
