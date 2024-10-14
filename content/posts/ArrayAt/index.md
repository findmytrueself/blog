---
title: 'Array.at과 Array[idx]의 차이'
author: '임훈'
date: 2024-08-02T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---
`Array.at(idx)`은 `es2022`부터 등장한 메소드이다.

기존 방식의 `Array[idx]`과 어떤 차이가 있는지 알아보자.

**차이**

가장 극명한 차이는 음수 인덱스 지원에 있다.

```js
const arr = [10, 20, 30]
console.log(arr[-1]) // undefined

const arr = [10, 20, 30]
console.log(arr.at(-1)) // 30
```
