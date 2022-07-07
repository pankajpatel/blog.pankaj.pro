## You don't need Libraries for internationalisation (i18n) of Dates

All apps use of dates in some sort of way. And one common operation with a date is to display it in a readable format.

To make it readable and understandable, we need to localize the date-strings. And along with localization, also the formats which are familiar to the user.

There are so many libraries to easily work with localization or internationalization (**i18n**) of the Date Objects. Some of these libraries are:
- [moment](https://www.npmjs.com/package/moment)
- [dayjs](https://www.npmjs.com/package/dayjs) ([An underdog in this area](https://time2hack.com/moving-away-from-moment-js-with-dayjs/ "Optimize your Front End Applications by migrating from Moment to Dayjs"))
- [date-fns](https://www.npmjs.com/package/date-fns)

But, you might not need any of these libraries to have some basic Formatting and Localization on the Date Objects.

You can use `Date.prototype.toLocaleString` and its customizable API to get the localized output easily. Like in the following example, we will use the Locale String to easily get the Readable Date desired by the user:

```js
const date = new Date() // Date from current timestamp
console.log(+date, date);
/* 👆
  1581422692394
  Tue Feb 11 2020 13:04:52 GMT+0100 (Central European Standard Time)
*/

// For above date, let's see different locales
console.log(
  'For USA Users: ', date.toLocaleString('en-US'))
// 👆 For USA based Users:  2/11/2020, 1:04:52 PM

console.log(
  'For UK Users: ', date.toLocaleString('en-GB'))
// 👆 For UK based Users:  11/02/2020, 13:04:52

console.log(
  'For DE Users: ', date.toLocaleString('de-DE'))
// 👆 For DE based Users:  11.2.2020, 13:04:52
```

But it is not that; you have only seen the basic operations.

This function also accepts the second parameter as a JavaScript Object. You can customize the result of `toLocaleString` by adding some specific keys & related values to this JS Object. For example, you can remove Weekday; change Month Display etc.

Lets looks at some examples of more customization to the output with the second parameter
```js
const date = new Date() // Date from current timestamp
console.log(+date, date);
/* 👆
  1581422692394
  Tue Feb 11 2020 13:04:52 GMT+0100 (Central European Standard Time)
*/

const readableDate = date.toLocaleString('de-DE', {
  day: "2-digit",
  month: "long",
  year: "2-digit"
});
// 👆 "11. Februar 20"
/*
year: "numeric" → "11. Februar 2020"
month: "numeric" → "11.2.20"
month: "2-digit" → "11.02.20"
month: "short" → "11. Feb. 20"
month: "narrow" → "11. F 20"
*/
```

The signature of the object to customize the output of the toLocaleString function can have the following keys. _Though there are many more keys, I am listing here only the common ones._
- **dateStyle**: `[ 'full', 'long', 'medium', 'short' ]`
- **timeStyle**: `[ 'full', 'long', 'medium', 'short' ]`
- **timeZone**: Time Some from [IANA TimeZone Database](https://www.iana.org/time-zones)
- **month**: `[ 'numeric', '2-digit', 'long', 'short', 'narrow' ]`
- **year**: `[ 'numeric', '2-digit' ]`
- **weekday**: `[ 'long', 'short', 'narrow' ]`
- **day**, **hour**, **minute** and **second**: `[ 'numeric', '2-digit' ]`
- **timeZooneName**: `[ 'long', 'short' ]`

---

## Things to Notice
Few things to pay attention here:
- Any numeric value has these two values: `numeric` and `2-digit`
- And string values are from `full`, `long`, `medium`, `short` and `narrow`
- Anything with possible one character Abbreviation can have `narrow` as its value

Another big thing to notice here is:
> you don’t need to stick to a specific timeZone if you need the desired output.  
> For `.` as the separator, use `de-DE`; for `/` as the separator, use `en-GB` and etc.
You can read more about this function at[MDN documentation of toLocaleString](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString).

---

## Specific Functions

Like the above function `toLocaleStrung`; there are exactly the same functions for only Date or Time. Hence you don't need to do some string operation to separate Date and Time.

### Date

For only Date outputs, you can use `date.toLocaleDateString`

```js
const readableDate = date.toLocaleDateString('de-DE', {
  day: "2-digit",
  month: "long",
  year: "2-digit"
});

console.log(readableDate);
// 👆 11. Februar 20
```

You can read more about this function at [MDN documentation of toLocaleDateString](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString).

### Time

If you want only the Time strings, you can use `date.toLocaleTimeString`

```js
const readableTime = date.toLocaleTimeString('de-DE', {
  hour: "2-digit",
  minute: "2-digit",
  second: "2-digit"
});

console.log(readableTime);
// 👆 13:04:52
```

You can read more about this function at [MDN documentation of toLocaleString](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleTimeString).

---

## Conclusion
Dates are very important information to display in any application. Applications which are as simple as a ToDo or this Blog. Dates are also complex objects.

But you don’t need heavy lifting libraries all the time. This post gives a brief idea to have some desired outputs without jumping into heavy (but very useful) packages.

What do you use for Date object manipulation?

Let me know through comments 💬 or on Twitter at [@patel_pankaj_](https://twitter.com/patel_pankaj_) and [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

----

Originally published at [https://time2hack.com](https://time2hack.com/you-dont-need-libraries-for-internationalisation-i18n-of-dates/) on February 18, 2020.