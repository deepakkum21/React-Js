## Problems with Props drilling

1. **Reduced Reusability**

- Components that are meant to be `generic now have to know about and accept props that aren’t relevant to them`, reducing modularity and reusability.

2. **Code Becomes Harder to Maintain**

- As your component tree grows, `passing props through many layers clutters your code and makes it harder to understand.`

3. **Performance Issues**

- When the top-level state changes, `it causes re-renders all the way down the tree, even in components that don’t care about the updated data`.

4. Alternatives

- React Context API – Good for global state or themes, but overuse can lead to performance issues.
- State Management Libraries (e.g., Redux, Zustand, Jotai) – Better for complex state.
- Component Composition / Render Props / Hooks – Cleaner and more flexible in some scenarios.

---

## Use() Vs useContext()

- both can be used for Drilling down state
- `use() can be used inside if condition`
- use() in React19+

```jsx
if (someCondition) {
  const useCtx = use(); // ✅ valid
}

if (someCondition) {
  const useCtx = useContext(); // ❌ not valid
}
```

---

## useContext()

- `<ThemeContext.Consumer>` was used in version < 19

1. **Create the Context**

```jsx
// ThemeContext.js
import React, { createContext, useState } from 'react';

// default property for autocompletion
export const ThemeContext = createContext({
  theme: '',
  toggleTheme: () => {},
});

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const handleToggleTheme = () =>
    setTheme((prev) => (prev === 'light' ? 'dark' : 'light'));

  const ctxValue = {
    theme: theme,
    toggleTheme: handleToggleTheme,
  };

  return (
    <ThemeContext.Provider value={ctxValue}>{children}</ThemeContext.Provider>
  );
}
```

2. **Use `useContext()` in a Component**

```jsx
// ThemedComponent.js
import React, { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

function ThemedComponent() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <div
      style={{
        backgroundColor: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#000' : '#fff',
        padding: '20px',
      }}
    >
      <p>Current Theme: {theme}</p>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
}

export default ThemedComponent;
```

3. **Wrap Your App with the Provider**

```jsx
// App.js
import React from 'react';
import { ThemeProvider } from './ThemeContext';
import ThemedComponent from './ThemedComponent';

function App() {
  return (
    <ThemeProvider>
      <ThemedComponent />
    </ThemeProvider>
  );
}

export default App;
```

---

## useReducer()

- `React Hook used as an alternative to useState for managing more complex state logic`

```jsx
const [state, dispatch] = useReducer(reducerFunction, initialState);
// state: Current state value
// dispatch: Function used to send an action to the reducer
// reducer: Function that receives (state, action) and returns the new state
// initialState: The initial state value
```

```jsx
const initialForm = {
  name: '',
  email: '',
  subscribed: false,
};

function formReducer(state, action) {
  switch (action.type) {
    case 'UPDATE_FIELD':
      return { ...state, [action.field]: action.value };
    case 'TOGGLE_SUBSCRIBE':
      return { ...state, subscribed: !state.subscribed };
    case 'RESET':
      return initialForm;
    default:
      return state;
  }
}

function Form() {
  const [formState, dispatch] = useReducer(formReducer, initialForm);

  return (
    <form>
      <input
        type="text"
        placeholder="Name"
        value={formState.name}
        onChange={(e) =>
          dispatch({
            type: 'UPDATE_FIELD',
            field: 'name',
            value: e.target.value,
          })
        }
      />
      <input
        type="email"
        placeholder="Email"
        value={formState.email}
        onChange={(e) =>
          dispatch({
            type: 'UPDATE_FIELD',
            field: 'email',
            value: e.target.value,
          })
        }
      />
      <label>
        <input
          type="checkbox"
          checked={formState.subscribed}
          onChange={() => dispatch({ type: 'TOGGLE_SUBSCRIBE' })}
        />
        Subscribe
      </label>
      <button type="button" onClick={() => dispatch({ type: 'RESET' })}>
        Reset
      </button>
    </form>
  );
}
```

### When to Use useReducer Instead of useState

| Use `useState` when...                        | Use `useReducer` when...                          |
| --------------------------------------------- | ------------------------------------------------- |
| State logic is simple                         | State logic is complex or involves many variables |
| State updates are independent                 | Updates depend on previous state or actions       |
| Minimal state transitions                     | You want a Redux-like pattern                     |
| You don’t need to group logic in one function | You want all logic centralized in a reducer       |

---
