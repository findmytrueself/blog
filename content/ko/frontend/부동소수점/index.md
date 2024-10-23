---
title: '부동 소수점'
author: '임훈'
date: 2024-10-23T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript']
---

JavaScript에서 부동소수점은 숫자를 표현하기 위해 IEEE 754 표준의 64비트 부동소수점 형식을 사용합니다.

일부 숫자를 정확하게 표현하지 못하고 오차가 발생할 수 있습니다.

### 부동소수점의 문제점

1. 이진수 표현의 한계: 일부 십진수 소수는 이진수로 정확하게 표현될 수 없습니다. 예를 들어, 0.1이나 0.2와 같은 숫자는 이진 부동소수점으로 표현할 때 무한소수가 되어 근사치로 저장됩니다.

```js
console.log(0.1 + 0.2); // 예상: 0.3, 실제: 0.30000000000000004
```

2. 정밀도 손실: 계산 과정에서 작은 오차들이 누적되어 큰 오차로 이어질 수 있습니다. 이는 특히 반복 계산이나 금융 계산에서 문제가 됩니다.

### 해결책

1. toFixed() 메서드 사용

toFixed() 메서드는 숫자를 지정된 소수점 자리까지 반올림하여 문자열로 반환합니다.

```js
let sum = 0.1 + 0.2;
console.log(sum.toFixed(2)); // "0.30"
```

주의사항: toFixed()는 문자열을 반환하므로, 추가 계산이 필요하면 parseFloat()나 Number()로 숫자로 변환해야 합니다.

2. 소수점 자리수를 고려한 계산

모든 숫자를 정수로 변환하여 계산하고, 결과를 다시 소수로 변환합니다.

```js
let a = 0.1;
let b = 0.2;
let sum = (a * 100) + (b * 100); // 10 + 20 = 30
sum = sum / 100; // 0.3
console.log(sum); // 0.3
```

장점: 부동소수점 오차를 피할 수 있습니다.

단점: 처리해야 할 숫자의 범위에 따라 스케일링 팩터를 관리해야 합니다.

3. EPSILON을 사용한 비교

두 부동소수점 숫자가 거의 같은지 비교할 때 Number.EPSILON을 사용합니다.

```js
function areAlmostEqual(a, b) {
  return Math.abs(a - b) < Number.EPSILON;
}

console.log(areAlmostEqual(0.1 + 0.2, 0.3)); // true
```

4. 외부 라이브러리 사용

* Decimal.js
```js
const Decimal = require('decimal.js');

let a = new Decimal(0.1);
let b = new Decimal(0.2);
let sum = a.plus(b);

console.log(sum.toString()); // "0.3"

```

* BigNumber.js
```js
const BigNumber = require('bignumber.js');

let a = new BigNumber(0.1);
let b = new BigNumber(0.2);
let sum = a.plus(b);

console.log(sum.toString()); // "0.3"
```

장점: 매우 높은 정밀도의 계산이 가능합니다.

단점: 기본 숫자 타입보다 느리고, 코드가 복잡해질 수 있습니다.

