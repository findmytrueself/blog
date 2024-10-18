---
title: 'Overview of React Query'
author: 'Hun Im'
date: 2023-01-12T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'React']
---
- React Query efficiently manages data through unique keys, preventing duplicate fetch calls and improving performance.

For example, if you create a fetch function via a custom hook, it will be called each time it's used, leading to unnecessary calls for the same data.

**Query Keys**

React Query manages cached queries using query keys, organizing complex objects and long strings as arrays.

Since these keys are serializable, they serve as unique identifiers for the cached data.

```js
useQuery(['todos'], () => {...})
useQuery(['todos', 5, { preview : true }], () => {...})
useQuery(['todos', { type: 'done'}], () => {...})
```

- By requesting desired data in an array, you create a unique key for caching the data.

Example usage:

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
- A fetch request is made when the `checked` state changes.

**Issues**

Although it may seem like caching is effective, you might notice in the console that duplicate data (such as already rendered UI or unchanged data) is repeatedly requested.

**Key Points of React Query**

React Query is aggressively set up but also provides options for fine-tuning, which can be difficult for beginners to understand and debug.
- By default, `useQuery` and `useInfiniteQuery` consider cached data as stale (stale: inactive or expired).
- The `staleTime` option can be set globally to control this behavior
  : While `staleTime` is set, cached query data will not be refetched

Refetching can occur under the following conditions:

  1. New instances of the query mount
  2. The window is refocused
  3. The network is reconnected
  4. The query is optionally configured with a refetch interval
- If refetching happens unexpectedly, consider the window focus as a possible cause:

1.  The `refetchOnWindowFocus` feature is enabled by default, which can cause refetching when switching between tabs, especially when using dev tools.
    -> Adjust settings for `refetchOnMount`, `refetchOnWindowFocus`, `refetchOnReconnect`, and `refetchInterval`.

2.  If `useQuery` or `useInfiniteQuery` are not used for a while, they will become inactive. After 5 minutes in this state, they will be automatically garbage collected.
    -> Change the `cacheTime` setting to a value greater than `1000 * 60 * 5 (5 minutes)`.

3.  When a query fails, it retries three times by default, with increasing intervals.
    -> Use the `retry` and `retryDelay` settings to control this behavior.

**Data Communication Sequence Based on Stale State**

![stale](images/stale.png)

**Updating Data**

When updating, you can invalidate the existing query, which will automatically trigger a refetch.

```jsx
const client = useQueryClient()

<button onClick={()=>client.invalidateQueries(['products', false])}>정보 업데이트 하기!</button>
```

**Transitioning from Redux to React Query**

Link : [Transitioning from Redux to React Query][https://youtu.be/hcvcb36wzzk]
