# React Cheatsheet

Complete reference for essential React concepts and patterns.

## Table of Contents

- [Components](#components)
- [JSX Rules](#jsx-rules)
- [Props](#props)
- [Import/Export](#importexport)
- [State Management](#state-management)
- [Conditional Rendering](#conditional-rendering)
- [Lists & Keys](#lists--keys)
- [Events](#events)
- [Hooks](#hooks)
- [Forms](#forms)

---

## Components

### Function Components

```jsx
function Welcome() {
  return <h1>Hello, World!</h1>;
}
```

### Component with Props

```jsx
function Greeting({ time, name }) {
  return (
    <h1>
      Good {time}, {name}!
    </h1>
  );
}
```

### Component Usage

```jsx
function App() {
  return <Greeting time="evening" name="Alice" />;
}
```

**Rules:**

- Component names must start with capital letter
- Always return JSX from components
- Components must be pure (same inputs = same output)
- Never call components as functions: use `<Component />`, not `Component()`

---

## JSX Rules

### Basic Syntax

```jsx
const element = <h1>Hello, World!</h1>;
```

### Embedding Expressions

```jsx
const name = "Sarah";
const element = <h1>Hello, {name}!</h1>;
```

### Multiple Lines

```jsx
const element = (
  <div>
    <h1>Title</h1>
    <p>Paragraph</p>
  </div>
);
```

### JSX Must Have One Root Element

```jsx
// ❌ Wrong
function Component() {
  return (
    <h1>Title</h1>
    <p>Text</p>
  );
}

// ✅ Correct - wrapped in parent
function Component() {
  return (
    <div>
      <h1>Title</h1>
      <p>Text</p>
    </div>
  );
}

// ✅ Correct - using Fragment
function Component() {
  return (
    <>
      <h1>Title</h1>
      <p>Text</p>
    </>
  );
}
```

### Key Rules

- All tags must be closed: `<img />`, `<input />`
- Use `className` instead of `class`
- Use `htmlFor` instead of `for`
- Style attribute takes objects: `style={{ color: 'red', fontSize: '14px' }}`
- camelCase for attributes: `onClick`, `onChange`, `tabIndex`

---

## Props

### Passing Props

```jsx
function App() {
  return <Welcome name="Alice" age={25} isActive={true} />;
}
```

### Receiving Props

```jsx
// Object parameter
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Destructuring (preferred)
function Welcome({ name, age, isActive }) {
  return (
    <h1>
      Hello, {name}, age {age}!
    </h1>
  );
}
```

### Default Props

```jsx
function Welcome({ name = "Guest" }) {
  return <h1>Hello, {name}!</h1>;
}
```

### Props Spreading

```jsx
const userProps = { name: "Alice", age: 25 };

function App() {
  return <Profile {...userProps} />;
}
```

### Children Prop

```jsx
function Card({ children }) {
  return <div className="card">{children}</div>;
}

// Usage
<Card>
  <h1>Title</h1>
  <p>Content</p>
</Card>;
```

### Passing Functions as Props

```jsx
function AlertButton({ message, children }) {
  return <button onClick={() => alert(message)}>{children}</button>;
}

function Toolbar() {
  return (
    <div>
      <AlertButton message="Playing!">Play Movie</AlertButton>
      <AlertButton message="Uploading!">Upload Image</AlertButton>
    </div>
  );
}
```

**Rules:**

- Props are read-only (immutable)
- Always pass props down (parent → child)
- Props can be any data type: strings, numbers, booleans, objects, arrays, functions

---

## Import/Export

### Named Export/Import

```jsx
// Profile.jsx
export function Profile() {
  return <div>Profile</div>;
}

export function Settings() {
  return <div>Settings</div>;
}

// App.jsx
import { Profile, Settings } from "./Profile";
```

### Default Export/Import

```jsx
// Profile.jsx
export default function Profile() {
  return <div>Profile</div>;
}

// App.jsx
import Profile from "./Profile";
```

### Mixed Export/Import

```jsx
// utils.jsx
export function helper() {}
export default function main() {}

// App.jsx
import main, { helper } from "./utils";
```

### Import React

```jsx
// Modern React (17+) - JSX transform
import { useState, useEffect } from "react";

// Older React
import React from "react";
import { useState } from "react";
```

**Rules:**

- One default export per file
- Multiple named exports allowed
- Default exports can be renamed on import
- Named exports must match exact name

---

## State Management

### useState Hook

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### Multiple State Variables

```jsx
function Form() {
  const [name, setName] = useState("");
  const [age, setAge] = useState(0);
  const [isActive, setIsActive] = useState(false);
}
```

### State with Objects

```jsx
function Profile() {
  const [user, setUser] = useState({
    name: "Alice",
    age: 25,
  });

  // Update object state (spread to preserve other properties)
  const updateName = (newName) => {
    setUser({ ...user, name: newName });
  };
}
```

### State with Arrays

```jsx
function TodoList() {
  const [todos, setTodos] = useState(["Task 1", "Task 2"]);

  const addTodo = (task) => {
    setTodos([...todos, task]); // Add item
  };

  const removeTodo = (index) => {
    setTodos(todos.filter((_, i) => i !== index)); // Remove item
  };
}
```

### useReducer Hook

```jsx
import { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </div>
  );
}
```

**Rules:**

- State updates are asynchronous
- Never mutate state directly
- Use setter functions to update state
- State is local to component instance

---

## Conditional Rendering

### if/else Statement

```jsx
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✅</li>;
  }
  return <li className="item">{name}</li>;
}
```

### Ternary Operator

```jsx
function Item({ name, isPacked }) {
  return <li className="item">{isPacked ? name + " ✅" : name}</li>;
}
```

### Logical AND (&&)

```jsx
function Notification({ message, show }) {
  return <div>{show && <p>{message}</p>}</div>;
}
```

### Variable Assignment

```jsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = <del>{name + " ✅"}</del>;
  }
  return <li className="item">{itemContent}</li>;
}
```

### Returning null

```jsx
function Warning({ show }) {
  if (!show) {
    return null; // Render nothing
  }
  return <p>Warning!</p>;
}
```

---

## Lists & Keys

### Rendering Lists with map()

```jsx
function VideoList({ videos }) {
  return (
    <ul>
      {videos.map((video) => (
        <li key={video.id}>{video.title}</li>
      ))}
    </ul>
  );
}
```

### Lists with Components

```jsx
const people = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" },
];

function PeopleList() {
  return (
    <ul>
      {people.map((person) => (
        <Person key={person.id} name={person.name} />
      ))}
    </ul>
  );
}
```

### Filtering Lists

```jsx
function ActiveUsers({ users }) {
  const activeUsers = users.filter((user) => user.isActive);

  return (
    <ul>
      {activeUsers.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### Fragments in Lists

```jsx
import { Fragment } from "react";

function Blog() {
  return posts.map((post) => (
    <Fragment key={post.id}>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </Fragment>
  ));
}
```

**Key Rules:**

- Always provide `key` prop for list items
- Keys must be unique among siblings
- Keys should be stable (not random or index if list can change)
- Use IDs from your data as keys
- Avoid array index as key (only if list is static)

---

## Events

### Click Events

```jsx
function Button() {
  const handleClick = () => {
    console.log("Clicked!");
  };

  return <button onClick={handleClick}>Click me</button>;
}
```

### Inline Event Handlers

```jsx
function Button() {
  return <button onClick={() => console.log("Clicked!")}>Click me</button>;
}
```

### Events with Parameters

```jsx
function Button({ id }) {
  const handleClick = (id) => {
    console.log("Clicked button:", id);
  };

  return <button onClick={() => handleClick(id)}>Click me</button>;
}
```

### Event Object

```jsx
function Input() {
  const handleChange = (event) => {
    console.log("Input value:", event.target.value);
  };

  return <input onChange={handleChange} />;
}
```

### Preventing Default

```jsx
function Form() {
  const handleSubmit = (event) => {
    event.preventDefault();
    console.log("Form submitted");
  };

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Common Event Props

- `onClick` - mouse click
- `onChange` - input change
- `onSubmit` - form submit
- `onFocus` - element focused
- `onBlur` - element lost focus
- `onKeyDown` - key pressed
- `onMouseEnter` - mouse enters element
- `onMouseLeave` - mouse leaves element

**Rules:**

- Event handlers receive event object as first parameter
- Use camelCase for event names
- Pass function reference, not function call: `onClick={handleClick}`, not `onClick={handleClick()}`

---

## Hooks

### useState

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

### useEffect

```jsx
import { useEffect, useState } from "react";

function Timer() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setCount((c) => c + 1);
    }, 1000);

    // Cleanup function
    return () => clearInterval(timer);
  }, []); // Empty array = run once on mount

  return <p>Seconds: {count}</p>;
}
```

### useEffect with Dependencies

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetchUser(userId).then((data) => setUser(data));
  }, [userId]); // Re-run when userId changes

  return <div>{user?.name}</div>;
}
```

### Conditional Effects

```jsx
function Component({ isLoggedIn }) {
  useEffect(() => {
    if (isLoggedIn) {
      fetchUserData();
    }
  }, [isLoggedIn]);
}
```

### useCallback

```jsx
import { useCallback } from "react";

function ProductPage({ productId, referrer, theme }) {
  const handleSubmit = useCallback(
    (orderDetails) => {
      post("/product/" + productId + "/buy", {
        referrer,
        orderDetails,
      });
    },
    [productId, referrer]
  );

  return (
    <div className={theme}>
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

**Hooks Rules:**

- Only call hooks at top level (not in loops, conditions, or nested functions)
- Only call hooks in React function components or custom hooks
- Hook names start with `use`

---

## Forms

### Controlled Input

```jsx
function Form() {
  const [name, setName] = useState("");

  return <input value={name} onChange={(e) => setName(e.target.value)} />;
}
```

### Multiple Inputs

```jsx
function Form() {
  const [formData, setFormData] = useState({
    name: "",
    email: "",
    age: "",
  });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData((prev) => ({
      ...prev,
      [name]: value,
    }));
  };

  return (
    <form>
      <input name="name" value={formData.name} onChange={handleChange} />
      <input name="email" value={formData.email} onChange={handleChange} />
      <input name="age" value={formData.age} onChange={handleChange} />
    </form>
  );
}
```

### Form Submission

```jsx
function Form() {
  const [name, setName] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Submitted:", name);
    setName(""); // Clear form
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Checkbox

```jsx
function Form() {
  const [isChecked, setIsChecked] = useState(false);

  return (
    <input
      type="checkbox"
      checked={isChecked}
      onChange={(e) => setIsChecked(e.target.checked)}
    />
  );
}
```

### Select Dropdown

```jsx
function Form() {
  const [selectedOption, setSelectedOption] = useState("option1");

  return (
    <select
      value={selectedOption}
      onChange={(e) => setSelectedOption(e.target.value)}
    >
      <option value="option1">Option 1</option>
      <option value="option2">Option 2</option>
      <option value="option3">Option 3</option>
    </select>
  );
}
```

### Form with useFormStatus

```jsx
import { useFormStatus } from "react-dom";

function Submit() {
  const status = useFormStatus();
  return <button disabled={status.pending}>Submit</button>;
}

function App() {
  return (
    <form action={action}>
      <Submit />
    </form>
  );
}
```

### Form with useActionState

```jsx
import { useActionState } from "react";

function MyComponent() {
  const [state, formAction] = useActionState(action, null);

  return (
    <form action={formAction}>
      <input name="username" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Rules:**

- Controlled components: React state controls input value
- Always use `value` and `onChange` together for controlled inputs
- For checkboxes, use `checked` instead of `value`
- Prevent default form submission with `e.preventDefault()`

---

## Quick Reference

### Component Checklist

- ✅ Name starts with capital letter
- ✅ Returns JSX
- ✅ Accepts props parameter
- ✅ Uses hooks at top level only
- ✅ Pure function (no side effects in render)

### JSX Checklist

- ✅ Single root element or Fragment
- ✅ All tags closed
- ✅ Use `className` not `class`
- ✅ Use `{}` for JavaScript expressions
- ✅ camelCase for attributes

### Props Checklist

- ✅ Passed from parent to child
- ✅ Read-only (immutable)
- ✅ Can be any data type
- ✅ Destructure for cleaner code

### State Checklist

- ✅ Use `useState` for simple state
- ✅ Use `useReducer` for complex state
- ✅ Never mutate state directly
- ✅ Use setter function to update
- ✅ Updates are asynchronous

### Lists Checklist

- ✅ Use `map()` to render arrays
- ✅ Always provide `key` prop
- ✅ Keys must be unique among siblings
- ✅ Use stable IDs as keys

---

_Generated with React documentation from react.dev_
