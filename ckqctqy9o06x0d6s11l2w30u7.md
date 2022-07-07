## Today's npm package: watchr

Today's npm package is [watchr](https://www.npm/js.com/package/watchr)

When creating CLI utilities, if you are dealing with files, would't it be great to be able to react to the file changes.

This package would help you do the same.

Here is how you can use this package to watch for currently open directory context

```js
const watchr = require('watchr');

const listener = (change, path, newStats, oldStats) => {
  console.log(path, change);
  // or something more useful
}

const spy = watchr.open(
  process.cwd(),
  listener,
  (err) => {
    if(err) {
      console.error(err);
      spy.close();
      return process.exit();
    }
    console.log("now watching", process.cwd());
  }
);
```

<video src="https://user-images.githubusercontent.com/251937/123484896-61469f00-d609-11eb-9f23-304a61599d93.mp4">
https://user-images.githubusercontent.com/251937/123484896-61469f00-d609-11eb-9f23-304a61599d93.mp4
</video>


Stay tuned for tomorrow's package of the Day.