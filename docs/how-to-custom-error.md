---
sidebar_position: 2
---

# How to create and use custom JavaScript errors

Here is how to create a custom error type in JavaScript.

```js
class CustomError extends Error {
  constructor(message, data, ...rest) {
    super(`CustomError: ${message}`, ...rest);
    this.name = "CustomError";
    this.data = data;
  }
}
```

Usage looks like this:

```js
throw new CustomError("something bad happened", data);
```

You can also check `instanceof CustomError`
