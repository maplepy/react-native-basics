# JavaScript Cheatsheet

## Arrow Functions Cheatsheet

Arrow functions provide a concise syntax for writing functions in JavaScript. They also lexically bind the `this` value (i.e., they do not have their own `this`) which can be useful in certain contexts.

### Basic forms

- No params

  ```js
  () => 42;
  ```

- One param

  ```js
  (x) => x * 2; // implicit return
  (x) => {
    return x * 2;
  }; // explicit return
  ```

- Multiple params
  ```js
  (a, b) => a + b;
  ```

### Implicit vs explicit return

- Implicit (no `{}`):

  ```js
  (x) => x * 2; // returns x * 2
  ```

- Explicit (with `{}`):
  ```js
  (x) => {
    const y = x * 2;
    return y; // must use return
  };
  ```

### Returning objects

- Must wrap in `()`:

  ```js
  (name) => ({ name }); // { name: <value> }
  ```

### Destructuring + defaults

```js
({ name, age = 0 }) => `${name} (${age})`;
```

### Common array usages

```js
items.map((x) => x * 2);
items.filter((x) => x > 0);
items.reduce((acc, x) => acc + x, 0);
```

### Optional chaining + nullish coalescing

```js
(user) => user?.name ?? "Anonymous";
```

- `?.`: stop safely if left side is null/undefined.
- `??`: use right side only if left is null/undefined.

---

Question to check understanding:
How would you write an arrow function that takes `{ firstName, lastName }` and returns `"firstName lastName"` using destructuring?
