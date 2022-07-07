## Today's npm package: objectorarray

Today's npm package is [objectorarray](https://www.npm/js.com/package/objectorarray)

This package can help you quickly checking if the variable is an Object or Array.

Of course there are direct and vanilla-js ways to check if any value is an object or array.

But having a centralized package to avoid writing long comparison is easier to keep the project more readable and sane.

```js
const isObjectOrArray = require('objectorarray');

console.log(isObjectOrArray(null)) // false
console.log(isObjectOrArray(false)) // false
console.log(isObjectOrArray([])) // true
console.log(isObjectOrArray({})) // true
console.log(isObjectOrArray(() => {})) // false
console.log(isObjectOrArray(new Date)) // true
```

Stay tuned for tomorrow's package of the Day.
