---
title: 'Understanding React Custom Hooks'
author: 'Hun Im'
date: 2023-01-10T18:22:35+09:00
category: ['React', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'React']
---
While using React, I mostly relied on the built-in hooks to handle most of my logic. However, I often pondered how to increase reusability for repetitive code.

I believed that effectively using custom hooks would help me become a higher-level frontend developer.

The most important principle of custom hooks is "reusability of logic, not values."

By specifying only the values related to state changes as parameters within the hook, we can define the logic based on side effects.

```jsx
import { useState } from 'react'
function App() {
  const [inputValue, setInputValue] = useState('')

  const handleChange = (e) => {
    setInputValue(e.target.value)
  }

  const handleSubmit = () => {
    setInputValue('')
  }
  return (
    <div>
      <input value={inputValue} onChange={handleChange} />
      <button onClick={handleSubmit}>확인</button>
    </div>
  )
}
```

Let's convert the above code into a custom hook.

```js
import { useState } from 'react'

export function useInput(initialValue, submitAction) {
  const [inputValue, setInputValue] = useState(initialValue)

  const handleChange = (e) => {
    setInputValue(e.target.value)
  }

  const handleSubmit = () => {
    setInputValue('')
    submitAction(inputValue)
  }

  return [inputValue, handleChange, handleSubmit]
}
```

```jsx
import { useInput } from 'hooks/useInput'

function displayMessage(message) {
  alert(message)
}

function App() {
  const [inputValue, handleChange, handleSubmit] = useInput(
    'Hello',
    displayMessage
  )

  return (
    <div>
      <input value={inputValue} onChange={handleChange} />
      <button onClick={handleSubmit}>확인</button>
    </div>
  )
}
```

```jsx
import { useState, useEffect } from 'react'

const baseUrl = 'https://jsonplaceholder.typicode.com'

function App() {
  const [data, setData] = useState(null)

  const fetchUrl = (type) => {
    fetch(baseUrl + '/' + type)
      .json()
      .then((res) => {
        setData(res)
      })
  }

  useEffect(() => {
    fetchUrl('users')
  }, [])

  return (
    <div>
      <button onClick={() => fetchUrl('users')}>Users</button>
      <button onClick={() => fetchUrl('posts')}>Posts</button>
      <button onClick={() => fetchUrl('todos')}>Todos</button>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  )
}
```

Now, let's refactor the above code using a custom hook.

```js
import { useEffect, useState } from 'react'

export function useFetch(baseUrl, initialType) {
  const [data, setData] = useState(null)

  const fetchUrl = (type) => {
    fetch(baseUrl + '/' + type)
      .json()
      .then((res) => {
        setData(res)
      })
  }

  useEffect(() => {
    fetchUrl(initialType)
  }, [])

  return {
    data,
    fetchUrl,
  }
}
```

```jsx
import { useFetch } from 'hooks/useFetch'

const baseUrl = 'https://jsonplaceholder.typicode.com'

function App() {
  const { data, fetchUrl } = useFetch(baseUrl, 'users')

  return (
    <div>
      <button onClick={() => fetchUrl('users')}>Users</button>
      <button onClick={() => fetchUrl('posts')}>Posts</button>
      <button onClick={() => fetchUrl('todos')}>Todos</button>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  )
}
```

```jsx
import { useFetch } from 'hooks/useFetch'

const baseUrl = 'https://jsonplaceholder.typicode.com'

function App() {
  const { data: userData, fetchUrl } = useFetch(baseUrl, 'users')
  const { data: postData, fetchUrl } = useFetch(baseUrl, 'posts')
  const { data: todoData, fetchUrl } = useFetch(baseUrl, 'todos')

  return (
    <div>{userData ? <pre>{JSON.stringify(userData[0], null, 2)}</pre> : null}</div>
    <div>{postData ? <pre>{JSON.stringify(postData[0], null, 2)}</pre> : null}</div>
    <div>{todoData ? <pre>{JSON.stringify(todoData[0], null, 2)}</pre> : null}</div>
  )
}
```
