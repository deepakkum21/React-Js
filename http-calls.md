## Promise VS Observable

| Feature        | Promise             | Observable                    |
| -------------- | ------------------- | ----------------------------- |
| **Execution**  | `Immediately`       | `On subscription`             |
| **Values**     | `Single`            | `Multiple`                    |
| **Cancelable** | `No`                | `Yes`                         |
| Native Support | Yes                 | No (RxJS required)            |
| **Operators**  | Basic (`then`)      | Extensive (map, filter, etc.) |
| **Use Case**   | One-time async task | Streams, complex async logic  |
