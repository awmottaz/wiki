---
sidebar_position: 2
---

# How to modify `rem` size

Browsers have settings to configure the base font size. By default, most browsers use `16px`. As a developer, you cannot rely on this value. As a responsible developer, you should respect users who choose a different scale factor for this.

If you want to adjust how big `1rem` is for a web page, you should do so as a scale factor of the user's configured `rem` size, not as an absolute size. Do this:

```css
:root {
  font-size: 1.2rem;
}
```

This will change the effective `rem` size for the rest of your CSS styles while still properly scaling with the user's browser settings.

See also: https://web.dev/learn/design/typography/#text-size

They recommend this kind of approach instead:

```css
@media (min-width: 30em) {
  html {
    font-size: 125%;
  }
}

@media (min-width: 40em) {
  html {
    font-size: 150%;
  }
}

@media (min-width: 50em) {
  html {
    font-size: 175%;
  }
}

@media (min-width: 60em) {
  html {
    font-size: 200%;
  }
}
```
