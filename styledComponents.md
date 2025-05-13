## Styling in REACT

1. **inline**

```jsx
<input
  type="email"
  style={{
    backgroundColor: emailNotValid ? '#fed2d2' : '#d1d5db',
  }}
  // className={emailNotValid ? 'invalid' : undefined}
  onChange={(event) => handleInputChange('email', event.target.value)}
/>
```

2. **conditional css**

```jsx
<input
  type="email"
  className={emailNotValid ? 'invalid' : undefined}
  onChange={(event) => handleInputChange('email', event.target.value)}
/>
```

3. **css modules**

- css file should be named as `${fileName}.module.css`
- that file should be imported as default import

```jsx
import classes from './Header.module.css';

export default function Header() {
  return (
    <header>
      <img src={logo} alt="A canvas" />
      <h1>ReactArt</h1>
      <p className={classes.paragraph}>
        A community of artists and art-lovers.
      </p>
    </header>
  );
}
```

4. **Styled component**

- Styled Components is a popular library that allows you to `write actual CSS code to style your components, using a technique called CSS-in-JS`. This means you write CSS styles directly in your JavaScript files and attach them to components.
- `npm install styled-components`
- Scoped Styles: `Styles are scoped to the component`.
- Dynamic Styling: `You can pass props to styled components`.
- Theming: Built-in support for themes.
- Automatic Critical CSS: Only the styles needed are loaded.

```jsx
import React from 'react';
import styled from 'styled-components';

// Define a styled component
const Button = styled.button`
  background-color: #4caf50;
  color: white;
  padding: 12px 24px;
  border: none;
  border-radius: 4px;
  cursor: pointer;

  &:hover {
    background-color: #45a049;
  }
`;

function App() {
  return (
    <div>
      <Button>Click Me</Button>
    </div>
  );
}

export default App;
```

```jsx
// Example with Props
const Button = styled.button`
  background: ${props => props.primary ? "#4CAF50" : "#e7e7e7"};
  color: ${props => props.primary ? "white" : "black"};
`;

<Button primary>Primary Button</Button>
<Button>Default Button</Button>

```
