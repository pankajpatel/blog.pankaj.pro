## Getting started with Eleventy (11ty)

JAMStack is Fast. Fast for Development and to deliver ultra Fast websites.

To develop with JAMStack, one of the ways is Static Site Generation.

## Static Site Generators (SSG)
SSGs are the tools which will take the data from any data source and generate Static HTML pages.

Static Sites are way faster than any dynamic site because:
* No content generated on runtime, which means no time spent in this process
* The server doesn’t have to match for the dynamic URLs. HTML files delivered straight to the browser without any Route/URL matching
* As the content is Static, it will be cached for longer time
* Again, being Static, the websites can be delivered through CDNs. This way users don’t have to wait very long for the response.

And to build WebSites with SSGs, Eleventy (11ty) is fast and easy to use the tool.

---

## Eleventy (11ty)
11ty is a JavaScript alternative of Jackyl. It’s built with no-configuration in mind. Though, it supports many templating languages; for example MarkDown, Pug, Handlebars etc.

We will make a simple Blogging website with following features in consideration:
* Index Page with
	* Blog Intro
	* List of Posts
* Blog Post page
* Tags with Tag Index
* Comments with Disqus
* Deployed on Netlify

First, we need to create a Project and add 11ty as the dev dependency, lets do that with following commands:

```sh
# make project directory
mkdir awesome-blog

# switch to the project directory
cd awesome-blog

# initialize the Node Project
yarn init -y

# Add 11ty as a dev dependency
yarn add -D @11ty/eleventy

# open VS Code in the directory
# (if you use VSCode and have set up CLI command)
code.
```

Now we edit the `package.json` file to add the following to the scripts:
```json
{
  ...
	"scripts" : {
    "start" : "eleventy --serve"
  },
  ...
}
```

After adding `start` script in the package.json, launch `yarn start` on the root of your Project directory from CLI.

Now that 11ty is up and running, we need to add some content to see it building.

By default, 11ty will output the generated HTML files in `_site` directory.

Let's go ahead and create out index page with `index.md` file in the root of the project as:

```md
# Hello World
---
Welcome to the `awesome-blog`
```

Which will get generated as the following `body` in `_site/index.html`:
```html
<h1>Hello World</h1>
<hr>
<p>Welcome to the <code>awesome-blog</code></p>
```

Well, that was super simple; and blank with no CSS. Let's try to add some Style by adding Bootstrap CSS.

But where do we add them? This is where layouts in the 11ty come into the play.

As the name suggests, Layouts are the Page Generator Templates where the data can be filled by selected page.

The layouts must be inside the `_includes` directory.

Lets try to make a Handlebar layout as `home.hbs` for the home page:
```hbs
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{ title }}</title>
  <link rel="stylesheet" href="//stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" />
</head>
<body class="p-3">
  <div class="container">
    {{{content}}}
  </div>
</body>
</html>
```

To use the above template, we need to add some context to our markdown file. We will add the context with the FrontMatter format inside the markdown.

FrontMatter is the data format go git more contextual data about the file. For a blog post, it can be title, tags etc. For a landing page, it can be with Sections, Images and other blocks of information.

For our markdowns, we will add `title` and `tags` in the front matter all the time and use it in our layouts.

So with decided FrontMatter, here is our `index.md`:
```md
---
layout: layouts/home.hbs
title: Hello World
tags: [intro, random, gif]
---
# Hello World

---

Welcome to the `awesome-blog`

Here we will add some awesome gifs rolling around the internet.

Starting with this:

![me thinking 'I can do this'](https://media.giphy.com/media/YTJXDIivNMPuNSMgc0/giphy-downsized.gif)
```

Now we have Title repeating two times, we will keep the FrontMatter and remove form the content of the markdown to make it as follows:
```md
---
layout: layouts/home.hbs
title: Hello World
banner: https://placeimg.com/1000/480/nature
tags: [intro, random, gif]
---
Welcome to the `awesome-blog`

Here we will add some awesome gifs rolling around the internet.

Starting with this:

![me thinking 'I can do this'](https://media.giphy.com/media/YTJXDIivNMPuNSMgc0/giphy-downsized.gif)
```

And with this, we will update our layout as following inside the `body`:

```html
<body class="p-3">
  <div class="container">
    <h1>{{title}}</h1>
    {{#if banner}}
      <img src="{{banner}}" alt={{title}} />
    {{/if}}
    {{#if tags}}
      <div class="tags">
        {{#each tags}}
          <div class="badge badge-dark">{{this}}</div>
        {{/each}}
      </div>
    {{/if}}
    <hr />
    {{{content}}}
  </div>
</body>
```

Now with basics in mind, we want to make the blog posts and make all the posts listed on the homepage.

In 11ty, by default, the HTML files will be generated in the same directory structure as of the data file.  And the URLs will be like the data file-names.

So, in our case, we can make all the _posts_ inside the `posts` directory with post `slug` as the file-names

And our Markdown files will follow the following structure:
```
/
├── index.md
└── web
    ├── hello-world.md
    ├── ...
    └── trip-to-new-york.md
```

---

Now we need to add these post lists in the Home Page.

For this, let’s make a rough try making the data of post in the FrontMatter of the Home Page and create a new layout for it.

For the list of post, I will approach the data of front matter as follows:
```
title: Home
posts:
- 0:
  title: Hello World
  url: posts/hello-World/
  banner: //source.unsplash.com/user/pankajpatel/1024x400
- 1:
  title: Random Post
  url: posts/random/
  banner: //source.unsplash.com/user/pankajpatel/likes/1024x400
```

