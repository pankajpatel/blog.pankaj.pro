## Today's npm package: split.js

Today's npm package is [split.js](https://www.npm/js.com/package/split.js)

Layout's with adjustable width/height can be tricky to handle and may take a lot of time to get it right.

[split.js](https://www.npm/js.com/package/split.js) is aims to solve this with very minimal JS.

Here's how to use it for the following HTML

```html
<div>
  <aside>Menu</aside>
  <main>Content area</main>
</div>
```

Following would make the vertical resizable split:

```js
const Split = require('split.js');

Split(['aside', 'main'], {
    minSize: [100, 300],
    minSize: [300, Infinity],
    direction: 'vertical'
});
```

%[https://codepen.io/pankajpatel/pen/VwpJZap]

Stay tuned for tomorrow's package of the Day.