---
title: 'Floating-Point Numbers'
author: 'Hun Im'
date: 2024-10-23T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript']
---

In JavaScript, floating-point numbers are represented using the 64-bit floating-point format defined by the IEEE 754 standard.

Some numbers cannot be represented exactly, which can lead to errors.

### Issues with Floating-Point Numbers

1. **Limitations of Binary Representation**: Some decimal fractions cannot be represented exactly in binary form. For example, numbers like 0.1 or 0.2 become repeating decimals when converted to binary floating-point and are stored as approximations.

```js
console.log(0.1 + 0.2); // 예상: 0.3, 실제: 0.30000000000000004
```

2. **Loss of Precision**: Small errors during calculations can accumulate into significant errors. This is particularly problematic in iterative calculations or financial computations.

### Solutions

1. Using the `toFixed()` Method

The `toFixed()` method rounds a number to a specified number of decimal places and returns it as a string.

```js
let sum = 0.1 + 0.2;
console.log(sum.toFixed(2)); // "0.30"
```

**Note**: Since toFixed() returns a string, you'll need to convert it back to a number using parseFloat() or Number() if further calculations are required.

2. Calculations Considering Decimal Places

Convert all numbers to integers before performing calculations, then convert the result back to a floating-point number.

```js
let a = 0.1;
let b = 0.2;
let sum = (a * 100) + (b * 100); // 10 + 20 = 30
sum = sum / 100; // 0.3
console.log(sum); // 0.3
```

Pros: Avoids floating-point errors.

Cons: You need to manage scaling factors depending on the range of numbers you're processing.


3. Comparison Using `EPSILON`

When comparing two floating-point numbers for near equality, use `Number.EPSILON`.

```js
function areAlmostEqual(a, b) {
  return Math.abs(a - b) < Number.EPSILON;
}

console.log(areAlmostEqual(0.1 + 0.2, 0.3)); // true
```

4. Using External Libraries

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

Pros: Allows for very high-precision calculations.

Cons: Slower than using the basic number type, and the code can become more complex.



