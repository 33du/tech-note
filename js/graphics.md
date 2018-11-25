## SVG
```
<svg xmlns="http://www.w3.org/2000/svg">  
  <circle r="50" cx="50" cy="50" fill="red"/>  
  <rect x="120" y="5" width="90" height="90"  
        stroke="blue" fill="none"/>  
</svg>  

<script>  
  let circle = document.querySelector("circle");  
  circle.setAttribute("fill", "cyan");  
</script>  
```

## Canvas
```
<canvas width="120" height="60"></canvas>  

<script>  
  let canvas = document.querySelector("canvas");  
  let context = canvas.getContext("2d");  
  context.fillStyle = "red";  
  context.fillRect(10, 10, 100, 50);  
  
  context.strokeStyle = "blue";
  context.lineWidth = 5;
  context.strokeRect(135, 5, 50, 50);
</script>
```
Context: **2d** or **webgl** for 3D

### load image
```
<canvas></canvas>  
<script>  
  let cx = document.querySelector("canvas").getContext("2d");  
  let img = document.createElement("img");  
  img.src = "img/hat.png";  
  img.addEventListener("load", () => {  
      cx.drawImage(img, x, 10);  
  });  
</script>
```
