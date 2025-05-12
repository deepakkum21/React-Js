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

3. **getting-values-formdata**

- FormData can give all the data from the input form, few things to be taken care
  - `new FormData(event.target)`
  - all form `fields should have name attribute`
  - `arrays data are noy captured like [multiple selection combobox]` when `using Object.fromEntries to get all values`
    - need to add separately

```jsx
export default function Signup() {
  function handleSubmit(event) {
    event.preventDefault();

    const fd = new FormData(event.target);
    const acquisitionChannel = fd.getAll('acquisition');
    const data = Object.fromEntries(fd.entries()); // array values are ignored
    data.acquisition = acquisitionChannel; // add separately
    console.log(data);
  }

  return (
    <form onSubmit={handleSubmit}>
      <h2>Welcome on board!</h2>
      <p>We just need a little bit of data from you to get you started 🚀</p>

      <div className="control">
        <label htmlFor="email">Email</label>
        <input id="email" type="email" name="email" />
      </div>

      <div className="control-row">
        <div className="control">
          <label htmlFor="password">Password</label>
          <input id="password" type="password" name="password" />
        </div>

        <div className="control">
          <label htmlFor="confirm-password">Confirm Password</label>
          <input
            id="confirm-password"
            type="password"
            name="confirm-password"
          />
        </div>
      </div>

      <hr />

      <div className="control-row">
        <div className="control">
          <label htmlFor="first-name">First Name</label>
          <input type="text" id="first-name" name="first-name" />
        </div>

        <div className="control">
          <label htmlFor="last-name">Last Name</label>
          <input type="text" id="last-name" name="last-name" />
        </div>
      </div>

      <div className="control">
        <label htmlFor="phone">What best describes your role?</label>
        <select id="role" name="role">
          <option value="student">Student</option>
          <option value="teacher">Teacher</option>
          <option value="employee">Employee</option>
          <option value="founder">Founder</option>
          <option value="other">Other</option>
        </select>
      </div>

      <fieldset>
        <legend>How did you find us?</legend>
        <div className="control">
          <input
            type="checkbox"
            id="google"
            name="acquisition"
            value="google"
          />
          <label htmlFor="google">Google</label>
        </div>

        <div className="control">
          <input
            type="checkbox"
            id="friend"
            name="acquisition"
            value="friend"
          />
          <label htmlFor="friend">Referred by friend</label>
        </div>

        <div className="control">
          <input type="checkbox" id="other" name="acquisition" value="other" />
          <label htmlFor="other">Other</label>
        </div>
      </fieldset>

      <div className="control">
        <label htmlFor="terms-and-conditions">
          <input type="checkbox" id="terms-and-conditions" name="terms" />I
          agree to the terms and conditions
        </label>
      </div>

      <p className="form-actions">
        <button type="reset" className="button button-flat">
          Reset
        </button>
        <button type="submit" className="button">
          Sign up
        </button>
      </p>
    </form>
  );
}
```

---

## Reset the form Data

1. **use button type="reset" of form tag**
   - `<button type="reset" className="button button-flat">Reset</button>`

```jsx
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
    <button type="reset" className="button button-flat">
      Reset
    </button>
    <button className="button">Login</button>
  </p>
</form>
```

2. `use event.target.reset()`

```jsx
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
    <button className="button button-flat" onClick={handleReset}>
      Reset
    </button>
    <button className="button">Login</button>
  </p>
</form>;

function handleReset(event) {
  event.target.reset();
}
```

---

## Validations

- `should use combination of built-in browser forms validation with custom`

1. **form-validation-keystroke-blur**

- `using stateful`

```jsx
export default function Login() {
  const [enteredValues, setEnteredValues] = useState({
    email: '',
    password: '',
  });

  const [didEdit, setDidEdit] = useState({
    email: false,
    password: false,
  });

  const emailIsInvalid = didEdit.email && !enteredValues.email.includes('@');

  function handleSubmit(event) {
    event.preventDefault();

    console.log(enteredValues);
  }

  function handleInputChange(identifier, value) {
    setEnteredValues((prevValues) => ({
      ...prevValues,
      [identifier]: value,
    }));
    setDidEdit((prevEdit) => ({
      ...prevEdit,
      [identifier]: false,
    }));
  }

  function handleInputBlur(identifier) {
    setDidEdit((prevEdit) => ({
      ...prevEdit,
      [identifier]: true,
    }));
  }

  return (
    <form onSubmit={handleSubmit}>
      <h2>Login</h2>

      <div className="control-row">
        <div className="control no-margin">
          <label htmlFor="email">Email</label>
          <input
            id="email"
            type="email"
            name="email"
            onBlur={() => handleInputBlur('email')}
            onChange={(event) => handleInputChange('email', event.target.value)}
            value={enteredValues.email}
          />
          <div className="control-error">
            {emailIsInvalid && <p>Please enter a valid email address.</p>}
          </div>
        </div>

        <div className="control no-margin">
          <label htmlFor="password">Password</label>
          <input
            id="password"
            type="password"
            name="password"
            onChange={(event) =>
              handleInputChange('password', event.target.value)
            }
            value={enteredValues.password}
          />
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

2. **on submit using Ref**

```jsx
export default function Login() {
  const [emailIsInvalid, setEmailIsInvalid] = useState(false);

  const email = useRef();
  const password = useRef();

  function handleSubmit(event) {
    event.preventDefault();

    const enteredEmail = email.current.value;
    const enteredPassword = password.current.value;

    const emailIsValid = enteredEmail.includes('@');

    if (!emailIsValid) {
      setEmailIsInvalid(true);
      return;
    }

    setEmailIsInvalid(false);

    console.log('Sending HTTP request...');
  }

  return (
    <form onSubmit={handleSubmit}>
      <h2>Login</h2>

      <div className="control-row">
        <div className="control no-margin">
          <label htmlFor="email">Email</label>
          <input id="email" type="email" name="email" ref={email} />
          <div className="control-error">
            {emailIsInvalid && <p>Please enter a valid email address.</p>}
          </div>
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

3. Browser built in validations

- required
- minLength
- maxLength
- pattern

```html
<div className="control">
  <label htmlFor="email">Email</label>
  <input id="email" type="email" name="email" required />
</div>

<div className="control-row">
  <div className="control">
    <label htmlFor="password">Password</label>
    <input
      id="password"
      type="password"
      name="password"
      required
      minlength="{6}"
    />
  </div>

  <div className="control">
    <label htmlFor="confirm-password">Confirm Password</label>
    <input
      id="confirm-password"
      type="password"
      name="confirm-password"
      required
    />
  </div>
</div>
```
