---
title: 'Sorting Objects in Ascending Order, Comparing Objects for Equality'
author: '임훈'
date: 2024-11-04T10:00:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: '/images/gamer.png'
keywords: ['Javascript', 'Algorithm']
---

* Sorting an Object's Keys in Ascending Order
```js
function sortObjectByKeys(obj) {
    return Object.keys(obj)
        .sort() // Sort keys in ascending order
        .reduce((sortedObj, key) => {
            sortedObj[key] = obj[key];
            return sortedObj;
        }, {});
}
```

* Sorting an Object's Keys in Descending Order
```js
function sortObjectByKeysDescending(obj) {
  return Object.keys(obj)
    .sort((a, b) => b.localeCompare(a)) // Sort keys in descending order
    .reduce((sortedObj, key) => {
      sortedObj[key] = obj[key]; // Insert into new object in the order of sorted keys
      return sortedObj;
    }, {});
}
```

* Comparing Two Objects for Equality of Keys and Values
```js
function areObjectsEqual(obj1, obj2) {
    if (Object.keys(obj1).length !== Object.keys(obj2).length) return false;
    for (let key in obj1) {
        if (obj1[key] !== obj2[key]) return false;
    }
    return true;
}
```