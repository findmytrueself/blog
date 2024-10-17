---
title: 'Ethereum'
author: 'Hun Im'
date: 2024-09-11T17:30:13+09:00
category: ['POSTS']
tags: ['Blockchain', 'Ethereum']
og_image: "/images/gamer.png" 
keywords: ['Blockchain', 'Ethereum']
---
### What is Ethereum?

1. A blockchain-based platform used to run smart contracts and decentralized applications (dApps).

2. It offers various features beyond cryptocurrency transactions, including smart contract functionality.

3. The Ethereum network consists of numerous nodes, each responsible for maintaining and validating the Ethereum blockchain.

**Ethereum Virtual Machine (EVM)**

1. A virtualized computing environment that runs smart contracts within the Ethereum network.

2. The core component of smart contracts and decentralized applications.

3. Acts as a virtual computer that operates in a decentralized manner on the Ethereum network.

4. Smart contracts are written in the Solidity language, compiled into bytecode, and executed by the EVM.

**ERC**

ERC (Ethereum Request for Comments) is a process used to propose and define standards for smart contracts on the Ethereum blockchain.

1.	ERC-20:
* Description: The most widely used token standard, used to create fungible tokens on the Ethereum blockchain. ERC-20 tokens have the same value and properties, with each unit treated equally.
* Use Cases: Tokens used in ICOs (Initial Coin Offerings), utility tokens for various projects.
* Functions: Defines basic token transfer and approval functions such as `transfer`, `approve`, and `transferFrom`.

2.	ERC-721:
* Description: The standard for **non-fungible tokens (NFTs)**, where each token has unique properties and cannot be exchanged one-to-one with other tokens.
* Use Cases: Represents ownership of digital assets (e.g., CryptoKitties), art, and in-game items.
* Functions: Tracks and transfers ownership of specific tokens using functions like `ownerOf` and `transferFrom`.

3.	ERC-1155:
* Description: A multi-token standard that allows for the management of both fungible and non-fungible tokens within a single contract.
* Use Cases: Useful in games where various types of items (both regular and unique) need to be managed.
* Functions: Efficiently transfers and manages multiple types of tokens using functions like `safeTransferFrom` and `balanceOf`.

4.	ERC-777:
* Description: An improved token standard over ERC-20, offering a more flexible transfer mechanism and enhanced security features. It allows the use of custom hooks during token transfers.
* Use Cases: Used to overcome limitations of ERC-20 and in more complex smart contract systems.

5.	ERC-1400:
* Description: A standard for security tokens, providing stricter control over token ownership to meet legal requirements.
* Use Cases: Security tokens (STOs), financial assets where regulatory compliance is essential.