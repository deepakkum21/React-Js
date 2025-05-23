# Redux

## Redux vs UseContext

| Feature            | `useContext`                       | `Redux`                                              |
| ------------------ | ---------------------------------- | ---------------------------------------------------- |
| **Type**           | React built-in hook                | External state management library                    |
| **Best for**       | Small, simple global state         | Large, complex, or shared global state               |
| **Boilerplate**    | Very little                        | More setup and boilerplate                           |
| **DevTools**       | ❌ None built-in                   | ✅ Excellent DevTools support                        |
| **Middleware**     | ❌ Not supported directly          | ✅ Supports middleware (e.g. logging, async)         |
| **Performance**    | Re-renders all consumers on change | Fine-grained control with `connect()`                |
| **Async Logic**    | Manual via effects or custom hooks | Handled with Redux middleware like `thunk` or `saga` |
| **Learning Curve** | Very easy                          | Moderate to steep                                    |

## Redux core

![Redux Core](./img/redux-core.png)

## Reducer func

- will be executed when action is dispatched
- always has two param
  - `oldState`
  - `dispatchedActions` which will have action type and payloads
- it is accepted as a param by `createStore(reducer)`

![Reducer](./img/redux-reducer.png)

## Sample example of basic redux in JS

```js
const redux =  require('redux');

const initialState = {
    counter: 0
}

const counterReducer = (state = initialState, action) = {
    if(action.type === 'increment') {
        return {
            ...state,
            counter = state.counter + 1
        }
    }
    if(action.type === 'decrement') {
        return {
            ...state,
            counter = state.counter + 1
        }
    }
    return state;
}

const store = redux.createStore(counterReducer);

const counterSubscriber = () = {
    const latestStoreState = store.getState();
    console.log(latestStoreState);
};

store.subscribe(counterSubscriber);

store.dispatch('increment');
store.dispatch('decrement');

// output
// counter: 1
// counter: 0
```

---

## React REdux [lib]

- `redux`
- `react-redux`
  - gives `<Provider></Provider>` component which will wrap the app which needs teh store access just like contextProvider using `useContext`
- redux-toolkit
  - `configureStore`
  - `createSlice`

```jsx
<Provider store={storeCreatedUsingRedux}>
  <App />
</Provider>
```

---

1. **redux**
   - `createStore`
   - `combineReducer`
2. **react-redux**
   - `Provider`
   - `useSelector`
   - `useDispatch`
3. **redux-toolkit [@reduxjs/toolkit]**
   - `configureStore`
   - `createSlice`

---

## createStore

## combineReducer

## middleware

## https://www.youtube.com/watch?v=sziLH_2EiME&list=PLfEr2kn3s-br2OoBNSR7S5eJ-kcEeUQcG&index=5

## useSelector()

- To `select a piece of state from the Redux store`.
- To `re-render the component only when that slice of state changes`.
- `Replaces mapStateToProps from the HOC connect()` API.
- it `internally creates a subscription with store` which is required.

```js
const selectedState = useSelector(selectorFn);

//selectorFn: A function that takes the entire Redux store state and returns a slice of it.
```

### ⚠️ Important Notes

- `Avoid selecting the entire state like useSelector(state => state) — this causes re-renders on any state change`.
- You can use shallow comparison or useMemo to optimize performance if you're selecting objects or arrays.
- Each useSelector() is independent, so you can use multiple calls if needed.

---

## useDispatch()

- To `send actions to the Redux store to update state`.
- It's the hook-based `alternative to mapDispatchToProps from connect()` in class components

```jsx
const dispatch = useDispatch();

dispatch({ type: 'ACTION_TYPE', payload: data });
```

| Hook            | Purpose                      | Example Use                         |
| --------------- | ---------------------------- | ----------------------------------- |
| `useDispatch()` | Sends actions to Redux store | `dispatch(increment())`             |
| `useSelector()` | Reads state from Redux store | `useSelector(state => state.value)` |

---

```jsx
// store.js
const initialState = {
    counter: 0
}

const counterReducer = (state = initialState, action) = {
    if(action.type === 'increment') {
        return {
            ...state,
            counter = state.counter + 1
        }
    }
    if(action.type === 'decrement') {
        return {
            ...state,
            counter = state.counter + 1
        }
    }
    if(action.type === 'increase') {
        return {
            ...state,
            counter = state.counter + state.value
        }
    }
    return state;
}

const store = createStore(counterReducer);
```

