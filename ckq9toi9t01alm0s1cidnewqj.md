## Today's npm package: html-minifier-terser

Today's npm package is [html-minifier-terser](https://www.npm/js.com/package/html-minifier-terser)

Minifying the final HTML is better for reducing the size of network response and hence speeding up your website.

[html-minifier-terser](https://www.npm/js.com/package/html-minifier-terser) as a highly customizable minifier is going to help us in this direction.

Here's how to use it for the following HTML

```html
<div id="app" class="container">
  <aside class="app-menu">Menu</aside>
  <main class="app-content">Content area</main>
</div>
```

Following Node.js script would minify the supplied HTML:

```js
const { minify } = require('html-minifier-terser');
const result = minify(
  `<div id="app" class="container">
    <aside class="app-menu">Menu</aside>
    <main class="app-content">Content area</main>
  </div>`,
  { removeAttributeQuotes: true }
);

console.log(result);
// <div id=app class=container>\n    <aside class=app-men…  <main class=app-content>Content area</main>\n  </div>
```

See it in action here: https://runkit.com/pankaj/html-minifier-terser

%[https://runkit.com/pankaj/html-minifier-terser]

Stay tuned for tomorrow's package of the Day.