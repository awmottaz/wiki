---
sidebar_position: 2
---

# Aborting fetch and other Promises

## Aborting fetch

Abort a `fetch` using an [`AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController).

```js
const context = {};

async function fetchVideo() {
  context.controller = new AbortController();

  try {
    const response = await fetch("/video.mp4", {
      signal: context.controller.signal,
    });
    console.log("Download complete", response);
  } catch (error) {
    // if the fetch is aborted, there will be an abort error here.
    console.error(`Download error: ${err.message}`);
  }
}

document.querySelector("#cancel-button").addEventListener("click", () => {
  context.controller?.abort();
});

document
  .querySelector("#download-button")
  .addEventListener("click", fetchVideo);
```

## Aborting any Promise

We can also use `AbortSignal` to early-reject a Promise.

```ts title="cancellable.js"
/**
 * Use this to pair any `Promise` with an `AbortSignal`. You can call the
 * `abort()` method on your controller to reject the Promise early.
 */
export function cancellable<T>(
  wrappedPromise: Promise<T>,
  signal: AbortSignal,
) {
  return new Promise((resolve, reject) => {
    signal.addEventListener("abort", () => {
      reject("Promise aborted.");
    });
    wrappedPromise.then(resolve).catch(reject);
  });
}
```

Here is an example of usage.

```js
// Here is a generic async function that resolves after 5 seconds.
function doStuff() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("success!");
    }, 5000);
  });
}

// Create your controller.
const controller = new AbortController();

// In a UI application, you might call `abort()` in response to a user
// action, such as clicking a button. For this example, we will wait
// 2 seconds and then call it. Play around with this timeout to see what
// happens with your cancellable promise!
setTimeout(() => {
  controller.abort();
}, 2000);

try {
  const result = await cancellable(doStuff(), controller.signal);
  console.log({ result });
} catch (error) {
  console.log({ error });
}

console.log("the Promise has settled and we can move on");
```
