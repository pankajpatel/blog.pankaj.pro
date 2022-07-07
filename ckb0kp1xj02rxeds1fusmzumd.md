## What is JAMStack & Why you should care?

JAMStack or JavaScript, APIs and Markup Stack is a modern shift in the FrontEnd Space to develop fast web applications.

JAMStack has been around for a while, though recent developments in SSG (Static Site Generators) has pushed JAMStack to be one of the favourite Stack Choice.

- - -

## What is JAMStack?

JAMStack is a stack (duh), workflow and way to build websites where dynamic behaviour is provided by **JavaScript**, Data is fed only via **APIs** and **Markup** provides the necessary structure/placeholder for the content which is static or dynamic.

> **The main idea is that the Static Markup will always be faster than the dynamically generated markup from Server**

So we will serve the static content at first and use JavaScript to add dynamic content through API.

One very common sidestep is SSR, Server Side Rendering, where for the Dynamic Content, we generate the Static Pages beforehand and deploy them. When a Client request for the Page, we will deliver the static content and Data to re-link the page's JavaScript with Markup.

The final render will be non-noticeable change from SSR HTML to JavaScript Generated Components.

And if the JS Renderer is intelligent enough, there will not be any change to the DOM itself. Many Front End Libraries and Frameworks are doing so with the help of Virtual DOM (vDOM) and applying only the diff of vDOM & actual DOM.

### Benefits

-   **Ultra Fast**; as content generation step is removed, so is the time to do so. The requested pages can be delivered as soon as the server finds the content to deliver.
-   **Low Server Cost**; Server Cost is low as we not spending Server time and resources on building the markup dynamically.
-   **Backends For Frontend (BFF)**; Now backend can only focus on serving the needs of the front end with APIs rather than spending energy on caring about the response markup generation.  
      
    Hence Backend will only exist to satisfy FrontEnd needs. This also means Backend Teams can focus on solving problems at the API level.  
      
    Serving static content is gonna be primarily handled at DevOps level.
-   **Better Caching**; As the static content is less likely to change, the caching can be more extensive to speed up the Content Delivery. The age of cached content can be longer.
-   **Leverage CDN**; CDN (Content Delivery Networks) can be leveraged to deliver the static markup as well; not just the media files

### Problems

As there are shiny benefits; there are some problems as well that need to be addressed when choosing to go with JAMStack. Problems like:

-   **TTI or Time to Interactive**; Longer [TTI](https://web.dev/tti/)s can be a huge pain if the JS is not performant or not bundled in an optimized way
-   **Optimization is at Discretion**; The JavaScript and CSS delivery need to be optimized and there are tools to do this automatically but Developers' discretion is heavily required.
-   **JS Parsing Overhead**; As all the dynamicity is moved to JS, User would have to wait for JS make the page functional and ready to use and parsing time of JS is another bottleneck.  
      
    Hence JS delivered to clients should be optimized, small in size and should only contain the pieces which will be needed immediately.
-   **SEO**; SEO is not a big problem as the Crawlers can execute the necessary JS; though it is an extra step for crawlers to execute. SSR and HTML Snapshots can fix this problem but this is an extra step for the build of the site.

- - -

## Why should you care?

As a developer, no matter which part of Application you are working on, you have to be aware of the Stack you are using or you are going to use.

### As Frontend Developer

As a frontend developer, JAMStack brings a majority of the application responsibilities to you. You might need to be aware of the DevOps of the application as well

### As Backend Developer

As we discussed above, JAMStack promotes BFF (Backend For Frontend) for application development.

This means that API hardening is much more essential. Security, Access, Authorization etc becomes highly important.

The backend can be developed as a monolith or microservice, but this detail of implementation is none of the Frontend's concern. It up to you how you break the application down and when you do it.

### As Fullstack Developer

Well, everything written above for Frontend and Backend is your concern now. You might also have to be more aware of System Architecture and DevOps for smooth development and running of the application.

As the idea of DevOps as code is being favoured more and more by developers and DevOps engineers; you are kind of a One-Man Army in JAMStack.

- - -

## When to say "_No!"_ to JAMStack?

No matter how shiny it is, sometimes JAMStack is an over-engineering practice as a solution to application design.

You can try to ask yourself the following questions to see if JAMStack is a right fit for your application design:

-   How important it is to have an ultra-fast web application
-   Does your team have independent Frontend and Backend Developer?
-   How often the dynamic part of your application changes?

![](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/what-is-jam-stack-why-you-should-care-Comparision.png)

-   Can you spend on multiple servers and CDN service? And how much?
-   etc.

- - -

## How to _"JAMStack"_?

Like we discussed in the beginning, JAMStack has three major parts

-   JavaScript
-   APIs
-   Markup

Markup is always HTML and JavaScript is always going to be there to add interactivity to HTML.

APIs are a whole different challenge in themselves; though for JAMStack; let's consider that the APIs are in place and follow the majority of the best practices.

Now the question is about the tools and development workflow.

Major tools can be put in the brackets of:

### SSG (Static Site Generators)

SSGs are the tools responsible for the generation of Static Page and that's where the name comes from. Some commonly used Generators are:

-   [Gatsby](https://www.gatsbyjs.org/)
-   [Next.js](https://nextjs.org/)
-   [React Static](https://github.com/react-static/react-static)
-   [Nuxt](https://nuxtjs.org/)
-   [VuePress](https://vuepress.vuejs.org/)
-   More generators at [https://www.staticgen.com/](https://www.staticgen.com/)

### Build and Deployment

Build and Deployment sections are also known as CI (Continuous Integration) and CD (Continuous Deployment). This is where above-mentioned SSGs will execute and generate the Pages and publish them to the designated host.

You can find a guide to [host your static site for free here](https://www.time2hack.com/host-your-static-site-on-gitlab-pages/) and [here](https://www.time2hack.com/ways-to-host-single-page-application-spa-static-site-for-free/)

Popular CI/CD Tools in the market you can choose from:

-   [Netlify](https://www.netlify.com/)
-   [Vercel (now.sh)](https://vercel.com/)
-   [Github Actions](https://github.com/features/actions)
-   [Gitlab CI/CD](https://about.gitlab.com/stages-devops-lifecycle/continuous-integration/)
-   [BitBucket pipelines](https://bitbucket.org/product/features/pipelines)

### CMS (Content Management System)

CMSes are the place where we will manage the Content. This is not necessary for all the JAMStack Sites, though the sites where API is for content, choice of CMS is a crucial part.

For CMSes to play well with the JAMStack, they should be able to execute in a Headless Manner. Some of the popular choices are:

-   [Contentful](https://www.contentful.com/)
-   [Ghost](https://ghost.org/)
-   [Netlify CMS](https://www.netlifycms.org/)
-   [Wordpress (Headless Mode)](https://wordpress.com/)
-   More headless CMSes at [https://headlesscms.org/](https://headlesscms.org/)

- - -

## Conclusion

JAMStack is very fast when done properly. And there are so many choices to make to build a fast solution with JAMStack.

**_What is your JAMStack?_**

Let me know through comments 💬 or on Twitter at  [@patel\_pankaj\_](https://twitter.com/patel_pankaj_)  and/or  [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

Originally published at [https://time2hack.com](https://time2hack.com/what-is-jam-stack-why-you-should-care/) on June 2, 2020.