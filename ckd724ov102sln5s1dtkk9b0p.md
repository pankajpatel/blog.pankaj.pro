## Animating the Progress Percent Change in React

Visual Feedback is very important in UI design. It keeps the user informed and engaged with their action.

One of that visual feedback is showing Progress related to the action via Percent. There are two ways to show this feedback
* Progress Bar
* Text % updating

The Progress Bars are easy as there is an HTML element for that. Here is an example for Progress Bars:
```html
<progress id="file" max="100" value="70">70%</progress>
```

And text `%` is a `span` 😏

```html
<span>70%</span>
```

In the case of text representation, there is no visible change or transition from the old value to the new value.

---

Here we will take a look at animating the change in number in React Component

For let’s see a basic component to see a progress in basic text:

```jsx
export default ({ value = 0, symbol = '%' }) => (
  <span>{value}{symbol}</span>
)
```

Now, to animate and visualize a change in values, we need an intermediate value. 

Let’s add a used state function

```jsx
export default ({ value = 0, symbol = '%' }) => {
  const [display, setDisplay] = useState(value)

  return <span>{display}{symbol}</span>
}
```

Now we need to make the intermediate values increase but slow enough to make the changes visible.

We will achieve this by `setInterval` and increment the intermediate value by `1`. We are using `1` because we are trying to show at the per-cent increase by step of one. You can choose to have other values for the steps and make necessary changes. 

```jsx
export default ({ value = 0, symbol = '%' }) => {
  const [display, setDisplay] = useState(value)

  setInterval(() => {
    setDisplay(val => val < value ? val+1 : val)
  }, 50)

  return <span>{display}{symbol}</span>
}
```

This will keep running the interval till infinity; so we need to stop it when we don’t need it. 

We need to keep the reference of the interval and clear it later. We will store its reference with the hook `useRef`

```jsx
export default ({ value = 0, symbol = '%' }) => {
  const interval = useRef(null)
  const [display, setDisplay] = useState(0)

  interval.current = setInterval(() => {
    setDisplay(val => {
      if (val >= value) {
        clearInterval(interval.current)
        return value;
      }
      return val + 1
    })
  }, 100)

  return <span>{display}{symbol}</span>
}
```

And Voila, our per cent text is animating for initial value to the provided value.

---

Though the above component will not render on any change to the `value` prop as we are not using it in any of the Markup. 

If we do  `<span>{display}{symbol} - {value}</span>` it we re-render on any change in the prop `value`. It will do so because virtual DOM will generate different DOM tree on each `value` change.

So if we don’t want to use `value` in the DOM tree and still want to react to the changes in `value`; we need to use `useEffect` hook.

There are the changes in the component with `useEffect` on `value` change:
```jsx
export default ({ value = 0, symbol = '%' }) => {
  const interval = useRef(null)
  const [display, setDisplay] = useState(0)

  useEffect(() => {
    interval.current = setInterval(() => {
      setDisplay(val => {
        if (val >= value) {
          clearInterval(interval.current)
          return value;
        }
        return val + 1
      })
    }, 50)  
  }, [value])

  return <span>{display}{symbol}</span>
}
```

---

Now, we have another problem; on every change to the `value` our animation starts from `0`

We want it to start from the old value and reach the new value.

If it were classical components of old times 😉, we could have used ` [componentWillReceiveProps()](https://reactjs.org/docs/react-component.html#unsafe_componentwillreceiveprops)`.

*But its not.*

So here we will use `useRef` to keep the intermediate values in the component lifecycle. *Remember, it is different from `useState`*

Let’s add a ref to store the intermediate values and use the the value from ref to animate:

```jsx
export default ({ value = 0, symbol = '%' }) => {
  // initialization of ref with value only happens first time
  const oldValue = useRef(value);
  const interval = useRef(null);
  const [display, setDisplay] = useState(oldValue.current);

  useEffect(() => {
    interval.current && clearInterval(interval.current);
    interval.current = setInterval(() => {
      setDisplay((val) => {
        console.log(val);
        if (val >= value) {
          oldValue.current = value;
          clearInterval(interval.current);
          return val;
        }
        return val + 1;
      });
    }, 50);

    return () => clearInterval(interval.current);
  }, [value]);

  return <span>{display}{symbol}</span>
}
```

And now our progress per cent animation is complete. This is how it looks like:

%[https://youtu.be/U62rBf4Sscg]

%[https://codepen.io/pankajpatel/pen/ExPMMOR]

---

## Conclusion

Visual Feedback of any action makes the UI more intuitive and humane. 

The Changing values of percentage in progress-of-action is a small addition to code. 

Although it is a big aide for the User to know that something is happening and what is its status.

*Did you make any such visual feedback changes that made UX more intuitive?* 

Let me know through comments 💬 or on Twitter at  [@patel\_pankaj\_](https://twitter.com/patel_pankaj_)  and [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

#### Credits
* Icon from [IconFinder](https://www.iconfinder.com/icons/3887447/advertising_discount_marketing_mouthpiece_percent_icon)
* Photo by  [Agê Barros](https://unsplash.com/@agebarros?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/text?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

---

Originally published at [https://time2hack.com](https://time2hack.com/animating-the-progress-percent-change-in-react/) on July 29, 2020.