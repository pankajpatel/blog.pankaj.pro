## Now you can manage Aliases for FrontEnd Workflow at one place

Aliases are very handy; be it in CLI or in FrontEnd development.

In previous post, we had seen the [benefits of Aliases in Webpack](https://time2hack.com/why-are-you-not-using-aliases-in-webpack-config/)

But what about other bundlers like Rollup? Or Test environment like Jest?

---

## Problems

### Fragmentation
In different tools, there is a different way to define aliases and this makes tricky to define and maintain the aliases.

### Source of Truth
Another trouble is to maintain a single source of truth for the Aliases, as this configuration is everywhere, who gets to become the source of truth.

### Onboarding Experience
Also for new joiners of Team, its hard to wrap the head around aliases once you are using them everywhere.

### Manual Knowledge
These many configurations are Manual Knowledge because
* Has to be maintained manually
* Documentation is spread around and can easily go out of sync
* Once the person who introduced aliases is gone, it starts falling apart

---

## Solution
To solve the above mentioned problems, here is *Alias-HQ*

Alias-HQ picks up the configuration style of [TypeScript Config](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) (`tsconfig.json`) or [VSCode Project Config](https://code.visualstudio.com/docs/languages/jsconfig) (`jsconfig.json`)

And from those configs allows plugin style usage and manipulation of the aliases

And once the aliases configs are in place, you can imply import those aliases in your Bundler or Test setup scripts.

---
Lets consider following `jsconfig.json`:

```json
{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": {
      "@components/*": ["components/*"],
      "@assets/*": ["assets/*"],
      "@config/*": ["config/*"]
    }
  }
}
```

Now with the above project config, you can add aliases to your webpack in `webpack.config.js` as:
```js
const aliases = require('alias-hq')

module.exports = {
  ...
  resolve: {
    alias: aliases.get('webpack')
  }
  ...
}
```

For rollup in `rollup.config.js` as:
```js
import alias from '@rollup/plugin-alias';
import aliases = require('alias-hq')

module.exports = {
  ...
  plugins: [
    alias({
      entries: aliases.get('rollup')
    })
  ]
};

```

Or jest in `jest.config.js`
```js
import aliases from 'alias-hq'

module.exports = {
  ...
  moduleNameMapper: aliases.get('jest'),
}
```

If are configuring Jest through `package.json`, you have to move to `jest.config.js` to be able to use AliasHQ for dynamic alias mapping generation

---
### What about using aliases in Create React App?

To customise CRA, you need to use some packages which will tap into the configs from CRA. Though CRA does not recommend it and might break.

Such packages are:
* [timarney/react-app-rewired: Override create-react-app webpack configs without ejecting](https://github.com/timarney/react-app-rewired)
* [arackaf/customize-cra: Override webpack configurations for create-react-app 2.0](https://github.com/arackaf/customize-cra)
* [gsoft-inc/craco: Create React App Configuration Override](https://github.com/gsoft-inc/craco)
* [harrysolovay/rescripts: 💥 Use the latest react-scripts with custom configurations for Babel, ESLint, TSLint, Webpack,… ∞](https://github.com/harrysolovay/rescripts)

Another way is to eject the configs to take full control over the Bundling and Testing configuration.

I tried to tap into webpack and jest configs with [react-app-rewired](https://github.com/timarney/react-app-rewired), and this is my config to allow overrides and inject aliases:
```js
// config-overrides.js
const aliases = require('alias-hq')

module.exports = {
  // The Webpack config to use when compiling your react app for development or production.
  webpack: (config, env) => ({
    ...config,
    resolve: {
      ...(config.resolve || {}),
      alias: {
        ...(config.resolve.alias || {}),
        ...aliases.get('webpack')
      }
    }
  }),
  // The Jest config to use when running your jest tests - note that the normal rewires do not
  // work here.
  jest: (config) => ({
    ...config,
    moduleNameMapper: {
      ...(config.moduleNameMapper || {}),
      ...aliases.get('jest')
    }
  })
}
```

With alias path configs in `jsconfig.json` as:
```json
{
  "compilerOptions": {
    "baseUrl": "./src",
    "paths": {
      "@components/*": ["components/*"],
      "@reducers/*": ["reducers/*"],
      "@helpers/*": ["helpers/*"]
    }
  }
}
```

---

### But what about TypeScript Project?

Though if you are using TypeScript, the configuration for `alias-hq` are picked up from `tsconfig.json`, so you need to add the path settings to `tsconfig.json` as:
```json
{
  "compilerOptions": {
    ...,
    "baseUrl": "./src",
    "paths": {
      "@components/*": ["components/*"],
      "@reducers/*": ["reducers/*"],
      "@helpers/*": ["helpers/*"]
    }
  },
  ...
}
```

In some cases, the configuration of paths might be invalid and get removed automatically. 

In such case, [you will need to make a base config for TypeScript compiler and extend the main config with the base config](https://github.com/facebook/create-react-app/issues/8909#issuecomment-624681463)

```json
// tsconfig.base.json
{
  "compilerOptions": {
    ...,
    "baseUrl": "./src",
    "paths": {
      "@components/*": ["components/*"],
      "@reducers/*": ["reducers/*"],
      "@helpers/*": ["helpers/*"]
    }
  },
  ...
}
```


And the main config of TypeScript Compiler looks like: 
```json
// tsconfig.json
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {}
}
```


---

## Conclusion

Here we learned how to use AliasHQ to add aliases to
* Webpack
* Rollup
* Jest
* Create React App

> How do you manage your aliases?

Let me know through comments 💬 or on Twitter at [@patel\_pankaj\_](https://twitter.com/patel_pankaj_) and/or [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

#### Credits

* Photo by  [Ferenc Almasi](https://unsplash.com/@flowforfran)  on  [Unsplash](https://unsplash.com/s/photos/javascript) 
* Icons by [@bitfreak86 on Iconfinder](https://www.iconfinder.com/bitfreak86)

---

Originally published at [https://time2hack.com](https://time2hack.com/aliases-in-frontend-development-workflow/) on Aug 26, 2020.