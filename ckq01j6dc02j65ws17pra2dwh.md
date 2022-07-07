## Today's npm package: html-minifier

Today's npm package is [html-minifier](https://www.npm/js.com/package/html-minifier)

Optimization is the core goal of Computer Science and can bee applied to small areas like delivering HTML to browsers.

And to optiimize thee HTML delivery, first thing to come in mind is the minification of HTML

[html-minifier](https://www.npm/js.com/package/html-minifier) is going to help us in this direction. Let's see how to use it:

```js
const HTMLMinifier = require("html-minifier");

const minify = HTMLMinifier.minify;

const minifiedHtml = minify(
  '<h1 class="primary" id="main">Title</h1>',
  { removeAttributeQuotes: true }
);

console.log(minifiedHtml);
 // '<h1 class=primary id=main>Title</h1>'
```

See it in action here: https://runkit.com/pankaj/html-minifier

%[https://runkit.com/pankaj/60ca7877e54151001da08865]

Stay tuned for tomorrow's package of the Day.