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
