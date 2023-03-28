---
title: "Too many useState? Let's useReducer"
seoTitle: "Too many useState? Let's useReducer!"
seoDescription: "Feeling like your React component is cluttered with too many useState? Let's use useReducer to declutter applications & their understanding 💪"
datePublished: Tue Mar 28 2023 12:44:17 GMT+0000 (Coordinated Universal Time)
cuid: clfs92avq000409kx99x50dwo
slug: too-many-usestate-lets-usereducer
canonical: https://time2hack.com/too-many-usestate-lets-usereducer-in-react/
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680005611573/ac63f103-484e-4288-b6d8-032232f9c4fe.jpeg
tags: javascript, web-development, reactjs, frontend-development

---

Modern Frontend is similar to good old web development, but with one catch: the page in the browser is as sophisticated as the backend.

Since we learned to load things asynchronously, we want to keep the user as productive as possible and not show any loading screen. This means that the Website became THE WebApp.

And the most common thing in any WebApp is state management.

All this prologue was to answer one question: "Why do we have a state in the Frontend Application?"

This post will examine better state management in our favourite library, React.

As almost all of you know, the hooks and the most commonly used hook to manage a local state: `useState`

`useState` works great in many scenarios and provides a stable solution without looking into heavier solutions like redux, mobx etc.

Let's take a small look into the basic example of the `useState` hook:

```javascript
const { useState } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);
  return (
    <article>
      <h1>Count: <code>{count}</code></h1>
      <button onClick={() => setCount(i => i++)}>Increase count</button>
    </article>
  )
}
```

Looks very straightforward, very innocent.

And most of the leaf components would look more or less like this, even without the state.

I would use props most of the time as compared to the state. If you have to use the state, that component must not contain much of the JSX, only the barebones passing the state to other Components as props (of course, there are exceptions).

But that's an argument for another time.

For this post, we will imagine a scenario where we don't want to introduce the large-scale state management library for one use case, but our state is getting complicated.

Consider the following wireframe for our application:

