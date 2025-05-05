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

### What's the purpose of "State" in React apps?

- its the data when managed causes React to re-evaluate a component

---

### useState()

- `lets you add state to functional components`

```jsx
const [state, setState] = useState(initialValue);

// state: current state value
// setState: function to update the state
// initialValue: value used for the first render

const [count, setCount] = useState(0);
<button onClick={() => setCount(count + 1)}>Increment</button>;
```

### Why We See the Old Value in console.log After setState()

```jsx
const [count, setCount] = useState(0);

const handleClick = () => {
  setCount(count + 1);
  console.log(count); // 🛑 Logs OLD value!
};
```

`❓ Why?`

- setState() in React is `asynchronous`. That means:
- `It schedules an update`.
- The `actual state change (and re-render) happens after the function exits`.
- So in the console.log(count), you're logging the value before React has updated it.

| Concept                                | Explanation                                                  |
| -------------------------------------- | ------------------------------------------------------------ |
| **State is preserved across renders**  | React keeps the state between function calls                 |
| **State updates cause re-renders**     | Updating state tells React to re-render the component        |
| **State updates are async**            | You won’t see changes immediately after `setState()`         |
| **You must not mutate state directly** | Always use `setState` to update it                           |
| **Can be used multiple times**         | You can have multiple `useState` hooks in a single component |

❌ Mutating state directly

```jsx
state.count++; // Don't do this
setCount((prev) => prev + 1); // correct way
```

---

### different ways to render the content in react conditionally

| Method                 | Use When                      | Example Syntax                   |
| ---------------------- | ----------------------------- | -------------------------------- |
| `if` / `else`          | Multi-branch or verbose logic | `if (isLoggedIn) { return ... }` |
| Ternary operator `? :` | Short inline conditional      | `{condition ? A : B}`            |
| `&&` logical operator  | One-way condition             | `{condition && A}`               |
| `switch`               | Multiple specific values      | `switch (value) { ... }`         |
| IIFE                   | Inline but with complex logic | `{(() => { ... })()}`            |
| Optional chaining      | Safe access to nested props   | `{user?.name}`                   |

```jsx
// if Statements (inside function)
function MyComponent({ isLoggedIn }) {
  if (isLoggedIn) {
    return <h1>Welcome back!</h1>;
  } else {
    return <h1>Please sign in.</h1>;
  }
}

// Ternary Operator
<p>{isLoggedIn ? "Logout" : "Login"}</p>
{isAdmin ? <AdminPanel /> : <UserPanel />}

// Logical AND (&&) Operator
{isLoading && <p>Loading...</p>}

// Switch Statements
function getContent(role) {
  switch (role) {
    case "admin":
      return <AdminDashboard />;
    case "user":
      return <UserDashboard />;
    default:
      return <GuestView />;
  }
}

// Immediately Invoked Function Expressions (IIFE)
{(() => {
  if (error) return <ErrorMessage />;
  if (loading) return <Spinner />;
  return <DataView />;
})()}

// Conditional Classes or Styles
<div className={isDark ? "dark-mode" : "light-mode"}></div>

// Using Optional Chaining
<p>{user?.name}</p>
```

---

### Fragment

- `used to group multiple elements without adding an extra node to the DOM`.
- Since `return can return only one statement`, we tend to wrap with some div, but div or any other tag gets rendered to dom, but fragment doesn't.
- `If you need to pass a key (especially in list rendering), you must use the long form`

```jsx
function App() {
  return (
    <h1></h1>
    <div></div> // ❌invalid as only one statement can be returned
  )
}

function App() {
  return (
    <div>
      <h1></h1>
      <div></div> // valid but div or any other tag gets rendered to dom
    </div>
  )
}

function App() {
  return (
    <React.Fragment>
      <h1></h1>
      <div></div> // ✅ valid as fragment is not rendered
    </React.Fragment>
  )
}

function App() {
  return (
    <>
      <h1></h1>
      <div></div> // ✅ valid Shorthand <>...</>
    </>
  )
}

function List() {
  return items.map(item => (
    <React.Fragment key={item.id}>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>  // If you need to pass a key (especially in list rendering), you must use the long form
    </React.Fragment>
  ));
}
```

---

### Forwarded Props [Proxy Props]

- `pattern where a component receives props and passes (forwards) them down to a child or underlying DOM element`, often without explicitly knowing or using them itself.

1. **Why**

- `Reusability`:
  - You can `build wrapper components (like buttons, inputs, modals)` that accept arbitrary props and pass them along.
- `Custom Styling/Behavior`:
  - Allows consumers to add custom props (like onClick, style, className, etc.) without modifying the wrapper.
- `Better Abstraction`:
  - Components can remain generic and flexible.

2. **How to Forward Props**

```jsx
function CustomButton(props) {
  return <button {...props} />;
}

<CustomButton onClick={handleClick} className="primary" />;
```

```jsx
function CustomInput({ label, ...rest }) {
  return (
    <>
      <label>{label}</label>
      <input {...rest} />
    </>
  );
}
```

3. ⚠️ **Limitations**

- `Prop Collisions`:
  - If the component uses a prop (e.g., className or onClick), blindly forwarding can override or duplicate it.
- `Harder Debugging`:
  - Overuse or careless forwarding can make components unpredictable.
- No Type Safety (in JS):
  - You may pass irrelevant props that aren't intended for the underlying component.
- `React.forwardRef Needed for ref`:
  - If you want to `forward a ref, you must explicitly use React.forwardRef`.

```jsx
const FancyInput = React.forwardRef((props, ref) => (
  <input ref={ref} {...props} />
));
```
