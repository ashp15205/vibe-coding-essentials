# Anti-Hallucination Rules: Next.js

> Prevents common AI mistakes when working with Next.js projects.
> **Tested against Next.js 14/15 — June 2026**

## App Router vs Pages Router (Critical Context)

Always check whether you are in the `app/` directory or `pages/` directory. They have completely different routing, data fetching, and component paradigms. Do not mix them.

- **App Router (`app/`)**: Default in Next.js 13+. Uses Server Components by default.
- **Pages Router (`pages/`)**: Legacy. Uses `getServerSideProps` and `getStaticProps`.

**Never** use `getServerSideProps`, `getStaticProps`, or `getInitialProps` inside the `app/` directory.

---

## Server vs Client Components

In the App Router (`app/`), components are Server Components by default.

### Server Components
- Cannot use hooks (`useState`, `useEffect`, `useContext`)
- Cannot use browser APIs (`window`, `document`)
- Can use `async`/`await` directly in the component body
- Cannot be imported into Client Components directly (must be passed as `children` or props)

### Client Components
- Must have `"use client";` at the very top of the file
- Can use hooks and browser APIs
- Cannot use `async`/`await` in the component body (use `use()` or `useEffect` for data fetching)

```jsx
// Bad: Using hooks in a Server Component
export default function UserProfile() {
  const [user, setUser] = useState(null); // Error
  return <div>Profile</div>;
}

// Good: Adding "use client" when hooks are needed
"use client";
import { useState } from 'react';

export default function UserProfile() {
  const [user, setUser] = useState(null);
  return <div>Profile</div>;
}

// Better: Keep data fetching on the server, pass to a Client Component if interactivity is needed
export default async function UserProfile() {
  const user = await fetchUser(); // Valid in Server Component
  return <ProfileCard user={user} />;
}
```

---

## The "use client" Directive

- Only use `"use client";` when a component *requires* interactivity (state, effects, browser APIs, event listeners).
- Adding `"use client";` to the root layout or a high-level page forces all children into the client bundle, ruining performance. Push `"use client";` as deep down the component tree as possible.
- `"use client";` defines the boundary. You don't need to add it to files that are imported *only* by other Client Components, but it's safest to declare it on files that use React hooks.

---

## Image Component Gotchas

The `next/image` component API changed significantly between Next.js 12 and 13+.

| Hallucinated Pattern (Legacy Next 12) | Correct Alternative (Next 13+) |
|--------------------------------------|--------------------------------|
| `layout="fill"`                      | `fill` prop (boolean)          |
| `layout="responsive"`                | Use `sizes` and standard CSS   |
| `objectFit="cover"`                  | Use `style={{ objectFit: 'cover' }}` |
| Cannot use `alt`                     | `alt` is strictly required     |

```jsx
// Bad (Hallucinated Legacy Code)
<Image src="/hero.jpg" layout="fill" objectFit="cover" />

// Good (Next.js 13+)
<Image src="/hero.jpg" alt="Hero" fill style={{ objectFit: 'cover' }} />
```

---

## Next.js Navigation

- **App Router (`app/`)**: Import from `next/navigation`
- **Pages Router (`pages/`)**: Import from `next/router`

If you are in the `app/` directory and use `import { useRouter } from 'next/router'`, the app will crash.

```jsx
// App Router
"use client";
import { useRouter } from 'next/navigation';

export default function BackButton() {
  const router = useRouter();
  return <button onClick={() => router.back()}>Back</button>;
}
```
