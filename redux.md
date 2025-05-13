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

1. **react-redux**
   - `Provider`
   - `useSelector`
   - `useDispatch`
2. **redux-toolkit**
   - `configureStore`
   - `createSlice`

---

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
