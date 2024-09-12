---
title: '순열(Permutation)'
author: '임훈'
date: 2024-09-02T11:53:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
---
순열은 고등학교 수학 때 배우는 개념이다.

그때는 무작정 공식만 외웠기 때문에, 순열이 무엇인지, 언제 쓰이는지 몰랐다.(어릴때 공부를 잘못했던 이유인듯 ...)

**순열이란?**

순열(Permutation)은 집합에 속한 모든 원소의 순서를 고려하여 배열하는 것을 의미합니다.

즉, 주어진 n개의 원소를 모두 사용하여 만들 수 있는 가능한 모든 순서의 조합을 말합니다.

**순열의 특징**
- 순열은 순서가 중요합니다. 즉, {1, 2, 3}과 {3, 2, 1}은 서로 다른 순열로 취급됩니다.
- n개의 원소로 만들 수 있는 순열의 개수는 n! (n 팩토리얼)로 계산됩니다. 예를 들어, 3개의 원소가 있을 때 순열의 개수는 3! = 3 × 2 × 1 = 6개입니다.

**순열의 활용**
- 모든 가능한 순서를 고려해야 할 때 (예: 일정을 짜는 문제)
- 경우의 수를 모두 나열하여 최적의 선택을 해야 할 때 (예: 최단 경로 찾기)

**JS 함수**

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