And the layout can be change to the following:

```pug
<body class="p-3">
  <div class="container text-center">
    <h1 class="m-5">{{title}}</h1>

    {{#if posts}}
      <div class="row">
        {{#each posts}}
          <a class="col mb-3 text-decoration-none" href={{this.url}} data-index={{@key}}>
            <article class="card" href={{this.url}} data-index={{@key}}>
              <img src="{{this.banner}}" class="card-img-top" alt="{{this.title}}">
              <div class="card-body text-left">
                <h1 class="card-title font-weight-light">{{this.title}}</h1>
                <p class="card-text"><small class="text-muted">Last updated 3 mins ago</small></p>
              </div>
            </article>
          </a>
        {{/each}}
      </div>
    {{/if}}
    <hr />
  </div>
</body>
```

But this FrontMatter data were prepared manually. What can we do to build it automatically?

It’s time to get our hands dirty with configuration files.

For 11ty, the config file is `.eleventy.js`

In 11ty’s config file,  there needs to be one exported function. This function accepts the current eleventyConfig.

The current eleventyConfig has some API methods to define different behaviours like:
* Adding/Modifying collection
* Adding new filters
* etc

For us, the concerned part is to add a new collection for our posts and use this collection to list the posts on the homepage.

To get the collection of all the posts, we had created FrontMatter data `type`. And for all the posts, we set the `type` as `post` 

Now our 11ty config looks like the following:

```js
module.exports = (eleventyConfig) => {
  eleventyConfig.addCollection("posts", (collection) => {
    return collection.getAll().filter((item) => {
      return 'type' in item.data && item.data.type === 'post'
    })
  })
}
```

Now with above added `posts` collection, we can update our homepage template as:
```hbs
<body class="p-3">
  <div class="container text-center">
    <h1 class="m-5">{{title}}</h1>

    {{#if collections.posts}}
    <div class="row">
      {{#each collections.posts}}
      <a class="col mb-3 text-decoration-none" href={{this.data.page.url}} data-index={{@key}}>
        <article class="card" href={{this.data.url}} data-index={{@key}}>
          {{#if this.data.banner}}
            <img src="{{this.data.banner}}" class="card-img-top" alt="{{this.data.title}}">
          {{/if}}
          <div class="card-body text-left">
            <h1 class="card-title font-weight-light">{{this.data.title}}</h1>
            <p class="card-text"><small class="text-muted">Last updated 3 mins ago</small></p>
          </div>
        </article>
      </a>
      {{/each}}
    </div>
    {{/if}}
    <hr />
  </div>
</body>
```

Now that our homepage and posts are ready, we can publish it.

To publish our site, first, we need a git repository and commit the changes.

```sh
git init
echo "node_modules\n_site" > .gitignore
git add .
git commit -m "🚀 personal blog launch initiated"
```

Now you have committed the code on your local repo. You can create a repository on GitHub and add remote to your local repo. Then you can push your branch and it is available remotely.

Now it is time to publish our blog through this repository.

Following are the way to publish the website:

### Publish on Netlify

Netlify publish is a matter of a few clicks.

* Log into Netlify with Github from here: [Netlify App](https://app.netlify.com/)
* Click on `New Site from Git` button
* Connect to your Github if not connected
* Select the repo that you had before created
* Netlify will detect the type of Project and will suggest a build command
* Click on `Deploy Site`
* Yous site is deployed

You can watch the following video to see the above steps in action

[11ty on Netlify - YouTube](https://youtu.be/zLulIIeYreo)


### Publish on GitHub Pages

To publish on GitHub pages, you need to add build script to your `package.json`

You can do so by adding following line the scripts:
```json
{
  ...
	"scripts" : {
    "build" : "eleventy",
    "start" : "eleventy --serve"
  },
  ...
}
```

Now that the build script is added. We need to add [GitHub action to Auto Publish our site to Github Pages](https://time2hack.com/auto-publish-github-pages-github-actions/). Following is the YAML file placed at `.github/workflows/publish.yaml`

```yaml
name: publish

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"

      - name: Get yarn cache
        id: yarn-cache
        run: echo "::set-output name=dir::$(yarn cache dir)"

      - name: Cache dependencies
        uses: actions/cache@v1
        with:
          path: ${{ steps.yarn-cache.outputs.dir }}
          key: ${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}
          restore-keys: |
            ${{ runner.os }}-yarn-
      
      - name: Installing Dependencies
        run: yarn install
      
      - name: Building App
        run: yarn build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./_site
```

Now you need to enable the GitHub pages from your repository’s Settings. 

![gh-pages-11ty](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/gh-pages-11ty.png)

Now commit and push the above file. This will trigger the build and publish the site 

![gh-pages-11ty-buil-and-publish](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/gh-pages-11ty-buil-and-publish.png)

---

## Conclusion
11ty is loved and recommended by many people. And after using it for a while, I also think that it is by far the simplest Static Site Generator.

What do you think?

Let me know through comments 💬 or on Twitter at [@patel\_pankaj\_](https://twitter.com/patel_pankaj_) and/or [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.
---

#### Credits

Photo by  [Andrew Neel](https://unsplash.com/@andrewtneel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/website?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

---

Originally published at [https://time2hack.com](https://time2hack.com/getting-started-with-eleventy-11ty/) on Sep 11, 2020.