---
title: 'Difference Between Array.at and Array[idx]'
author: 'Hun Im'
date: 2024-08-02T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---

`Array.at(idx)` is a method introduced in `ES2022`.

Let's explore the differences between the traditional Array[idx] and Array.at(idx).

**Differences**

The most significant difference is the support for negative indices.

```js
const arr = [10, 20, 30]
console.log(arr[-1]) // undefined

const arr = [10, 20, 30]
console.log(arr.at(-1)) // 30
```
