## Ways to convert String to Number in JS

Converting from one type to another (or simply called typecasting) is needed very frequently in any programming language. So is in JavaScript.

Today we are going to take a look at some of the ways to Typecast Strings to Number.

## parseInt

As the name suggests, `parseInt` the function parses the argument as Integer. Though `parseInt` is made to parse to String to different kinds of Integers like Decimal, Binary, Octal etc

With the above definition, `parseInt` accepts two parameters:
- `string`: The value that needs to be converted to an integer
- `integer`: The `radix` base number between `0` and `32`

Examples:

```js
parseInt('123', 10) // 123
parseInt('111', 2) // 7
parseInt('111', 8) // 73
parseInt('111', 9) // 91
parseInt('111', 10) // 111
parseInt('111', 16) // 273
parseInt('111', 32) // 1057
parseInt('111', 36) // 1333
parseInt('111', 37) // NaN
```

Few things to remember here when using `parseInt`:
1.  radix base must be number; if not, it will be coerced to Number
2.  the base must be provided

---

## parseFloat

Similar to `parseInt`, `parseFloat` the function will parse the string as a Floating number.

As there is no Floating representation in other number systems except Decimal; there is only Decimal parsing of String.

Example usage of `parseFloat` can be:

```js
const stringInt = '10';
const parsedStrInt = parseFloat(stringInt);

console.log(parsedStrInt, typeof parsedStrInt);
// 10 "number"

const stringFloat = '10.66';
const parsedStrFlt = parseFloat(stringFloat);

console.log(parsedStrFlt, typeof parsedStrFlt);
// 10.66 "number"
```

---

## Number
Another way to convert/typecast Strings to Integer/Float is `Number` function. It works in the same way as of `parseFlot`

Applying the same example of `parseFloat` on `Number` will give us the same results

```js
const stringInt = '10';
const parsedStrInt = Number(stringInt);

console.log(parsedStrInt, typeof parsedStrInt);
// 10 "number"

const stringFloat = '10.66';
const parsedStrFlt = Number(stringFloat);

console.log(parsedStrFlt, typeof parsedStrFlt);
// 10.66 "number"
```

Benefits of using `Number` over `parseFloat` can be verbosity and readability of the JavaScript program.

---

## Unary Operators

Unary operators are not really the type-casters but because of the way JS works, we can use the Unary operators to convert String to Number without hassle.

Let's take a look at an example first:
```js
const oldNumber = '5'
const newNumber = +oldNumber

console.log(oldNumber, typeof oldNumber)
// 5 "string"

console.log(newNumber, typeof newNumber)
// 5 "number"
```

Here on the like `2` if we see that we used unary operator `+` to convert a String value to a Number.

For the purpose of converting the String to Number, will use only two unary operators:
- `+`
- `-`

### Unary Plus

Unary Plus will convert the String to Number without making any effort on changing the direction on the number axis

```js
const oldNumber = '5'
const newNumber = +oldNumber

console.log(oldNumber, typeof oldNumber)
// 5 "string"

console.log(newNumber, typeof newNumber)
// 5 "number"

const oldNegativeNumber = '-5'
const newNegativeNumber = +oldNegativeNumber

console.log(oldNegativeNumber, typeof oldNegativeNumber)
// -5 "string"

console.log(newNegativeNumber, typeof newNegativeNumber)
// -5 "number"
```

---

### Unary Minus

Unary Minus will try to convert the String and Number and reverse the Sign on Number (reverse the direction on the number axis)

```js
const oldNumber = '5'
const newNumber = -oldNumber

console.log(oldNumber, typeof oldNumber)
// 5 "string"

console.log(newNumber, typeof newNumber)
// -5 "number"

const oldNegativeNumber = '-5'
const newNegativeNumber = -oldNegativeNumber

console.log(oldNegativeNumber, typeof oldNegativeNumber)
// -5 "string"

console.log(newNegativeNumber, typeof newNegativeNumber)
// 5 "number"
```


---

## Binary Operators

Another way to convert string to number is by using Binary operators. Operators like `-`, `*` and `/`.

For example:

```js
const num = '1234';

const minusNum = num - 0;
console.log(minusNum, typeof minusNum);
// 1234 "number"

const multiplyNum = num * 1;
console.log(multiplyNum, typeof multiplyNum);
// 1234 "number"

const divideNum = num / 1;
console.log(divideNum, typeof divideNum);
// 1234 "number"
```

But how? here are the few things going on:

1. JS evaluates an expression from left to right
2. JS will try to match the type of operands on both sides of the operator
3. End result is dependent on the type of operands needed by the operator
4. One of the operands will be a number that will not result in any changes to the final value like _multiplication & division_ by `1` or _addition or removal_ of `0`

Note: We can use `+` but it has concatenation behaviour which will try to convert Number to String which we don't want here.

With Binary Operators, you can also change the Sign of Number on go. Let's do so with the above code example:
```js
const num = '1234';

const minusNum = 0 - num;
console.log(minusNum, typeof minusNum);
// -1234 "number"

const multiplyNum = num * -1;
console.log(multiplyNum, typeof multiplyNum);
// -1234 "number"

const divideNum = num / -1;
console.log(divideNum, typeof divideNum);
// -1234 "number"
```

---

Let me know through comments 💬 or on Twitter at [@patel\_pankaj\_](https://twitter.com/intent/follow?screen_name=patel_pankaj_) and/or [@time2hack](https://twitter.com/intent/follow?screen_name=time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

_Originally published at_ [_https://time2hack.com_](https://time2hack.com/ways-to-convert-string-to-number-in-javascript/) _on June 13, 2021._