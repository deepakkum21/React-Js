# useImperativeHandle()

- lets you `customize the instance value exposed to parent components` when using ref with forwardRef

```jsx
// using ref
import React, { useRef, useImperativeHandle } from 'react';

function MyInput({ ref }) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current?.focus();
    },
    clear: () => {
      inputRef.current.value = '';
    },
  }));

  return <input ref={inputRef} />;
}

function Parent() {
  const inputRef = useRef(null);

  const handleFocus = () => {
    inputRef.current?.focus();
  };

  const handleClear = () => {
    inputRef.current?.clear();
  };

  return (
    <div>
      <MyInput ref={inputRef} />
      <button onClick={handleFocus}>Focus Input</button>
      <button onClick={handleClear}>Clear Input</button>
    </div>
  );
}
```

```jsx
// using forwardRef
import React, { useRef, useImperativeHandle, forwardRef } from 'react';

// Child Component
const FancyInput = forwardRef(function FancyInput(props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    },
    clear: () => {
      inputRef.current.value = '';
    }
  }));

  return <input ref={inputRef} placeholder="Type something" />;
});

// Parent Component
const App = () => {
  const fancyInputRef = useRef();

  return (
    <>
      <FancyInput ref={fancyInputRef} />
      <button onClick={() => fancyInputRef.current.focus()}>Focus Input</button>
      <button onClick={() => fancyInputRef.current.clear()}>Clear Input</button>
    </>
  );
};
```
