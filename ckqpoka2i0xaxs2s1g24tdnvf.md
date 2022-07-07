## Today's npm package: style-to-js

Today's npm package is [style-to-js](https://www.npm/js.com/package/style-to-js)

If you are working with a UI project where you are accepting CSS styles from users to be applied at a later time.

It will be easier & better to save them as JS Object as it can help in removing the rules duplication.

Lets consider the following block of CSS rules:
```css
border: 1px solid #ccc;
border-bottom-color: #aaa;
color: #555;
font-size: 16px;
margin: 0 0.25rem;
```

And with the use of today's package; we can convert above CSS rules' block to JS object as:

```js
const parse = require('style-to-js').default;

const styles = `
  border: 1px solid #ccc;
  border-bottom-color: #aaa;
  color: #555;
  font-size: 16px;
  margin: 0 0.25rem;
`;

parse(styles);

/*
{
  border: "1px solid #ccc",
  borderBottomColor: "#aaa",
  color: "#555",
  fontSize: "16px",
  margin: "0 0.25rem"
}
*/
```

You can see it in action here: https://runkit.com/pankaj/style-to-js

%[https://runkit.com/pankaj/style-to-js]

Stay tuned for tomorrow's package of the Day.