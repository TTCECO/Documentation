
<h1 align="center">Building Blockchain for Billions</h1>
<p align="center" class="version">The First Open letter to TTC Community</p>

The distributed ledger technology, better known as Blockchain technology, is the most disruptive technology in a decade. It has the potential to change the world in ways not seen since the first days of the Internet. Yet, because the technology itself is at an early stage, it still has hurdles to overcome in order to support large-scale services.

This open letter is to share our goal, our strategic approach to surmounting these hurdles, and our future plans for the project. At the same time, we ask developer communities to join us and work together to develop Blockchain technology to create a better future. We will continue to share our thoughts and beliefs openly in the community.

- [Background](#background)
- [Consensus (BFT-DPoS)](#consensus-bft-dpos)
- [Account System](#account-system)
- [Smart Contract](#smart-contract)
- [Cross-chain](#cross-chain)
- [Multi-dimensional Data Storage](#multi-dimensional-data-storage)
- [Oracle](#oracle)
- [Ecosystem Toolkit](#ecosystem-toolkit)
- [Mobile Service Enabled Blockchain](#mobile-service-enabled-blockchain)
- [Anti-spam Strategy ](#anti-spam-strategy )
- [Conclusion](#conclusion)

## Background

TTC Protocol is a decentralized and token-incentivized protocol for social network services and online communities, seeking faster user growth. Every member in the TTC Ecosystem works for one aligned goal of creating a more efficient, transparent, and meritocratic network.

To achieve such goal, we are building a blockchain protocol with improved performance, scalability, robustness, latency and mobile adaptation for large-scale social networking platforms.

Ethereum with PoW consensus algorithm is not suitable for mainstream services due to its limited transaction speed. Hence, TTC Protocol utilizes BFT-DPoS consensus algorithm to support DAPPs with large user base.

The TTC Ecosystem is structurally composed of three major layers: persistence layer, domain layer, and service layer. Persistence layer has the account system, BFT-DPoS consensus mechanism, smart contracts and more. Domain layer consists of the cross-chain protocol suite, oracle protocol suite, data mapping and storage protocol suite based on the smart contract. Finally, the service layer of the platform consists of complete tripartite protocol, API and SDK, dashboard and related components. The service layer also provides access to developers, advertisers, and other related parties. The entire ecosystem realizes the process of information generation, consumption and transfer.

## Consensus (BFT-DPoS)

In a decade of development, the consensus algorithm - also known as the core of the blockchain technology - has evolved from a single PoW to multiple forms, such as PoS, DPoS and BFT. Each consensus algorithm has its pros and cons and a blockchain project must decide one consensus algorithm that is best suitable for its purpose and ideology.

We are building TTC Protocol to close the gap between blockchain technology and existing social networking applications used by billions of people everyday. BFT-DPoS consensus is implemented in TTC Protocol to enable large transaction processing capabilities and fast verification.

BFT-DPoS is a consensus algorithm built based on the DPoS consensus, where multiple super-node producers elected in real time can ensure the stability and efficiency of new blocks under fair premise. Its processing speed of a single chain can reach thousands of transactions per second without multi-chain parallelism or sharding processing mechanism. It also uses the BFT mechanism to increase the speed of confirmation for each transaction. Ideally, the time of confirmation is the same as generation of a new block, which improves the execution efficiency of the entire blockchain.

## Account System

There are two types of mainstream account systems in blockchain society: asset-oriented (bitcoin UTXO) and user-oriented (Ethernet). The TTC applies the user-oriented account system, which distinguishes external owner accounts and smart contract accounts. While Ethereum matches one private key to one address, TTC allows one private key to correspond with multiple addresses, and also supports the authority transfer of the address owner.

DAPPs in the TTC Ecosystem create and assign one default account for data storage and reward distribution to each user. Each user can request the authority transfer of this account from the address owner(a DAPP) and bind to his or her universal account in the TTC Ecosystem.

## Smart Contract

The TTC Virtual Machine (**TTVM**), a hard-forked version of Ethereum Virtual Machine (EVM), is the runtime environment for smart contracts with Turing completeness, high level of security, and high extensibility. TTVM has the following characteristics:   

- Provides compilation tools with a comprehensive security check mechanism

- Supports multiple languages, such as Python, JavaScript, Solidity and Go, to embrace more developer communities

- Improves the development efficiency by providing more standard libraries

- Provides developer-friendly IDE, online debugging and compilation environment

TTVM extends the functionality and use scenarios of smart contracts significantly by allowing smart contracts to access not only data on the blockchain but also external services and data modules to implement the mechanism of the oracle.

## Cross-chain

Cross-chain is one of the hot topics among current blockchain developer communities, as it maximizes the expandability and connectivity of blockchain. There are four major cross-chain protocols being widely used today: notary schemes, side chains/relays, hash-locking and distributed private key control. Each protocol is different in terms of interoperability, trust model and implementation difficulty. 

TTC Protocol uses cross-chain to achieve the flexible transfer of digital assets between multiple chains. More specifically, we chose a distributed private key control protocol, since it supports cross chain asset transfer and mortgage, Oracle, multi-token smart contracts, and it will not suffer from "51% attacks".

Initially, we mainly focuses on development of the cross-chain operation among the isomorphic chains. In the later stage, we will gradually expand the compatibility of cross-chain operation for nonisomorphic chains. 

In the TTC Ecosystem, we extended the concept of cross-chain so that it exists not only to run on the blockchain but also to implement specific requirements such as data exchange and sharing at the service layer. It is implemented based on the SSO solution and the distributed data exchange protocols.


## Multi-dimensional Data Storage

To embrace DAPPs with different data requirements, TTC Protocol provides a  range of data storage methods, multi-dimensional data storage framework and services.

For digital asset, privacy, copyright, and other related data, TTC Protocol provides data storage and numerous encryption methods using cryptographic modules. For behavior data, different storage methods, such as a distributed storage structure TTFS, can be selected according to the needs and scenarios. A relationship mapping of the corresponding data on TTC Protocol will be created at the point of data storage.

Each data in the TTC Ecosystem is tagged with a clear ownership. Data can be shared and transferred according to specific business scenarios. For example, an advertiser can build a smart contract, requesting an access to users’ information for better advertising strategies. When a user agrees, related information stored in the blockchain will be decrypted first, then encrypted with the advertiser’s public key before it is shared to the advertiser.


## Oracle

TTC Protocol supports blockchain oracle, so that DAPPs and smart contracts can interact with external trusted interfaces and data. An oracle is a trusted entity that uses the signature to feed information about the state of the uncertain external world to smart contracts. Smart contracts can access the local data verified by the oracle signature without pulling data from a third-party interface, which ensures both the access efficiency and the price consistency.

The TTC Ecosystem supports multi-node oracle solution to ensure reliability, stability and decentralization. A transaction that carries external data will be initiated to the blockchain only when half of the node data is consistent. The actual confirmation time of a transaction in an oracle is usually equal to a block that is produced, because of the BFT-DPoS consensus mechanism.


## Ecosystem Toolkit

TTC Ecosystem provides numerous features and toolkits for different members of TTC.


(1) For DAPP developers: 
A complete set of TTC test chain and corresponding cloud storage resource as well as detailed documents, use cases, multi-platform cross-language SDKs and APIs can be obtained from the developer community. There is no need for developers to deploy servers to use the test chain for developing and debugging. After completing the QA process, DAPPs can be connected to the main chain just by replacing the relevant address and application key. Also, there exists another project, “[git.eco](http://git.eco)”, which is a token incentivized collaboration community for developers. Participating contributors will get rewarded for creating issues, committing code, merging branches, forking repositories, and reviewing code.


(2) For DAPP operators: 
Statistic data of users can be accessed through the visualized charts in real time, as well as the amount of tokens acquired by users of the DAPP.


(3) For advertisers: 
Not only can they acquire advertising value in real time, they can also optimize the delivery strategy base on detailed user behavior data authorized by users.


(4) For users: 
A Single Sign-on Account (SSO) can be used to access all DAPPs in the TTC Ecosystem. A universal wallet controls all assets, enabling the cross-linked digital asset transfer. Also, users can authorize transferring, sharing, and other operations regarding information on multiple DAPPs with the same account. It is easy for users to earn and withdraw token rewards from various DAPPs.


## Mobile Service Enabled Blockchain

While most of the popular services/platforms are mobile-centric, the current blockchain technology is not ready for mobile applications due to issues with manipulation, stability, and connectivity. As a result, the blockchain-based services are not yet easily accessible to many internet users.

To ensure mobile services to operate issue free on a blockchain, TTC Protocol utilizes a dual side authorization solution.

## Anti-spam Strategy 

In order to create a fair environment for all DAPPs in the TTC Ecosystem, a multi-dimensional anti-spam strategy is implemented.

Multi-point detection verifies the authenticity of interest-related behavior in DAPPs. When a user behavior is triggered, SDK is invoked by the mobile client, to record on the smart contract. As the server is notified of such operation, corresponding smart contract is called to do relevant verification through the API interface.

The authenticity of the transaction is determined with machine learning algorithms based on user behaviors. It will affect the service such as value distribution through the blockchain oracle mechanism.



## Conclusion


Despite the fact that Blockchain technology has remaining limitations to be overcome regarding large scale services, it has the potential to achieve true social collaboration between, and within, communities. All through a transparent and efficient token economy. Understanding the problems that are required to be solved and improved, we believe our team and users of the TTC protocol will be the avant-garde of Blockchain innovation. We know each and every one’s contribution will not only make the TTC ecosystem better, but eventually make this world a better place.

We are very optimistic of the future that will be created by “us.” We welcome anyone and everyone who believes in the same idea of working together and developing Blockchain technology for the greater good of our future.


—— TTC Protocol



**TTC Protocol** is a decentralized, token-incentivized social networking protocol.

Telegram (English): http://t.me/ttc_en

Telegram (Korean): http://t.me/ttc_kr

Email: official@ttc.eco

Homepage : http://www.ttc.eco/

Facebook : http://www.facebook.com/ttceco

Twitter : http://www.twitter.com/ttceco
