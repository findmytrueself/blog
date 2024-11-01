---
title: 'Sorting an Object Using Map'
author: 'Hun Im'
date: 2024-06-02T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---

While arrays can easily be sorted in ascending or descending order using the sort method, how can we `sort` objects by their values?

Here’s one approach:

1. Convert the object into an array for sorting.

2. Sort the array by the object’s values.

3. Convert the sorted array back into an object. If order needs to be maintained, use a `Map`.


```js
const const fruitCounts = { apple: 10, banana: 2, orange: 7, mango: 5 };

const sortedByValue = Object.entries(fruitCounts)
    .sort(([, a], [, b]) => a - b);

console.log(sortedByValue); 
// Output: [ [ 'banana', 2 ], [ 'mango', 5 ], [ 'orange', 7 ], [ 'apple', 10 ] ]

// Convert to Map to maintain order
const sortedMap = new Map(sortedByValue);

console.log(sortedMap);
// Output: Map {'banana' => 2, 'mango' => 5, 'orange' => 7, 'apple' => 10}

console.log(Array.from(sortedMap))
// Converting back to the sorted array

for(const [key, value] of sortedMap){
    console.log(`${key}: ${value}`)
}
// Iterate with for...of to handle keys and values.
```

