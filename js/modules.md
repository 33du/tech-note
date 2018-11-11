## CommonJS (before ES6)
```
// Shapes.js  
class Color ...

module.exports = Color;
```

```
var Color = require('./Shapes');
```

### Export multiple components
```
class Color ...  
class Animal ...

module.exports = {  
  Color: Color,  
  Animal: Animal  
}
```

```
var {Color} = require('./Shapes');  
var {Animal} = require('./Shapes');
```

### Export a single function
```
// format-date.js  
exports.formatDate = function(date, format) {};
```

```
const {formatDate} = require("./format-date");  
formatDate(...);
```

## ES modules (ES6)
```
import ordinal from "ordinal";  
import {days, months} from "date-names";  
import {days as dayNames} from "date-names";

export function formatDate(date, format) { ...   
export class Color extends Component { ...  
export let x = ...
```

#### export default
when a module exports only one component
```
// my-module.js  
export default function cube(x) {}  

import myFunction from 'my-module';
```
