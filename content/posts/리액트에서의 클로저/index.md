---
title: '리액트에서의 클로저'
author: '임훈'
date: 2024-09-11T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
---

클로저는 자바스크립트에서 기본이 되는 아주 중요한 개념이다.

항상, 기술면접으로 1순위로 물어보는 개념이기 때문에, 말그대로 답변을 달달달 외우며, 기술면접을 준비했던 기억이 난다.

하지만, 여전히 실무에서 어떻게 쓰여아하는지는 이해를 못했고, 응용을 못하고 있었던 때, 해당 개념을 더 심도있게 공부해보고자 한다.

**문제**

*customHook에 함수를 parameter로 넘겨줄 때, useCustom 컴포넌트가 unmount되는 상황에 실행될 때, 해당 함수가 출력되고, 함수에 포함된 state값은 어떻게 출력이 되는가?*

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

1. 초기 렌더링 할 때, state는 초기로 설정 된 값 0
2. useCustom 내부 console.log 함수에 들어가는 state는 0
3. 이때, useCustom 컴포넌트에서 실행 된 func 함수 내부의 state는 0으로 기억이 되고 있다.(useCustom 컴포넌트가 unmount 될 때 까지)
4. 그리고 unmount될 때 비로소, 기억한 함수를 실행한다. 이때 의존성 배열이 빈배열이기 때문에, 한번만 실행되고, state가 바뀔때마다 재호출을 하기 위해서는 의존성배열에 state를 추가해준다.
5. state가 추가 되어도, params state와 inner func state는 다를 수 밖에 없다. 클로저가 생성 될 때, 이전의 상태 값을 참조하기 때문이다.