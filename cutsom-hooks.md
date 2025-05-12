# Custom Hooks

1. A custom hook in React is a `JavaScript function that starts with "use"`
2. allows you to `extract and reuse logic involving React state, effects, or context across multiple components.`

## Why Use Custom Hooks?

- `Eliminate code duplication.`
- Improve `readability and maintainability`.
- Separate concerns and logic cleanly.
- Share behavior between components without HOCs or render props.

```jsx
// basic example
// useCounter.js
import { useState } from 'react';

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);

  const increment = () => setCount((c) => c + 1);
  const decrement = () => setCount((c) => c - 1);
  const reset = () => setCount(initialValue);

  return { count, increment, decrement, reset };
}

export default useCounter;
```

```jsx
// usage
import useCounter from './useCounter';

function CounterComponent() {
  const { count, increment, decrement, reset } = useCounter(10);

  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>−</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

## 🔹 Rules of Hooks (Apply to Custom Hooks Too)

- `Only call hooks at the top level`
  — `don’t call them inside loops, conditions`, or nested functions.
- `Only call hooks from React functions`
  — either functional components or other custom hooks.

```jsx
// Advanced Example: useFetch Hook
// useFetch.js
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const controller = new AbortController();
    setLoading(true);

    fetch(url, { signal: controller.signal })
      .then((res) => {
        if (!res.ok) throw new Error('Network response was not ok');
        return res.json();
      })
      .then(setData)
      .catch((err) => {
        if (err.name !== 'AbortError') setError(err);
      })
      .finally(() => setLoading(false));

    return () => controller.abort();
  }, [url]);

  return { data, loading, error };
}

export default useFetch;
```

```jsx
// usage
function PostList() {
  const { data, loading, error } = useFetch(
    'https://jsonplaceholder.typicode.com/posts'
  );

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

| Benefit                | Description                                     |
| ---------------------- | ----------------------------------------------- |
| `Reusability`          | Encapsulate logic once, reuse everywhere        |
| Separation of concerns | Keeps components clean and focused on rendering |
| Easier testing         | Can test hook logic independently               |
| Composition            | You can build custom hooks from other hooks     |
