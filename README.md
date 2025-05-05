# React-Js

### React app creation

- `create-react-app` deprecated
- `vite` recommended

```bash
npm create vite@latest my-app --template react
```

---

### props.children

- special prop that allows you to `pass components or elements between the opening and closing tags of a custom component`.
- It's often used to `create wrapper` or layout components.

```jsx
function Card(props) {
  return <div className="card">{props.children}</div>; // everything inside <Card>...</Card> (the <h1> and <p>) is passed into the Card component as props.children
}

// Usage
<Card>
  <h1>Hello</h1>
  <p>This is content inside the Card component.</p>
</Card>;
```

---

### props.children vs props attribute

| Feature              | `props.children`                | Regular props (`props.foo`)        |
| -------------------- | ------------------------------- | ---------------------------------- |
| Automatically passed | ✅ Yes                          | ❌ No — you must explicitly define |
| Content type         | Usually JSX elements or strings | Any type (string, number, object…) |
| Use case             | Wrapping/nesting content        | Configuration/data passing         |

---

### Listeners

```jsx
function Button() {
  function handleClick() {
    alert('Button clicked!');
  }

  return <button onClick={handleClick}>Click Me</button>;
}

// onClick is the event listener
// handleClick is the event handler function
```

```jsx
function InputExample() {
  function handleChange(event) {
    console.log('Input value:', event.target.value);
  }

  return <input type="text" onChange={handleChange} />;
}
```

| Event Name     | When It Triggers             |
| -------------- | ---------------------------- |
| `onClick`      | When an element is clicked   |
| `onChange`     | When input value changes     |
| `onSubmit`     | When a form is submitted     |
| `onMouseEnter` | When mouse enters an element |
| `onKeyDown`    | When a key is pressed down   |
| `onFocus`      | When an element gains focus  |

---

### Passing Listeners as PROPS

```jsx
function Parent() {
  function handleButtonClick() {
    alert('Button clicked in Child!');
  }

  return <Child onButtonClick={handleButtonClick} />;
}
```

```jsx
function Child(props) {
  return <button onClick={props.onButtonClick}>Click Me</button>;
}
```

### Passing Event Handler with Parameters

```jsx
function Parent() {
  function handleItemClick(itemId) {
    alert(`Item clicked: ${itemId}`);
  }

  return (
    <div>
      <Child onItemClick={() => handleItemClick(1)} />
    </div>
  );
}
```

```jsx
function Child(props) {
  return (
    <div>
      <button onClick={onItemClick}>Item 1</button>
    </div>
  );
}
```

---

### When you try to update the UI in React without using state

1. `You change a variable directly — but React doesn't re-render`

```jsx
let counter = 0;

function Counter() {
  function handleClick() {
    counter++; // Update variable
    console.log(counter); // Logs updated value
  }

  return (
    <>
      <p>Counter: {counter}</p>
      <button onClick={handleClick}>Increment</button>
    </>
  );
}
```

- What happens?
  - Clicking the button changes the counter variable.
  - But the UI doesn't update because `React doesn't know it needs to re-render`.
- Why?
  - `React only re-renders when state (via useState) or props change.`
  - `Changing regular variables doesn't trigger a re-render.`

---

### Hooks

- special functions introduced in React 16.8
- Hooks let functional components do everything class components could

| Hook                                                   | Purpose                                                   |
| ------------------------------------------------------ | --------------------------------------------------------- |
| `useState`                                             | Adds state to functional components                       |
| `useEffect`                                            | Handles side effects (data fetching, subscriptions)       |
| `useContext`                                           | Access React context                                      |
| `useRef`                                               | Persist values across renders without causing re-renders  |
| `useMemo`                                              | Memoize expensive calculations                            |
| `useCallback`                                          | Memoize functions to prevent unnecessary re-renders       |
| `useReducer`                                           | For complex state logic (Redux-style)                     |
| `useLayoutEffect`                                      | Like `useEffect`, but fires synchronously after DOM paint |
| `useImperativeHandle`                                  | Customize instance exposed to parent with `ref`           |
| `useId`, `useSyncExternalStore`, `useTransition`, etc. | More advanced hooks                                       |

### Where Can Hooks Be Called?

- `At the top level of a React functional component`
- `Inside another custom hook`

```jsx
function MyComponent() {
  const [count, setCount] = useState(0); // ✅ valid  called in top level of a React functional component
}
```

```jsx
function useCustomLogic() {
  const theme = useContext(ThemeContext); // ✅ valid  called Inside another custom hook
}
```

### You CANNOT call hooks:

- `Inside loops, conditions, or nested functions`
- `Inside class components`
- `Outside of React components or hooks`

```jsx
// ❌ DON'T DO THIS
if (someCondition) {
  useEffect(() => {
    // Invalid! Hooks must be unconditional
  }, []);
}
```

```jsx
function handleClick() {
  useState(); // ❌ Invalid inside event handlers
}
```
