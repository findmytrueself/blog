---
title: 'Set을 사용하는 배열'
author: '임훈'
date: 2024-09-06T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---
**Set의 사용**

일반 배열을 사용한 방식의 차이는 `중복된 요소를 어떻게 처리하는가`이다.

```js
let setArr = new Set(['a','b','c','d','a'])

console.log(setArr)
// 출력: Set(4) { 'a', 'b', 'c', 'd' }
```

* Set은 중복을 허용하지 않는 데이터 구조이다.
* 즉, Set에 같은 값을 여러 번 추가하려고 해도, 중복된 값은 하나만 유지된다.
* Set을 배열로 변환하려면 `Array.from()`을 사용 할 수 있다.

```js
let report = ["muzi frodo", "apeach frodo", "frodo neo", "muzi neo", "muzi frodo"];
let uniqueReports = new Set(report);

console.log(uniqueReports); 
// 출력: Set(4) { 'muzi frodo', 'apeach frodo', 'frodo neo', 'muzi neo' }
```

**Set의 특징**
* `중복을 자동으로 제거`
* 순서가 중요한 상황이 아니라면, 중복 요소를 제거 할 때 유용하다.



