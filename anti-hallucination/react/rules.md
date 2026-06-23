# Anti-Hallucination Rules: React

> Prevents common AI mistakes when working with React projects.
> **Tested against React 18/19 — June 2026**

## Version Awareness

Check `package.json` for the `react` version before suggesting patterns.

- **React 16/17**: Class components common, `componentDidMount`, legacy Context API
- **React 18**: Concurrent features, `useTransition`, `useDeferredValue`, automatic batching, `createRoot`
- **React 19**: Server Components in React core, `use()` hook, Actions

---

## Hooks Rules

### Core Rules
- Hooks must only be called at the top level of a React function component or a custom hook.
- Never call hooks inside conditionals, loops, or nested functions.
- Never call hooks in class components.

```jsx
// Bad
function Component({ show }) {
  if (show) {
    const [value, setValue] = useState(''); // Violates rules of hooks
  }
}

// Good
function Component({ show }) {
  const [value, setValue] = useState('');
  if (!show) return null;
}
```

### useEffect Dependency Array
- Missing dependencies cause stale closures — always include all values used inside `useEffect`.
- An empty dependency array `[]` means "run once after mount" — not "never re-run."
- Never suppress the exhaustive-deps lint rule without understanding why.

```jsx
// Bad: stale closure — count never updates in the interval
useEffect(() => {
  const id = setInterval(() => console.log(count), 1000);
  return () => clearInterval(id);
}, []); // count missing from deps

// Good
useEffect(() => {
  const id = setInterval(() => console.log(count), 1000);
  return () => clearInterval(id);
}, [count]);
```

### useCallback and useMemo
- Do not add `useCallback` or `useMemo` by default for "performance."
- Only add them when there is a measured performance problem or when a function is passed to a memoized child component.
- Premature memoization adds complexity with no benefit.

---

## State Rules

### Never Mutate State Directly

```jsx
// Bad
const [items, setItems] = useState([]);
items.push(newItem); // Mutation — React won't re-render
setItems(items);

// Good
setItems(prev => [...prev, newItem]);
```

### State Updates Are Asynchronous

```jsx
// Bad: reading state immediately after setting it
setCount(count + 1);
console.log(count); // Still the old value

// Good: use the updater function for dependent updates
setCount(prev => prev + 1);
```

---

## Common Hallucinations to Avoid

| Hallucinated Pattern | Correct Alternative |
|---------------------|-------------------|
| `React.FC` with `children` prop (React 18) | `children: React.ReactNode` in props type |
| `ReactDOM.render()` (React 18+) | `createRoot(el).render(<App />)` |
| `useEffect` for data fetching without cleanup | Add abort controller or check `mounted` |
| `defaultProps` on function components | Default parameter values in function signature |
| `propTypes` for TypeScript projects | Use TypeScript types/interfaces instead |
| `componentWillMount`, `componentWillReceiveProps` | These were removed — use equivalent hooks |
| `forwardRef` wrapper for all components | Only needed when the parent needs a DOM ref |

---

## React 18 Specific

### Automatic Batching
In React 18, state updates are batched automatically — even inside `setTimeout`, Promises, and native event handlers. This is a behavior change from React 17.

### Strict Mode Double Invocation
React 18 Strict Mode invokes effects twice (mount → unmount → mount) in development to detect side effect bugs. This is intentional. Do not suggest removing Strict Mode to "fix" this behavior.

### createRoot
```jsx
// React 17 (deprecated)
ReactDOM.render(<App />, document.getElementById('root'));

// React 18
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

---

## Performance

- Profile before optimizing. Do not add `useMemo`/`useCallback` without profiling data.
- Use React DevTools Profiler to find actual bottlenecks.
- `React.memo` prevents re-renders only when props are the same (shallow compare).
- Key prop in lists must be stable and unique — not array index for dynamic lists.

---

## Security

- Never use `dangerouslySetInnerHTML` with unsanitized user input.
- Never store sensitive data (tokens, passwords) in component state — it is visible in React DevTools.
- Sanitize HTML before passing to `dangerouslySetInnerHTML`: use `DOMPurify`.
