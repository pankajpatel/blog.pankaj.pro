## Today's npm package: theming

Today's npm package is [theming](https://www.npm/js.com/package/theming)

With rise of CSS-in-JS, managing and maintaining the design tokens can be hard.

Today's package [theming](https://www.npm/js.com/package/theming) will help you manage the design tokens for your React project with CSS-in-JS in consistent way.

Here is a small example of injecting and using design tokens as theme to React App.

```jsx
import React from 'react';
import { ThemeProvider, withTheme } from 'theming';
 
const DemoBox = props => {
  console.log(props.theme);
  return (<div />);
}
 
const ThemedDemoBox = withTheme(DemoBox);

const theme = {
  color: 'black',
  background: 'white',
};
 
const App = () => (
  <ThemeProvider theme={theme}>
    <ThemedDemoBox /> {/* { color: 'black', background: 'white' } */}
  </ThemeProvider>
)
 
export default App;
```

Stay tuned for tomorrow's package of the Day.