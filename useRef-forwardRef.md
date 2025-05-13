# useRef Vs useState

1. **useRef**

- Purpose: `Holds a mutable reference to a DOM element `or a value across renders.
- Triggers Re-render: `❌ No — changing a ref does not trigger a re-render`.
- Usage: `Accessing DOM nodes directly, storing timers, or persisting data between renders without causing re-renders`
- `ref.current.{attributeName}`

```jsx
const inputRef = useRef(null);

const focusInput = () => {
  inputRef.current.focus();
};

return (
  <>
    <input ref={inputRef} />
    <button onClick={focusInput}>Focus Input</button>
  </>
);
```

2. **useState**

- Purpose: Stores component data that affects rendering.
- Triggers Re-render: `✅ Yes — changing state causes the component to re-render`.
- Usage: When UI needs to update in response to data changes.

| Feature                    | `state` (useState) | `ref` (useRef) |
| -------------------------- | ------------------ | -------------- |
| Triggers re-render?        | ✅ Yes             | ❌ No          |
| Use for UI rendering?      | ✅ Yes             | 🚫 No          |
| Mutable without re-render? | ❌ No              | ✅ Yes         |
| DOM access?                | 🚫 No              | ✅ Yes         |

## ForwardRef() [react 19 before]

- before React19 `ref could not be passed as a prop`.
- HOF forwardRef is used.

```jsx
const CustomInput = React.forwardRef(function CustomInput(props, ref) => {
  return <input ref={ref} {...props} />;
});
export default CustomInput;

const App = () => {
  const inputRef = useRef();

  return <CustomInput ref={inputRef} />;
};
```
