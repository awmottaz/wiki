Abort a `fetch` using an [`AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController).

```ts
let controller;
const url = "video.mp4";

const downloadBtn = document.querySelector(".download");
const abortBtn = document.querySelector(".abort");

downloadBtn.addEventListener("click", fetchVideo);

abortBtn.addEventListener("click", () => {
  if (controller) {
    controller.abort();
    console.log("Download aborted");
  }
});

function fetchVideo() {
  controller = new AbortController();
  const signal = controller.signal;
  fetch(url, { signal })
    .then((response) => {
      console.log("Download complete", response);
    })
    .catch((err) => {
      console.error(`Download error: ${err.message}`);
    });
}
```

We can also use `AbortSignal` to early-reject a Promise.

https://gist.github.com/awmottaz/61c08a9bbcace18e7ae7da88e349cd44
