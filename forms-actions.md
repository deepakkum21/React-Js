# Forms Action [V 19+]

1. `<form action={actionHandler}>` in react will add few things
   - `override the form action by adding preventDefault automatically`
   - add `formData parameter to the handlerMethod` instead of event in normal onSubmitHandler, so `no need to do new FormData(event.target)`
   - also byDefault `on click of submit, the input fields are reset`

## useActionState()

- `Manage the state of a form action.`
- Handle form submissions and transitions (like pending, success, error states).
- Replace useState() + useTransition() + custom form logic for submitting to server actions.
- to avoid resetting of form inputs after submit

  - use `defaultValue` attribute of the fields

          <input
              name="title"
              type="text"
              placeholder="Enter todo"
              defaultValue={state.formDefaultData?.title}
          />

  - also `return the values in a key from the handleSubmitHandler`

- syntax

```jsx
const [state, formAction, isPending] = useActionState(action, initialState, dependencies?);
```

**Parameters**:

- `action`: A function where logic to handle formSubmission (e.g., form handler).
- `initialState`: The initial value of the state.
- `dependencies`: (Optional) An array of dependencies for re-running the action logic.

**Returns**:

- `state`: The current state after the action.
- `formAction`: A function you can pass to a `<form action={formAction}>`.
- `isPending`: A boolean indicating if the action is in progress.

```jsx
const initialState = { message: '', error: '' };

export default function TodoForm() {
  function createTodo(prevState, formData) {
    const title = formData.get('title');

    if (!title) {
      return {
        error: 'Title is required',
        formDefaultData: {
          title,
        },
      };
    }

    // Imagine DB call here
    return { message: `Created todo: ${title}` };
  }

  const [state, formAction, isPending] = useActionState(
    createTodo,
    initialState
  );

  return (
    <form action={formAction}>
      <input
        name="title"
        type="text"
        placeholder="Enter todo"
        defaultValue={state.formDefaultData?.title}
      />
      <button type="submit" disabled={isPending}>
        {isPending ? 'Creating...' : 'Create Todo'}
      </button>
      {state.message && <p>{state.message}</p>}
      {state.error && <p style={{ color: 'red' }}>{state.error}</p>}
    </form>
  );
}
```

---

## useFormStatus()

### ✅ Purpose

- Know `if a form is currently submitting`.
- `Show loading indicators, disable buttons`, or handle optimistic UI easily.

### Where Can It Be Used?

- useFormStatus() is designed to `work inside a child component of a <form> that uses a Server Action`.
- It only works within `the same React tree as the form` using Server Actions (e.g., inside the submit button or feedback messages).

```jsx
const { pending, data, method, action } = useFormStatus();
```

| Property  | Type       | Description                              |
| --------- | ---------- | ---------------------------------------- |
| `pending` | `boolean`  | Whether the form is currently submitting |
| `data`    | `FormData` | The submitted form data (if available)   |
| `method`  | `string`   | HTTP method (typically "POST")           |
| `action`  | `string`   | URL or function used for the form action |

```jsx
// app/actions.js

export async function submitMessage(formData) {
  const message = formData.get('message');
  console.log('Received:', message);
  // Simulate saving message...
}
```

```jsx
// app/components/SubmitButton.tsx

import { useFormStatus } from 'react-dom';

export default function SubmitButton() {
  const { pending } = useFormStatus();

  return (
    <button type="submit" disabled={pending}>
      {pending ? 'Sending...' : 'Send Message'}
    </button>
  );
}
```

```jsx
import { submitMessage } from './actions';
import SubmitButton from './components/SubmitButton';

export default function Page() {
  return (
    <form action={submitMessage}>
      <input type="text" name="message" placeholder="Your message" required />
      <SubmitButton />
    </form>
  );
}
```

### Common Use Cases

- `Disable submit button while form is submitting`.
- `Show loading spinners`.
- Display optimistic UI changes.
- Improve form UX with minimal code.

