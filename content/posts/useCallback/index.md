---
title: 'useCallback'
author: '임훈'
date: 2024-09-02T11:53:04+09:00
category: ['POSTS']
tag: ['Frontend', 'React', 'useCallback']
---

# 인트로

해당 궁금증을 해소 하기 전 배경에는, useEffect dependency 배열에 함수가 들어가는 상황에 대해서 공부하고 있던 시점이다.

보통 커스텀 훅을 만들 때에는 해당 훅 컴포넌트의 props에 상태 값을 받아오고, 그 상태 값을 useEffect 내부 dependency배열에 넣어 동작을 처리 한다.

그렇다면 함수를 props로 받아올 땐 어떤가? 고민해보았다.

## 함수가 의존성 배열에 들어가는 이유

리액트 컴포넌트가 렌더링될 때마다 함수는 다시 생성된다.

자바스크립트에서는 함수가 일급 객체이므로, 동일한 이름의 함수라도 매 렌더링 시 새롭게 생성된 함수는 이전 함수와 다르게 취급된다.

따라서, 의존성 배열에 함수가 포함되면 그 함수가 매 렌더링마다 재생성될 때마다 useEffect가 다시 실행된다.

### useCallback을 사용 할 때 예제

```js
import React, { useState, useEffect, useCallback } from 'react'

function SearchComponent() {
  const [searchQuery, setSearchQuery] = useState('')
  const [results, setResults] = useState([])

  const fetchData = useCallback(() => {
    fetch(`https://api.example.com/search?q=${searchQuery}`)
      .then((response) => response.json())
      .then((data) => setResults(data.results))
      .catch((error) => console.error('Error fetching data:', error))
  }, [searchQuery]) // fetchData 함수는 searchQuery에 의존합니다.

  useEffect(() => {
    fetchData()
  }, [fetchData]) // fetchData 함수가 의존성 배열에 포함됩니다.

  return (
    <div>
      <input
        type="text"
        value={searchQuery}
        onChange={(e) => setSearchQuery(e.target.value)}
      />
      <ul>
        {results.map((result, index) => (
          <li key={index}>{result.name}</li>
        ))}
      </ul>
    </div>
  )
}

export default SearchComponent
```

### useCallback을 사용하지 않을 때 예제

```js
import React, { useState, useEffect } from 'react'

function SearchComponent() {
  const [searchQuery, setSearchQuery] = useState('')
  const [results, setResults] = useState([])

  useEffect(() => {
    const fetchData = () => {
      fetch(`https://api.example.com/search?q=${searchQuery}`)
        .then((response) => response.json())
        .then((data) => setResults(data.results))
        .catch((error) => console.error('Error fetching data:', error))
    }

    fetchData()
  }, [searchQuery]) // searchQuery가 변경될 때마다 useEffect가 실행됩니다.

  return (
    <div>
      <input
        type="text"
        value={searchQuery}
        onChange={(e) => setSearchQuery(e.target.value)}
      />
      <ul>
        {results.map((result, index) => (
          <li key={index}>{result.name}</li>
        ))}
      </ul>
    </div>
  )
}

