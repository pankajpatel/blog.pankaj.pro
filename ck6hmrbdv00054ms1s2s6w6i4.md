## Different ways to create Arrays in JavaScript

In most of the programming languages, A collection of a certain finite number of items is an Array. Or the Sets in the Mathematics.

In JavaScript as well, there are many ways to create arrays. We will be taking a look into some of them to create Arrays. 

***Table of Contents:***

- [Basic way](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#basic-way)
- [With Array Constructor](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#with-array-constructor)
- [Spread Operator](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#spread-operator)
- [From another Array](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#from-another-array)
- [From Array-Like Objects](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#from-array-like-objects)
- [Using Loops like Map and Reduce](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#using-loops-like-map-and-reduce)
    - [Array Map](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#array-map)
    - [Array Reduce](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#array-reduce)
- [New Array of Length and Fill with some value](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#new-array-of-length-and-fill-with-some-value)
- [Form Objects using Object.keys and Object.values](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#form-objects-using-object-keys-and-object-values)
- [Array Concat Function](https://time2hack.com/different-ways-to-create-arrays-in-javascript/#array-concat-function)


---

## Basic way
At first, the basic way to create arrays is here as follows:
```js
const animals = ['🐼', '🦁', '🐷', '🦊'];
```

---

## With Array Constructor
Another way to create array is by using Array Constructor function. 
```js
const Animals = new Array('🐼', '🦁', '🐷', '🦊');
```

You can achieve same with new Array function `of`. Like in the following example for `Array.of` , we create array of mixed values:
```js
const Animals = Array.of('🐼', null, '🦊', undefined);
console.log(Animals);
// 👆 (4) ["🐼", null, "🦊", undefined]
```

> Interesting thing to notice about the Constructor function is its handy override. The override is that if you pass only one argument and it is an integer, the Constructor function will create an empty array for you of that specified length.

---

## Spread Operator
Spread operator; as we saw in the different ways to create objects; works similarly and helps in creating the Arrays faster.

Like in the following example, we will add the new item and spread the old array to create a complete new Array.
```js
const moreAnimals = ['🐵', ...animals ];
```

---

## From another Array
`Array.from` will allow you to create the Arrays from another array.

The newly created array is completely new copyrights and is not gonna mutate any changes to the old array.
```js
const Animals = new Array('🐼', '🦁', '🐷', '🦊');
const copyOfAnimals = Array.from(Animals);
```

---

## From Array-Like Objects
Some Lists look like Arrays but are not arrays. And, at that time you might wanna convert it to Array to better operability and readability on the data structure.

One of such list is NodeList which you receive as an output of `document.quaerySelectorAll`

```js
const divs = document.querySelectorAll('div');
const divsArray = Array.prototype.slice.call(divs);
```

Here you can use the `Array.from` function as well to create the array from the Array-like objects. Let’s see that in the following example:

```js
const divs = document.querySelectorAll('div');
const divsArray = Array.from(divs);
```

![nodelist-to-array](https://dev-to-uploads.s3.amazonaws.com/i/jsmu17xdde9xb4ug6eb2.png)

---

## Using Loops like Map and Reduce
[Event though map and reduce are used to loop over the Arrays](https://time2hack.com/iterating-data-with-map-reduce-foreach-and-filter/). Their non-mutating nature allows us to create new Arrays in different ways.
### Map

```js
const animals = ['🐼', '🦁', '🐷', '🦊'];
const animalsCopy = animals.map(a => `${a}'s kid`);
console.log(animalsCopy);
// 👆 (4) ["🐼's kid", "🦁's kid", "🐷's kid", "🦊's kid"]
```

### Reduce

```js
const animals = ['🐼', '🦁', '🐷', '🦊'];
const animalsCopy = animals.reduce((gang, animal) => [
  ...gang,
  { animal }
], []);
console.log(animalsCopy);
/* 👆 
    .    (4) [{…}, {…}, {…}, {…}]
    .    0: {animal: "🐼"} 
    .    1: {animal: "🦁"} 
    .    2: {animal: "🐷"}
     .    3: {animal: "🦊"} 
    .    length: 4
*/
```
---
## New Array of Length and Fill with some value
We can quickly create new Arrays of any finite length with Array constructor. 

All we have to do is to pass that indefinite length of the desired array as a number to the constructor.

Like in the following example, we will create a new Array of length `6`.

Though creating an empty array is useless because you will not be able to use the Array functions until it has items in it.

One quick way to do so is to use the `.fill` method of the array and put an arbitrary value in each index of the Array.

Once the array is Filled, [you can use the loops to enhance it more with the different values](https://time2hack.com/iterating-data-with-map-reduce-foreach-and-filter/).

```js
const emojis = new Array( 6 ).fill( '😎' );
console.log(emojis);
// 👆 (6) ["😎", "😎", "😎", "😎", "😎", "😎"]

// Breakdown: 
const arr = new Array( 6 );
console.log(arr);
/* 👆
    .    (6) [empty × 6]
    .    length: 6
*/
arr.fill( Math.random().toFixed(2) );
/* 👆
    .    (6) ["0.80", "0.80", "0.80", "0.80", "0.80", "0.80"]
    .    0: "0.80" 
    .    1: "0.80" 
    .    2: "0.80"
     .    3: "0.80" 
    .    4: "0.80" 
    .    5: "0.80"
     .    length: 6
*/
```
---
## Form Objects using Object.keys and Object.values
You can create array of Keys or Values of any Object with functions `Object.keys` and `Object.values` respectively.

```js
const days = {
  1: 'Monday',
  2: 'Tuesday',
  3: 'Wednesday',
  4: 'Thursday',
  5: 'Friday',
  6: 'Saturday',
  7: 'Sunday'
};
```


![object.keys & object.values](https://dev-to-uploads.s3.amazonaws.com/i/fywttqudb2pfcm6mjxes.png)

---

## Array Concat Function
You can use the Array Concat function to create new Arrays as well.

If you use an empty array as the starting point, the output of `[].concat` will be a new copy of concatenated Arrays.

```js
const birds = ['🦆', '🦉'];
const moreBirds = [].concat(birds, '🦅', ['🐦']);
console.log(moreBirds);
// (4) ["🦆", "🦉", "🦅", "🐦"]
```
---

## Conclusion

As we have seen some different ways to create Arrays in JavaScript.

Not all of these methods can be used in the same ways and every method has its perk for specific use cases.

Let me know through comments 💬 or on Twitter at  [@patel_pankaj_](https://twitter.com/patel_pankaj_)  and  [@time2hack](https://twitter.com/time2hack) 

If you find this article helpful, please share it with others 🗣

Subscribe to **Time to Hack** to receive new posts right to your inbox.

---

Originally published at [https://time2hack.com](https://time2hack.com/different-ways-to-create-arrays-in-javascript/) on February 10, 2020.