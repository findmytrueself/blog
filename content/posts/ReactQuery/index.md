---
title: 'React Query 정리'
author: '임훈'
date: 2023-01-12T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React', '비동기','상태관리']
---
- 특정한 키를 통해 데이터를 관리하므로, 중복 호출이 될 때, 한번만 호출 하므로 성능에 좋다.

ex) custom hook을 통해, fetch문을 만들어, 사용하게 되면, 사용 될 때 마다 호출을 하기 때문에, 같은 데이터라도 불필요한 호출을 하게 된다.

**Query Keys**

React Query는 캐싱화된 쿼리들을 쿼리 키로써 관리한다. 복잡한 객체와 복잡하고 많은 문자열들을 배열로 관리하고 있다.

Serializable하므로, 유니크한 쿼리 데이터로 관리된다.

```js
useQuery(['todos'], () => {...})
useQuery(['todos', 5, { preview : true }], () => {...})
useQuery(['todos', { type: 'done'}], () => {...})
```

- 원하는 요청을 배열로 요청하면, 유니크한 키로써 캐싱화하여 데이터를 관리한다.

활용 예제

```jsx
const Products = () => {
  const [checked, setChecked] = useState(false)
  const {
    isLoading,
    error,
    data: products,
  } = useQuery(['products', checked], async () => {
    return fetch(`data/${checked ? 'sale_' : ''}products.json`).then((res) =>
      res.json()
    )
  })
}
```
- checked 상태가 변화 할 때 fetch요청을 한다.

**문제점**

캐싱화되어있다고 생각 하는데 console로 확인해보면, 중복 데이터(이미 렌더링 처리 된 UI나 변화없는 데이터 등등의 상황) 매순간 반복 요청 하는 문제점

**React Query의 중요한점**

React Query는 공격적으로 설정되어있지만, 잘 분별 하게도 설정되어있다. 이러한 점은 잘모르는 뉴비들에게 배우기 어렵고 디버깅하기 어렵게 설정되어있다.

- `useQuery`, `useInfiniteQuery`는 기본적으로 stale한 캐시데이터로 간주된다. (stale : 죽은 상태)

- `staleTime`이라는 옵션을 전역적으로 설정해주므로써 컨트롤 할 수 있다.
  : `staleTime`이 설정되어 있는 동안에는 캐싱화된 query 데이터를 refetch하지 않을 것이다.

  1. New instances of the query mount
  2. The window is refocused
  3. The network is reconnected
  4. The query is optionally configured with a refetch interval

- refetch가 내 의도와 상관없이 될 때는, window가 focus가 될 때를 의심해보자

1.  `refetchOnWindowFocus`기능이 기본값으로 켜져있기 때문에 발생한다. dev 환경일 때, devtools를 사용하게 되면, 또한 window를 왔다갔다 하기 때문에, refetch가 계속해서 일어나게 된다.
    -> `refetchOnMount`,`refetchOnWindowFocus`, `refetchOnReconnect`,`refetchInterval` 설정을 바꾸자

2.  `useQuery`, `useInfiniteQuery`를 더이상 사용하지 않으면, `inactive`상태가 된다. 이 상태가 5분간 지속되면 자동적으로 garbage collect가 된다.
    -> `cacheTime`의 설정을 바꾸자. `1000 * 60 * 5` 이상의 값(5분이상)을 준다.

3.  Query가 실패할 때, 3번 재반복 실행을 한다. 3번 반복(기본값 : 3)할 때, 간격이 점점 길어짐.
    -> `retry`, `retryDelay`로 설정한다.

**stale 상태에 따른 데이터 통신 순서**

![stale](images/stale.png)

**업데이트 하기**

업데이트 되면 기존 쿼리는 invalidate 시켜주면 다시 저절로 fetch를 하게 된다.

```jsx
const client = useQueryClient()

<button onClick={()=>client.invalidateQueries(['products', false])}>정보 업데이트 하기!</button>
```

**Redux -> React-Query 전환**

Link : [Redux -> React-Query 전환][https://youtu.be/hcvcb36wzzk]
