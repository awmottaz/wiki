# How to conditionally render in React

Here is an alternative to using ternary expressions (and especially nested ternary expressions): use an IIFE with conditionals or `switch` statements.

```jsx
function MyComponent({ someValue }) {
    return (
        <WrappingComponent>
        {(() => {
            if (someValue === 'option1') {
                return <div>Option 1</div>
            }
            if (someValue === 'option2') {
                return <div>Option 2</div>
            }
            return <div>Default</div>
        })()}
        </WrappingCompnent>
    )
}
```
