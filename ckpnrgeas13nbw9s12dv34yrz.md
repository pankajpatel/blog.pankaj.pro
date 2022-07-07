## Today's npm package: is-directory

Today's npm package is  [is-directory](https://www.npmjs.com/package/is-directory) 

This package will help you check if the supplied path is a directory or not. It will return true if the file path exists on the file system and its directory.

```js
const fs = require('fs');
const isDirectory = require('is-directory');

isDirectory('dist', function(err, dir) {
  if (err) { // directory doesn't exist or isn't a directory
    fs.mkdirSync('dist') // make the directory
  }

  console.log(dir);
  //=> true
});
```

Or synchronized version as:

```js
const fs = require('fs');
const isDirectory = require('is-directory');

if (!isDirectory.sync('dist')) { // if directory doesn't exist
  fs.mkdirSync('dist') // make the directory
}
```