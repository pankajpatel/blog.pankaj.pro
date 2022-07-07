## Where do you initialize state in React Component?

Functional Components are super cool. Though, Classical components are still used & so is the conventional state in those.

State initialization is a very common scenario while developing the components. 

But where do you initialize your components’ state?

Let’s looks at the places where it could be initialized. 

---

## The obvious constructor
One obvious place to initialize is the constructor of the component. Similar to the following:

```jsx
class Contacts extends React.Component {
  // ...
  constructor(props) {
    super(props)
    this.state = {
      isLoading: false,
      contacts: [],
      errors: []
    }
  }
  // ...
}
```

### Pros:
* Very visible and verbose
* Can access props to determine the new state

### Cons:
* Unnecessary use of super and constructor
* Can use props but most of the time it is not needed

---

## The Class property way
```jsx
class Contacts extends React.Component {
  // ...
  state = {
    isLoading: false,
    contacts: [],
    errors: []
  }
  // ...
}
```

### Pros:
* Follows the OOP style of the property declaration
* Verbose

### Cons:
* Can’t use props for initialisation
* Less readable for to those who prefer old-style JS

---

## Arguments
### Consistency
If you are using one style, you should follow the same style everywhere. As the software will always be evolving, consistency should not block you from writing better code.

### Readability
Expecting some pieces of code to be there. Is state is there, I would expect it to be in the constructor or at the beginning of the component. Readability is subjective and prone to habitual needs.

### Tools
Modern dev toolchain in Front End will allow you to write small and readable code. And with transpiling (transform + compile), it will be usable for all Browsers.

Using tools at disposal will bring more creative ways to solve problems.

### Legacy Code
If the code is Legacy and is stopping you to write better code, it’s time to. Refactor.

### ReactJS Specific Reasons
* Start thinking about Functional Components and Hooks
```jsx
const Header = props => (
  <header>
    <h1>Title</h1>
    <strong>SubTitle</strong>
  </header>
)
```

* Keep the state minimal, try to move the state to parent and use a prop to pass it down

* Stateless components are Better as they are better testable
```jsx
const Button = props => {
  const [disabled, setDisabled] = useState(false)
  return (
     <button
       disabled={disabled}
       onClick={() => setDisabled(prev => !prev)}
     >
       {props.text}
     </button>
  )
}

// can become
const Button = props => (
   <button
     disabled={props.disabled}
     onClick={props.setDisabled}
   >{props.text}</button>
)
```

* Compose Components from Props
```jsx
const Button = props => (
   <button
     disabled={props.disabled}
     onClick={props.setDisabled}
   >{props.spinner}{props.text}</button>
)

// can become
// children will hold spinner
// & parent can decide when to show/hide spinner
const Button = props => (
   <button
     disabled={props.disabled}
     onClick={props.setDisabled}
   >{props.children}</button>
)
const App = () => {
  const [loading] = false
  return <Button>
    {loading && <Spinner />}
    <span>Click me</span>
  </Button>
}
```
* Use DefaultProps in case of Class components
* Use prop destructuring along with Default Values for Functional components
```jsx
const Button = ({
  disabled = false,
  setDisabled = () => {},
  children = null
}) => {
  if (!children) {
    // Dont render without any Button content
    return null 
  }
  return (
    <button
      disabled={disabled}
      onClick={setDisabled}
    >{children}</button>
  )
}
```

---

## Conclusion

A small thing to ask where to initialise the state. But in a large codebase, these decisions will improve your daily code efficiency.

> What style do you prefer?

Let me know through comments 💬 or on Twitter at [@patel\_pankaj\_](https://twitter.com/patel_pankaj_) and/or [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

#### Credits

Photo by  [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/react?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

---

Originally published at [https://time2hack.com](https://time2hack.com/reactjs-component-state/) on Nov 4, 2020.