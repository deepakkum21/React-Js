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
