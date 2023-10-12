---
sidebar_position: 2
---

# Detect the OS from the browser

AFAICT, there is no stable API for looking at the client's platform.

- [`navigator.platform`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/platform) is deprecated.
- [`navigator.userAgentData.platform`](https://developer.mozilla.org/en-US/docs/Web/API/NavigatorUAData/platform) is experimental and not supported everywhere.

Therefore we should use both to detect the platform.

```js
const platform =
  navigator.userAgentData?.platform ?? navigator.platform ?? "unknown";

const isMac = /mac/i.test(platform);
const isWin = /win/i.test(platform);
const isLinux = /linux/i.test(platform);
```
