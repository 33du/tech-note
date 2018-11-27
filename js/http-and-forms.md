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
