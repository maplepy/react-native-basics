# React

## React components

A component in React is a reusable piece of code that represents a part of the user interface. Components can be thought of as custom HTML elements that you can create and use in your React applications.

```jsx
export default function Profile() {
  return <img src="https://i.imgur.com/MK3eW3Am.jpg" alt="Katherine Johnson" />;
}
```

### Step 1: Export the component

The export default prefix is a standard JavaScript syntax (not specific to React). It lets you mark the main function in a file so that you can later import it from other files. (More on importing in Importing and Exporting Components!)

### Step 2: Define the function

With function Profile() { } you define a JavaScript function with the name Profile.

> [!WARNING]
> React components are regular **JavaScript functions**, but their names **must start with a capital letter** or they won’t work!

### Step 3: Add markup

Return statements can be written all on one line, as in this component:

```jsx
return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
```

But if your markup isn’t all on the same line as the return keyword, you must wrap it in a pair of parentheses:

```jsx
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

### Using a component

Now that you’ve defined your Profile component, you can nest it inside other components. For example, you can export a Gallery component that uses multiple Profile components:

```jsx
function Profile() {
  return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

### What the browser sees

Notice the difference in casing:

- `<section>` is lowercase, so React knows we refer to an HTML tag.
- `<Profile />` starts with a capital `P`, so React knows that we want to use our component called Profile.

And `Profile` contains even more HTML: `<img />`. In the end, this is what the browser sees:

```html
<section>
  <h1>Amazing scientists</h1>
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
</section>
```

### Nesting and organising components

Because the `Profile` components are rendered inside `Gallery`—even several times!—we can say that `Gallery` is a **parent component**, rendering each `Profile` as a “child”. This is part of the magic of React: you can define a component once, and then use it in as many places and as many times as you like.

Components can render other components, but **you must never nest their definitions**:

```jsx
export default function Gallery() {
  // 🔴 Never define a component inside another component!
  function Profile() {
    // ...
  }
  // ...
}
```

The snippet above is very slow and causes bugs. Instead, define every component at the top level:

```jsx
export default function Gallery() {
  // ...
}

// ✅ Declare components at the top level
function Profile() {
  // ...
}
```

When a child component needs some data from a parent, pass it by props instead of nesting definitions.

## Importing and exporting components

### Default vs named exports

There are two primary ways to export values with JavaScript: default exports and named exports. So far, our examples have only used default exports. But you can use one or both of them in the same file. A file can have no more than one default export, but it can have as many named exports as you like.

How you export your component dictates how you must import it. You will get an error if you try to import a default export the same way you would a named export! This chart can help you keep track:

| Syntax  | Export statement                      | Import statement                        |
| ------- | ------------------------------------- | --------------------------------------- |
| Default | `export default function Button() {}` | `import Button from './Button.js';`     |
| Named   | `export function Button() {}`         | `import { Button } from './Button.js';` |

When you write a _default_ import, you can put any name you want after `import`. For example, you could write `import Banana from './Button.js'` instead and it would still provide you with the same default export. In contrast, with named imports, the name has to match on both sides. That’s why they are called named imports!

### Mixing default and named exports

> [!TIP]
> To reduce the potential confusion between default and named exports, some teams choose to only stick to one style (default or named), or avoid mixing them in a single file. Do what works best for you!

**People often use default exports if the file exports only one component, and use named exports if it exports multiple components and values**. Regardless of which coding style you prefer, always give meaningful names to your component functions and the files that contain them. Components without names, like export default () => {}, are discouraged because they make debugging harder.

```jsx
import Gallery from "./Gallery.js"; // Default import
import { Profile } from "./Gallery.js"; // Named import

export default function App() {
  return <Profile />;
}
```

```jsx
// Named export
export function Profile() {
  return <img src="https://i.imgur.com/QIrZWGIs.jpg" alt="Alan L. Hart" />;
}

// Default export
export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

- `Gallery.js`:
  - Exports the `Profile` component as a **named export called `Profile`**.
  - Exports the `Gallery` component as a **default export**.
- `App.js`:
  - Imports `Profile` as a **named import called `Profile`** from `Gallery.js`.
  - Imports `Gallery` as a **default import** from `Gallery.js`.
  - Exports the root `App` component as a **default export**.

## Recap

You’ve just gotten your first taste of React! Let’s recap some key points.

- React lets you create components, **reusable UI elements** for your app.
- In a React app, every piece of UI is a component.
- React components are regular JavaScript functions except:
  1. Their names always begin with a capital letter.
  1. They return JSX markup.

## The Rules of JSX

### 1. Return a single root element

To return multiple elements from a component, wrap them with a single parent tag.

For example, you can use a `<div>`:

```jsx
<div>
  <h1>Hedy Lamarr's Todos</h1>
  <img
    src="https://i.imgur.com/yXOvdOSs.jpg"
    alt="Hedy Lamarr"
    className="photo"
  >
  <ul>
    ...
  </ul>
</div>
```

If you don’t want to add an extra `<div>` to your markup, you can write `<>` and `</>` instead:

```jsx
<>
  <h1>Hedy Lamarr's Todos</h1>
  <img
    src="https://i.imgur.com/yXOvdOSs.jpg"
    alt="Hedy Lamarr"
    className="photo"
  >
  <ul>
    ...
  </ul>
</>
```

This empty tag is called a _Fragment_. Fragments let you group things without leaving any trace in the browser HTML tree.

> [!NOTE]
> JSX looks like HTML, but under the hood it is transformed into plain JavaScript objects. You can’t return two objects from a function without wrapping them into an array. This explains why you also can’t return two JSX tags without wrapping them into another tag or a Fragment.

### 2. Close all the tags

JSX requires tags to be explicitly closed: self-closing tags like `<img>` must become `<img />`, and wrapping tags like `<li>oranges` must be written as `<li>oranges</li>`.

### 3. camelCase ~~all~~ most of the things!

JSX turns into JavaScript and attributes written in JSX become keys of JavaScript objects. In your own components, you will often want to read those attributes into variables. But JavaScript has limitations on variable names. For example, their names can’t contain dashes or be reserved words like `class`.

This is why, in React, many HTML and SVG attributes are written in camelCase. For example, instead of `stroke-width` you use `strokeWidth`. Since `class` is a reserved word, in React you write `className` instead, named after the [corresponding DOM property](https://developer.mozilla.org/en-US/docs/Web/API/Element/className).

### Pro-tips

- In JSX, you can use JavaScript expressions inside curly braces `{}`. For example, to dynamically set an image's `src` attribute, you can do:

  ```jsx
  const imageUrl = "https://i.imgur.com/MK3eW3As.jpg";
  return <img src={imageUrl} alt="Dynamic Image" />;
  ```

- Comments in JSX are written inside curly braces using JavaScript comment syntax:

  ```jsx
  return (
    <div>
      {/* This is a comment in JSX */}
      <h1>Hello, World!</h1>
    </div>
  );
  ```

- To conditionally render elements in JSX, you can use JavaScript logical operators. For example:

  ```jsx
  const isLoggedIn = true;
  return <div>{isLoggedIn && <h1>Welcome back!</h1>}</div>;
  ```

- Use a JSX [Converter tool](https://transform.tools/html-to-jsx) to help you convert HTML code snippets into JSX format quickly.

## Props

React components can accept inputs called "props" (short for properties) and manage internal data using "state". Props are passed to components (children) from their parent, while state is managed within the component itself.

### Familiar props

Props are the information that you pass to a JSX tag. For example, `className`, `src`, `alt`, `width`, and `height` are some of the props you can pass to an `<img>`:

```jsx
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}

export default function Profile() {
  return <Avatar />;
}
```

### Step 1: Pass props to the child component

First, pass some props to `Avatar`. For example, let’s pass two props: `person` (an object), and `size` (a number):

```jsx
export default function Profile() {
  return (
    <Avatar person={{ name: "Lin Lanying", imageId: "1bX5QH6" }} size={100} />
  );
}
```

> [!NOTE]
> If double curly braces after `person=` confuse you, recall they’re [merely an object](https://react.dev/learn/javascript-in-jsx-with-curly-braces#using-double-curlies-css-and-other-objects-in-jsx) inside the JSX curlies.

Now you can read these props inside the `Avatar` component.

### Step 2: Read props inside the child component

You can read these props by listing their names `person`, `size` separated by the commas inside `({` and `})` directly after `function Avatar`. This lets you use them inside the `Avatar` code, like you would with a variable.

```jsx
function Avatar({ person, size }) {
  // person and size are available here
}
```
