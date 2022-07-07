## Fixing WebpackChunkName for Dynamic Imports

Lazy Loading is a hot topic for optimization of the web application.

I was trying to optimize the React App and as we already have `splitChunks` in our webpack configuration, it was for granted to pay more attention to code splitting.

---

I thought of analyzing our bundle with [Webpack Bundle Analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) and see how `splitChunks` has a done the splitting.

For some reason, I could not identify the Chunks by name as they were pretty random as `1234.asdfd23534kjh346mn63m46.chunk.js`

![](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/webpack-chunk-name-not-working-time2hack.png)

So to resolve this, I updated the `chunkName` in `output` of webpack config to `[name].[contenthash].chunk.js`

But still no luck! Bundle analyzer was still showing the chunk names similar to `1234.asdfd23534kjh346mn63m46.chunk.js`

> 🚀 Web Search to the rescue, I found Magic Comments in Webpack

And to name my chunks I added magic comments similar to following on all dynamic imports in the codebase

```
export default Loadable({
  loader: () => import(
    /* webpackChunkName: Dasahboard */
    './containers/Dashboard'
  ),
  loadaing: () => <Spinner />
})
```

*Still no luck 😕*

---

> Getting on to more Web Search 💪

Then I came across a comment in one of the web pack’s repo:

> Turn the comment `on` in your babel configuration for the project

[![](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/github.com_webpack_webpack_issues_5857.png)](https://github.com/webpack/webpack/issues/5857#issuecomment-338118561)

After struggling for a few minutes and a few trial and errors, I realised that I don’t need to configure comments in babel configuration. Its because I am using the presets in Babel; comments are on by default.

---

*Still no luck 😫. Magic Comments are not reaching Webpack.*

If Magic Comments (or Any Comment) are not reaching webpack, then they are lost in transpiling process. Which means I need to dig deeper in Babel Configuration.

Then I started going through all of the plugins in the Babel configuration.

```json
{
  ...
	"plugins": [
    "dynamic-import-webpack",
    "@babel/plugin-proposal-class-properties",
    "@babel/plugin-syntax-object-rest-spread",
    [
      "@babel/plugin-transform-runtime",
      {
        "corejs": 3
      }
    ]
  ],
  ...
}
```

From this list of plugins, the only plugin that might be the culprit is `dynamic-import-webpack`

A small plugin to make dynamic imports i.e. `import()` work. Which you can see here: [GitHub - airbnb/babel-plugin-dynamic-import-webpack: Babel plugin to transpile import() to require.ensure, for Webpack](https://github.com/airbnb/babel-plugin-dynamic-import-webpack)

What’s special here? The First line of the Readme of the repo:
> Babel plugin to transpile `import()` to `require.ensure`, for Webpack.

And this is what is causing all the trouble. As imports are transformed to `require.ensure` there are no more magic comments.

![](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/webpack-chunk-name-fixed-time2hack.png)

---

So as a solution, removed this plugin `dynamic-import-webpack` from Babel and Magic Comments take effect in Webpack.

Now the Chunks have names similar to  `List. asdfd23534kjh346mn63m46.chunk.js`

---

## Conclusion

Configuring webpack can be tricky when there are so many things going on. If you want the Chunks to be named properly; I would suggest going through the following checklist:

* `chunkName` in output is configured properly
* Magic comment `/* webpackChunkName: MyChunk */` is used to name the chunk
* Babel is configured to NOT remove the comments
* And remove plugin `dynamic-import-webpack`

### Bonus tip

Use `webpackPrefetch: true` magic comment with `webpackChunkName` . And consider adding service workers with good caching strategy.

This will cache the Files on Browser and avoid problems related to Chunks not found (Chunk loading failed) with multiple deploys.

As you are using `[contenthash]` in the output file names, only the changed modules will be cached again by service workers, not all the files. 


---

Let me know through comments 💬 or on Twitter at  [@patel_pankaj_](https://twitter.com/intent/follow?screen_name=patel_pankaj_)  and/or  [@time2hack](https://twitter.com/intent/follow?screen_name=time2hack) 

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

#### Credits

Photo by  [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/webpack?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

---

Originally published at [https://time2hack.com](https://time2hack.com/fixing-webpackchunkname-for-dynamic-imports-in-webpack-with-babel/) on Jan 21, 2021.