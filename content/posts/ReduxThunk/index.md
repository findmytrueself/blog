---
title: 'Redux Thunk 정리'
author: '임훈'
date: 2022-12-23T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React', '비동기', '상태관리']
---

1. createAsyncThunk는 비동기 작업을 처리하는 Action을 만들어준다.

```js
const asyncUpFetch = createAsyncThunk('counterSlice/asyncUpFetch', async () => {
  const resp = await fetch('https://api.aergo.io/~~')
  const data = await resp.json()
  return data.value
})
```
- createAsyncThunk Flow
  ![createAsyncThunk](images/reduxthunk1.png)
- reducers를 사용하면, actionCreator를 툴킷이 자동으로 만들어주지만, 비동기작업에서는 actionCreator를 자동으로 만들어주지 못하기 때문에, extraReducers에 직접 만들어 주어야 한다.
  ![actionCreator](images/reduxthunk2.png)