| Feature    | `useActionState()`                       | `useFormStatus()`                             |
| ---------- | ---------------------------------------- | --------------------------------------------- |
| Type       | Full hook to manage server action state  | Lightweight hook for form status              |
| Returns    | \[state, actionFn, pending]              | { pending, data, method, action }             |
| Use Case   | Manage form result and server-side state | Track submission state inside form components |
| Common Use | Main form component                      | Submit buttons, loading indicators            |

---

## Registering multiple formAction | One Form, Multiple Submit Buttons with formAction

```jsx
<form>
  <input name="itemId" defaultValue="123" hidden />

  <button formAction={deleteItem}>Delete</button>
  <button formAction={updateItem}>Update</button>
</form>
```

**How it works:**

- The `<form> tag has no action, so buttons define it`.
- `Each button can define a different formAction` server function.
- React determines which formAction was used based on the clicked button.

```jsx
// action.js

export async function deleteItem(formData) {
  const id = formData.get('itemId');
  console.log('Deleting', id);
}

export async function updateItem(formData) {
  const id = formData.get('itemId');
  const name = formData.get('name');
  console.log('Updating', id, 'with name:', name);
}
```

```jsx
// formComponent

import { deleteItem, updateItem } from './actions';

export default function ItemForm() {
  return (
    <form>
      <input type="hidden" name="itemId" value="123" />
      <input type="text" name="name" placeholder="New name" />

      <button formAction={deleteItem}>Delete</button>
      <button formAction={updateItem}>Update</button>
    </form>
  );
}
```

### Notes

- formAction `only works on <button>, not on <input type="submit">`.
- If you're using `useFormStatus() inside buttons, each button will track its own pending state`.

---

## useOptimistic()

- Optimistic UI is `when you pretend the server has already responded positively and update the UI immediately, then roll back if the server fails.`

```jsx
const [optimisticState, addOptimistic] = useOptimistic(currentState, updateFn);
```

**Parameters**:

- `currentState`: The actual state (e.g. from the server).
- `updateFn`: A function to compute the next optimistic state based on previous state and user input.
  - 1st param is previousState,
  - other param can be what is passed by addOptimistic fnc

**Returns**:

- `optimisticState`: What to render optimistically.
- `addOptimistic`: A function to trigger optimistic updates (often from a formAction).

```jsx
// app/action.js
export async function addComment(prevState, formData) {
  const comment = formData.get('comment');
  // Simulate server-side DB logic here...
  return { success: true, comment };
}
```

```jsx
//

import { useOptimistic, useState } from 'react';
import { addComment } from './actions';

export default function CommentForm() {
  const [comments, setComments] = useState<string[]>([]);
  const [optimisticComments, addOptimisticComment] = useOptimistic(
    comments,
    (prev, newComment: string) => [...prev, newComment]
  );

  async function formAction(prevState: any, formData: FormData) {
    const newComment = formData.get('comment') as string;
    addOptimisticComment(newComment); // Show optimistic update

    const result = await addComment(prevState, formData);
    if (result.success) {
      setComments(prev => [...prev, newComment]);
    }
  }

  return (
    <form action={formAction}>
      <input name="comment" placeholder="Write a comment..." required />
      <button type="submit">Post</button>

      <ul>
        {optimisticComments.map((c, i) => (
          <li key={i}>{c}</li>
        ))}
      </ul>
    </form>
  );
}
```

| Benefit                   | Description                                                 |
| ------------------------- | ----------------------------------------------------------- |
| Fast perceived UX         | UI updates instantly, even before server responds           |
| Declarative & simple      | Keeps logic localized and easy to understand                |
| Works with server actions | Designed to pair with `useActionState()` and server actions |
| Revert on failure         | You can roll back easily if the action fails                |

| Feature             | `useState()`          | `useOptimistic()`                         |
| ------------------- | --------------------- | ----------------------------------------- |
| UI reacts to server | After response        | Before response (optimistically)          |
| Revert on failure   | Manual                | Easier with separation of true state      |
| Intended for        | Regular state updates | Server Action forms and async UI feedback |
