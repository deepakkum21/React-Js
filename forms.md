# Forms

```jsx
<form>
  <label htmlFor="name"> Name </label>
  <input id="name" name="userName" />
  <button onClick={handleSubmit}>Submit</button>
</form>
```

1. By default button inside form tag are of `type=submit` which simulates the behavior of Submit form which will result in
   - `network call by adding all input fields as query params to the same server`
   - `refresh of the application`

```jsx
<form>
  <label htmlFor="name"> Name </label>
  <input id="name" name="userName" />
  <button type="button" onClick={handleSubmit}>
    Submit
  </button>
</form>
```

2. To avoid default behavior do any of the below things
   - add `type="button"` which will override default type of submit.
   - instead of adding onClick on button `add the same handler to submit attribute of form tag` and `also call preventDefault() inside the submitHandler`

```jsx
<form submit={handleSubmit}>
  <label htmlFor="name"> Name </label>
  <input id="name" name="userName" />
  <button>Submit</button>
</form>;

function handleSubmit(event) {
  event.preventDefault(); // to avoid reload
}
```

## Ways to accept input data from forms

1. \*\*from
