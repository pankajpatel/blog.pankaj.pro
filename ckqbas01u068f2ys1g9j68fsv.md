## Today's npm package: color-alpha

Today's npm package is [color-alpha](https://www.npm/js.com/package/color-alpha)

Color Alpha will let you customize the Alpha i.e. opacity of the colour in JS

Very useful in cases when 
→ Adjusting minimal inline styles
→ Working with CSS-in-JS
→ Maintaining a Design System
→ Creating and Maintaining Style Guides
→ PostProcssing CSS Files

Here is how you can use it:

```js
const A = require('color-alpha');

console.log(A('red', .35)) // "rgba(255,0,0,0.35)"
console.log(A('#f00', .35)) // "rgba(255,0,0,0.35)"
console.log(A('#ff0000', .35)) // "rgba(255,0,0,0.35)"
console.log(A('rgb(255, 0, 0)', .35)) // "rgba(255,0,0,0.35)"
console.log(A('rgba(255, 0, 0, 1)', .35)) // "rgba(255,0,0,0.35)"
```

See it in action here: https://runkit.com/pankaj/color-alpha

%[https://runkit.com/pankaj/color-alpha]

Stay tuned for tomorrow's package of the Day.