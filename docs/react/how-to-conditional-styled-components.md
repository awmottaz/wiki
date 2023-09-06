# How to conditionally apply styles in styled-components

Here I describe a few patterns for how to conditionally set styles on components using [styled-components](https://styled-components.com/). These are presented in order of preference, but each includes a brief description of the suitability of the technique to different goals.

## Pattern 1: Attribute selectors

Attribute selectors are a fantastic way to conditionally apply styles based on the _semantics of your HTML_. Ideally, your DOM elements will convey certain semantic states inherently. For example, `checked` states for checkboxes and radio inputs or various `aria-` attributes.

Adding to this list, I also like the idea of using `data-` attributes to reflect certain state in the DOM.

All of these states are natively accessible to CSS selectors, and using them improves both your semantics and the performance of styled-components, since it does not need to generate additional styles.

```jsx
import styled from "styled-components";

const Checkbox = styled.input.attrs({ type: "checkbox" })`
  font-weight: normal;

  :checked {
    font-weight: bold;
  }
`;
```

:::tip

Once the `:has` selector [has full browser support](https://caniuse.com/css-has), this pattern will be even more useful!

Suppose you have a wrapper element around the checkbox that you want to style conditionally based on whether the checkbox was checked. Before `:has`, the only option you had was reading the `checked` prop in your styled component and writing conditional styles based on the value of that prop:

```jsx
const CheckboxWrapper = styled.span`
  color: ${({ checked }) => (checked ? "green" : "red")};
`;
```

But with `:has`, we can fully represent this in native CSS:

```jsx
const CheckboxWrapper = styled.span`
  color: red;
  :has(${Checkbox}:checked) {
    color: green;
  }
`;
```

:::

Here is another example: suppose you have a lot of CSS rules that you want to conditionally apply, but you don't have a good pseudo-element selector or ARIA attribute selector to use.

The traditional way I've seen to solve this problem was by using the [`css` helper](https://styled-components.com/docs/api#css) and conditionally including that block of CSS in your component.

```jsx
// I do not recommend this pattern...
import styled, { css } from "styled-components";

const someVariantStyles = css`
  // a bunch of CSS rules here...
`;

const SomeComponent = styled.div`
  ${({ $someCondition }) => (someCondition ? someVariantStyles : "")}
`;

export function MyComponent({ someCondition, children }) {
  return (
    <SomeComponent $someCondition={someCondition}>{children}</SomeComponent>
  );
}
```

Instead, set up your own `data-` attributes and use the attribute selector to conditionally apply those styles.

```jsx
import styled from "styled-components";

const SomeComponent = styled.div`
  [data-condition="true"] {
    // a bunch of CSS rules here...
  }
`;

export function MyComponent({ someCondition, children }) {
  return (
    <SomeComponent data-condition={someCondition}>{children}</SomeComponent>
  );
}
```

This pattern is not only for booleans! You can do this for multi-valued `data-` attributes as well. This is good for setting up different style variants on a component, for example.

:::note

This isn't to say that you should _never_ use the `css` helper! It can be a really useful tool for defining _mixins_ of preset styles that you want to share across several styled components. This section is specifically recommending against using `css` only for _conditionally including_ a block of CSS.

:::

## Pattern 2: Using the `style` prop with CSS custom properties

This pattern is good for supporting an arbitrary number of values or "tweening" between values on a CSS rule.

```tsx
import type { ReactNode } from "react";
import styled from "styled-components";

const BackgroundDiv = styled.div`
  opacity: var(--opacity, 1);
`;

type Props = {
  opacity: number;
  children: ReactNode;
};

export function Background({ opacity, children }: Props) {
  return (
    <BackgroundDiv style={{ "--opacity": opacity }}>{children}</BackgroundDiv>
  );
}
```

:::note

A special thanks to [Josh W. Comeau for this tip](https://www.joshwcomeau.com/css/styled-components/#css-variables-1)!

:::

## Pattern 3: Styled props

This pattern is a last resort. Interpolating props is a marquee feature of styled-components, but keep in mind that it is syntax sugar for generating different class names for each combination of prop values you use, and those styles incur some runtime overhead:

1. Generate the relevant CSS
2. Inject that into the `head`
3. Modify the required DOM nodes to use the generated class names

```tsx
import styled from "styled-components";

type ButtonProps = {
  $variant: "normal" | "success" | "danger";
};

export const Button = styled.button<ButtonProps>`
  background-color: ${({ $variant }) => {
    switch ($variant) {
      case "success":
        return "green";
      case "danger":
        return "red";
      case "normal":
      default:
        return "blue";
    }
  }};
`;
```

:::tip

The dollar-sign prefix on these props is intentional. It uses the [transient props](https://styled-components.com/docs/api#transient-props) feature of styled-components to avoid passing the props to wrapped components or as attributes on the underlying component. I tend to use this by default on all of my styled-components props.

:::
