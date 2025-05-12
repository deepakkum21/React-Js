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
<form onSubmit={handleSubmit}>
  <label htmlFor="name"> Name </label>
  <input id="name" name="userName" />
  <button>Submit</button>
</form>;

function handleSubmit(event) {
  event.preventDefault(); // to avoid reload
}
```

## Ways to accept input data from forms

1. **form input data using generic handlers**

- `using useState`

```jsx
const [enteredValues, setEnteredValues] = useState({
  email: '',
  password: '',
});

function handleSubmit(event) {
  event.preventDefault();

  console.log(enteredValues);
}

function handleInputChange(identifier, value) {
  setEnteredValues((prevValues) => ({
    ...prevValues,
    [identifier]: value,
  }));
}

<form onSubmit={handleSubmit}>
  <h2>Login</h2>

  <div className="control-row">
    <div className="control no-margin">
      <label htmlFor="email">Email</label>
      <input
        id="email"
        type="email"
        name="email"
        onChange={(event) => handleInputChange('email', event.target.value)}
        value={enteredValues.email}
      />
    </div>

    <div className="control no-margin">
      <label htmlFor="password">Password</label>
      <input
        id="password"
        type="password"
        name="password"
        onChange={(event) => handleInputChange('password', event.target.value)}
        value={enteredValues.password}
      />
    </div>
  </div>

  <p className="form-actions">
    <button className="button button-flat">Reset</button>
    <button className="button">Login</button>
  </p>
</form>;
```

2. **getting input data from Ref's**

```jsx
export default function Login() {
  const email = useRef();
  const password = useRef();

  function handleSubmit(event) {
    event.preventDefault();

    const enteredEmail = email.current.value;
    const enteredPassword = password.current.value;

    console.log(enteredEmail, enteredPassword);
  }

  return (
    <form onSubmit={handleSubmit}>
      <h2>Login</h2>

      <div className="control-row">
        <div className="control no-margin">
          <label htmlFor="email">Email</label>
          <input id="email" type="email" name="email" ref={email} />
        </div>

        <div className="control no-margin">
          <label htmlFor="password">Password</label>
          <input id="password" type="password" name="password" ref={password} />
        </div>
      </div>

      <p className="form-actions">
        <button className="button button-flat">Reset</button>
        <button className="button">Login</button>
      </p>
    </form>
  );
}
```
