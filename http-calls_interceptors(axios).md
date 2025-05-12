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

---

## Interceptors [Axios]

1. **Create a Custom Axios Instance**

```jsx
// src/axiosInstance.js
import axios from 'axios';

const axiosInstance = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 5000,
});
```

2. **Set Up Interceptors With Conditions**

```jsx
// Add request interceptor
axiosInstance.interceptors.request.use(
  (config) => {
    // EXCLUDED URLS (e.g., login and register)
    const excludedUrls = ['/login', '/register'];
    const shouldExclude = excludedUrls.some((url) => config.url?.includes(url));

    if (!shouldExclude) {
      const token = localStorage.getItem('authToken');
      if (token) {
        config.headers.Authorization = `Bearer ${token}`;
      }
    }

    // Handle custom context (example: tracking a request purpose)
    if (config.context && config.context.purpose) {
      config.headers['X-Request-Purpose'] = config.context.purpose;
    }

    return config;
  },
  (error) => Promise.reject(error)
);

// Optional: Response interceptor for global error handling
axiosInstance.interceptors.response.use(
  (response) => response,
  (error) => {
    // You can access context info here too
    const context = error.config?.context;
    if (context?.handleUnauthorized && error.response?.status === 401) {
      console.warn('Unauthorized! Redirecting to login...');
    }

    return Promise.reject(error);
  }
);
```

3. **Use the Axios Instance With Custom Context**

```jsx
// src/App.js or a service file
import axiosInstance from './axiosInstance';

const fetchData = async () => {
  try {
    const response = await axiosInstance.get('/user/profile', {
      context: {
        purpose: 'fetch-user',
        handleUnauthorized: true,
      },
    });
    console.log(response.data);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};
```

| Feature              | Description                                          |
| -------------------- | ---------------------------------------------------- |
| **Excluded URLs**    | `config.url.includes('/login')` to skip auth headers |
| **Context Support**  | `config.context` holds any custom logic or flags     |
| **Header Injection** | Adds token unless the URL is excluded                |
| **Extendability**    | Add language headers, timestamps, or request IDs     |
