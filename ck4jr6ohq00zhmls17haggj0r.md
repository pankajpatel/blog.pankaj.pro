## NPX: work faster with npm package binaries

With Node.js, building CLI utilities and development tools have gotten so much easier.

Though, it also means that you need to install the CLI package on your computer globally, to use/execute that package as a regular bash command.

Like for example, a little while ago, I create a utility called `list-repos` which allowed me to check the status of the Git repositories in a directory. You can read more about it here: https://time2hack.com/introducing-list-repos/

> `list-repos` does some cool stuff if you are working on so many open source projects with Git

I can ramble more about the utility I created, but that’s not important for this post here.

Important thing is that, to use this utility; you need to install it globally on your computer as the following command:

```bash
npm i -g list-repos
```

And then to use it, you need to execute the following command:

```bash
list-repos .. # from any project

list-repos # parent where all projects reside
```

Now with new versions of the npm, it installs another utility called `npx`

## What is NPX?
This utility will allow you to execute any executable package without installing it globally.

This means that now you don’t need to fire `npm i -g list-repos`

---

## How to use NPX?

> So, how to use `npx`?

You need to provide the following things to `npx`:
* package name, let’s say `my-package`
* parameters that need to be passed to `my-package`

This means that, for `list-repos`, all you need to do is to fire following command:

```bash
npx list-repos ..
```

---

## Passing params bash style
You can pass the params to the binaries in a similar way you would pass the arguments to any bash utility.

![List Repos with NPX](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/npx-output.png)

---
## A bit of the History

Originally, *npx* started in May 2017; it was a npm package installable as other npm binaries from [npx  -  npm](https://www.npmjs.com/package/npx) 

And now it is part of npm and installed by default.

So if your system says that `npx` is not found, you can 
* either update the npm by `npm i -g npm` 
* or just install `npx` on current npm as `npm i -g npx`

---

## Using with NVM

If you are using nodejs with nvm, then it can be a bit tricky. 

* If you are using the npm version which *internally supports npx* 
    * moving to a version which doesn’t, then
        * you can install *npx* manually
        * or update npm on that node version
    * moving to a version which does have npx
        * then you can use it as usual

* If you are using npm version which internally *doesn’t support npm*
    * moving to a version which supports
        * then you can enjoy using *npx*
    * moving to a version which also doesn’t support npx
        * then you can install node with flag
 `-—reinstall-packages-from=<from-node-version>`; with new command as 
```bash
nvm install v6.9.2 --reinstall-packages-from=v4.4.5
```

---

## Few Hacks with NPX

Use aliases on your preferred terminal to assign some aliases to your favorite commands
```bash
alias lrs="npx list-repos"
```

If you have already installed any npm package globally on your computer, npx will pick it up from your global installation.

And if any package is added as a dependency in your node project and you are using npx in your `npm scripts`, npx will use the package form local dependency space i.e. `node_modules`

This give a chance to use packages like `yarn`, `create-react-app` or any similar binary always from the latest version.

(Almost) No need to reinstall the latest version and then retry to use the binaries.

---

## Conclusion

npx is a cool utility to make use of in the daily development workflow. Though it still does not replace the globally installable package because if the package is not installed, npx will always take the package from the internet.

And which might not be a very happy case of
* Slow Internet Connection
* Inconsistent Internet Connection
* No Internet Connection for longer time

And also it takes some time to download the package and its dependencies to execute locally.

So let me know *how would you make use of npx* and *what do you think about this article* through comments 💬 or on Twitter at  [@patel_pankaj_](https://twitter.com/patel_pankaj_)  and  [@time2hack](https://twitter.com/time2hack) 

If you find this article helpful, please share it with others 🗣; subscribe to the blog for new posts and see you the next time.