---
title: '객체 오름차순 정렬, 객체 같은지 비교'
author: '임훈'
date: 2024-11-04T10:00:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: '/images/gamer.png'
keywords: ['Javascript', 'Algorithm']
---

* 객체 오름 차순 정렬
```js
function sortObjectByKeys(obj) {
    return Object.keys(obj)
        .sort() // 키를 오름차순으로 정렬
        .reduce((sortedObj, key) => {
            sortedObj[key] = obj[key];
            return sortedObj;
        }, {});
}
```

* 객체 내림 차순 정렬
```js
function sortObjectByKeysDescending(obj) {
  return Object.keys(obj)
    .sort((a, b) => b.localeCompare(a)) // 키를 내림차순으로 정렬
    .reduce((sortedObj, key) => {
      sortedObj[key] = obj[key]; // 정렬된 키 순서대로 새 객체에 삽입
      return sortedObj;
    }, {});
}
```

* 같은 객체인지 내부 키와 값 비교
```js
function areObjectsEqual(obj1, obj2) {
    if (Object.keys(obj1).length !== Object.keys(obj2).length) return false;
    for (let key in obj1) {
        if (obj1[key] !== obj2[key]) return false;
    }
    return true;
}
```