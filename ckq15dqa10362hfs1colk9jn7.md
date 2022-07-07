## Today's npm package: merge-anything

Today's npm package is [merge-anything](https://www.npm/js.com/package/merge-anything)

Manipulating data is basic need for any kind of application development.

And for manipulation of data, sometimes merging of similar data is needed.

And, [merge-anything](https://www.npm/js.com/package/merge-anything) is going to help us in this direction. Let's see how to use it:

```js
const { merge, mergeAndConcat } = require('merge-anything');

const objA = { 
  name: 'Pankaj',
  lang: ['js', 'css'],
  social: { twitter: 'patel_pankaj_' } 
};

const objB = { 
  name: 'Pankaj Patel',
  lang: ['html', 'go'],
  social: { instagram: 'pankaj_patel' } 
};

console.log(
  'Merge',
  merge(objA, objB)
);
/* {
  name: 'Pankaj Patel',
  lang: ['html', 'go'],
  social: {
    instagram: 'pankaj_patel',
    twitter: 'patel_pankaj_'
  } 
} */

console.log(
  'MergeAndConcat',
  mergeAndConcat(objA, objB)
 );
/* {
  name: 'Pankaj Patel',
  lang: ['js', 'css', 'html', 'go'],
  social: {
    instagram: 'pankaj_patel',
    twitter: 'patel_pankaj_'
  } 
} */
```

One thing to note here is that merge by default will overwrite the arrays. To use concatenation for merge, use `mergeAndConcatenation` 

See it in action here: https://runkit.com/pankaj/merge-anything

%[https://runkit.com/pankaj/merge-anything]

Stay tuned for tomorrow's package of the Day.