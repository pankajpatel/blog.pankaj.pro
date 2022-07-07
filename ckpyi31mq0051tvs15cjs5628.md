## Today's npm package: javascript-stringify

Today's npm package is [javascript-stringify](https://www.npm/js.com/package/javascript-stringify) 

You can use this package to convert JavaScript to string so that it can be evaluated later with `eval`

```js
const { stringify } = require("javascript-stringify");

const square = (x) => Math.pow(x, 2);

const stringifiedSquare = stringify( square )

console.log(typeof stringifiedSquare, stringifiedSquare)
```

![javascript-stringify](https://cdn.hashnode.com/res/hashnode/image/upload/v1623788914556/15UUG7a-6.png)

See it in action here: https://runkit.com/pankaj/javascript-stringify

%[https://runkit.com/pankaj/javascript-stringify]

Stay tuned for tomorrow's package of the Day.