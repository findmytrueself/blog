---
title: 'Combinations'
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
        return arr.map((value) => [value]); // Return each element as an array
    }

    arr.forEach((fixed, index, array) => {
        const rest = array.slice(index + 1); // The array after the current element
        const combinations = getCombinations(rest, selectNumber - 1); // Recursive call
        const attached = combinations.map((combination) => [fixed, ...combination]); // Attach the current element to the combinations
        results.push(...attached);
    });

    return results;
}
```
