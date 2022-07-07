## FREE Image Hosting with Gitlab

Images are an amazing way to show information and create interest in readers.

But good representations means big and sharp images. And that comes at the cost of file size and bandwidth cost.

Today we will see how to use GitLab pages, or any Git service provider's static site feature as image hosting.

----

## Setup
It is fairly simple.

%[https://www.youtube.com/watch?v=gO7YobILPdI]

Create a repository for images or files you wanna host. Let's say for example images is the repository

After creating the repository, just create a directory named public as this directory gets published to the static pages. You can create the public directory either by using the web tool or by cloning and committing the director with .gitkeep file.

Now as we have the public directory present; let's go ahead and set up out pipeline to build and publish the static site; Initially, we will use the following `.gitlab-ci.yml` code to build our pipeline:

```
image: alpine:latest

pages:
  stage: deploy
  script:
  - echo 'Nothing to do...'
  artifacts:
    paths:
    - public
  only:
  - master
```

We took this code from the previous pose about [how to host a static site on gitlab]( https://time2hack.com/2018/07/host-your-static-site-on-gitlab-pages/)
And we are ready.

Now we just need to upload the file before linking to the blog or any other place and they are available as a CDN.

------

## Optimizations

As we have the hosting now, we can think about optimizing it.

For optimization, we will try the compression for the images. And for compression, we will be using the package called [`imagemin`](https://www.npmjs.com/package/imagemin) We choose to use this package because it has the possibilities to use plugins to choose from varieties of compression algorithms.

To begin with compression, we will create a `package.json` and install the required packages. 
```
npm i -S imagemin imagemin-mozjpeg imagemin-pngquant imagemin-webp
```
And then the index.js files which will execute every time we make a commit to the repository.
```
const fs = require('fs');
const mkdirp = require('mkdirp');
const getDirName = require('path').dirname;

const imagemin = require('imagemin');
const imageminWebp = require('imagemin-webp');
const imageminMozjpeg = require('imagemin-mozjpeg');
const imageminPngquant = require('imagemin-pngquant');

const config = {
  sourceDir: 'images',
  destinationDir: 'public',
  preserveDirectoryStructure: true,
  patterns: [
    { // compress jpegs
      pattern: '/**/*.{jpg,jpeg}',
      plugins: [
        imageminMozjpeg(),
      ]
    },
    { // compress pngs
      pattern: '/**/*.{png}',
      plugins: [
        imageminPngquant({ quality: [0.7, 0.8] }),
      ]
    },
    { // convert all images to webp
      pattern: '/**/*.{jpg,jpeg,png}',
      plugins: [
        imageminWebp({quality: 70}),
      ]
    }
  ]
}

const flattenDeep = (arr) => {
   return arr.reduce((acc, val) => Array.isArray(val) ? acc.concat(flattenDeep(val)) : acc.concat(val), []);
}

const saveFile = (path, buffer) => {
  return new Promise((resolve, reject) => {
    mkdirp(getDirName(path), function (err) {
      if (err) return cb(err);

      fs.writeFile(path, buffer, (err, result) => {
        if (err) reject('error writing file: ' + err);
        else resolve(result);
      });
    });
  })
}

Promise.all(
  config.patterns.map(({plugins, pattern}) => {
    const options = { plugins };
    const glob = [`${config.sourceDir}${pattern}`]
    if (!config.preserveDirectoryStructure) {
      options.destination = config.destinationDir;
    }
    return imagemin(glob, options);
  }),
)
.then(flatten)
.then(files => Promise.all(
  files.map((file) => saveFile(
    file.sourcePath.replace('images', config.destinationDir),
    file.data
  ))
))
.catch(console.error);
```

And to run this JS file on every commit, we need to update our `.gitlab-ci.yml` file as following:

```
image: node:latest

cache:
  paths:
    - node_modules/
pages:
  script:
    - npm install
    - node index.js
  artifacts:
    paths:
      - public
  only:
    - master
```

And it will optimize the images and publish the `public` directory.

-----

## Conclusion

You can use any free Git provider with Pages feature to have Free hosting. Which one you prefer or would use for your Image hosting?

Let me know what do you think about this article through comments 💬 or on Twitter at [`@patel_pankaj_`](https://twitter.com/patel_pankaj_) and [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣 and see you the next time.

-----

This article was originally published on [Time to Hack](https://time2hack.com) at [https://time2hack.com/2019/07/free-image-hosting-with-github-gitlab-pages/](https://time2hack.com/2019/07/free-image-hosting-with-github-gitlab-pages/)