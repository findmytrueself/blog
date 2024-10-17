---
title: 'Consensus Algorithms'
author: 'Hun Im'
date: 2024-09-12T13:30:13+09:00
category: ['POSTS']
tags: ['Blockchain']
og_image: "/images/gamer.png" 
keywords: ['Blockchain']
---
### Types of Major Consensus Algorithms

1. Proof of Work (PoW)
* Description: A method where network participants compete to solve very complex mathematical problems to create blocks. The node that solves the problem first earns the right to add a new block.
* Examples: Bitcoin, Ethereum (pre-2.0).
* Pros: High security and operates in a decentralized network without centralized authority.
* Cons: Extremely high energy consumption and slow transaction speeds.

2. Proof of Stake (PoS)
* Description: A method where nodes obtain the right to create blocks based on the amount of cryptocurrency they hold (stake). The more stake, the higher the chance to add a new block.
* Examples: Ethereum (2.0), Cardano, Polkadot.
* Pros: Energy efficient and faster block generation compared to PoW.
* Cons: Potential centralization by nodes with large stakes.

3. Delegated Proof of Stake (DPoS)
* Description: Users elect a set number of delegates who are responsible for creating blocks. These representatives are chosen by network participants through voting, and they perform block verification and generation tasks.
* Examples: EOS, Tron, Steem.
* Pros: Faster transaction speeds than PoS and energy efficiency.
* Cons: Risk of centralization due to reliance on a small number of elected nodes.

4. Proof of Authority (PoA)
* Description: A small, trusted group of validators are responsible for creating blocks. These validators must be verified as trustworthy within the network and are responsible for adding blocks.
* Examples: VeChain, certain private blockchains.
* Pros: Very fast and operates efficiently in a trusted environment.
* Cons: High likelihood of centralization and reliance on a small group of validators.

5.	Delayed Proof of Work (dPoW)
* Description: Provides an additional security layer by using the hash power of an existing PoW blockchain to secure a secondary chain. The secondary chain leverages the security of the primary PoW chain.
* Example: Komodo.
* Pros: Utilizes the security of existing PoW blockchains while increasing efficiency.
* Cons: Dependency on an existing PoW chain.

6.	Byzantine Fault Tolerance (BFT)
* Description: A method to address the Byzantine Generals Problem, allowing the system to continue functioning correctly even if some nodes act maliciously or fail to respond.
* Examples: Hyperledger, Tendermint.
* Pros: Provides high security and ensures reliable outcomes even with untrustworthy nodes.
* Cons: More complex than PoW or PoS and may slow down as the network grows.

7.	Practical Byzantine Fault Tolerance (PBFT)
* Example: Hyperledger Fabric.
* Features: Requires more than half of the network nodes to reach the same state for consensus.

8. QBFT (Quorum-based Byzantine Fault Tolerance)
* Description: QBFT is a consensus algorithm in the BFT family that requires the approval of more than half of the nodes in the network to reach consensus. Developed to maintain both security and efficiency while tolerating Byzantine faults, it is an evolution of the PBFT algorithm.
* Example: Used in Hyperledger Besu, it is known to be well-suited for enterprise blockchains.
* Pros: Even if malicious nodes are present, the network can securely reach consensus as long as there is a majority of trustworthy nodes. It also does not consume as much energy as PoW and can process transactions quickly.
* Cons: As the network size grows, communication costs for consensus increase, which may degrade performance. The larger the number of nodes, the slower the network becomes.

QBFT is primarily used in enterprise blockchain environments, where it balances Byzantine fault tolerance with performance and scalability.

9.	Hybrid Consensus Algorithm
* Example: Decred.
* Features: Combines two or more consensus algorithms to maximize stability and efficiency.

10. Raft Algorithm
* Description: Raft operates as a leader-based consensus algorithm in distributed systems to maintain consistency across multiple nodes. The leader handles client requests and replicates logs to follower nodes to maintain consistency. If the leader fails, a new leader is elected through voting.
* Examples: Etcd, Consul.
* Pros: Simple structure, easy to understand and implement, and fast, stable leader election and log replication.
* Cons: Since the structure relies on the leader, frequent leader changes may degrade performance, and recovery time is needed if the leader fails.

11. Tendermint:
* Example: Cosmos.
* Features: A BFT-based algorithm that achieves fast and stable consensus. Often combined with PoS for use.

### Other Consensus Algorithms

* Federated Byzantine Agreement (FBA): Used by Ripple and Stellar.
* Leased Proof of Stake (LPoS): Used by Waves.
* Proof of Burn (PoB): A method where a certain amount of cryptocurrency is burned to gain the right to create blocks.