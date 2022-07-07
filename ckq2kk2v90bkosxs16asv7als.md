## Today's npm package: reinterval

Today's npm package is [reintereval](https://www.npm/js.com/package/reintereval)

Some times in an application, you might have to do certain tasks repeatedly. Obvious solution is `setInterval`

Though as the project and related solution expands, it becomes bigger and harder to manage.

And, [reintereval](https://www.npm/js.com/package/reintereval) is going to help us in this direction.

It allows to create, stop and restart such interval tasks with just one instance and effectively increases the readability.

Let's see how to use it:

```js
const reinterval = require('reinterval');

const poller = reinterval(() => {
  console.log('Hello', new Date());
}, 3500);

setTimeout(() => {
  console.log('cleared')
  poller.clear()
}, 15000);

setTimeout(() => {
  console.log('rescheduled')
  poller.reschedule(1000)
}, 16000);

setTimeout(() => {
  console.log('destroyed')
  poller.destroy()
}, 19000)
```

See it in action here: https://runkit.com/pankaj/reintereval

%[https://runkit.com/pankaj/reintereval]

Stay tuned for tomorrow's package of the Day.