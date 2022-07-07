## Why aren't you using Aliases in webpack config?

Are you Developer doing FrontEnd? Are you using Webpack?

If any answer is No, you can skip this post. 

But if Yes, are you using aliases in your webpack configuration?

If Yes, you can leave this page. 

If No, then let me ask you a question:

> **Why not?**

---

## Problem

Let’s start with the problem here. 

In a large scale application where the codebase is huge and you often see imports like the following:

```
import Gallery from '../../../commons/components/Gallery'

import { getUserPrefefences } from '../../../utils/storage/browser/user'
```

The `../` or relative imports are the concern here. 

For a small codebase, these are fine but for a large codebase where the developer works in parallel and move things around frequently, this introduces the following problems:

* Need to mentally traverse the directory 
* Spend time in fixing the *imported modules not found*

And for the new developers in the teams, this problem becomes 10 folds

---

## Solution

As you have read the title of the article, this problem can be solved by using aliases in the repack config. 

How does it work?

To explain the usage of aliases; consider the following directory structure:

```
webpack.config.js
src
  - commons
    - components
      - Gallery
        - Gallery.js
      - User
        - User.js
      - Avatar
        - Avatar.js
      - Button
        - Button.js
  - pages
    - components
		- Layout
        - Wide.js
      - Grid
      - Page
    - Messages.js
    - Profile.js
    - Dashboard.js
    - Login.js
    - Register.js
  - utils
    - url

```


For above structure

Consider the following scenarios:

* Accessing some component in the `Dashboard.js` would look like the following:
```js
import React from 'react'
import WideLayout from './components/Layout/Wide'
import Gallery from '../commons/components/Gallery/Gallery'
```

Now we try to rearrange the tree structure and put the gallery Component in the directory: `<project-root>/src/pages/components/commons/Gallery`

And not we have to refactor the above code to keep working. As our project structure taken for example is small, so it is easy to remember the Component rearrange in code as well:

```diff
  import React from 'react'
  import WideLayout from './components/Layout/Wide'
- import Gallery from '../commons/components/Gallery/Gallery'
+ import Gallery from './components/commons/Gallery/Gallery'
```

Now let’s try to add a few lines to our web pack config:

```js
module.exports = {
  //...
  resolve: {
    alias: {
      Components: path.resolve(__dirname, 'src/commons/components/'),
      Pages: path.resolve(__dirname, 'src/pages/')
    }
  }
};

```

What above lines in your web pack configuration will do is to enable you to write the previous component	file in the following manner:

```js
import React from 'react'
import WideLayout from 'Pages/components/Layout/Wide'
import Gallery from 'Components/Gallery/Gallery'
```

Hence, now you exactly know where these components are loaded from, provided that you know about the aliases in your config.

And then refactoring the components will also be less painful.

---

You can read more about the Aliases in the webpack configuration here: [Resolve | webpack](https://webpack.js.org/configuration/resolve/#resolvealias)

---

# Conclusion

Using Aliases is a way to speed up the development; though not in all cases:

* Small projects don’t need it
* Bigger Teams need to agree on Aliases
* Bigger projects should include the list of Aliases in the ReadMe files


> What do you guys think about the Aliases?

> Would you use them?

> Why or Why not?


Let me know what do you think about this article through comments 💬 or on Twitter at [@patel_pankaj_](https://twitter.com/patel_pankaj_) and [@time2hack](https://twitter.com/time2hack) 

If you find this article helpful, please share it with others 🗣; subscribe for the new posts and see you the next time.

---

Originally published at [https://time2hack.com](https://time2hack.com/why-are-you-not-using-aliases-in-webpack-config/) on December 4, 2019.