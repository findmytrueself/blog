---
title: 'Array Using Set'
author: 'Hun Im'
date: 2024-09-06T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---
**Using Set**

The main difference between using a regular array and a Set is how they handle `duplicate elements`.

```js
let setArr = new Set(['a','b','c','d','a'])

console.log(setArr)
// 출력: Set(4) { 'a', 'b', 'c', 'd' }
```

* A Set is a data structure that does not allow duplicates.
* This means that even if you try to add the same value multiple times to a Set, only one instance of that value will be kept.
* To convert a Set into an array, you can use `Array.from()`.

```js
let report = ["muzi frodo", "apeach frodo", "frodo neo", "muzi neo", "muzi frodo"];
let uniqueReports = new Set(report);

console.log(uniqueReports); 
// 출력: Set(4) { 'muzi frodo', 'apeach frodo', 'frodo neo', 'muzi neo' }
```

**Characteristics of Set**
* `Automatically removes duplicates`
* It is useful when you need to remove duplicate elements and order is not important.



