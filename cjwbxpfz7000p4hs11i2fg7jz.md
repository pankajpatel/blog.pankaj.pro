## CustomEvents: JavaScript Events on steroids 🏋️‍♂️

Events, the core part of JavaScript and still painful area for many Developers.

Events have scared people off to enter JS; Events have terrified the debugging sessions. But they are like little Pikachu; you just need to understand them and it is like a play of your left hand to make the best use of them.

Today we will see how we can use Custom Events to build good communication in our App Components. We will use  [WebComponents](https://time2hack.com/2017/12/introduction-to-webcomponents-and-shadowdom/)  to go through the Use Cases and Solutions.

But before we jump into the discussion, let's take a quick look at the Event Loop in JavaScript.

It is worth time to read about the event loop as it will give you an understanding of how Asynchronous Execution works in JavaScript at the core level.

If you know about it, you can skip this part and  [jump to CustomEvents](#custom-events) 

## Environment

JS is single threaded and has Stack, Heap, Event Queue and Event Loop. Let's briefly go through their nature and what they are.

![JS Environment Thread](https://res.cloudinary.com/time2hack/image/upload/q_auto:good/js-engine%402x.png)

### JS Stack

It keeps the information of currently executing functions in the order to time of execution; as older the function had started, deeper level it will be in the stack.

As the function finishes executing, it gets removed from the stack.

### Heap

Heap is the storage area from where the memory will be allocated to the executing functions, variables, etc.

### Event Queue

The Event Queue is the place where all the events get added and are processed by the JS Engine thread. As the name 'Queue' suggests, the events are First-In-First-Out (FIFO)

### Event Loop

Event Loop is the synchronous watcher for the events i.e. it will wait for the messages/events.

As the events appear in the queue, it processes them. And if they don't, it will synchronously wait for the next message. So in terms of code, it can be written as follows:

```js
while(queue.waitForMessage()) {
  queue.processNextMessage();
}
```

And in general the is how they are working together with each other:

![JS code execution on http://latentflip.com/loupe/](https://res.cloudinary.com/time2hack/image/upload/q_auto:good/loupe.gif)

-----

Now let's come to some common problem faced when handling Events in JavaScript:

## Same Name Events

Many elements have same name events like click is available on All HTML Elements; change event is available on all Input elements etc. And as we know that the events are bubbling in nature, you can capture the same event at any level until the document's visible root body.

The problem occurs when you wanna handle the same kind of events in a different part of the App.

Consider following HTML and JS as example:
```html
<article data-id="12345">
  <header>
    <a href="https:/time2hack.com/">
      <h1>Callbacks, Promises and Async/Await</h1>
    </a>
  </header>
  <span>Lorem ipsum dolor sit amet consectetur adipisicing elit. Hic inventore debitis id reprehenderit</span>
  <footer>Written by: <a href="/author/pankaj">Pankaj</a></footer>
</article>
```

```js
const author = document.querySelector('article span[author]');

author.addEventListener('click', (e) => {
  const author = e.target.getAttribute('author');
  const url = `/author/${author}`
  window.location.href= url;
});
```

Now as you can see, if only click event was being listened on the article tag, it would have caused the wrong action of redirecting to author page where it meant to go to the post page.

More common scenarios happen when there is data tracking done based on these events.

## PreventDefault and StopPropagation

`event.preventDefault()` and `event.stopPropagation()`

These two guys have quickly fixed many bugs but have been a pain in the ass most of the times because of those fixes.

> **How?**

I stopped propagation at one point and forgot that at the upper level, I am waiting for that event to happen.

I prevented default on some events so that my handlers could work but now there are other children elements whose defaults might also get prevented.

I am not saying I do the same mistakes now as well but during the times of my start in the FrontEnd, debugging event based problems was a major part of the workday.

-----

> **Solution?** Use Custom Events.

## Custom Events

Custom Events are the same as regular events and at the same time different from regular/native events.

### Similarities
- Instance of `Event` constructor
- Can bubble; means you can use `stopPropagation`
- Cancellable; means you can use `preventDefault`
- Same Object Properties and Methods like `target`, `currentTarget`, etc.
- Can not be copied with `Object.assign`

### Differences
- Instance of `CustomEvent` constructor
- Can have custom Data with Event
- Different Names thank the regular ones (Obviously)

> So how do we create a Custom Event?

```js
const myEvent = new Event('my-event');

//or with data
const myNewEvent = new CustomEvent('my-new-event', {
  bubbles: true,  
  detail: {
    text: 'From somewhere in the app',
    version: '0.1.1',
  },
  cancelable: true,
});
```

That's it. With the `CustomEvent` constructor, you can create a simple custom event and pass additional params as an object in second params. Config options include:
- To define data, you need to add detail key and data as its value
- `cancelable` to define the cancel-ability of the event
- `bubbles` property to make the event be catchable on the upper hierarchy

> But how do we trigger this new CustomEvent on our element? 

With `dispatchEvent` on any element; following is the example:

```js
const author = document.querySelector('article span[author]');
author.dispatchEvent(myEvent);

//or event with the data
author.dispatchEvent(myNewEvent);
```

> How do we listen to this Event? Here is how:

```js
const article = document.querySelector('article');

article.addEventListener('my-event', (e) => {
  console.log('my-event', e);
});

article.addEventListener('my-new-event', (e) => {
  console.log('my-new-event', e, e.detail);
});
```

So this is how you can create CustomEvents and use them in your project.

There is a bit of coding overhead but it will be so manageable and you will thank yourself when you return to the project after a few months.

----

## Use Case

Let's look at an example to explain the case of `CustomEvents`.

Let's create a Twitter tweet screen with web components. And we will use custom events to facilitate the data communication between the components.

Here we have a global state in the top level application. This component will bind the user with the tweets and  [send it back to the server with Fetch](https://time2hack.com/2017/11/goodbye-xmlhttprequest-ajax-with-fetch-api-demo/) .

And following stateless components :
- New tweet
- Tweet view
- Feed

Among all the above complement, the new tweet and the main controller are the components where we will concentrate.

Firstly, let’s create the **new tweet** component. Following is the code for new tweet component:

```js
import template from './form.html';
import customeEvent from '../../custom-event';

export const FORM_COMPONENT = 'tc-form';
export const EVENTS = {
  CREATE_TWEET: 'tc-event-new-tweet'
}

class Form extends HTMLElement {
  connectedCallback() {
    this.render();
  }
  render() {
    this.innerHTML = template({});
    this.collectRefs();
    this.refs.button.addEventListener('click', (e) => {
      if(!this.refs.text.value) {
        return;
      }
      this.dispatchEvent(customeEvent(EVENTS.CREATE_TWEET, {
        text: this.refs.text.value,
      }));
      this.refs.form.reset();
    });
  }
  collectRefs() {
    this.refs = {
      text: this.querySelector('[ref="text"]'),
      form: this.querySelector('[ref="form"]'),
      button: this.querySelector('[ref="btn-submit"]'),
    }
  }
}

customElements.define(FORM_COMPONENT, Form);
```

For the above code, I made a **small wrapper to create `customEvent`** which is as follows:

```js
module.exports = (
  name,
  eventData,
  bubbles = true,
  cancelable = true,
) => new CustomEvent(
  name, {
    detail: eventData,
    bubbles,
    cancelable
  }
);
```

As now we have our form and `customEvent` wrapper, we will now prepare our main controller component. Here is the code for the main controller which will handle the new tweet and send it to backend and alongside, push to the feed:

```js
import customeEvent from '../custom-event';
import { 
  EVENTS as FORM_EVENTS,
  FORM_COMPONENT,
} from './form/form.component';
import { FEED_COMPONENT } from './feed/feed.component';
import template from './app.html';
import css from './app.css';

export const EVENTS = {
  PUSHED_TWEET: 'tc-event-pushed-tweet'
}
const STORE = {};
const USERS = {
  time2hack: {
    username: 'time2hack',
    name: 'Time to Hack',
    photo: '',
  },
}
class Clone extends HTMLElement {
  connectedCallback() {
    this.dom = this.attachShadow({ mode: 'open' });
    this.dom.innerHTML = template({ title: 'Twitter', css });
    this.refs = {
      form: this.dom.querySelector(FORM_COMPONENT),
      feed: this.dom.querySelector(FEED_COMPONENT),
    }
    this.dom.addEventListener(FORM_EVENTS.CREATE_TWEET, (e) => {
      const time = +new Date()
      const tweet = {
        tweet: Object.assign({}, e.detail, {time}),
        user: USERS.time2hack,
      };
      STORE[time] = Object.assign({}, tweet);
      this.dispatchEvent(customeEvent(EVENTS.PUSHED_TWEET, { tweet }))
    });

    this.addEventListener(EVENTS.PUSHED_TWEET, (e) => {
      this.refs.feed.pushTweet(e.detail.tweet);
    })

  }
}

customElements.define('twitter-clone', Clone);
```

And as you can see above, The main controller just has to worry about the event name-spaced from the child component and no native events like `click`,  `change` etc.

**And in a similar way, we can fire these CustomEvents on the feed to add new tweets.**

Following is the Feed component to receive the new tweets to render:

```js
import { TWEET_COMPONENT } from '../tweet/tweet.component';

export const FEED_COMPONENT = 'tc-feed';
const template = (scope) => `<div class="list-group" ref="list">
  <h4>Your Feed</h4>
  ${scope.tweets.map(getTweetMarkup).join('')}
</div>`;

const TWEET = {
  user: {
    username: 'patel_pankaj_',
    name: 'Pankaj Patel',
    photo: '',
  },
  tweet: {
    text: 'lsjdhjgj gdfgdfg gdfgdfg dfgfd',
    time: 1558425005922,
  }
}

const getTweetMarkup = (tweet) => `
<${TWEET_COMPONENT} 
  tweet='${JSON.stringify(tweet)}'
></${TWEET_COMPONENT}>`;

class List extends HTMLElement {
  connectedCallback() {
    this.render();
  }
  render() {
    console.log(TWEET);
    this.innerHTML = template({
      tweets: [].concat([TWEET, TWEET]),
    });
  }
  pushTweet(_tweet) {
    const tweet = document.createElement('div');
    tweet.innerHTML = getTweetMarkup(_tweet);
    this.querySelector('[ref="list"]').appendChild(tweet.firstChild);
  }
}

customElements.define(FEED_COMPONENT, List);
```

And of course the tweet component to render the view of the tweet:

```js
import { timeAgo } from '../../time-ago';

const template = (scope) => `<div class="card">
  <div class="card-body">
    <h5 class="card-title">${scope.user.name}</h5>
    <p class="card-text">${scope.tweet.text}</p>
    <p class="card-text">
      <a href="#" class="card-link">Retweet</a>
      <a href="#" class="card-link">Like</a>
      <small class="float-right text-muted">${timeAgo(scope.tweet.time)}</small>
    </p>
  </div>
</div>`;

export const TWEET_COMPONENT = 'tc-tweet';

class Tweet extends HTMLElement {
  connectedCallback() {
    this.data = JSON.parse(this.getAttribute('tweet') || '{}'); 
    this.innerHTML = template(this.data);
  }
}

customElements.define(TWEET_COMPONENT, Tweet);
```
----

[![](https://res.cloudinary.com/time2hack/image/upload/v1559296992/demo.svg)](https://time2hack.github.io/twitter-clone/)

[![](https://res.cloudinary.com/time2hack/image/upload/v1559296992/code.svg)](https://github.com/time2hack/twitter-clone/)

------

## Conclusion

In the above code examples and argument; you can see that the custom events are a better approach to handle & manage; while reducing the chances of unexpected error because of native events.

Let me know what do you think about custom events and how would you use them in your app through comments 💬 or on Twitter at  [@patel_pankaj_](https://twitter.com/patel_pankaj_)  and  [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣; subscribe to the blog for new posts and see you the next time.

----

## Credits
- Photos by  [Irvan Smith](https://unsplash.com/photos/cwqG1N1AtI0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on  [Unsplash](https://unsplash.com/@pankajpatel) 
- Icon made by  [Freepik](https://www.freepik.com/)  from  [Flaticon](https://www.flaticon.com/)  & licensed under  [CC 3.0 BY](http://creativecommons.org/licenses/by/3.0/) 
- time-ago functions from  [https://muffinman.io/javascript-time-ago-function/](https://muffinman.io/javascript-time-ago-function/) 