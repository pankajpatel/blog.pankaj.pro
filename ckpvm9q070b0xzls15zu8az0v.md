## Today's npm package: jsdoc-to-markdown

Today's npm package is [jsdoc-to-markdown](https://www.npmjs.com/package/jsdoc-to-markdown) 

Documentation is as important as solving the problem. And whats better than [`jsdocs`](https://jsdoc.app/) for JavaScript apps.

The way you can define a simple JSDoc is as follows:

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

And what better than auto generating the Markdown Readme from JSDoc.

Todays package [jsdoc-to-markdown](https://www.npmjs.com/package/jsdoc-to-markdown) will allow you to do so.

All you have to do is to add [jsdoc-to-markdown](https://www.npmjs.com/package/jsdoc-to-markdown) as a dependency and then you can run it via npm scripts

Install as:

```sh
npm i -D jsdoc-to-markdown
```

Add to scripts. in `package.json`

```json
{
  ...
  "scripts": {
    "gen-docs": "jsdoc2md index.js > API.md"
  }
}
```

And then executing the command on CLI as:

```sh
npm run gen-docs
```

will give us following `API.md` file:

```md
<a name="square"></a>

## square(x) ⇒ <code>Number</code>
Function to generate the Square of a Number

**Kind**: global function  
**Returns**: <code>Number</code> - Squared value of passed Number  

| Param | Type | Description |
| --- | --- | --- |
| x | <code>Number</code> | The number to be squared |
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1623614630428/lhUY0SBgp.png)

Stay tuned for tomorrow's package of the Day.