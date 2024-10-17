---
title: 'Creating an Array of N Numbers'
author: 'Hun Im'
date: 2024-06-02T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---
During a coding test, I wanted to create an array of N numbers and use `forEach`.

 While I could have used a simple `for` or `while` loop, I was more comfortable using array methods, so I started searching for a suitable method.

**1D Array (Array of N numbers)**

1. for loop

```js
function createArray(n) {
  const result = []
  for (let i = 1; i <= n; i++) {
    result.push(i)
  }
  return result
}
```

2. while loop

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

**Creating a 2D Array, e.g. [[1], [1,2], [1,2,3], [1,2,3,4], [1,2,3,4,5]]**

1. for loop

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

2. while loop

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

**Creating an n x n 2D Array**

```js
Array.from({ length: n }, () => Array(n).fill(0))
```
