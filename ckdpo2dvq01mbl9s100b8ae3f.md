## AutoPublish on GitHub Pages with Github actions

GitHub pages are a great place to host the Demos and Personal Sites. But, often, you need to publish it by yourself.

Github Actions is a new Continuous Integration and Deployment solution from Github.

You can use GitHub Actions to deploy to GitHub pages on different events like
* Push to the main branch
* New Release created
* etc

---

Let's take React app for an example.

Let's make a simple app with Create React App to list latest posts from dev.to

The main idea is of having a GitHub Action to build the app and deploy on GitHub pages.  

Let's initialize new app with CRA and add a new component named `Posts` and `Post`

```sh
npx create-react-app dev-news

cd dev-news # change dir to dev-news

mkdir src/components # create components dir in src

touch src/components/Posts.js \
 src/components/Post.js # create Posts.js & Post.js Files

code . # open VSCode
```

And following are the components to show the list of the posts; I’m putting some barebones of the Component code

```jsx
import React, { useEffect, useState } from 'react'
const devTo = 'https://dev.to/api/articles'

const Post = ({post}) => (
  <a href={post.url} key={post.id} className="post">
    <article>
      <img src={post.social_image} alt={post.title} />
      <div>
        <h1>{post.title}</h1>
        <p>{post.description}</p>
      </div>
    </article>
  </a>
)

const Posts = () => {
  const [posts, setPosts] = useState([])
  useEffect(() => {
    fetch(devTo).then(r=> r.json()).then(setPosts)
  }, [])

  return posts.map(post => <Post post={post} />)
}

export default Posts
```

And it will look like the following

![dev.to-posts](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/dev.to-posts.png)

Now, we will deploy above on the GitHub pages. But, first, let’s make a new repository and push our new project to the repository.

![github.com_organizations_time2hack_repositories_new](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/github.com_organizations_time2hack_repositories_new.png)

And now will commit all the changes to the above repo with following git commands.

```sh
git init # initialize the repo
git add . # stage all changes
git commit -m "🚀 init" # commit the staged changes
# add GitHub origin remote of the new repo
git remote add origin git@github.com:time2hack/dev.to.git
git push -u origin master # push master to the origin
```

Now let’s make a directory with the name `.github` and `workflows` inside `.github`.

Inside the `workflows` directory, we can create workflows which will run the GitHub actions.

As we want to publish to GitHub pages, we will name our workflow file `publish.yaml`

Now in this file, we need to write some actions/commands that need to run in specific order to publish on the GitHub pages

Following will be the contents of our `publish.yaml` file:

```yaml
name: github-pages

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
          publish_dir: ./build
```

Now we will commit the above file and push it to the master.

---

Now let’s take a look at what this YAML file has as Jargon:

1. `name` specifies the name for the script or action. `name` used anywhere in the above file is to display helpful labels on the GH Actions UI instead of commands
2. `on` is the event listener specification, here we want to react only on the `push` to `master` branch
3. `jobs` specify the tasks related to this workflow file. There can be more than one and will be running one by one
4. `deploy` is the job name
5. `steps` describes the steps that need to run for the job
6. `uses: actions/checkout@v2` specifies to execute another action. In this case, it is the `v2` of [GitHub - actions/checkout: Action for checking out a repo](https://github.com/actions/checkout)
7. `with` sets up named parameters for actions or commands
8. `run` executes the command on the shell
9. `id` gives a unique identifier to the step, so that we can access the output of this step in other steps

---

/As the YAML keywords are out of the way, let's see what this workflow’s YAML is doing:/

1. Uses `ubuntu-18.04` as OS/execution environment
2. Uses `actions/checkout@v2`  checks out the repo for the workflow
3. Setup node `12.x` with action `actions/setup-node@v1`
4. Setting environment variable for the Yarn cache
5. Caching the Yarn cache directory with cache key `${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}` . 
Here `hashFiles('**/yarn.lock')` is generating a hash of yarn.lock file.
If this file has not changed, cache with similar hash exists, it will be restored by the `restore-keys`
6. Install dependencies with `yarn install`
7. Build the project with `yarn build`
8. Using action `peaceiris/actions-gh-pages@v3`, deploy the `./build` directory to Github Pages

![github.com_time2hack_dev.to_runs](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/github.com_time2hack_dev.to_runs.png)

> An important thing to note here is that Create React App’s build depends on the `homepage` key in `package.json`

So make your build work with the GitHub pages, you need to set the homepage in `package.json`.

For this example, the repository is [https://github.com/time2hack/dev.to/](https://github.com/time2hack/dev.to/); the Github Pages’ URL or homepage will be [https://time2hack.github.io/dev.to/](https://time2hack.github.io/dev.to/). This URL is in the `homepage` key in `package.json`

---

> *And yes, Don’t forget to enable GitHub pages for your repository*

![enable-github-pages](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/enable-github-pages.png)

---

## Conclusion
We saw how the Github Actions are super easy to deploy the website on Github Pages.

We also saw what are basic parts of Github Actions’ Workflow File & used these parts to write `deploy` job.

Let me know through comments 💬 or on Twitter at [@patel\_pankaj\_](https://twitter.com/patel_pankaj_) and [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

#### Credits

* [WHCompare Servers & Web Hosting icons by Alexiuz AS](https://www.iconfinder.com/iconsets/whcompare-servers-web-hosting)
* Photo by  [Brandi Redd](https://unsplash.com/@brandi1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/website?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

---

Originally published at [https://time2hack.com](https://time2hack.com/create-activate-github-profile-readme/) on Aug 10, 2020.
