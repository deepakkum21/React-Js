# useEffect

- `1st time useEffect runs after every component execution`.
- and later on `depending on the dependencies array, if any dependency has changed the useEffect will re trigger`
- `perform side effects in function components`. Side effects include tasks like:
  - Fetching data from an API
  - Manually modifying the DOM
  - Setting up subscriptions or timers
  - Logging
- **Cleanup code runs**
  - `component unmount`
  - `before again dame useEffect runs`

```jsx
useEffect(() => {
  // code to run (effect)

  return () => {
    // optional cleanup function
  };
}, [dependencies]);
```

```jsx
useEffect(() => {
  console.log('Runs on every render');
});

// Run Only Once (on Mount)
useEffect(() => {
  console.log('Runs only once on mount');
}, []);

//Run on Specific State/Prop Change
useEffect(() => {
  console.log('Runs when `count` changes');
}, [count]);

//Cleanup Example
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Running...');
  }, 1000);

  return () => {
    clearInterval(timer); // Clean up on unmount or before next run
  };
}, []);
```

### Notes

- React ensures the `DOM is updated before running the effect`.
- Effects `run after paint, not during rendering`.
- `Cleanup helps avoid memory leaks`, especially in subscriptions or timers.
- while `passing function as dependencies, use useCallback() hook for that function to avoid getting re-created`, as on every render the function will be recreated, and just like Object, function recreated with same name are different and this will cause re-run of useEffect

---

## UseCallBack()

- helps `optimize performance by memoizing a function` — meaning it `will return the same function instance between renders unless its dependencies change`.

```jsx
const memoizedCallback = useCallback(() => {
  // function logic
}, [dependencies]);
// memoizedCallback will have the same reference between renders — until any dependency in the array changes.
```

### 🧠 Why Use useCallback?

- every time a component re-renders, all functions declared inside it are re-created.
- If these functions are passed as props to child components (especially memoized ones), it can cause unnecessary re-renders.
- useCallback prevents this by ensuring the function reference stays the same unless dependencies change.

### ⚠️ Caveats

- `Don’t use useCallback everywhere — only when passing functions to optimized children or inside useEffect/useMemo`.
- It adds complexity and a tiny memory cost — use it when profiling shows a performance bottleneck.

### Difference Between useCallback and useMemo

| Hook          | Returns                 | Use Case                             |
| ------------- | ----------------------- | ------------------------------------ |
| `useCallback` | A **memoized function** | Avoid recreating the same function   |
| `useMemo`     | A **memoized value**    | Avoid recalculating expensive values |
