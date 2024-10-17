---
title: 'Normal Transactions and Internal Transactions'
author: 'Hun Im'
date: 2024-10-17T17:30:13+09:00
category: ['POSTS']
tags: ['Blockchain', 'Ethereum']
og_image: "/images/gamer.png" 
keywords: ['Blockchain', 'Ethereum']
---

## What is a Normal Transaction?

A typical transaction on the Ethereum blockchain refers to the simple transfer of Ether (ETH) or tokens between two wallet addresses.

These transactions are initiated by users and are directly recorded on the blockchain, making them easy to track.

### Characteristics

* User-Initiated: A normal transaction is typically initiated by an individual or entity sending ETH or tokens from one address to another.

* Recorded on the Blockchain: All normal transactions are permanently recorded on the Ethereum blockchain, providing a transparent and immutable record.

* Transaction Fees: Users must pay gas fees to miners to process and confirm the transaction. The fees vary depending on network congestion and transaction complexity.

* Simple Structure: A normal transaction includes basic information like the sender's and receiver's addresses, the amount transferred, and the gas limit.

* Visibility: These transactions can be viewed by anyone on Etherscan, allowing users to track transfers and check balances.

### How Do Normal Transactions Work?

* Transaction Creation: The user specifies the recipient's address and the amount of ETH or tokens to send in a wallet app.

* Transaction Signing: The transaction is signed with the sender's private key to verify authenticity.

* Transaction Broadcasting: The signed transaction is sent to the Ethereum network for validation.

* Transaction Pool: The transaction enters the mempool, where miners select transactions to include in the next block.

* Mining and Confirmation: Miners solve a cryptographic puzzle and add the block containing the transaction to the blockchain.

* Finality: Once confirmed, the transaction is recorded on the blockchain, and the recipient's balance is updated.

* Gas Fees: The sender pays gas fees based on the complexity of the transaction and network congestion.

### Use Cases for Normal Transactions

* P2P Transfers: Sending ETH or tokens between individuals.

* Payments for Goods and Services: Making purchases using cryptocurrency.

* Funding Wallets: Transferring assets to another wallet for trading or investment purposes.

### How to View a Normal Transaction?

1. Visit Etherscan: Go to etherscan.io.

2. Search for the address: Enter the wallet address in the search bar.

3. Access the address page: Click the address from the search results.

4. Navigate to the "Transactions" tab: Click the "Transactions" tab to see a list of all normal transactions.

5. Review details: Each transaction shows the transaction hash, sender and receiver addresses, value, gas fees, and timestamp.

## What is an Internal Transaction?

Internal transactions, also known as "message calls" or "contract interactions," occur within smart contracts on the Ethereum blockchain.

Unlike regular transactions that involve direct transfers of Ether or tokens between wallet addresses, internal transactions result from executing code within smart contracts.

### Characteristics

* Triggered by smart contracts: Internal transactions are created when a smart contract calls another smart contract or sends Ether during its execution.

* Not directly recorded on the blockchain: While internal transactions can be traced back to the point where the original regular transaction was initiated, they don't have their own transaction hash and are not explicitly recorded on the blockchain like regular transactions.

* Complex logic: Internal transactions often involve complex logic, such as token transfers, multi-signature operations, or automated functions triggered by contract conditions.

* Visibility on Etherscan: Although not shown in the main transaction list, internal transactions can be viewed in the "Internal Transactions" tab of a specific contract or wallet on Etherscan.

* Use Cases: Internal transactions are typically used in decentralized applications (dApps) and decentralized finance (DeFi) protocols to execute complex contract interactions.

### How Do Internal Transactions Work?

* Event Trigger: Internal transactions are initiated when a smart contract is executed, usually in response to a regular transaction.

* Contract Interaction: During execution, the smart contract may call another contract or send Ether as part of its logic.

* No Separate Transaction Hash: Internal transactions don’t have unique transaction hashes and are tied to the original regular transaction.

* Execution Logic: Internal transfers happen as part of the smart contract code, which may involve complex tasks such as token transfers or multi-signature approvals.

* Visibility: While not visible in the main transaction list, internal transactions can be found in the "Internal Transactions" tab on Etherscan for the associated contract or wallet.

### Use Cases for Internal Transactions

* Decentralized Finance (DeFi): Automating lending, borrowing, and liquidity provisioning within protocols.

* Token Swaps: Managing asset transfers between liquidity pools on decentralized exchanges (DEXs).

* Automated Market Makers (AMMs): Adjusting token prices based on supply and demand through internal movements.

* Crowdfunding and ICOs: Handling fund allocation and token distribution transparently during fundraising events.

* Gaming and NFTs: Enabling the seamless transfer of in-game assets and NFTs between players.

### How to View Internal Transactions?

1. Visit Etherscan: Go to etherscan.io.

2. Search for an Address or Transaction: Enter a wallet address or transaction hash in the search bar.

3. Access the Address or Transaction Page: Click the relevant result to open the dedicated page.

4. Go to the "Internal Transactions" Tab: Find the “Internal Transactions” tab on the address or transaction page.

5. Review Details: View the list of internal transactions, including the sender, receiver, value, and the associated regular transaction.

## Normal Transactions vs. Internal Transactions

The following table highlights the differences between regular and internal transactions on Etherscan.

| Category         | Normal Transactions                                       | Internal Transactions                                           |
| ------------ | ----------------------------------------------- | --------------------------------------------------- |
| **Definition**     | Direct transfers of ETH or tokens between wallets.             | Transfers occurring within a smart contract’s execution.                    |
| **Initiation**     | Triggered by user actions (e.g., sending ETH).  | Triggered by smart contract execution or interaction.       |
| **Visibility**     | Easily visible on the blockchain and Etherscan.     | Not recorded as a separate entry; visible in a special tab. |
| **Transaction Hash**| Each transaction has a unique hash.        | No unique transaction hash, linked to the initial transaction. |
| **Gas Fee**| The sender must pay a gas fee.         | Usually included in the initial transaction’s gas fee.        |
| **Use Cases**| Peer-to-peer transfers, payments, and token transfers. | DeFi operations, token swaps, game transactions, contract interactions.    |
| **Complexity**   | Generally simple.                         | Can involve complex logic and automated processes.   |
| **Recording**     | Directly recorded on the Ethereum blockchain.             | Recorded in the context of smart contract execution.             |

*Reference*

<https://www.geeksforgeeks.org/normal-transactions-vs-internal-transactions-in-etherscan/>