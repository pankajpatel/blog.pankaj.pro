## ToDo app in React with Hooks & Context API

Today, Making a react app is very easy and fast as compared to in the past. 

This is the time of Functional Components, Hooks and Context API. Let’s remake [our todo app from the past](https://time2hack.com/todo-app-with-reactjs/) with Modern React.

---

> First of all; *What are React Hooks and Context API?*

_***Hooks:***_ Hooks are the construct in React app development which will allow you to extract the state logic of a component and make it reusable and testable.

Read more about the hooks here: [Introducing Hooks – React](https://reactjs.org/docs/hooks-intro.html#motivation)

_***Context API:***_ Context API provides you with a way to share data among components in the component tree without needing to pass props to components which are not going to use that data.

Read more about the Context API here:  [Context – React](https://reactjs.org/docs/context.html)

Context API requires the creation of Context via `React.createContext`.
New  Context will provide `Provider` and `Consumer` components of that Context.
* Provider will allow you to change the data of Context
* Consumer will allow you to listen to the changes in the Context

---

With these topics in mind, we will be using create-react-app to start our react app. 

And to use create-react-app, we will [npx it to get up and running](https://time2hack.com/npx-work-faster-without-installing-npm-package-binaries/). 

```
npx create-react-app todo-react
```

Now that we have our project ready with us, we will do an initial run of the project with `yarn start` or `npm start`

This will start the local development server for our react project. Now launch https://localhost:3000 on your browser (provided port 3000 is free). You will see the following screen on the browser:

![](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/create-react-app-initial-launch.png)

Now important file for us is `App.js` which will be the entry point for us i.e. we will assemble our little todo app here. 

As we have Three main functions on our todo app:
* List of ToDo
* Add ToDo
* Manage (Mark as Done and Delete/Hide Completed)

And we will share some basic configurations and utility functions through Context API. 

Let’s take the todo creation function from Todo Text provided in props.

This function can also hydrate the todo state to build the UI of the todo task.

---

We will start with some basic structure and random data to make a list of ToDo. Let’s consider the following data structure of ToDo task:

```json
{
  text: "First Todo",
  description: "First Todo's Description",
  createdOn: new Date().toUTCString()
}
```

And for an array, we will create following functional component:

```js
// ToDos.js
import React from "react";

export const Todo = ({ task, ...extra }) => (
  <div className="card mb-3 bt-3" {...extra}>
    <div className="card-body">
      <h5 className="card-title">{task.text}</h5>
      <p className="card-text">{task.description}</p>
      <div className="footer">
        <small>{task.createdOn}</small>
      </div>
    </div>
  </div>
);

export default ({ tasks }) => (
  <>
    {(tasks || []).map((task, index) => (
      <Todo task={task} key={index} />
    ))}
  </>
);
```

Few important things to notice here about Functional Components:
* React needs to be in the context of these Functional Components
* You can return JSX from arrow functions
* `<>` is a shorthand for `React.Fragment` which is similar to document fragment; which allows us to keep the DOM clean.
* On the line: `export default ({ todos }) => (`; we have used the Object de-structuring on the props

The App container will keep todos and use the above component to render the todos. The todos component looks like following:

```js
import React, { useState } from "react";
import Header from "./components/Header";
import ToDos from "./components/Todos";
import NewTask from "./components/NewTask";
import _tasks from "./_initial";

const App = () => {
  const [tasks, updateTasks] = useState(_tasks);

  return (
    <>
      <Header />
      <div className="container">
        <NewTask addTodo={task => updateTasks([...tasks, task])} />
        <hr />
        <ToDos tasks={tasks} />
      </div>
    </>
  );
};

export default App;
```

Till now, have a local application state of ToDos and the new Todo. And we can use state hook to keep the local state of ToDos at the application level.

Now let’s take a look at the Component for the New ToDo Form:

```js
import React from "react";

export default ({ addTodo }) => {
  const handleAdd = e => {
    e.preventDefault();
    // we need data from Form; for that we can use FormData API
    const formData = new FormData(e.target);
    console.log("---Form---", formData);
    addTodo({
      text: formData.get("text"),
      description: formData.get("description"),
      createdOn: new Date().toUTCString()
    });
    e.target.reset();
  };

  return (
    <form onSubmit={handleAdd}>
      <div className="form-group">
        <label htmlFor="text" className="text-muted">
          Task:
        </label>
        <input name="text" type="text" id="text" className="form-control" />
      </div>
      <div className="form-group">
        <label htmlFor="description" className="text-muted">
          Description:
        </label>
        <textarea
          name="description"
          id="description"
          className="form-control"
        />
      </div>
      <div className="form-group">
        <button type="submit" className="btn btn-primary">
          Add
        </button>
      </div>
    </form>
  );
};
```

Here we will use FormData API to collect the values from Form Fields.

> P.S. If you wanna know more about Form Data API, you can head over here:
> [FormData API: Handle Forms like Boss 😎 - Time to Hack](https://time2hack.com/formdata-api-handle-forms-like-a-boss/)

Now let’s assemble the Components and have our App in running state:

```js
import React, { useState } from "react";
import Header from "./components/Header";
import ToDos from "./components/Todos";
import NewTask from "./components/NewTask";
import _tasks from "./_initial";

const App = () => {
  const [tasks, updateTasks] = useState(_tasks);

  return (
    <>
      <Header />
      <div className="container">
        <NewTask
          addTodo={task => updateTasks([...tasks, task])}
        />
        <hr />
        <ToDos tasks={tasks} />
      </div>
    </>
  );
};

export default App;
```

Now our todo app is in place.

At this state, our app looks like following:

![](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/create-react-app-todo-with-hooks.png)

---

Now to make our app more customisable, we will add some configurations; like as following:

```js
const app = {
  title: "Time to Hack",
  url: "https://time2hack.com",
  logo:
    "https://res.cloudinary.com/time2hack/image/upload/q_auto:good/t2h-text-banner.png"
};

const config = {
  sortBy: "createdOn",
  sortOrder: "DESC"
};

const sorters = {
  ASC: (a, b) => a[config.sortBy] - b[config.sortBy],
  DESC: (a, b) => b[config.sortBy] - a[config.sortBy]
};

const sorter = sorters[config.sortOrder];

export default {
  ...config,
  app,
  sorter
};
```

Now let's create a context as in the following file:
```js
import React from "react";

const Config = React.createContext({});
Config.displayName = "Config";

export default Config;
``` 

And then seed the value to the Context Provider in the Entry of our app:

```diff
  import React, { useState } from "react";
  import Header from "./components/Header";
  import ToDos from "./components/Todos";
  import NewTask from "./components/NewTask";
+ import Config from "./TodoContext";
+ import config from "./config";
  import _tasks from "./_initial";

  const App = () => {
    const [tasks, updateTasks] = useState(_tasks);

    return (
-      <>
+.     <Config.Provider value={config}>
        <Header app={config.app} />
        <div className="container">
          <NewTask addTodo={task => updateTasks([...tasks, task])} />
          <hr />
          <ToDos tasks={tasks} />
        </div>
-      </>
+      </Config.Provider>
    );
  };

  export default App;
```


Now we can use the `useContext` hook to use the Context Value in the following header of the app:

```js
import React from "react";

export default ({ app }) => (
  <header className="mb-3">
    <nav className="navbar navbar-dark bg-dark">
      <div className="container">
        <a className="navbar-brand" href={app.url}>
          <img src={app.logo} height="30" alt={app.title} />
        </a>
      </div>
    </nav>
  </header>
);
```

And use the Sorting Config from Context to list the Tasks is a sorting order:

```diff
    import React, { useContext } from "react";
+   import Config from "../TodoContext";

    export const Todo = ({ task, ...extra }) => (
      <div className="card mb-3 bt-3" {...extra}>
        <div className="card-body">
          <h5 className="card-title">{task.text}</h5>
          <p className="card-text">{task.description}</p>
          <div className="footer">
            <small>
              {new Date(task.createdOn).toUTCString()}
            </small>
          </div>
        </div>
      </div>
    );

    export default ({ tasks = [] }) => {
+      const conf = useContext(Config);

      return (
        <>
          {tasks
+           .sort(conf.sorter)
            .map((task, index) => (
              <Todo task={task} key={index} />
            ))}
        </>
      );
    };
```

And that’s how we can use Hooks and Context to manage state and share global app data with ease.

And our app now looks like this:

![](https://res.cloudinary.com/time2hack/image/upload/q_auto:good,f_auto/create-react-app-todo-with-hooks-context-api.png)

---

## Conclusion

Here we saw the following things:
* Starting React app with create-react-app
* Using Hooks to maintain state with `useState`
* Using Context API to share data among components
* Consuming Context data with `useContext` hook

What do you think about React Hooks and Context API?

Let me know through comments 💬 or on Twitter at  [@patelpankaj](https://twitter.com/patel_pankaj_)  and  [@time2hack](https://twitter.com/time2hack) 

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

# Credits
Photo by  [Filiberto Santillán](https://unsplash.com/@filijs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

---

Originally published at [https://time2hack.com](https://time2hack.com/todo-app-in-react-with-hooks-context-api/) on March 24, 2020.