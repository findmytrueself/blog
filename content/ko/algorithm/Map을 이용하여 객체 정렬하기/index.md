---
title: 'Map을 이용하여 객체 정렬하기 '
author: '임훈'
date: 2024-06-02T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---

배열의 정렬은 sort 메소드를 사용하여 오름,내림차순 정렬이 가능하다.

하지만 객체의 value의 값에 맞춰서 정렬은 어떻게 해야할까?

1. 객체를 배열로 변환하여 정렬한다.

2. 정렬된 배열을 다시 객체로 변환한다

3. 순서를 유지하고 싶으면 Map을 사용하여 변환한다.

```js
const fruitCounts = { apple: 10, banana: 2, orange: 7, mango: 5 };

const sortedByValue = Object.entries(fruitCounts)
    .sort(([, a], [, b]) => a - b);

console.log(sortedByValue); 
// 출력: [ [ 'banana', 2 ], [ 'mango', 5 ], [ 'orange', 7 ], [ 'apple', 10 ] ]

// Map으로 변환하여 순서 보장
const sortedMap = new Map(sortedByValue);

console.log(sortedMap);
// 출력: Map {'banana' => 2, 'mango' => 5, 'orange' => 7, 'apple' => 10}

console.log(Array.from(sortedMap))
```

