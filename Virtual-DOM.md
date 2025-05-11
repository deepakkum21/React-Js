# Virtual DOM

1. React checks for necessary DOM updates via a `“Virtual DOM”`
2. It `creates & compares virtual DOM snapshots to find out which parts of the rendered UI` need to be updated
3. Steps
   - Step 1: `Creating a Component Tree`
   - Step 2: `Creating a Virtual Snapshot of the Target HTML Code`
   - Step 3: `Compare New Virtual DOM Snapshot to Previous (Old) Virtual DOM Snapshot`
   - Step 4: `Identify & Apply Changes to the “Real DOM”`

---

# React Vs Angular

| Feature              | **React**                       | **Angular**                        |
| -------------------- | ------------------------------- | ---------------------------------- |
| **Type**             | Library (for UI)                | Full-fledged framework             |
| **Language**         | JavaScript (JSX)                | TypeScript (strict superset of JS) |
| **Creator**          | Facebook                        | Google                             |
| **Initial Release**  | 2013                            | 2016 (Angular 2+)                  |
| **DOM**              | Virtual DOM                     | Real DOM (with change detection)   |
| **Data Binding**     | One-way binding                 | Two-way binding (with `ngModel`)   |
| **State Management** | External (Redux, Zustand, etc.) | Built-in services and RxJS         |
| **Learning Curve**   | Easier, flexible                | Steeper, more opinionated          |
| **Routing**          | External (`react-router`)       | Built-in                           |

| Feature              | **React**                            | **Angular**                            |
| -------------------- | ------------------------------------ | -------------------------------------- |
| Component Style      | Functional (with Hooks)              | Class-based with decorators            |
| Templates            | JSX (JS + HTML in one)               | HTML + Angular syntax (`*ngIf`, etc.)  |
| Dependency Injection | Manual (via context or libraries)    | Built-in DI container                  |
| CLI                  | Basic (`create-react-app`, Vite)     | Advanced (`ng new`, scaffolding tools) |
| Form Handling        | Uncontrolled / controlled components | Template-driven & reactive forms       |

| Aspect       | **React**                         | **Angular**                                        |
| ------------ | --------------------------------- | -------------------------------------------------- |
| Rendering    | Virtual DOM                       | Change detection via Zones , now uses Signals[17+] |
| Optimization | Fine-grained with `useMemo`, etc. | Ahead-of-Time (AOT) compilation                    |
| Bundle Size  | Smaller (depends on setup)        | Larger by default                                  |
