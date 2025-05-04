# React-Js

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
