---
title: 'Number Strings and Words (Level 1)'
author: '임훈'
date: 2024-10-16T01:40:04+09:00
category: ['POSTS']
tags: ['Algorithm', 'Javascript']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---

```js
function solution(s) {
    const obj = {'zero':0, 'one':1, 'two':2, 'three':3, 'four':4, 'five':5, 'six':6, 'seven':7, 'eight':8, 'nine':9}
    let stringNum = ''
    let answer = ''
    for(let i=0; i<s.length; i++){
        let temp = +s[i]
        if(isNaN(temp)){
            stringNum += s[i]
              if(obj[stringNum] || obj[stringNum] === 0){ // A counterexample is 'one0zero0'. Since 0 is a falsy value, you need to explicitly handle the value to ensure accuracy. 
            answer += obj[stringNum]
            stringNum = ''
            }   
        } else {
            answer += s[i]
        }
        
    }
    return +answer
}
```


