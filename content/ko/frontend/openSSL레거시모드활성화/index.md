---
title: 'openSSL 레거시모드 활성화 방법'
author: "임훈"
date: 2024-10-30T14:26:25+09:00
category: ['POSTS']
tags: ['Javascript', 'Node.js']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Node.js']
---

node.js 버전업 이후에, 스크립트를 불러올 때, 에러가 뜰 때가 있다.

openSSL 에러가 날 때는, 레거시모드를 활성화 해보자.

```
export NODE_OPTIONS=--openssl-legacy-provider

"scripts": {
  "build": "NODE_OPTIONS=--openssl-legacy-provider webpack --mode production"
}
```
