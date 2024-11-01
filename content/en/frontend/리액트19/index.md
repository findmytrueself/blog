---
title: 'Summary of React Version 19'
author: 'Hun Im'
date: 2024-10-22T10:40:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
og_image: '/images/gamer.png'
keywords: ['Javascript', 'React']
---

## Major New Features

1. Actions

- Enhanced support for asynchronous functions: By using `useTransition`, you can automatically manage pending states, error handling, and optimistic updates in asynchronous functions.

```jsx
const [isPending, startTransition] = useTransition()

const handleSubmit = () => {
  startTransition(async () => {
    const error = await updateName(name)
    if (error) {
      setError(error)
      return
    }
    redirect('/path')
  })
}
```

2. New Hook: `useActionState`

- Easily handle common cases of Actions: Manage the results of asynchronous actions, pending states, errors, and more.

```jsx
const [error, submitAction, isPending] = useActionState(
  async (prevState, formData) => {
    const error = await updateName(formData.get('name'))
    if (error) return error
    redirect('/path')
    return null
  },
  null
)
```

3. React DOM: <form> Actions and `useFormStatus`

- Pass functions as `action` and `formAction` props: Automatically manage form submissions and reset the form after submission.

- Added `useFormStatus` hook: Supports access to form state in design systems.

```jsx
function DesignButton() {
  const { pending } = useFormStatus()
  return <button type="submit" disabled={pending} />
}
```

4. New Hook: `useOptimistic`

- Manage optimistic updates: Provide immediate feedback to users while asynchronous requests are in progress.

```jsx
const [optimisticName, setOptimisticName] = useOptimistic(currentName)

const submitAction = async (formData) => {
  const newName = formData.get('name')
  setOptimisticName(newName)
  const updatedName = await updateName(newName)
  onUpdateName(updatedName)
}
```

5. New API: `use`

- Support reading resources during rendering: Use `use` to read promises or contexts and suspend components if necessary.

```jsx
function Comments({ commentsPromise }) {
  const comments = use(commentsPromise)
  return comments.map((comment) => <p key={comment.id}>{comment}</p>)
}
```

6. React Server Components

- Support for server components: Pre-render components on the server to reduce client bundle sizes and improve performance.

- Server Actions: Call asynchronous functions that execute on the server from client components.

## Improvements

1. Passing ref as a Prop

- No need for forwardRef: You can directly receive and use ref as a prop in functional components.

```jsx
function MyInput({ placeholder, ref }) {
  return <input placeholder={placeholder} ref={ref} />
}
```

2. Improved Debugging for Hydration Errors

- Providing diffs: More clear messages and debugging information for errors occurring during hydration.

3. Using <Context> as a Provider

- More concise context usage: You can provide context values using <Context> instead of <Context.Provider>.

```jsx
const ThemeContext = createContext('')

function App({ children }) {
  return <ThemeContext value="dark">{children}</ThemeContext>
}
```

4. Support for Cleanup Functions in ref

- Return cleanup functions: You can return a cleanup function from a ref callback to execute when a component unmounts.

```jsx
<input
  ref={(ref) => {
    // Executes when ref is created
    return () => {
      // Executes during ref cleanup
    }
  }}
/>
```

5. Initial Value Support in `useDeferredValue`

- Setting an initial value: Add an `initialValue` option to `useDeferredValue` to specify the value used during initial rendering.

```jsx
const value = useDeferredValue(deferredValue, '')
```

6. Support for Document Metadata

- Natural use of `<title>, <meta>, <link>` tags: Define document metadata directly within components, and React automatically hoists them to `<head>`.

```jsx
function BlogPost({ post }) {
  return (
    <article>
      <h1>{post.title}</h1>
      <title>{post.title}</title>
      <meta name="author" content="Author Name" />
    </article>
  )
}
```

7. Improved Stylesheet Support

- Managing stylesheet precedence: Control the insertion order of stylesheets using the precedence attribute on `<link>` tags.

```jsx
<link rel="stylesheet" href="style.css" precedence="high" />
```

8. Support for Asynchronous Scripts

- Managing load order and deduplication: Declare asynchronous scripts within components, and React manages load order and deduplication.

```jsx
function MyComponent() {
  return (
    <div>
      <script async src="script.js" />
      Content
    </div>
  )
}
```

9. Support for Resource Preloading

- Performance optimization: Provide APIs like `preload`, `prefetchDNS`, `preconnect` to optimize browser resource loading.

```jsx
import { prefetchDNS, preconnect, preload, preinit } from 'react-dom'

function MyComponent() {
  preinit('https://example.com/script.js', { as: 'script' })
  preload('https://example.com/font.woff', { as: 'font' })
}
```

10. Improved Compatibility with Third-Party Scripts and Extensions

- Enhanced hydration: Prevent hydration errors caused by unexpected tags or elements, minimizing conflicts with third-party scripts or browser extensions.

11. Improved Error Reporting

- Removing duplication and providing detailed information: Eliminate duplicate error messages and add new root options like `onCaughtError`, `onUncaughtError` for flexible error handling.

12. Support for Custom Elements

- Improved attribute and property management: Enhanced handling of attributes and properties for custom elements, ensuring consistent behavior in client and SSR environments.
