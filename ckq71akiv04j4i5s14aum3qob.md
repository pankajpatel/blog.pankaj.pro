## Today's npm package: strip-comments

Today's npm package is [strip-comments](https://www.npm/js.com/package/strip-comments)

If you are working with a large codebase and want to apply a code mod, this package can be very useful.

Or before you ship your application to the production, you might wanna remove the comments from your codebase and this package can be very useful.

Leet's see how to use it. Consider following the `index.js` file from https://blog.pankaj.pro/todays-npm-package-jsdoc-to-markdown where we have discussed how to use JSDoc to generate Documentation in Markdown files.

```js
// index.js

/**
 * Function to generate the Square of a Number
 *
 * @param {Number} x - The number to be squared
 * @return {Number} Squared value of passed Number
 */
const square = (x) => {
  return Math.pow(x, 2);
};

export default square;
```

For above file, we can use `strip-comments` as follows:

```js
const fs = require('fs');
const path = require('path');
const strip = require('strip-comments');

const file = fs.readFileSync(
  path.join(process.cwd(), 'index.js')
);

const str = strip(file.toString());

console.log(str);
/*
const square = (x) => {
  return Math.pow(x, 2);
};

export default square;
*/
```

You can see it in action here: https://runkit.com/pankaj/strip-comments

%[https://runkit.com/pankaj/strip-comments]

Stay tuned for tomorrow's package of the Day.