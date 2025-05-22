# Promise

- A Promise is an object that represents the eventual completion or failure of an asynchronous operation.

```js
function myPromise(shouldResolve, a, b) {
  return new Promise((resolve, reject) => {
    if (shouldResolve) {
      resolve(b); // Resolves with value b
    } else {
      reject(a); // Rejects with value a
    }
  });
}

myPromise(true, 'error-value', 'success-value')
  .then((result) => console.log('Resolved with:', result))
  .catch((error) => console.log('Rejected with:', error));
```

--

## What are the states of a Promise?

- `Pending`: Initial state, neither fulfilled nor rejected.
- `Fulfilled`: The operation completed successfully.
- `Rejected`: The operation failed.

## What is Promise.all() vs Promise.race()?

- Promise.all([p1, p2]): Resolves `when all promises resolve, or rejects if any fails`.
- Promise.race([p1, p2]): `Resolves/rejects as soon as any one promise resolves/rejects`.

## What happens if an awaited promise rejects?

```js
async function example() {
  try {
    const data = await fetch('bad-url');
  } catch (error) {
    console.error('Fetch failed:', error);
  }
}
```

## What is the difference between Promise.all() and Promise.allSettled()?

- Promise.all() fails fast: `rejects if any promise fails`.
- Promise.allSettled() `waits for all promises, then returns an array of objects with status and value or reason`

```js
const results = await Promise.allSettled([
  Promise.resolve(1),
  Promise.reject('error'),
  Promise.resolve(3),
]);

// Output example:
[
  { status: 'fulfilled', value: 1 },
  { status: 'rejected', reason: 'error' },
  { status: 'fulfilled', value: 3 },
];
```

## How do you cancel a Promise?

- Promises themselves cannot be truly cancelled. You can simulate cancellation using AbortController or a flag.

```js
const controller = new AbortController();

fetch('https://api.example.com', { signal: controller.signal })
  .then((response) => console.log(response))
  .catch((err) => {
    if (err.name === 'AbortError') {
      console.log('Request was cancelled');
    }
  });

controller.abort(); // Cancels the fetch
```

## How do you retry a failed promise using async/await?

```js
async function retry(fn, retries = 3) {
  for (let i = 0; i < retries; i++) {
    try {
      return await fn();
    } catch (err) {
      if (i === retries - 1) throw err;
    }
  }
}
```
