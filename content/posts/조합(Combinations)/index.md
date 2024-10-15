---
title: '조합(Combinations)'
author: '임훈'
date: 2024-10-15T16:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---


```js
function getCombinations(arr, selectNumber) {
    const results = [];
    
    if (selectNumber === 1) {
        return arr.map((value) => [value]); // 하나씩 반환
    }

    arr.forEach((fixed, index, array) => {
        const rest = array.slice(index + 1); // 현재 요소 이후의 배열
        const combinations = getCombinations(rest, selectNumber - 1); // 재귀 호출
        const attached = combinations.map((combination) => [fixed, ...combination]); // 현재 요소와 조합을 붙임
        results.push(...attached);
    });

    return results;
}
```
