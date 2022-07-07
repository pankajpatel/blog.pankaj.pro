## Add Test Coverage Labels with Github Actions

A good Testing setup will help you release with confidence and sleep with peace.

The Testing setup starts with Unit Testing. It increases to the Integration testing, End-to-End testing etc.

How much of code you have tested is measured with Test Coverage.

Knowing Test Coverage while writing the code is easy to know. There is no long waiting time. 

Though how would the people review any Code Change will know about the coverage?

---

In this post, we are going to take a look at a simple GitHub action to test coverage labels to the Pull Requests (PR)

This will help in determining the following:
* General idea of how big the change or PR is
* If the change is big `diff` count, proportionate increase or decrease in Coverage

---

First, let me share the Action YAML file with you to enable the Coverage Labels

```yaml
name: test-coverage

on: pull_request

jobs:
  install-and-test:
    runs-on: ubuntu-latest

    strategy:
      fail-fast: true
      matrix:
        node-version: [10]

    steps:
      - name: Add Running Label
        # https://github.com/actions/github-script#apply-a-label-to-an-issue
        uses: actions/github-script@v2
        with:
          github-token: ${{secrets.GITHUB_TOKEN}}
          script: |
            github.issues.setLabels({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              labels: [':arrows_counterclockwise:']
            })


      - uses: actions/checkout@v2
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}

      # https://github.com/actions/cache/blob/main/examples.md#node---yarn
      - name: Get yarn cache directory path
        id: yarn-cache-dir-path
        run: echo "::set-output name=dir::$(yarn cache dir)"

      - uses: actions/cache@v2
        id: yarn-cache # use this to check for `cache-hit` (`steps.yarn-cache.outputs.cache-hit != 'true'`)
        with:
          path: ${{ steps.yarn-cache-dir-path.outputs.dir }}
          key: ${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}
          restore-keys: |
            ${{ runner.os }}-yarn-

      - name: Install Yarn and Project Dependencies
        run: |
          npm i -g yarn
          yarn install

      - name: Test and Generate Coverage
        run: yarn run test:coverage

      - name: Print the Total Coverage
        id: coverage-percent
        shell: bash
        run: |
          value=`sed -n 47p coverage/lcov-report/index.html | awk -F '>' '{print $2}' | awk -F '%' '{print $1}'`
          echo "::set-output name=coverage::$value"
          echo $value

      # https://github.com/actions/github-script#apply-a-label-to-an-issue
      - name: Add Coverage Label
        uses: actions/github-script@v2
        with:
          github-token: ${{secrets.GITHUB_TOKEN}}
          script: |
            github.issues.setLabels({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              labels: ['COV: ${{steps.coverage-percent.outputs.coverage}}']
            })
```
 

Steps Done above are:
1. Adds the 🔄 label indicating that the Checks are running (/optional/) 
2. Uses `actions/checkout@v2` git capabilities in Actions
3. Uses `actions/setup-node@v1` and sets up the node
4. Sets up Cache for Yarn, installs Yarn and dependencies of the project
5. Runs `yarn run test:coverage` for Test coverage generation
6. Uses `sed` and `awk` to extract coverage % from coverage report HTML
7. Sets the coverage % as the output to use in other steps
8. Removes the 🔄 and adds the label `COV: XX.XX` to the PR

### Sed Command used to extract line from file

```sh
sed -n 47p coverage/lcov-report/index.html
```

Here 47 is the line number where total coverage per cent resides

### AWK command used to clean up the HTML

```sh
awk -F '>' '{print $2}' | awk -F '%' '{print $1}'
```

Here, we break the string down with character `>` & _print the 2nd piece_.

The printed value becomes the input for the next awk command with pipe `|`.

We break the input by character `%` and _print the 1st piece_.

---

## Benefits
1. Labels are much accessible than comments
2. You can see the coverage immediately without needing to open the PR
3. If tests fail, the 🔄 label will stay there. You can see that something went wrong (with the tests, sometimes with other steps)

---

## Drawbacks
1. If you are pushing to your repo and there is an active PR, it will clutter the PR page a lot
2. If you have other Labels that you are relying on, this Action will remove them in the last step

![Label with Github Actions](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/test-coverage-label-with-github-actions-example.jpg)

---

## Assumptions
1. The above action YAML is for JavaScript projects
2. Yarn is used as the package manager, you can find all and replace `yarn` with `npm` and remove the yarn cache steps
3. The test coverage report is in the HTML file generated by `jest`
4. The line number and markup around total coverage %. For other types of test reports, you need to find the position of coverage & adjust the `sed` and `awk` commands

---

## Conclusion
Testing is good when maintained. Coverage is a good indicator of how the codebase is sane.

The goal is not to go crazy make it reach 100% but to stay informed about changes coverage.

*How is Test Coverage helping you in your Development workflow?*

Let me know through comments 💬 or on Twitter at [@patel\_pankaj\_](https://twitter.com/patel_pankaj_) and/or [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

#### Credits

* Photo by  [Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/report?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 
* Icon from [IconFinder](iconfinder.com/icons/3380369/ab_testing_comparison_correction_feedback_test_ui_icon)

---

Originally published at [https://time2hack.com](https://time2hack.com/test-coverage-label-with-github-actions/) on July 20, 2020.