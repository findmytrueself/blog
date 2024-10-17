---
title: 'Valid Parentheses (Level 2)'
author: '임훈'
date: 2024-10-14T13:53:35+09:00
category: ['POSTS']
tags: ['Javascript', 'Algorithm']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Algorithm']
---

This exact problem appeared in a live coding test for the company I want to join.

I had solved this problem three years ago while preparing for coding tests as a new graduate, but I couldn't remember it since it's been a while. 

It was so frustrating that I couldn't solve it halfway through, so I decided to review it.

```js
function solution(s) {
    let stack = [];
    let obj = { '(': ')', '{':'}', '[':']' };
    
    for (let i = 0; i < s.length; i++) {
        if (Object.keys(obj).includes(s[i])) { 
            stack.push(s[i]); // Only push the left parentheses onto the stack.
        } else {
            let last = stack.pop(); // Pop the last item from the stack.
            if (s[i] !== obj[last]) { // If it doesn't match the correct closing bracket, return false.
                return false;
            }
        }
    }
    if (stack.length !== 0) { // If there's anything left in the stack, it means not all parentheses were closed, so return false.
        return false;
    } else {
        return true; // If the stack is empty, all parentheses were correctly matched, so return true.
    }
}
```