![](https://res-1.cloudinary.com/time2hack/image/upload/q_auto/v1/blog-images/awesome-app-1.png align="left")

![](https://res-5.cloudinary.com/time2hack/image/upload/q_auto/v1/blog-images/awesome-app-2.png align="left")

![](https://res-4.cloudinary.com/time2hack/image/upload/q_auto/v1/blog-images/awesome-app-3.png align="left")

Considering the above wireframes, if we were to make the controller component, it would look something like this:

```javascript
const MyApp = () => {
  const [shoudlOpenNewDataForm, toggleNewDataForm] = useState(false);
  const [confirmSave, toggleSaveConirmationModal] = useState(false);
  const [confirmReset, toggleResetModal] = useState(false);

  // ...
}
```

Now assume that we need to add one more interaction where we want to allow data editing. This would need us to open the same Form but with some data as the New Data form.

And upon completion of the data adjustment, we want to confirm if the user wants to save the new state.

Additionally, we want to allow the deletion of the data from the Data Editing form.

This would need three additional states to handle this new addition of UI states:

```javascript
const MyApp = () => {
  const [shoudlOpenNewDataForm, toggleNewDataForm] = useState(false);
  const [confirmSave, toggleSaveConirmationModal] = useState(false);
  const [confirmReset, toggleResetModal] = useState(false);
  
  const [dataBeingEdited, setData] = useState({});
  const [confirmDelete, toggleDeleteConfirmationModal] = useState(false);

  // ... 
}
```

Still simple enough?

Let's assume a state of application where we want to allow users to add/update child-level data like access control, permissions, sharing etc., while creating/editing the new data.

This would need a few more additional `useState` and `useRefs`

---

I can keep adding a few more use cases to complicate the state.

And one thing will happen; ***the controller logic will keep getting bigger and more prone to bugs***.

---

Let's not make the situation insane and look at one of the ways we can keep the problem manageable.

Let's `useReducer`

## `useReducer`

`useReducer` hook is part of the core library, allowing us to work with the state by dispatching the actions.

This hook works in **almost** similar ways to useState

Let's take a look at a small example:

```javascript
import { useReducer } from 'react';

const reducer = (state, action) => {
  switch (action.type) {
    case "open-modal":
      return { modal: action.payload.modal };
    case "close-modal":
      return { modal: null };
    default:
      return state;
  }
};

const Modal = ({ close, name }) => (
  <div>
    <h3>{name} Modal</h3>
    <button onClick={close}>Close</button>
  </div>
);

export const MyApp = () => {
  const [state, dispatch] = useReducer(reducer, { modal: null });
  const openModal = (modal) =>
    dispatch({
      type: "open-modal",
      payload: { modal },
    });
  const closeModal = () => dispatch({ type: "close-modal" });
  return (
    <>
      {state.modal === "info" && <Modal close={closeModal} name="Info" />}
      {state.modal === "confirmation" && (
        <Modal close={closeModal} name="Confirmation" />
      )}

      <button onClick={() => openModal("info")}>Open Info Modal</button>
      <button onClick={() => openModal("confirmation")}>
        Open Confirmation Modal
      </button>
    </>
  );
};
```

In the above example, we created a Modal to show some content based on the passed parameters.

The above example scenario can be achieved in many ways, but the goal is to exemplify the `useReducer` hook.

Here are a few critical elements of `useReducer`:

### Parameters

#### Reducer Function

This function is responsible for changing the state based on the type of action. This function is passed to the `useReducer` hook as the first argument. This function should be pure, i.e. it should not cause any side effects so that it can be tested independently.  
`const [state, dispatch] = useReducer(reducer, { modal: null });`

#### Initial State

This is supplied as the initial value to the reducer when executed for the first time. It is also the first render value of the `state` returned by `useReducer`  
`const [state, dispatch] = useReducer(reducer,` `{ modal: null });`

### Returned Values

The useReducer hook returns an array:  
`const` `[state, dispatch]` `= useReducer(reducer, { modal: null });`

#### Derived `state`

This state must be used in the JSX to determine the next rendered component tree. This is generated by running the reducer function on the previous derived state.  
`const [state, dispatch] = useReducer(reducer, { modal: null });`

#### Dispatch Function

This is the second element of the returned array from useReducer. This function is used to issue the actions to the reducer function.  
`const [state,` `dispatch] = useReducer(reducer, { modal: null });`

And here, we can see the above example in action:

%[https://codesandbox.io/embed/usereducer-example-forked-2ov3dw?file=/src/App.js]

---

### Benefits of useReducer

#### Testability

Along with testing the UI, the reducer function should be pure in ideal scenarios. And this enables the testability of this reducer function.

#### Modularity

As your function to modify state lives in a separate function instead of the controller Component, you can now move it to a separate file and break it down more to achieve more Modular state derivation.

In the previous case, you must create a custom hook to encapsulate all the `useState` hooks.

#### Readability

As Modularity and Testability significantly improve the Code readability, your old component is shorter and has less mental overhead.

---

Now, let's refactor our initial useState example. As we have yet to define the fully `useState` model, our `useReducer` solution will look a lot bigger now.

But for a comparable solution which is scalable and manageable, it would be more significant than useReducer

```javascript
const removeModal = (modals, modal) => modals.filter((item) => item !== modal);

export const reducer = (state, action) => {
  switch (action.type) {
    case "init-add":
      return {
        ...state,
        currentlyEditing: {},
        modals: [...state.modals, "add-item"]
      };
    case "edit-item":
      return {
        ...state,
        currentlyEditing: action.payload.item,
        modals: [...state.modals, "add-item"]
      };
    case "init-delete":
      return {
        ...state,
        modals: [...state.modals, "confirm-delete"],
        currentlyEditing: action.payload.item
      };
    case "delete-item":
      return {
        ...state,
        modals: removeModal(state.modals, "confirm-delete"),
        currentlyEditing: {},
        data: state.data.filter((item) => item.id !== state.currentlyEditing.id)
      };
    case "init-reset":
      return {
        ...state,
        modals: [...state.modals, "confirm-reset"]
      };
    case "reset-editing-item":
      return {
        ...state,
        modals: removeModal(state.modals, "confirm-reset"),
        currentlyEditing: {
          ...(state.data.find(({ id }) => state.currentlyEditing.id) ||
            initialState.currentlyEditing)
        }
      };
    case "show-modal":
      return { ...state, modals: [...state.modals, action.payload.name] };
    case "hide-modal":
      return {
        ...state,
        modals: removeModal(state.modals, action.payload.name)
      };
    case "update-currently-editing":
      return {
        ...state,
        currentlyEditing: { ...state.currentlyEditing, ...action.payload }
      };
    case "save-currently-editing":
      const { currentlyEditing } = state;
      const probableNewId = new Date().getTime();
      const data = state.data
        .concat(
          currentlyEditing.id
            ? []
            : [{ ...currentlyEditing, id: probableNewId }]
        )
        .map((item) =>
          item.id === currentlyEditing.id ? { ...currentlyEditing } : item
        );
      return {
        ...state,
        data,
        currentlyEditing: {},
        modals: removeModal(state.modals, "add-item")
      };
    default:
      return state;
  }
};

export const initialState = {
  data: [
    { name: "A", id: 1 },
    { name: "B", id: 2 }
  ],
  currentlyEditing: {},
  modals: []
};
```

With the above example, all application changes can not be reflected in changes to Data; hence, the code is highly testable and reliable.

%[https://codesandbox.io/embed/usereducer-example-giedk0?file=/src/reducer.js]

If we want to check whether the Modal to add a new item is open or not, we need to check the presence of `add-item` element in the Modals array, let's look at the example.

```js
describe('data table reducer', () => {
  it('should add add-item modal when add is triggered', () => {
    expect(
      reducer({}, { type: 'init-add' }).modals
    ).toEqual(['add-item']);
  });

  it('should add add-item modal when edit is triggered', () => {
    const action = {
      type: 'edit-item',
      payload: { item: {a: 1, b: 2} }
    };
    const updatedState = reducer({}, action);
    expect(updatedState.modals).toEqual(['add-item']);
    expect(
      updatedState.currentlyEditing
    ).toEqual(action.payload.item);
  });
});
```

---

### Conclusion

Have you used useReducer?

What are some tips you would consider keeping in mind while using `useState` and `useReducer`?

Let me know through comments. or on Twitter at [@heypankaj\_](https://twitter.com/intent/follow?screen_name=heypankaj_) and/or [@time2hack](https://twitter.com/intent/follow?screen_name=time2hack)

If you find this article helpful, please share it with others.

Subscribe to the blog to receive new posts right in your inbox.