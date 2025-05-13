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
