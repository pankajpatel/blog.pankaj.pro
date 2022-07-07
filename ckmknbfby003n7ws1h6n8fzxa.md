## When NOT to choose Next.js

Next.js has been the talk of the town since its release.

And, all I could hear was ***how awesome is next.js***.

I decided to give it a try. I sat down throughout the weekend & followed the official documentation/getting-started guide. I managed to create a small Markdown-files-powered Blog.

😎

---

As I have a blog, I thought of generating my blog’s Next.js website with the previous code of Markdown files’ powered Blog.

https://beta.deckdeckgo.io/pankajpatel/nextjs/

And I finished it. I was super happy. Finally; after a long time, I finished a side project (sort of) 🎉.

With this, I started to like next.js.

🥰

---

🏎💨 Fast forward to 2 weeks later. 

I enjoyed using Next.js, so I suggested using Next.js for a new app (kind of new) for the company.

The plan was to break down the current frontend app and migrate a small part to a new App repository.

So I sat down to using Next.js for the new (splitter up & migrated) code

After two days of work, I somehow migrated some code and managed to make it work with Next.js. But now I am stuck with more work somehow.

On the third day, I decided to drop the Next.js idea and start the new app with CRA (Create React App)

----

Here are my conclusions on When not to choose Next.js

### Existing Codebase

If you are not ready for a major refactor and testing effort around it, I would suggest not to jump on to Next.js Bandwagon.

Next.js is and powerful and cool.

For a completely new project, I would recommend using Next.js

**But you should analyse the available time, refactoring efforts and testing needs of your existing codebase.**

### Runs on Node.js server

So far, all the conventional React apps had been on a single HTML file many JS files chunked-&-joined together on the need (URL, device, etc) basis.

Now, next.js provides fast response time, effective code-splitting, Server Side Rendering and many other things. All these awesome features with the help of a Node.js server responding to all the requests.

If your previous app was a conventional single HTML file, you should rethink it.

One can say, you can Export/Generate the site & use it in an old conventional way.

But then you are using Next.js for not what it is capable of doing. And hence, you don’t even need it in the First Place.

You wanted a needle to fix up a button on your shirt and here you are, handling the task with a sword.

### API Routes

Yes, Next.js provides API routes. You can pretty much create your server there which can act as your Backend and do a complex thing. All the BE and FE code in one place.

But, Next.js’s API capabilities should be used to create an API Proxy or Middleware and do the small scale operations.

Heavy lifting or Business Logic should be created in APIs which can be fine-tuned and secured.

Everything at one place will either bring real API to the standard of FE’s vulnerability or make FE move slowly.

### Refactoring Effort

In a conventional React App, where components are rendered on Routes from react-router (or similar), File-based routing will shake things a lot.

You might have to rethink your component arrangement and how they are being added to the Page-level components.

You would need to rethink the Store of your application.

This invites a lot of effort in refactoring and hence a lot of testing to make sure again that the app is working fine.


### CDN

As the assets are not static, you can not use CDNs anymore

Though if you choose to export your website, and you are using a hybrid of SSR and SSG, it is possible to use CDNs.

---

In the end, we went with Create React App as it solved all the initial configuration needs and gets out of the way. It fits our current application arrangements.

Though we needed some customisations. We managed to make adjustments with the help of [craco](https://www.npmjs.com/package/@craco/craco).

---

Conclusion

With next.js you can create blazing fast web apps.

Though be careful while choosing to use Next.js for your next application.

Let me know through comments 💬 or on Twitter at [@patel_pankaj_](https://twitter.com/intent/follow?screen_name=patel_pankaj_) and/or [@time2hack](https://twitter.com/intent/follow?screen_name=time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

Originally published at [https://time2hack.com](https://time2hack.com/when-not-to-choose-next-js/) on March 22, 2021.