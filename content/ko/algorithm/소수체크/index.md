---
title: '소수 체크'
author: '임훈'
date: 2024-10-22T10:00:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: '/images/gamer.png'
keywords: ['Javascript', 'Algorithm']
---

```js
function isPrime(num) {
  if (num === 2) {
    return true
  } else if (num % 2 === 0) {
    return false
  }
  let sqrt = parseInt(Math.sqrt(num))
  for (let i = 3; i <= sqrt; i = i + 2) {
    if (num % i === 0) {
      return false
    }
  }
  return true
}
```
