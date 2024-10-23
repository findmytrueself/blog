---
title: '리액트 19버전 정리'
author: '임훈'
date: 2024-10-22T10:40:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
og_image: '/images/gamer.png'
keywords: ['Javascript', 'React']
---

## 주요 새로운 기능

1. Actions

- 비동기 함수 지원 강화: `useTransition`을 사용하여 비동기 함수에서 자동으로 pending 상태, 오류 처리, 낙관적 업데이트를 관리할 수 있습니다.

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

2. 새로운 훅: `useActionState`

- Actions의 공통 사례를 쉽게 처리: 비동기 액션의 결과, pending 상태, 오류 등을 관리할 수 있습니다.

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

3. React DOM: <form> Actions 및 `useFormStatus`

- 함수를 action 및 formAction prop으로 전달: 폼 제출을 자동으로 관리하며, 제출 후 폼을 자동으로 재설정합니다.

- useFormStatus 훅 추가: 디자인 시스템에서 폼의 상태에 접근할 수 있도록 지원합니다.

```jsx
function DesignButton() {
  const { pending } = useFormStatus()
  return <button type="submit" disabled={pending} />
}
```

4. 새로운 훅: `useOptimistic`

- 낙관적 업데이트 관리: 비동기 요청이 진행되는 동안 사용자에게 즉각적인 피드백을 제공할 수 있습니다.

```jsx
const [optimisticName, setOptimisticName] = useOptimistic(currentName)

const submitAction = async (formData) => {
  const newName = formData.get('name')
  setOptimisticName(newName)
  const updatedName = await updateName(newName)
  onUpdateName(updatedName)
}
```

5. 새로운 API: `use`

- 리소스를 렌더링 중에 읽기 지원: use를 사용하여 프로미스나 컨텍스트를 읽고, 필요한 경우 컴포넌트를 일시 중단(Suspend)할 수 있습니다.

```jsx
function Comments({ commentsPromise }) {
  const comments = use(commentsPromise)
  return comments.map((comment) => <p key={comment.id}>{comment}</p>)
}
```

6. React 서버 컴포넌트

- 서버 컴포넌트 지원: 서버에서 사전 렌더링하여 클라이언트 번들 크기를 줄이고, 성능을 향상시킵니다.

- 서버 액션: 클라이언트 컴포넌트에서 서버에서 실행되는 비동기 함수를 호출할 수 있습니다.

## 개선 사항

1. ref를 Prop으로 전달

- forwardRef 불필요: 함수형 컴포넌트에서 ref를 직접 Prop으로 받아 사용할 수 있습니다.

```jsx
function MyInput({ placeholder, ref }) {
  return <input placeholder={placeholder} ref={ref} />
}
```

2. Hydration 오류에 대한 개선된 디버깅

- 차이점(diff) 제공: Hydration 시 발생하는 오류에 대해 더 명확한 메시지와 디버깅 정보를 제공합니다.

3. <Context>를 프로바이더로 사용

- 더 간결한 컨텍스트 사용법: <Context.Provider> 대신 <Context>를 사용하여 컨텍스트 값을 제공할 수 있습니다.

```jsx
const ThemeContext = createContext('')

function App({ children }) {
  return <ThemeContext value="dark">{children}</ThemeContext>
}
```

4. ref의 정리 함수 지원

- 클린업 함수 반환: ref 콜백에서 정리(cleanup) 함수를 반환하여 컴포넌트가 언마운트될 때 실행되도록 할 수 있습니다.

```jsx
<input
  ref={(ref) => {
    // ref 생성 시 실행
    return () => {
      // ref 정리 시 실행
    }
  }}
/>
```

5. useDeferredValue의 초기 값 지원

- 초기 값 설정: useDeferredValue에 initialValue 옵션을 추가하여 초기 렌더링 시 사용할 값을 지정할 수 있습니다.

```jsx
const value = useDeferredValue(deferredValue, '')
```

6. 문서 메타데이터 지원

- `<title>, <meta>, <link>`태그의 자연스러운 사용: 컴포넌트 내에서 문서의 메타데이터를 직접 정의하고, React가 이를 `<head>`로 자동 호이스팅합니다.

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

7. 스타일시트 지원 개선

- 스타일시트의 우선순위 관리: <link> 태그에 precedence 속성을 사용하여 스타일시트의 삽입 순서를 제어할 수 있습니다.

```jsx
<link rel="stylesheet" href="style.css" precedence="high" />
```

8. 비동기 스크립트 지원

- 중복 제거 및 로드 순서 관리: 비동기 스크립트를 컴포넌트 내에서 선언하고, React가 로드 순서와 중복을 관리합니다.

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

9. 리소스 프리로딩 지원

- 성능 최적화: preload, prefetchDNS, preconnect 등의 API를 제공하여 브라우저 리소스 로딩을 최적화합니다.

```jsx
import { prefetchDNS, preconnect, preload, preinit } from 'react-dom'

function MyComponent() {
  preinit('https://example.com/script.js', { as: 'script' })
  preload('https://example.com/font.woff', { as: 'font' })
}
```

10. 서드파티 스크립트 및 확장 프로그램과의 호환성 향상

- Hydration 개선: 예기치 않은 태그나 요소로 인한 Hydration 오류를 방지하고, 서드파티 스크립트나 브라우저 확장 프로그램과의 충돌을 최소화합니다.

11. 오류 보고 개선

- 중복 제거 및 상세 정보 제공: 오류 메시지의 중복을 제거하고, `onCaughtError`, `onUncaughtError` 등의 새로운 루트 옵션을 추가하여 오류 처리를 유연하게 합니다.

12. 커스텀 엘리먼트 지원

- 속성 및 프로퍼티 관리 개선: 커스텀 엘리먼트에 대한 속성과 프로퍼티 처리가 개선되어, 클라이언트 및 SSR 환경에서 일관성 있게 동작합니다.
