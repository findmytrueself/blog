---
title: 'React Custom hooks 정확한 이해'
author: '임훈'
date: 2023-01-10T18:22:35+09:00
category: ['React', 'Javascript']
---

**인트로**

그동안 리액트를 사용해 오면서, 기본 hooks를 이용하여 대부분의 로직을 수행했다. 하지만 반복되는 코드에 대해 항상 어떻게 하면 재사용성을 늘릴 수 있을까 고민 했었다.

커스텀 훅을 잘 사용하면 더 높은 레벨의 프론트엔드 개발자가 될 것 같았다.

커스텀 훅의 가장 중요한 원칙은 '값의 재사용이 아닌 로직의 재사용' 이다.

훅 내부에 상태 변화에 대한 값만 매개변수로 지정해주어, Side effect에 따른 로직을 정해준다.

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

위의 코드를 custom hook으로 만들어보자.

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

위의 코드를 custom hook을 이용한 로직으로 바꿔보자.

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
