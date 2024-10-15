---
title: '올바른 괄호 (LV2)'
author: '임훈'
date: 2024-10-14T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'Algorithm']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---

가고 싶은 회사 라이브 코딩테스트에서 이 문제가 그대로 나왔다. 

3년 전, 신입 준비 할 때 코딩테스트 준비 하면서 분명히 풀어봤던 문젠데... 오래 되어서 기억이 나질 않았다. 

중간에 못푼게, 너무 아쉬웠다. 그래서 복기를 해보는 중이다.

```js
function solution(s) {
    let stack = [];
    let obj = { '(': ')', '{':'}', '[':']' };
    
    for (let i = 0; i < s.length; i++) {
        if (Object.keys(obj).includes(s[i])) { 
            stack.push(s[i]); // 좌측의 괄호만 stack에 넣는다.
        } else {
            let last = stack.pop(); // stack 배열의 가장 마지막을 빼서
            if (s[i] !== obj[last]) { // 올바른 괄호에 맞춰보고, 아니면 false
                return false;
            }
        }
    }
    if (stack.length !== 0) { // stack에 어떤 값이 남아 있다면, 괄호가 모두 닫히지 않았다는 의미가 된다. 고로, false
        return false;
    } else {
        return true; // stack이 모두 제거가 되었다는 뜻은, 괄호가 모두 올바르게 닫혔다는 의미, 고로 true
    }
}
```