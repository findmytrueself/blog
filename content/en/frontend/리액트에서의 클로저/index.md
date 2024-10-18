---
title: '리액트에서의 클로저'
author: '임훈'
date: 2024-09-11T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'React']
---

---
title: 'Closures in React'
author: 'Im Hun'
date: 2024-09-11T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'React']
---

Closures are a fundamental concept in JavaScript.

It's a concept that is often asked about in technical interviews, and I remember memorizing it while preparing for my own technical interviews.

However, I didn't quite understand how closures are used in practice, and I often pondered how to apply them effectively. This time, I want to study the concept of closures in more depth.

**Problem**

*When passing a function as a parameter to `customHook`, let’s observe how the `state` value inside the function is output when the `useCustom` component is unmounted.*

```jsx
import useCustom from './hooks/useCustom'
import { useState } from 'react'

export default function Home() {
  const [state, setState] = useState(0)

  useCustom(() => console.log('inner func state:', state))

  return (
    <div className="grid grid-rows-[20px_1fr_20px] items-center justify-items-center min-h-screen p-8 pb-20 gap-16 sm:p-20 font-[family-name:var(--font-geist-sans)]">
      <div>{state}</div>
      <button onClick={() => setState(state + 1)}>+</button>
    </div>
  )
}
```

```jsx
import React, { useEffect } from 'react'

const useCustom = (func: Function, state: any) => {
  console.log('params state:', state)
  useEffect(() => {
    return () => func()
  }, [])

  return <div>useCustom</div>
}

export default useCustom
```
**Code Explanation**

1. On the initial render, the `state` is set to 0.
2. The `console.log` inside `useCustom` outputs the value of `state`, which is the initial value of 0.
3. At this point, the `func` function passed to `useCustom` also retains the state of 0 due to closures. When the `useCustom` component is unmounted, this function executes, but the `state` remains at 0.
4. Since the dependency array is empty, the `useEffect` runs only once, and it does not re-execute even if the `state` changes.
5. If you want to use the latest `state` value, you need to add `state` to the dependency array of `useEffect`. Otherwise, due to closures, it will reference the previous state.