---
title: 'Overview of Redux Thunk'
author: 'Hun Im'
date: 2022-12-23T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'React']
---

1. `createAsyncThunk` allows you to create actions for handling asynchronous operations.

```js
const asyncUpFetch = createAsyncThunk('counterSlice/asyncUpFetch', async () => {
  const resp = await fetch('https://api.aergo.io/~~')
  const data = await resp.json()
  return data.value
})
```
- createAsyncThunk Flow
  ![createAsyncThunk](images/reduxthunk1.png)
- While reducers automatically create action creators for you, this is not the case for asynchronous operations. You need to create the action creators manually within `extraReducers`.
  ![actionCreator](images/reduxthunk2.png)
