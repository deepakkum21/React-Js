## ReactDOM.createPortal

- lets you `render children into a different part of the DOM, outside the main parent hierarchy` — without breaking the React context

### Why Use createPortal

Normally, components are rendered inside their parent DOM tree. But sometimes you want to:

- Render modals that escape CSS overflow: hidden
- Place tooltips, dropdowns, or popups above all elements
- Attach elements to `<body>` or another part of the DOM

```jsx
<body>
  <div id="root"></div>
  <div id="modal-root"></div> <!-- Separate portal target -->
</body>
```

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

// child
export default Modal = ({ children }) => {
  return ReactDOM.createPortal(
    <div className="modal">{children}</div>,
    document.getElementById('modal-root')
  );
};

// parent
export default App = () => {
  const [open, setOpen] = React.useState(false);

  return (
    <>
      <button onClick={() => setOpen(true)}>Open Modal</button>
      {open && (
        <Modal>
          <div>
            <h2>Hello from the Portal!</h2>
            <button onClick={() => setOpen(false)}>Close</button>
          </div>
        </Modal>
      )}
    </>
  );
};
```
