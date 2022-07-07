## Today's npm package: memoizerific

Today's npm package is [memoizerific](https://www.npmjs.com/package/memoizerific) 

Many times in our JavaScript functions, we need to do some heavy lifting; like sorting 100s of tuples, formatting strings inside objects etc.

And if there operations are similar but executed very often, it is good bet to use Memoization for function.

Memoization is a technique where you sore the pair of function arguments and its output so that any subsequent call wont need the Function execution again.

For example: For a very simple function `(x, y) => (x*x + y)`, every call to this function would need to be executed. If we memoize the function, we would keep the parameter and output map as:
```
2,3 = 7    // new call
3,4 = 12   // new call
2,3 = 7    // memoized call
3,4 = 12   // memoized call
```

And to ease the process, we have our package of the day [memoizerific](https://www.npmjs.com/package/memoizerific) 

This package uses javascript's `Map` function internally to store the pair of parameters and output.

Let's see how to use it:

```js
const memoizerific = require('memoizerific');

const mathEquation = (x, y) => (x*x + y);

// 50 is the number of maximum memoized calls to store
const memoizedMathEquation = memoizerific(50)(mathEquation);

memoizedMathEquation(2, 3) // new call
memoizedMathEquation(3, 4) // new call
memoizedMathEquation(2, 3) // memoized call
memoizedMathEquation(3, 4) // memoized call
```

Stay tuned for tomorrow's package of the Day.