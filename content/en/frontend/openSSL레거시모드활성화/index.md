---
title: 'How to Enable openSSL Legacy Mode'
author: "Hun Im"
date: 2024-10-30T14:26:25+09:00
category: ['POSTS']
tags: ['Javascript', 'Node.js']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'Node.js']
---

After upgrading Node.js, you may encounter errors when loading scripts.

If you’re seeing openSSL errors, try enabling legacy mode as follows:

```
export NODE_OPTIONS=--openssl-legacy-provider

"scripts": {
  "build": "NODE_OPTIONS=--openssl-legacy-provider webpack --mode production"
}
```
