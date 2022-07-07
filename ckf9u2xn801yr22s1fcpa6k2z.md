## 5 Tips for Component Based Development

Component-based Development has taken the FrontEnd Development by storm.

And Components not being any language standard, there are many ways to Create and Use Components.

Here are some tips to help you with Component Driven Development.

These are not limited to modern frameworks like Angular, VueJS, React. These tips will help in any Component Driven Development/Setup.

---

## Composition
Try to imagine a component (A) having three Child Components (A1, A2, A3). Each of them need different data to render.

And for all three child components, you need to pass data through Parent Component.

```jsx
const App = () => {
  const dataA = {}, dataB = {}, dataC = {};
  const handleClickA = () => {};
  const handleClickB = () => {};
  const handleClickC = () => {};

  return (
    <ComponentA
      dataA={dataA}
      handleClickA={handleClickA}
      dataB={dataB}
      handleClickB={handleClickB}
      dataC={dataC}
      handleClickC={handleClickC}
    />
  );
}

const ComponentA = ({
  dataA,
  dataB,
  dataC,
  handleClickA,
  handleClickB,
  handleClickC
}) => (
  <>
    <ComponentA1 data={dataA} handleClick={handleClickA} />
    <ComponentA2 data={dataB} handleClick={handleClickB} />
    <ComponentA3 data={dataC} handleClick={handleClickC} />
  </>
);
```

With composition, you can rewrite the above arrangement as:

```jsx
const App = () => {
  const dataA = {}, dataB = {}, dataC = {};
  const handleClickA = () => {};
  const handleClickB = () => {};
  const handleClickC = () => {};

  return (
    <ComponentA>
      <ComponentA1
        data={dataA}
        handleClick={handleClickA}
      />
      <ComponentA2
        data={dataB}
        handleClick={handleClickB}
      />
      <ComponentA3
        data={dataC}
        handleClick={handleClickC}
      />
    </ComponentA>
  );
}

const ComponentA = ({children}) => (
  <>
    <h1>Hello world</h1>
    {children}
  </>
);
```

---

## Extract complex logic as functions
Any complex logic that can take an input and provided an output should be extracted as functions. Benefits to extract logic as function are:
* Testable Functions
* Better Code reusability
* Components stay small
* Easy for Code Review
* Components only need to be tested for interactivity

---

## Utilise CSS for common stuff
Functionalities like Hover actions, basic animations look very lucrative achieve with JavaScript. But consider achieving these functionalities in CSS itself.

CSS can achieve some functionalities very easily as compared to JS. Use CSS to your benefit.

```jsx
const App = () => {
  const [hovered, setHover] = useState(false)
  return (
    <Component
      className="container"
      onMouseEenter={() => setHover(true)}
      onMouseEenter={() => setHover(false)}
    >
      <Contact hovered={hovered} />
    </ComponentA>
  );
}

const Contact = ({hovered}) => {
	if (!hovered) {
    return null
  }

  return (
    <a href="mailto:info@time2hack.com">Contact Us</a>
  );
}
```

You can rewrite above components as:

```jsx
const App = () => {
  const [hovered, setHover] = useState(false);

  return (
    <Component
      className="container"
      onMouseEenter={() => setHover(true)}
      onMouseEenter={() => setHover(false)}
    >
      We provide User Interface Development Services
      <Contact className="contact-link"/>
    </ComponentA>
  );
}

const Contact = () => (
  <a href="mailto:info@time2hack.com">Contact Us</a>
);
```

With styles defined in SCSS as:
```scss
.container {
  display: block;

  .contact-link {
    display: none;
  }

  &:hover .contact-link {
    display: block; /* or any other visible display */
  }
}
```

With this way, re-render of component is not needed.

---

## Separation of Concern

Code blocks should only do what they were intended to do. 

Adding more conditions and parameters can make them lengthy and hard to debug and test. 

Take for example from the code block above, the `ContactUs`  component,

```jsx
const Contact = ({hovered}) => {
  if (!hovered) {
    return null
  }

  return (
    <a href="mailto:info@time2hack.com">Contact Us</a>
  );
}
```

Here it is more dependent on hovered prop values for render. Which means this Component needs testing for the various cases of `hovered` prop.

In this case, it is a boolean prop but it will multiply in case of complex object props. 

We can rewrite the component to remove the dependency on the hovered prop. 

The Container component should own the concern of render/no-render with itself.

`Contact` Component’s job is to render Contact Us button/link. Expecting it to do more logical things will introduce more edge cases.

We can either use CSS to handle hide and show the button on Hover; as we saw in the previous section. 

Or, conditionally render `Contact` component from the parent component, which is as follows:
```jsx
const App = () => {
  const [hovered, setHover] = useState(false);

  return (
    <Component
      onMouseEenter={() => setHover(true)}
      onMouseEenter={() => setHover(false)}
    >
      We provide User Interface Development Services
      {hovered && <Contact />}
    </ComponentA>
  );
}

const Contact = () => (
  <a href="mailto:info@time2hack.com">Contact Us</a>
);
```

---

## Use tools at the disposal 
Design Systems, Storybook, unit tests, coverage reports etc. I can go on and list many more tools. But the take away here is “Identify the key tools and get the best out of them”

For example,
### Storybook
Storybook is a great tool for building the basic examples and possible combinations. It also helps to build the documentation of Components.

### Testing
Unit, Integration, E2E etc. will help you Code and Release with Confidence. Disperse your testing in various strategies and keep it sane.

Test cases provide awesome documentation around restrictions and edge cases. *Cover your Code with Tests and Maintain them*.

> You can use Coverage Reports to get Overview on how much Testing has increased and  [add Coverage Report labels to PRs on Github](https://time2hack.com/test-coverage-label-with-github-actions/) 

### Linters
Linters will help you write beautiful code and address syntax/code style propblems. These problems usually appear in code reviews if not take care while development. 

Style Rules like spacing, code style, function signatures etc are common review comments. Avoiding them from start will help in efficient for Code Reviews.

---

> Bonus 😎

## Code for People 🤝
Code for application is easy. Code for people is very hard. 

Code can be very optimized and hard to read at the same time. Hard to read code can make it prone to much common error related to misunderstanding the code. 

Keeping the Lines Small, Easy to Read can lead to better code harmony.

I came across the argument of having a more disciplined team towards Code Structure. This argument is very valid but the code should also be ready for new joiners, be it for Senior Devs to Junior Devs. 

Team discipline can be different from the general community discipline. That’s why, Team discipline & community discipline should be with least friction.

Try to follow some widely used Code Styles like
* https://github.com/airbnb/javascript
* https://github.com/rwaldron/idiomatic.js/
* https://google.github.io/styleguide/jsguide.html
* [elsewhencode/project-guidelines: A set of best practices for JavaScript projects](https://github.com/elsewhencode/project-guidelines)
* [standard/standard: 🌟 JavaScript Style Guide, with linter & automatic code fixer](https://github.com/standard/standard)
* https://freefrontend.com/javascript-style-guides/
* etc.

---

## Conclusion

*With above tips, we can derive a better Front End Code.*

> What challenges did you face while doing Component Driven Development?

Let me know through comments 💬 or on Twitter at [@patel\_pankaj\_](https://twitter.com/patel_pankaj_) and/or [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

#### Credits

* [Expansion, game, puzzle, solution icon](https://www.iconfinder.com/icons/6916977/expansion_game_puzzle_solution_icon)
* Photo by  [UX Store](https://unsplash.com/@uxstore?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/ui-component?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

---

Originally published at [https://time2hack.com](https://time2hack.com/component-driven-development-tips/) on Sep 19, 2020.