---
title: 'N개의 숫자의 배열 만들기'
author: '임훈'
date: 2024-06-02T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
---
코딩테스트 중에 숫자 N이 주어졌을 때 N개의 배열을 만들어서 forEach를 하고 싶었다.

그냥 for문이나 while문으로 하면 되겠지만, 배열의 메소드가 익숙해버려서, 해당 방법을 찾기 시작했다.

**1중배열 (n개의 배열)**

1. for

```js
function createArray(n) {
  const result = []
  for (let i = 1; i <= n; i++) {
    result.push(i)
  }
  return result
}
```

2. while

```js
function createArray(n) {
  const result = []
  let i = 1

  while (i <= n) {
    result.push(i)
    i++
  }

  return result
}
```

3. Array.from

```js
//* Recommend
const array = Array.from({ length: n }, (_, i) => i + 1)
```

4. Array.map

```js
const array = Array(n)
  .fill()
  .map((_, i) => i + 1)
```

**2중배열로 만들기, ex)[[1], [1,2], [1,2,3], [1,2,3,4], [1,2,3,4,5]]**

1. for

```js
function createArrays(n) {
  const result = []
  for (let i = 1; i <= n; i++) {
    const arr = []
    for (let j = 1; j <= i; j++) {
      arr.push(j)
    }
    result.push(arr)
  }
  return result
}
```

2. while

```js
function createArrays(n) {
  const result = []
  let i = 1

  while (i <= n) {
    const arr = []
    let j = 1

    while (j <= i) {
      arr.push(j)
      j++
    }

    result.push(arr)
    i++
  }

  return result
}
```

3. Array.from

```js
//* Recommend
function createArrays(n) {
  return Array.from({ length: n }, (_, i) =>
    Array.from({ length: i + 1 }, (_, j) => j + 1)
  )
}
```

4. Array.map

```js
function createArrays(n) {
  return Array(n)
    .fill()
    .map((_, i) =>
      Array(i + 1)
        .fill()
        .map((_, j) => j + 1)
    )
}
```

**n x n 2차원 배열 만들기**

```js
Array.from({ length: n }, () => Array(n).fill(0))
```
