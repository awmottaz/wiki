---
sidebar_position: 2
---

# Be explicit with the callbacks you pass to Array and Object methods

https://jakearchibald.com/2021/function-callback-risks/

TL;DR — in general, don't pass a named function as the callback of Array and Object methods unless it was specifically designed for it.

```js
// DO NOT do this
arr.map(parseInt);

// Do this instead
arr.map((n) => parseInt(n));
```

The reason is because we usually see the first example and mentally apply the callback function only to the element of each array (that is, the two examples above look the same at a glance). But most Array and Object methods invoke the callback with multiple arguments, and sometimes the callback function accepts multiple arguments. This can result in unexpected behavior.

```js
["1", "2", "3"].map(parseInt); //=> [ 1, NaN, NaN ]
["1", "2", "3"].map((n) => parseInt(n)); //=> [ 1, 2, 3 ]
```
