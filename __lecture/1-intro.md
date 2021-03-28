# 5.1.1 Intro to Redux

---

### What is Redux?

Redux is a _state management library_ commonly used with React

---

# The Flux Wars

In 2014-2015, React was a very different library.

It had state, but no context, no reducers...

It was difficult to use it in a large application.

---

# The Flux Wars

**Flux** was an architectural idea around managing state.

It was not a library. It was a schematic.

---

# The Flux Wars

Every week, a new library was released claiming to be the best Flux implementation.

They were all pretty cumbersome.

---

# The Flux Wars

Redux was released in 2015. It won the war.

---

# The idea

- Your state lives in a **redux store**.
- You **connect** React components to that store.

---

<img src="./assets/redux-flowchart.png" alt="a complicated chart that explains redux" />

---

That diagram never makes sense to people at first.

Keep it around, look at it later. It'll make more sense.

---

## Two NPM Packages

- `redux` is technically not related to React at all. It's an independent state library.
- `react-redux` are the _react bindings_.

We need both to use Redux

---

## The Redux store

Your store is where your data lives.

Redux always uses a single store, even for very large applications.

---

## Creating a Redo Store

```js
import { createStore } from "redux";

function reducer(state, action) {
  switch (action.type) {
    case "SOMETHING":
      return 5;
    default:
      return state;
  }
}

const initialState = 10;

const store = createStore(reducer, initialState);
```

---

## Providing the store to React

```js
// In src/index.js
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";

import App from "./components/App";

const store = ReactDOM.render(
  // All the create-store stuff
  <Provider store={store}>
    <App />
  </Provider>,
  document.querySelector("#root")
);
```

---

### Selecting data from the Redux store

```js
// components/AppleFarm.js
import { useSelector } from "react-redux";

const AppleFarm = () => {
  const numberOfApples = useSelector((state) => {
    return state.numberOfApples;
  });

  return <div>Number of apples: {numberOfApples}</div>;
};
```

---

# Exercises

Write selector functions to pluck out the right state.

---

```js
// Exercise 1
/*
Our state shape:

{
  fridge: [
    'apple',
    'wine',
    'ketchup',
    'potatoes',
    'ice cream'
  ],
  oven: [
    'celery'
  ],
  pantry: [
    'salmon',
  ]
}
*/

const FridgeContents = () => {
  const fridgeItems = /* TODO */

  return (
    <div>
      <h1>Your fridge contains:</h1>

      {fridgeItems.map(item => (
        <div key={item}>
          {item}
        </div>
      ))}
    </div>
  );
};
```

---

```js
// Exercise 2
/*
Our state shape:

{
  myFavouriteGenre: 'scifi',
  myFavouriteMovies: {
    horror: 'Terror at Jarry Park',
    scifi: 'Deep Voyage: Laval and Beyond',
    romcom: 'Anjou Amour',
    kungfu: 'Fracas in the Plateau'
  },
  boyfriendFavouriteGenre: 'horror',
  boyfriendFavouriteMovies: {
    horror: 'Westmount Chainsaw Massacre',
    scifi: 'Island of the Nuns',
    romcom: 'He said bonjour, I said hi',
    kungfu: 'Scuffle in the Sud-Ouest'
  },
}
*/

const App = () => {
  // We're going to watch OUR favourite movie,
  // in our BOYFRIEND's favourite genre.
  // (Terror at Jarry Park)
  const movie = /* TODO */

  return (
    <div>
      Tonight, we'll watch: {movie}
    </div>
  );
};
```

---

# Dispatching actions

We have another hook, `useDispatch`.

---

```js
import { useDispatch, useSelector } from "react-redux"; // <-- 👀

const TodoList = () => {
  const todos = useSelector((state) => state.todos);
  const dispatch = useDispatch(); // <-- 👀

  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>
          {todo.value}
          <button
            onClick={() =>
              dispatch({
                type: "MARK_TODO_AS_COMPLETED",
                todoId: todo.id,
              })
            }
          >
            complete
          </button>
        </li>
      ))}
    </ul>
  );
};
```

---

## **NEW** Action Creators

An "action creator" is a function that produces an action.

---

By convention, action creators in redux all live together in the same file(s):

```js
// actions.js
export const addTodo = (todoItem) => {
  return {
    type: "ADD_TODO",
    todoItem,
  };
};

export const markTodoAsCompleted = (todoId) => {
  return {
    type: "MARK_TODO_AS_COMPLETED",
    todoId,
  };
};

export const deleteTodo = (todoId) => {
  return {
    type: "DELETE_TODO",
    todoId,
  };
};
```

---

```js
import { useDispatch, useSelector } from 'react-redux';
import { markTodoAsCompleted } from '../actions'; // <-- 👀

const TodoList = () => {
  const todos = useSelector(state => state.todos);
  const dispatch = useDispatch();

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          {todo.value}
          <button onClick={() => dispatch(markTodoAsCompleted(todo.id))}>  // <-- 👀
        </li>
      ))}
    </ul>
  );
};
```

---

# Exercises

Wire in the action and dispatch it.

---

```js
// Exercise 5
import { useDispatch } from "react-redux";
import { pokeUser } from "../actions";

const OnlineUsers = () => {
  // TODO: Something missing here...

  const onlineUsers = useSelector((state) => {
    return state.users.filter((user) => user.online);
  });

  return onlineUsers.map((user) => (
    <div key={user.name}>
      <button onClick={/* TODO */}>Message {user.name}</button>
    </div>
  ));
};
```

---

```js
// Exercise 6
import { useDispatch } from "react-redux";
import { addItemToFridge } from "../actions";

const FridgeForm = () => {
  const [value, setValue] = React.useState("");
  const dispatch = useDispatch();

  return (
    <form
      onSubmit={() => {
        /* TODO */
      }}
    >
      <input type="text" onChange={(ev) => setValue(ev.target.value)} />

      <button type="submit">Submit</button>
    </form>
  );
};
```

---

```js
// Exercise 7
import { useDispatch } from "react-redux";
import { dismissModal } from "../actions";

const Modal = () => {
  const dispatch = useDispatch();

  React.useEffect(() => {
    const handleKeydown = (ev) => {
      // TODO: Close modal when 'Escape' is pressed
      // (Hint: use ev.key)
    };

    window.addEventListener("keydown", handleKeydown);

    return () => {
      window.removeEventListener("keydown", handleKeydown);
    };
  }, []);

  return <div>Hello</div>;
};
```

---

# Quick note

As with everything, Redux has changed a lot recently.

If you're googling, be mindful that most redux resources are out-of-date!

---
