---
title: 'Developing a Blockchain Wallet Chrome Extension'
author: 'Hun Im'
date: 2023-03-21T13:30:13+09:00
category: ['POSTS']
tags: ['Javascript', 'Web3', 'Blockchain']
og_image: "/images/gamer.png" 
keywords: ['Blockchain', 'Javascript', 'Web3']
---
**September: Initial Setup and Code Analysis**

In September 2022, I was tasked with renewing an existing blockchain wallet that had already been deployed on Chrome.

When I first took on this project, I felt completely overwhelmed. The existing project was developed in Vue 2, and I had to modify it. Additionally, since Chrome follows the Manifest policy, I had to study that as well.

The Manifest v3 issue was there from the start, and since I was more familiar with React, I thought about rewriting the whole project from scratch. I attempted it, but I wasn’t confident enough to restructure everything from the ground up.

Thanks to my understanding of SPA structures, I was able to grasp the Vue 2 syntax and structure quickly, but Chrome development felt like a completely different domain compared to regular web development.

I didn't have enough resources, time, or skills to redevelop the already implemented features, so I reluctantly decided to refactor the existing code.

**October–November: Design Changes and Data Structure Modification**

The design was updated from the simple previous version to a more vibrant one, and the flow changed significantly as well.

Many of the features reused or slightly modified the existing code.

Since the frontend doesn't have a database, I used the browser's storage to save data and retrieve it when needed.

I refactored the system to fetch the data stored in Scan and display it to users. Any user-relevant data was stored in the storage for easy access.

All the states displayed in the view layer were managed in the Vuex store.

For asynchronous tasks, I used `action` `dispatch` and `mutations` `commit` methods to manage the state.

`getters` and `computed` properties were used to monitor state changes.

**December–January: Manifest Issue and Cause Identification**

It had already been announced that, for security reasons, Google Chrome would require an upgrade to Manifest v3 by 2023.

Even though we were aware of this issue, we continued developing, but it turned out to be a bigger problem than we expected.

A serious issue arose: the entire background logic had to be rewritten.

The main reason why upgrading the manifest version was difficult was due to the blockchain SDK we were referencing. The SDK threw an unknown error that prevented the background script from being read.

I reached out to the original foreign developer for help, and after the SDK was fixed, we were able to upgrade the Manifest version.

**February: Background Logic Modification**

With the Manifest version upgraded, the structure of the chrome.runtime object changed, and the background logic switched from running as an HTML page to a serviceworker.js file, which required significant changes.

Previously, the background script always stayed running, but due to security concerns, the new version automatically shut down background.js every 295 seconds. As a result, a logic to keep the background running was needed.

After extensive research, I found a way to keep the background alive and applied it.

[Persistent Service Worker in Chrome Extension](https://stackoverflow.com/questions/66618136/persistent-service-worker-in-chrome-extension)

**March: Preparing for Chrome Store Deployment and Bug Fixes**

It's time to focus on polishing the product details. By eliminating redundant logic from the previous implementation, I improved the stability of the extension.

While testing with the QA team, we fixed numerous small bugs and refined the details. Now, we are preparing for the official release.