```jsx
// Counter.js
import { useSelector, useDispatch } from 'react-redux';

function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  const handleIncrement() {
    dispatch({
        type: 'increment'
    })
  }

  const handleDecrement() {
    dispatch({
        type: 'decrement'
    })
  }

  const handleIncrementBy5() {
    dispatch({
        type: 'increase',
        value: 5
    })
  }

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={handleIncrement}>+ Increment</button>
      <button onClick={handleDecrement}>− Decrement</button>
      <button onClick={handleIncrementBy5}>Increase By 5</button>
    </div>
  );
}
```

```jsx
import { Provider } from 'react-redux';
import { store } from './store';
import Counter from './Counter';

function App() {
  return (
    <Provider store={store}>
      <Counter />
    </Provider>
  );
}
```

---

# React-toolkit [@reduxjs/toolkit]

## 1. createSlice

- it simplifies
  - Writing constants for action types
  - Writing action creators
  - Writing switch-case logic in reducers
- `With createSlice(), Redux Toolkit generates all of this automatically` for you.

```jsx
import { createSlice } from '@reduxjs/toolkit';

const slice = createSlice({
  name: 'featureName', // Name for the slice
  initialState: {}, // Initial state
  reducers: {
    // Reducer logic (mutating is allowed via Immer)
    someAction: (state, action) => {
      /* update state */
      // for any payload `action.payload`
    },
  },
});

export const { someAction } = slice.actions; // returns Action creators
export default slice.reducer; // return Reducer
```

### 🧠 Under the Hood

- createSlice() `uses Immer, so you can "mutate" the state safely (it’s still immutable behind the scenes)`.
- **Returns**:
  - A reducer function for use in configureStore()
    - `const { someAction } = slice.actions`
  - Auto-generated action creators
    - `const reducer = slice.reducer`

## 2. configureStore

- It wraps around createStore from plain Redux and automatically:
  - `Combines reducers`
  - `Adds useful middleware` (redux-thunk, devtools, etc.)
  - Enables Redux DevTools
  - Adds good default settings for development

```jsx
// Syntax
import { configureStore } from '@reduxjs/toolkit';

const store = configureStore({
  reducer: {
    sliceName: sliceReducer,
    // more reducers...
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(customMiddleware),
  devTools: true, // enabled by default in development
});

| Option           | Purpose                                                    |
| ---------------- | ---------------------------------------------------------- |
| `reducer`        | Root reducer (object of slices or single reducer function) |
| `middleware`     | Add or customize Redux middleware stack                    |
| `devTools`       | Enable/disable Redux DevTools                              |
| `preloadedState` | Initialize store with predefined state                     |
| `enhancers`      | Add store enhancers like `applyMiddleware` (rarely needed) |

```

### example uf configureStore & createSlice

```jsx
// 1. Create Slice (counterSlice.js)
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

```jsx
// 2. Configure Store (store.js)
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

```jsx
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement, incrementByAmount } from './counterSlice';

function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>−</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>+5</button>
    </div>
  );
}
```

```jsx
import { Provider } from 'react-redux';
import { store } from './store';
import App from './App';

function Root() {
  return (
    <Provider store={store}>
      <App />
    </Provider>
  );
}
```

| Traditional Redux                | Redux Toolkit with `configureStore()` |
| -------------------------------- | ------------------------------------- |
| Manual `createStore`, middleware | Automatic setup with smart defaults   |
| Needs `combineReducers()`        | Accepts an object of slice reducers   |
| Needs Redux DevTools extension   | Built-in support                      |

---

## Async/Side effects with Redux

![Async/Side effects with Redux](./img/redux-async-sideEffects.png)

![fat Reducer Vs Fat Component Vs Fat Actions](./img/fatReducerVsFatComponentVsFatActions.png)

## 1. Async via useEffect() in component

- adding the useEffect() in conjunction with useDispatch()
- since one `should not call side effects inside the reducer`, `reducer should be pure and sync`

```jsx
function App() {
  const dispatch = useDispatch();
  const showCart = useSelector((state) => state.ui.cartIsVisible);
  const cart = useSelector((state) => state.cart);
  const notification = useSelector((state) => state.ui.notification);

  useEffect(() => {
    const sendCartData = async () => {
      dispatch(
        uiActions.showNotification({
          status: 'pending',
          title: 'Sending...',
          message: 'Sending cart data!',
        })
      );
      const response = await fetch(
        'https://react-http-6b4a6.firebaseio.com/cart.json',
        {
          method: 'PUT',
          body: JSON.stringify(cart),
        }
      );

      if (!response.ok) {
        throw new Error('Sending cart data failed.');
      }

      dispatch(
        uiActions.showNotification({
          status: 'success',
          title: 'Success!',
          message: 'Sent cart data successfully!',
        })
      );
    };

    if (isInitial) {
      isInitial = false;
      return;
    }

    sendCartData().catch((error) => {
      dispatch(
        uiActions.showNotification({
          status: 'error',
          title: 'Error!',
          message: 'Sending cart data failed!',
        })
      );
    });
  }, [cart, dispatch]);

  return (
    <Fragment>
      {notification && (
        <Notification
          status={notification.status}
          title={notification.title}
          message={notification.message}
        />
      )}
      <Layout>
        {showCart && <Cart />}
        <Products />
      </Layout>
    </Fragment>
  );
}
```

