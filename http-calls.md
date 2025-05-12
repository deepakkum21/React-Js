## Promise VS Observable

| Feature        | Promise             | Observable                    |
| -------------- | ------------------- | ----------------------------- |
| **Execution**  | `Immediately`       | `On subscription`             |
| **Values**     | `Single`            | `Multiple`                    |
| **Cancelable** | `No`                | `Yes`                         |
| Native Support | Yes                 | No (RxJS required)            |
| **Operators**  | Basic (`then`)      | Extensive (map, filter, etc.) |
| **Use Case**   | One-time async task | Streams, complex async logic  |

## async / await - loading - error

- can't use async/await directly with useEffect or top level function in react
- create you own function

```jsx
const [userPlaces, setUserPlaces] = useState([]);
const [isFetching, setIsFetching] = useState(false);
const [error, setError] = useState();

const [errorUpdatingPlaces, setErrorUpdatingPlaces] = useState();

const [modalIsOpen, setModalIsOpen] = useState(false);

useEffect(() => {
  async function fetchPlaces() {
    setIsFetching(true);
    try {
      const places = await fetchUserPlaces();
      setUserPlaces(places);
    } catch (error) {
      setError({ message: error.message || 'Failed to fetch user places.' });
    }

    setIsFetching(false);
  }

  fetchPlaces();
}, []);
```
