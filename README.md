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
