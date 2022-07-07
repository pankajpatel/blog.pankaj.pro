## Today's npm package: cli-table3

Today's npm package is [cli-table3](https://www.npmjs.com/package/cli-table3) 

For CLI tools, sometimes it is need to show some tabular information.

CLI Table is to the rescue.

Here's how you can use it to generate tables. In this example, we will generate table from file stats showing `size`, creation time and modify time of the file.


```js
const fs = require('fs');
const path = require('path');
const table = require('cli-table3');
const file = process.argv.slice(2);

var table = new Table({
    head: ['Size', 'Created At', 'Modified At']
  , colWidths: [100, 200, 200]
});
 
// process the file
const stats = fs.statSync(path.resolve(process.cwd(), file));

// table is an Array, so you can `push`, `unshift`, `splice` and friends
table.push([stats.size, stats.birthtime, stats.mtime]);
 
console.log(table.toString());
```

Stay tuned for tomorrow's package of the Day.