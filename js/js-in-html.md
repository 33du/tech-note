### Insert js script
```
<script src="/code/hello.js"></script>
```

Or:
```
<button onclick="alert('Boom!');">DO NOT PRESS</button>
```

### Escape in html
ampersand (**&**) character + semicolon  

**<**: \&lt;  
**>**: \&gt;  
**"**: \&quot;  
**&**: \&amp;

can use in attribute, ...

## Document Object Model (DOM)
HTML data structure **(tree)** with every tag represents an object

Access these objects: **document**  
properties: **documentElement** -> \<html> (root of the tree), head, body

### Node properties
nodeType = Node.ELEMENT_NODE(like <p> or <div>), Node.TEXT_NODE, Node.COMMENT_NODE, ...  
  
#### Traverse: 
parentNode, childNodes, firstChild, lastChild, previousSibling, nextSibling, children(contains only element children)  

text node: nodeValue 

element node: **getElementsByTagName**("a")\[0\] returns all (indirect) child nodes with given tag as array-like object  
**getElementsByClassName**  
**getElementById**

#### Change the document
remove, appendChild, insertBefore, replaceChild

#### create nodes
document.createTextNode(string)  
document.createElement(tagName)

getAttribute, setAttribute: for unstandard attributes (standard attributes can be accessed directly by node's property)  
only "class" is accessed by node.className

### Layout
**offsetWidth, offsetHeight**: element size in pixels
**clientWidth, clientHeight**: size inside border  
**getBoundingClientRect**: size of an element and its position relative to the viewport (returns an object with top, bottom, left, and right properties)  
**pageXOffset, pageYOffset**: current position
