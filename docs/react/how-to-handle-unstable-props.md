# How to handle unstable props

Problem: you have a React hook that takes, as input, a value that might be referentially unstable. You want to ignore updates to the value, but use the latest value when your thing runs.

Solution:

```js
function useFoo(unstableValue) {
  const ref = React.useRef(unstableValue);

  React.useLayoutEffect(() => {
    ref.current = unstableValue;
  });

  // use ref.current everywhere else here
}
```
