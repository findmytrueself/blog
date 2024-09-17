---
title: '리액트에서의 클로저'
author: '임훈'
date: 2024-09-11T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
---

클로저는 자바스크립트에서 기본이 되는 아주 중요한 개념이다.

기술 면접에서 1순위로 자주 물어보는 개념이기 때문에, 나도 기술 면접을 준비하며 외웠던 기억이 난다.

하지만, 실무에서 클로저가 어떻게 쓰이는지 잘 이해하지 못했고, 이를 실제로 어떻게 응용해야 할지 고민하던 시기가 있었다. 이번에 클로저 개념을 더 심도 있게 공부해보려 한다.

**문제**

*`customHook`에 함수를 파라미터로 넘겨줄 때, `useCustom` 컴포넌트가 언마운트되는 상황에서 해당 함수가 실행될 때, 함수 내부의 `state` 값이 어떻게 출력되는지 살펴보자.*

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
**코드 설명**

1. 초기 렌더링 시 `state`는 0으로 설정된다.
2. `useCustom` 훅 내부의 `console.log`에 출력되는 `state` 값은 초기 값인 0이다.
3. 이때 `useCustom`에서 넘겨준 `func` 함수도 클로저로 인해 `state`가 0인 상태를 기억하게 된다. 이후 `useCustom` 컴포넌트가 언마운트될 때 해당 함수가 실행되지만, 그때의 state는 0으로 남아 있다.
4. 의존성 배열이 빈 배열이기 때문에 `useEffect`는 한 번만 실행되며, `state`가 바뀌더라도 재실행되지 않는다.
5. 만약 최신 `state` 값을 사용하고 싶다면, `useEffect`의 의존성 배열에 `state`를 추가해 주어야 한다. 그렇지 않으면 클로저로 인해 이전 상태를 참조하게 된다.