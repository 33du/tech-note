### Fetch - make HTTP request (without reloading page)
-> returns a promise resolving to a **Response** object
```
fetch("example/data.txt").then(response => {  
  console.log(response.status);
  // → 200
  console.log(response.headers.get("Content-Type"));
  // → text/plain
});
```

Resolves as soon as header received -> reading response body also returns a promise
```
fetch("example/data.txt")
  .then(resp => resp.text())
  .then(text => console.log(text));
```

Default method: **GET**  
With extra options and other method:
```
fetch("example/data.txt", {method: "POST", headers: {Range: "bytes=8-19"}})
  .then(resp => resp.text())
  .then(console.log);
```

### Communication between client and server
- remote procedure calls: client calls a server function like addUser
- resource: PUT request to /users/larry

## Form
```
<form action="example/submit.html">
  Name: <input type="text" name="name"><br>
  Password: <input type="password" name="password"><br>
  <button type="submit">Log in</button>
</form>

<script>
  let form = document.querySelector("form");
  console.log(form.elements[1].type); // → password
  console.log(form.elements.password.type); // → password
  console.log(form.elements.name.form == form); // → true
</script>
```
**elements**: acts both as an array-like object and a map

#### Event:
**"submit"**  
**"change"**: fires when the field loses focus after its content changed  
**"input"**: fires immediately

### Focus
get active element:
```
<input type="text">
<script>
  document.querySelector("input").focus();
  console.log(document.activeElement.tagName); // → INPUT
  document.querySelector("input").blur();
  console.log(document.activeElement.tagName); // → BODY
</script>
```

### Read in a file
can access the type, size, name of the file directly, reading the content is more complecated with a **FileReader** object and "load" event:
```
<input type="file" multiple>
<script>
  let input = document.querySelector("input");
  input.addEventListener("change", () => {
    for (let file of Array.from(input.files)) {
      if (file.type) console.log("File type:", file.type);
      
      let reader = new FileReader();
      reader.addEventListener("load", () => {
        console.log("File", file.name, "starts with",
                    reader.result.slice(0, 20));
      });
      reader.readAsText(file);
    }
  });
</script>
```

Catch error:
```
function readFileText(file) {
  return new Promise((resolve, reject) => {
    let reader = new FileReader();
    reader.addEventListener(
      "load", () => resolve(reader.result));
    reader.addEventListener(
      "error", () => reject(reader.error));
    reader.readAsText(file);
  });
}
```

## Storing data client-side
**localStorage** object: store strings under names (site-specific)
```
localStorage.setItem("username", "marijn");
console.log(localStorage.getItem("username")); // → marijn
localStorage.removeItem("username");
```
Can use **JSON.stringify** ...