export default SearchComponent
```

## 차이점

1. 함수 생성 및 메모리 사용

- useEffect 내부에서 함수를 직접 정의할 경우, 이 함수는 searchQuery가 변경될 때마다 useEffect와 함께 새로 정의됩니다.

- 매번 함수가 새로 생성되기 때문에 메모리 사용이 늘어날 수 있고, 함수의 정의 자체가 매번 새로 생성되는 것은 불필요한 연산을 초래할 수 있습니다.

- 이 차이는 성능에 민감한 애플리케이션에서 영향을 미칠 수 있으며, 특히 자주 렌더링되는 컴포넌트에서는 부정적인 영향을 줄 수 있습니다.

2. 의존성 관리

- useCallback을 사용하여 함수를 감싸지 않은 경우, 의존성 배열에서 함수 자체가 의존성으로 추가되지 않습니다.

- useEffect는 오직 searchQuery가 변경될 때만 실행됩니다. 이 경우 searchQuery가 변경될 때마다 fetchData 함수가 새로 정의되지만, 이로 인해 의존성 관리가 복잡해지지 않으며, 단순히 매번 함수가 새로 정의되는 방식으로 동작하게 됩니다.

3. 불필요한 리렌더링 방지

- useCallback을 사용하여 함수를 정의할 때, 이 함수는 의존성 배열에 있는 값이 변경될 때만 새로 생성됩니다. 이는 컴포넌트가 불필요하게 리렌더링되는 것을 방지합니다.

- useCallback을 사용하지 않으면 fetchData 함수가 매번 새로 생성되고, 이로 인해 만약 다른 의존성을 관리하는 부분이 있다면, 의도치 않게 불필요한 리렌더링이 발생할 수 있습니다.

## useCallback을 쓰는 상황

1. 복잡한 컴포넌트

- 상태 관리가 많을 때: 여러 개의 상태를 관리하고, 각 상태가 서로 복잡하게 상호작용할 때 컴포넌트가 복잡해집니다. 예를 들어, 폼(form)과 같은 컴포넌트에서 많은 입력 필드의 상태를 관리하거나, 다양한 이벤트에 따라 UI가 동적으로 변하는 경우가 있습니다.

- 자식 컴포넌트가 많을 때: 컴포넌트가 여러 자식 컴포넌트를 렌더링하고 이들 간에 많은 데이터를 주고받을 때 복잡성이 증가합니다. 특히 자식 컴포넌트들이 각각 독립적인 상태를 관리하거나, 부모 컴포넌트의 상태를 공유해야 하는 경우입니다.

- 조건부 렌더링이 많을 때: 컴포넌트가 다양한 조건에 따라 서로 다른 UI를 렌더링해야 하는 경우, 여러 가지 조건문(if, switch, 삼항 연산자 등)이 사용되면서 코드가 복잡해질 수 있습니다.

2. 성능이 민감한 경우

- 자주 렌더링되는 컴포넌트: 화면에 자주 렌더링되거나, 사용자가 인터랙션할 때마다 빠르게 반응해야 하는 컴포넌트입니다. 예를 들어, 실시간으로 업데이트되는 대시보드, 채팅 애플리케이션, 애니메이션 효과가 많은 UI 등이 여기에 해당합니다.

- 대량의 데이터를 처리할 때: 컴포넌트가 한 번에 많은 데이터를 처리하거나 렌더링해야 하는 경우입니다. 예를 들어, 테이블에 수천 개의 행을 렌더링해야 하는 경우나, 복잡한 그래프를 그려야 하는 상황이 이에 해당합니다.

- 최적화가 필요한 경우: 특정 작업이 자주 발생하면서 성능에 부정적인 영향을 미치는 경우 최적화가 필요합니다. 예를 들어, 불필요한 리렌더링을 줄이기 위해 React.memo, useCallback, useMemo와 같은 리액트의 최적화 훅을 사용해야 할 때가 있습니다.

이러한 상황에서는 컴포넌트가 효율적으로 동작하지 않으면 사용자 경험이 저하될 수 있으므로, 코드의 최적화나 리액트의 성능 관련 기능들을 활용하는 것이 중요합니다.

## 결론

- useCallback을 사용하지 않는 경우: useEffect 내부에서 직접 함수를 정의하고 실행하면, 해당 함수는 의존성 배열에 포함된 값이 변경될 때마다 새로 생성되며 메모리와 성능에 영향을 미칠 수 있습니다. 하지만 간단한 상황에서는 별다른 문제가 없을 수 있습니다.

- useCallback을 사용하는 경우: 함수가 불필요하게 재생성되는 것을 방지하여 성능 최적화에 기여할 수 있습니다. 특히, 함수가 자주 재생성되는 경우 이 방법이 더 효율적일 수 있습니다.

따라서, 작은 규모의 컴포넌트에서는 큰 차이가 없을 수 있지만, 복잡한 컴포넌트나 성능에 민감한 경우라면 useCallback을 사용하여 함수를 메모이제이션(memoization)하는 것이 더 나은 선택이 될 수 있습니다.
