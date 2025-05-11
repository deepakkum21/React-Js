# useMemo()

1. `memoize the result of a computation` — useful when you have expensive calculations or want to `avoid recalculating values unnecessarily during re-renders`

```jsx
const memoizedValue = useMemo(() => {
  // compute something expensive
  return result;
}, [dependencies]);
```

### 🧠 When to Use useMemo

- You’re `doing CPU-intensive calculations` (e.g., filtering a large array).
- You want to `avoid recalculating derived data that hasn't changed`.
- You’re passing derived values to memoized components or effect hooks.

### ❌ When NOT to Use It

- If the calculation is cheap.
- If you don't face performance issues.
- Overusing useMemo can make your code harder to read and adds unnecessary complexity.

```jsx
function ExpensiveComponent({ num }) {
  const computeFactorial = (n) => {
    console.log('Computing factorial...');
    return n <= 1 ? 1 : n * computeFactorial(n - 1);
  };

  const factorial = useMemo(() => computeFactorial(num), [num]);

  return (
    <div>
      Factorial of {num} is {factorial}
    </div>
  );
}
```
