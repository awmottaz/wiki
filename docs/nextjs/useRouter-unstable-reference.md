# `useRouter` methods are not a stable reference on re-render

This is a bug that affects Next.js prior to v13.4.9.

[Issue #18127 in Next.js](https://github.com/vercel/next.js/issues/18127).

This was fixed in the [v13.4.9 release](<https://github.com/vercel/next.js/releases?q=memo&expanded=true#:~:text=router%3A%20%2351869-,Memoize%20useRouter%20from%20next/navigation%20when%20used%20in%20Pages%20Router%3A%20%2352177,-chore(deps)%3A%20bump>) via [PR #52177](https://github.com/vercel/next.js/pull/52177).

## Symptoms

The `useRouter()` hook returns several state variables (such as `pathname` and `query`) and methods (such as `push` and `replace`). When the state changes, the methods are also recreated. They are an unstable reference.

This example can result in unexpected extra router pushes, or even an infinite loop in certain cases.

```js
const { push } = useRouter();

useEffect(() => {
  push(/*...*/);
}, [push]);
```

## Workaround

If you are using an affected version of Next.js, the best workaround I've found is to only use router methods from the global `Router` instance.

```js
import Router, { useRouter } from "next/router";

function SomeComponent() {
  // Only use state from this hook.
  const { pathname, query } = useRouter();

  useEffect(() => {
    Router.push(/*...*/);
  }, []);

  return; /*...*/
}
```

:::tip

You probably should not use `useEffect` like this, anyway! See https://react.dev/learn/you-might-not-need-an-effect

:::
