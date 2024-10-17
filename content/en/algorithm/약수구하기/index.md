---
title: 'Finding Divisors'
author: '임훈'
date: 2024-10-17T15:40:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---

Divisors: An integer that can divide another integer without a remainder.

1. Finding divisors by dividing all numbers

Condition: Add the number if the remainder is 0 when divided.

Skip if the condition is not met.

Range of condition: The divisor can be at most the number itself, so set the loop to iterate up to the number.

```js
    let num = 8; // 약수를 찾기 위한 정수 설정
    let result = []
    let index = 1;
    
    while (index <= num) {
      if (num % index === 0) result.push(index)
      index++
    }
    console.log(result) // [ 1, 2, 4, 8 ]
```
    

2. Checking only up to half of the given number

Divisors, except for the number itself, cannot be larger than `num / 2`, so check only up to half.

```js
    let num = 8;
    let result = []
    let index = 1;
    while (index <= num / 2) {
      if (num % index === 0) result.push(index)
      index++
    }
    result = [...result, num] // 본인 값 추가까지 추가
    console.log(result) // [ 1, 2, 4, 8 ]
```
    

3. Using the square root (Math.sqrt)

Find the divisors of num within the range of 1 to the square root of num.

If num is 100, divide by numbers from 1 to 10 and find those that result in a remainder of 0.

[1, 2, 4, 5, 10]

Also, the result of dividing num by these divisors will be divisors of num.

100 / 1 = 100

100 / 2 = 50

100 / 4 = 25

100 / 5 = 20

100 / 10 = 10 → duplicate

[1, 2, 4, 5, 10, 10, 20, 25, 50, 100]

**Remove duplicates**

Exclude the divisor from the result if it equals num / divisor.

[1, 2, 4, 5, 10, 20, 25, 50, 100]

**Using Set** 

```js
    let num = 100;
    let result = []
    let index = 1;
    while (index <= Math.sqrt(num)) {
      if (num % index === 0) {
        result.push(index)
        if (num / index !== index) result.push(num / index)
      }
      index++
    }
    result.sort((a, b) => a - b)
    
    console.log(result) // [ 1, 2, 4, 5, 10, 20, 25, 50, 100 ]

```

*Reference*

<https://velog.io/@woody_/JS-%EC%95%BD%EC%88%98-%EA%B5%AC%ED%95%98%EA%B8%B0-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98>
