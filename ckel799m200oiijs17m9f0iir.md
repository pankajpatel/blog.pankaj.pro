## Are you using Trailing Commas in your JavaScript?

I am presuming that you are using objects and arrays every day in your JavaScript. 

And I am also sure that most of them are not single line these arrays and objects with items spread in many lines.

Though my question here is:

> Do you leave a trailing comma at the end of the last item before closing the object or array creation?

If you are not using any trailing commas then why not?

---

Let’s start with *Why trailing comma could help you*

## Reasons to keep Trailing Comma
### No False diff counts
The Commit Messages are gonna be of exactly the line when you add or remove items i.e. no false diff counts 🤥

```diff
  const ACTIONS = {
    ADD_ITEM: 'ADD_ITEM',
-  UPDATE_ITEM: 'UPDATE_ITEM'
+  UPDATE_ITEM: 'UPDATE_ITEM',
+  DELETE_ITEM: 'DELETE_ITEM'
  }
```

Here we have diff count of `+2-1`
*OR*

```diff
const ACTIONS = {
  ADD_ITEM: 'ADD_ITEM',
  UPDATE_ITEM: 'UPDATE_ITEM',
+ DELETE_ITEM: 'DELETE_ITEM',
}
```
And here we have diff count of `+1`

> Which one do you prefer to in the Pull/Merge Requests?

---

### New items always in the end
We can add new items at the end of the Object or Array, rather than being in middle to avoid multiline changes 👻

Considering the above example, we would have to add the new item in the middle of the object to keep the edit count as `1` Line
```diff
const ACTIONS = {
	ADD_ITEM: 'ADD_ITEM',
+ DELETE_ITEM: 'DELETE_ITEM',
  UPDATE_ITEM: 'UPDATE_ITEM'
}
```

Or we can have trailing commas and add the new item at the end of the object all the time
```diff
const ACTIONS = {
  ADD_ITEM: 'ADD_ITEM',
  UPDATE_ITEM: 'UPDATE_ITEM',
+ DELETE_ITEM: 'DELETE_ITEM',
}
```

---

### No Fear of Breaking Production
Transpilers and Bundlers are will omit trailing comma and not break the production 😉

The modern browser will not complain about the trailing commas as it is part of the ES5 standard

But the old browser might complain and for that case, before IE9. 

Even though finding such an old browser is a good expedition. We can enable our bundlers to omit the trailing commas from production bundles so that it is safe to ship.

For that old browser, the trailing comma would be the least of the problems. It is because the old browsers’ capability to parse and run large JS apps is also questionable.

Use [babel-plugin-syntax-trailing-function-commas](https://babeljs.io/docs/en/babel-plugin-syntax-trailing-function-commas)  in babel configuration to enable trailing commas in your JavaScript source code.

If you are using [Prettier](https://prettier.io/),  [you can configure](https://prettier.io/docs/en/options.html#trailing-commas) with following:
* `trailingComma` in[`.prettierrc`](https://prettier.io/docs/en/configuration.html)
* `--trailing-comma` if [using Prettier via CLI](https://prettier.io/docs/en/cli.html)

Possible values are:
* `es5` only on arrays and objects
* `none`  nowhere, no trailing commas
* and  `all` everywhere possible; arrays, objects, function params etc

---

## Biases

### Looks like something is missing
 I have heard this argument many times. If there is a trailing comma, it feels like something is missing or it is wrong.

> You will get used to it. 

> We have got used to writing HTML in JS, trailing comma is a very small thing in front of it.

### Why extra character for nothing?
This comma is an extra character which is not useful for any execution.

> We write code in Clean way for other developers to understand.

> Machines can understand comma or no-comma in the same way. Then why not help the other developers in reviewing the code.


---

## References
[javascript - Are trailing commas in arrays and objects part of the spec? - Stack Overflow](https://stackoverflow.com/a/7246662/759045)

---

## Conclusion
The benefits of trailing comma are less though very good to have.

What do you think about trailing commas?

Let me know through comments 💬 or on Twitter at [@patel\_pankaj\_](https://twitter.com/patel_pankaj_) and/or [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

Originally published at [https://time2hack.com](https://time2hack.com/are-you-using-trailing-commas-in-your-javascript/) on Sep 1, 2020.