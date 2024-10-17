---
title: 'Permutation'
author: 'Hun Im'
date: 2024-09-02T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---
Permutation is a concept learned in high school mathematics.

At that time, I memorized the formulas without really understanding what permutations were or when to use them (I guess that’s why I didn’t study well as a kid...)

**What is a Permutation?**

A permutation refers to arranging all the elements of a set while considering the order.

In other words, it refers to all possible arrangements of n given elements in all possible orders.

**Characteristics of Permutations**
- In permutations, the order is important. For example, {1, 2, 3} and {3, 2, 1} are considered different permutations.
- The number of permutations that can be made with n elements is calculated as n! (n factorial). For example, with 3 elements, the number of permutations is 3! = 3 × 2 × 1 = 6.

**Applications of Permutations**
- When you need to consider all possible orders (e.g., scheduling problems)
- When you need to list all possible cases to make the optimal choice (e.g., finding the shortest path)

**JS Function**

```js
function getPermutations(arr) {
  let results = []

  function permute(current, remaining) {
    if (remaining.length === 0) {
      results.push(current)
    }

    for (let i = 0; i < remaining.length; i++) {
      let next = current.concat(remaining[i])
      let newRemaining = remaining.slice(0, i).concat(remaining.slice(i + 1))
      permute(next, newRemaining)
    }
  }

  permute([], arr)
  return results
}

const array = [1, 2, 3]
const permutations = getPermutations(array)
console.log(permutations)
/** [
  [1, 2, 3],
  [1, 3, 2],
  [2, 1, 3],
  [2, 3, 1],
  [3, 1, 2],
  [3, 2, 1]
] */
```
