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
