---
title: '약수 구하기'
author: '임훈'
date: 2024-10-17T15:40:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---

약수 : 어떤 정수를 나머지 없이 나눌 수 있는 정수

1. 모든 수를 나눠서 약수 구하기

조건 : 나누었을때 나머지의 값이 0이면 추가

조건을 만족 못했을때는 추가

조건 범위 : 약수는 최대 자기 자신까지 되기 때문에 반복문에서 자기 자신 값 이하로 설정

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
    

2. 주어진 수의 절반을 대상으로만 확인하기

약수는 본인을 제외하고 num/2 보다 클 수 없기 때문에 절반 값 까지만 체크

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
    

3. 제곱근(Math.sqrt) 사용하기

1 ~ num의 제곱근 범위로 num의 약수 구해

num이 100이면 1~10 까지 나눠서 나머지가 0 인 값 구한다.

[1, 2, 4, 5, 10]

num을 위의 약수로 나누었을때 값 역시 num의 약수

100 / 1 = 100

100 / 2 = 50

100 / 4 = 25

100 / 5 = 20

100 / 10 = 10 → 중복

[1, 2, 4, 5, 10, 10, 20, 25, 50, 100]

**중복제거**

num / 1번의 약수 === 1번의 약수 인거 제외

[1, 2, 4, 5, 10, 20, 25, 50, 100]

**Set 이용** 

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
