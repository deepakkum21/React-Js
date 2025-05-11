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

### NOtes

- React ensures the `DOM is updated before running the effect`.
- Effects `run after paint, not during rendering`.
- `Cleanup helps avoid memory leaks`, especially in subscriptions or timers.
