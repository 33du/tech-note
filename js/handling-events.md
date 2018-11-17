### Add event listener
```
window.addEventListener("click", () => {  
    console.log("You knocked?");  
});
```
Can also apply to nodes

Similar -> node.**onclick**  
but one node can only have one onclick method

button.**removeEventListener**("click", originalFuncName);

### Event objects
argument passed to event handler function

```
  button.addEventListener("mousedown", event => {  
    if (event.button == 0) {  
      console.log("Left button");  
    } else if (event.button == 1) {  
      console.log("Middle button");  
    } else if (event.button == 2) {  
      console.log("Right button");  
    }  
  });
```

### Propagation
every (indirect) parent of the node also gets the event, but the most specific listener gets it first  
-> event.**stopPropagation** to stop propagation further up

event.**target**: returns the originating node
```
<button>A</button>  
<button>B</button>  
<button>C</button>  
<script>  
  document.body.addEventListener("click", event => {  
    if (event.target.nodeName == "BUTTON") {  
      console.log("Clicked", event.target.textContent);  
    }  
  });
</script>
```

### Default action
click a link, scroll down page, ...  
-> event handler can call event.**preventDefault**(); (event handlers are called before default actions)

### Load event
When a page finishes loading, the **load** event fires on the window and the document body objects

```
  window.addEventListener("load", function(event) {  
    console.log("All resources finished loading!");  
  });
```

Elements such as images and script tags that load an external file also have a "load" event that indicates the files they reference were loaded.

**beforeunload** event: fires when a page is closed  
-> return non-null from handler: browser will show the user a dialog to confirm leaving the page

### Web workers
do sth time-consuming in the background

```
// code/squareworker.js  
addEventListener("message", event => {  
  postMessage(event.data * event.data);  
});

let squareWorker = new Worker("code/squareworker.js");  
squareWorker.addEventListener("message", event => {  
  console.log("The worker responded:", event.data);  
});  
squareWorker.postMessage(10); 
```

workers do not share global scope
-> **postMessage**: causes a "message" event for receiver, send value as JSON

### Denoucing
events can fire many times in a row: "mousemove", "scroll", ...   
-> use **setTimeout** to not handle it too often

```
<textarea>Type something here...</textarea>  
<script>  
  let textarea = document.querySelector("textarea");  
  let timeout;  
  textarea.addEventListener("input", () => {  
    clearTimeout(timeout);  
    timeout = setTimeout(() => console.log("Typed!"), 500);  
  });  
</script>
```
