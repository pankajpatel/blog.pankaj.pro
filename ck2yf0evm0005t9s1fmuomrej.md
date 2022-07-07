## Optimize your Front End Applications by migrating from Moment to Dayjs

Does your application use dates in some way?

I am pretty sure that there are almost no use cases that don’t use dates, and if they exist, they can be improved to a higher extent with dates as a pivot for history. 

So did ours at [HolidayPirates GmbH](https://holidaypirates.group), and we used `moment.js` in our frontend app to transform and manipulate them for our use cases.

Though moment.js is heavy for FE apps and we might not use all the capabilities provided by moment.js. 

We had already achieved a major cutdown on the package size by removing the unnecessary locales. But still, it wasn’t slim enough. 

- - - -

## Challenges
The major challenges to move away from moment.js were:
* **Magnitude of its Usage**: Because we have codebase which, in terms of users, is medium-to-semi-large in size. So the current use moment.js will also limit to migrate or move away from itself.
* **API**: As moment.js is used in so many places, and that’s why we need something with similar API to replace it.

- - - -

## Solution
If we look at [bundlephobia report of moment.js](https://bundlephobia.com/result?p=moment@2.24.0); we have suggested package as `dayjs` with 96% smaller package size as 2.76kB

![](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/moment-dayjs.png)

> And the repository states that the it has following features
> * Familier moment.js API  
> * Chain-able & Immutable  
> * Localised i.e. i18n (internationalization) support  

So we started the migration and we realized we had to de the following every time we use the days:
```js
import dayjs from 'dayjs';
import 'dayjs/locale/de';

dayjs.locale('de');
```

This seems doable, though it was needed to be done for every instance of dayjs being created.

We carried on, but the dayjs presented two different problems:
* Initialization with a format was not possible
* Locale assignment has to be done every time you import a module

- - - -

### Initialization with a Format

For that dayjs provides a plugin called `AdvancedFormat` which extends the capabilities of dayjs similar to moment.js

```js
import AdvancedFormat from ‘dayjs/plugin/advancedFormat’;

dayjs.extend(AdvancedFormat);
```

Though we are again in the same circle where we have to import and attach the plugin every single time.

The main reason for this to happen is that the node modules execute in a separate scope and expose the values that are only exported. And by nature, dayjs is very much immutable & tree shakable and its instance does not mutate the behavior of other instances.

- - - -

### Getting dayjs ready for use every single time

To solve this, we took the following approach:

```js
import dayjs from 'dayjs';
import 'dayjs/locale/de';
import AdvancedFormat from ‘dayjs/plugin/advancedFormat’;

dayjs.locale('de');
dayjs.extend(AdvancedFormat);

export default dayjs;
```

Now save it as `services/dayjs`; we made ourselves `dayjs` service which is already localized and patched for advanced format initialization.

Now in the place of its usage, instead of doing

```js
import dayjs from 'dayjs';
```

We do this:

```js
import dayjs from 'services/dayjs';
```

- - - -

> Note that for above import to work, we have aliases added in our web pack configuration as follows:  
> `aliases: { services: 'src/javascript/services' },`  

- - - -

## Conclusion
Moment.js is an awesome library and it helped a lot to make things easier to develop with the dates.

Though it was the time to part the ways.

What do you guys use for date object manipulation in JavaScript? Share your experience with us.

Let me know what do you think about this article through comments 💬 or on Twitter at  [@patel_pankaj_](https://twitter.com/patel_pankaj_)  and  [@time2hack](https://twitter.com/time2hack) 

If you find this article helpful, please share it with others 🗣; subscribe to the mailing list for the new posts and see you the next time.

- - - -

## Credits

* Icons made by [Flat Icons](https://www.flaticon.com/authors/flat-icons) from flaticon.com
* Photo by  [Curtis MacNewton](https://unsplash.com/@curtismacnewton?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/calendar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

----

Originally published at [https://time2hack.com](https://time2hack.com/moving-away-from-moment-js-with-dayjs/) on November 13, 2019.