## 2. Redux Thunk [Action creator]

- thunk is an `action creator that returns a function instead of an action object`.
- `Redux will call this function with dispatch` (and optionally getState), allowing you to:
  - `Make API calls`
  - `Dispatch multiple actions`
  - `Handle async flows` like loading/error states

```jsx
// syntax
const fetchData = () => {
  return async (dispatch, getState) => {
    dispatch({ type: 'FETCH_START' });

    try {
      const response = await fetch('/api/data');
      const data = await response.json();
      dispatch({ type: 'FETCH_SUCCESS', payload: data });
    } catch (error) {
      dispatch({ type: 'FETCH_ERROR', error });
    }
  };
};

// Normally
dispatch({ type: 'INCREMENT' }); // simple action

//With thunk:
dispatch(fetchData()); // fetchData returns a function, not an object
// Redux Thunk intercepts this and invokes the function, passing dispatch.
```

## 3. using redux-toolkit - createAsyncThunk

- a utility from Redux Toolkit used for h`andling asynchronous logic (like API calls) in Redux applications`.
- It simplifies the process of `dispatching lifecycle actions (pending, fulfilled, rejected) and reduces boilerplate`.

```jsx
// syntax
import { createAsyncThunk } from '@reduxjs/toolkit';

const fetchData = createAsyncThunk(
  'data/fetchData', // action type string
  async (arg, thunkAPI) => {
    const response = await fetch('https://api.example.com/data');
    return response.json(); // resolved value goes into action.payload
  }
);
```

**Parameters**

- a string action type value, example
  - 'users/requestStatus'
  - 'data/fetchData'
- a payloadCreator callback / A callback function that should return a promise containing the result of some asynchronous logic, arguments are
  - `arg`:- a value / Object for the API [id, requestBody]
  - `thunkApi`:-
    - dispatch
    - getState
    - rejectWithValue – for custom error handling
    - signal – to handle request cancellation
- an options object.

**Action Lifecycle**
When you dispatch fetchData(), Redux Toolkit automatically creates and dispatches:

- `fetchData.pending` – when the async function starts
- `fetchData.fulfilled` – if it resolves successfully
- `fetchData.rejected` – if it throws an error

```jsx
// example
import { createAsyncThunk } from '@reduxjs/toolkit';

export const fetchUserById = createAsyncThunk(
  'users/fetchUserById',
  async ({ userId, token }, thunkAPI) => {
    try {
      const response = await fetch(`https://api.example.com/users/${userId}`, {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      });
      if (!response.ok) throw new Error('Failed to fetch user');
      return await response.json();
    } catch (error) {
      return thunkAPI.rejectWithValue(error.message);
    }
  }
);
```

```jsx
import { createSlice } from '@reduxjs/toolkit';
import { fetchUserById } from './userThunks'; // assume this is where you define the thunk

const userSlice = createSlice({
  name: 'user',
  initialState: {
    user: null,
    status: 'idle', // 'idle' | 'loading' | 'succeeded' | 'failed'
    error: null,
  },
  reducers: {
    // add other non-async reducers here if needed
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUserById.pending, (state) => {
        state.status = 'loading';
        state.error = null;
      })
      .addCase(fetchUserById.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.user = action.payload;
      })
      .addCase(fetchUserById.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.payload || action.error.message;
      });
  },
});

export default userSlice.reducer;
```

```jsx
// sample for multiple extraReducers
const dataSlice = createSlice({
  name: 'data',
  initialState: {
    users: [],
    posts: [],
    loadingUsers: false,
    loadingPosts: false,
    errorUsers: null,
    errorPosts: null,
  },
  reducers: {},
  extraReducers: (builder) => {
    // Handle fetchUsers
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.loadingUsers = true;
        state.errorUsers = null;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loadingUsers = false;
        state.users = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loadingUsers = false;
        state.errorUsers = action.error.message;
      });

    // Handle fetchPosts
    builder
      .addCase(fetchPosts.pending, (state) => {
        state.loadingPosts = true;
        state.errorPosts = null;
      })
      .addCase(fetchPosts.fulfilled, (state, action) => {
        state.loadingPosts = false;
        state.posts = action.payload;
      })
      .addCase(fetchPosts.rejected, (state, action) => {
        state.loadingPosts = false;
        state.errorPosts = action.error.message;
      });
  },
});
```
