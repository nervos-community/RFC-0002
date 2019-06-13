# Nervos CKB: A Common Knowledge Base for Crypto-Economy 加密经济的共同知识库

## **Abstract**
Nervos is a layered crypto-economy network. Nervos separates the infrastructure of a crypto-economy into two layers: a verification layer (layer 1) that serves as a trust root and smart custodian, and a generation layer (layer 2) for high-performance transactions and privacy protection.
This document provides an overview of the Nervos Common Knowledge Base (CKB), a public permissionless blockchain and layer 1 of Nervos. CKB generates trust and extends this trust to upper layers, making Nervos a trust network. It's also the value store of the Nervos network, providing public, secure and censorship-resistant custody services for assets, identities and other common knowledge created in the network.
  <br />Nervos 是一个采用分层架构的加密经济网络，它将加密经济的基础设施分为两层：验证层 (Layer1) 是信任之锚，提供智能托管服务；生成层(Layer2) 提供高性能的交易以及隐私保护。
本文概述了 Nervos 的 Layer 1 —— Nervos Common Knowledge Base(CKB) 。CKB 是公有非许可链，它能创造信任，并将信任传递到上层，使整个 Nervos 网络可信。CKB 也是 Nervos 网络的价值存储层，为在网络中创建的资产、身份和其他公共知识提供公共、安全、不受审查的托管服务。

## **Contents 内容**
1.	Motivation 动机
2.	Overview 概述
3.	Consensus 共识
4.	Programming Model 编程模型
  <br />-State Generation and Verification 状态生成与验证
  <br />-Cell Cell模型
  <br />-VM 虚拟机
  <br />-Transaction 交易
5.	Economic Model 经济模型
6.	Network 网络
7.	Summary 总结
8.	References 文献
9.	Appendix 附录


## **1.Motivation 动机**

We want a peer-to-peer crypto-economy network.
In such a network, people can not only collaborate but also have incentives to do so. We need the ability to define, issue, transfer, and own assets in a peer-to-peer network to create such incentives. Blockchain technology brings us the last piece of the puzzle.
Bitcoin[1] was the first public permissionless blockchain, designed to be used solely as peer-to-peer cash. Ethereum[2] extends the use case of blockchain to create a general purpose trust computing platform on which people have built all kinds of decentralized applications. The booming applications on the Bitcoin and Ethereum networks have proven the concept of the future crypto-economy. However, these networks also suffer from the notorious scalability problem, their transaction processing capability cannot scale with the number of participants in the network, which severely limits their potential.
  <br />世界需要一个点对点加密经济网络。
这个网络不仅要允许人们在上面协作，更要能激励人们协作。为了创造这样的激励，我们必须要能在这个点对点网络中定义、发行、转移和拥有属于自己的资产。而区块链技术是我们得以实现这种网络的最后一块拼图。
比特币是第一种公有非许可链，其设计初衷是作为点对点现金。以太坊拓宽了区块链的使用场景，创造了一个能够创建各种去中心化应用的通用可信计算平台。在比特币和以太坊网络中激增的各种应用，已经证明了加密经济这个未来的概念。然而，这些网络受制于声名狼藉的可扩展性问题，它们的交易处理能力并不会随着节点数量的增加而提高，这严重限制了它们的应用潜力。


The blockchain community has proposed many scalability solutions in recent years. In general, we can divide these solutions into two categories, on-chain scaling and off-chain scaling. On-chain scaling solutions are those that try to scale at the same layer where consensus runs. The consensus process is the core of a blockchain protocol, in which nodes exchange network messages and reach agreement eventually. A consensus is slow almost by definition, because message exchange on a public and open network is slow and uncertain, nodes must wait and retry to reach agreement in the consensus process. 
  <br />近年来，区块链社区已经提出了多种扩容方案。通常我们可以将这些方案分为两类，链上扩容和链下扩容。链上扩容方案试图在共识层扩容。共识机制是区块链协议的核心，节点通过网络相互传递消息，并最终达成共识。然而，共识几乎是缓慢的代名词，因为在公开的网络中，消息的传递是缓慢且不确定的，节点必须等待和重试以达成共识。


To scale at this layer, we can either "scale up" by increasing the processing ability and network bandwidth of nodes (but sacrifice decentralization due to high hardware and infrastructure costs), or "scale out" by sharding. The idea of sharding is to divide nodes into many small "shards", and ask each shard to process only a fraction of network transactions. Sharding is widely adopted by Internet giants, as they face the same scalability issues when serving millions of users. However, sharding is well known for the complexity of shard coordination and cross-shard transactions, which even in a trusted environment, leads to performance degradation as the number of shards grows.
  <br />为了在这一层实现扩容，我们要么提高节点的处理能力和网络带宽(但是这种方式必然会提高硬件和基础设施的成本，降低去中心化)，要么进行分片。分片技术是将节点分散到很多个小分片中，每个分片只需要处理一部分交易。现在，分片的概念已经被众多互联网巨头广泛使用，因为它们在为数百万用户服务时，面临着相同的可扩展性问题。然而，分片技术以分片协作和跨片交易的复杂性著称，即使在可信环境中，分片数量的增加也会导致分片的性能下降。


In contrast, off-chain scaling solutions acknowledge the inherent complexity of the consensus process. They recognize that consensus within different scopes incur different costs, and the global consensus created by a public permissionless blockchain is the most expensive consensus. While it is hard to scale a global consensus, we can use it wisely. Most transactions between two or more parties don’t need to be known by every node in the network, except when they are securely settled; in other words, when users want to turn their transactions into common knowledge of the network. This network scales by offloading most of the work to upper layers, with no limit on scalability. Processing transactions off-chain also brings additional benefits, such as lower latency and higher privacy.
  <br />相反，链下扩容方案承认共识过程具有固有的复杂性。他们认识到不同范围的共识需要承担不同的成本，而一个公有非许可链所创造的全球共识是最昂贵的共识。虽然全球共识很难扩容，但我们可以更聪明地用它。像大多数两方或多方之间的交易，除非需要安全地固化（即作为网络中的公共知识），否则无需让网络中的每个节点都知道。使用链下扩容方案，网络会将大部分的工作转移到上层，所以没有可扩展性方面的制约。而且链下交易还能带来额外的好处，比如低延迟和高隐私性。


While we agree with the general ideas of off-chain scaling, we have found that there is no existing blockchain designed for it. For example, though the lightning network is one of the earliest explorations in off-chain scaling, it has taken years to launch its testnet and is still far from mass-adoption due to the limitations of the underlying Bitcoin protocol. Ethereum provides powerful programming ability, but its computation-oriented economic model doesn’t fit well with off-chain scaling. Because off-chain participants handle most of the computation, what is required is a blockchain that can keep their assets in secure custody and move assets according to the final state of their computation. The computation-oriented design of Ethereum also makes it difficult to execute transactions in parallel, which is an impediment to scalability.
  <br />尽管大家都认同链下扩容的思想，但我们发现目前还没有为链下扩容量身定制的区块链项目。例如，虽然闪电网络是最早的链下扩容方案，并且经过了数年的努力已发布测试网，但其受制于比特币的底层协议，离大规模的使用还很遥远。以太坊具有强大的可编程能力，但其受制于面向计算的经济模型，并不适合链下扩容。这是因为链下参与者要执行大部分的计算，并要求链上可以安全地托管资产，并根据计算得出的最终状态转移资产。而以太坊的面向计算的设计，使交易很难并发执行，这成了扩容的瓶颈。


The economic models of current blockchains also face challenges. With more users and applications moving to blockchain platforms, the amount of data stored on blockchains also increases. Current blockchain solutions are concerned more with the cost of consensus and computation, and allow a user to pay once and have their data occupy full nodes’ storage forever. Cryptocurrency prices also are highly volatile, and users may find it difficult to pay high transaction fees as the price of a cryptocurrency increases.
We propose Nervos CKB, a public permissionless blockchain designed for a layered crypto-economy network.
  <br />此外，现有的区块链项目还需要面对经济模型上的挑战。随着越来越多的用户和应用迁移到区块链平台，储存在区块链上的数据也会急剧增加。而现有的区块链方案更多地关注共识和计算的成本，使得用户可以只付一次费用就能永远占用全节点的存储空间。此外，加密货币的价格也非常不稳定，当加密货币价格上涨时，用户可能难以承担高额的交易费。
为了解决这些问题，我们提出了 Nervos CKB，一个为分层的加密经济网络而设计的公有非许可链。


## **2.Overview 概覽**
Nervos CKB (Common Knowledge Base) is a layer 1 blockchain, a decentralized and secure layer that provides common knowledge custody for the network. Common knowledge refers to states that are verified by global consensus. Crypto-assets are an example of common knowledge.
  <br />Nervos CKB (Common Knowledge Base，公共知识库) 是一条 layer1 的区块链，它是为网络提供公共知识托管的去中心化的安全层。这里的公共知识是指通过全球共识验证的状态。比如，加密资产就是一种公共知识。


In Nervos, the CKB and all layer 2 protocols work together to serve the crypto-economy. CKB (or layer 1) is where state is stored and defined, and layer 2 is the generation layer (or computation layer, these two terms are interchangeable) that processes most transactions and generates new states. Layer 2 participants submit newly generated states to the CKB eventually at the time they deem necessary. If those states pass the corresponding verification performed by nodes in a global network, the CKB stores them in a peer-to-peer node securely.
  <br />在 Nervos 中，CKB 和所有的 layer2 协议共同协作，一起为加密经济服务。CKB (或者说 layer1 )是定义和存储状态的地方，而 layer2 (生成层，或者说计算层，这两个名词可互换) 则是处理大多数交易以及生成新状态的地方。Layer2 的参与者最终会在必要时，提交一些新生成的状态到 CKB 上。如果这些状态通过了全节点的验证，则 CKB 会将它们安全地存储到全节点中。

The layered architecture separates state and computation, providing each layer more flexibility and scalability. For example, blockchains on the generation layer (layer 2) may use different consensus algorithms. CKB is the lowest layer with the broadest consensus and provides the most secure consensus in the Nervos network. However, different applications might prefer different consensus scopes and forcing all applications to use CKB’s consensus would be inefficient. Applications can choose the appropriate generation methods based on their particular needs. The only time these applications will need to submit states to CKB for broader agreement is when they need to make these states common knowledge that has been verified by the CKB’s global consensus.
  <br />分层架构将状态和计算分离，从而赋予了每一层更大的灵活性和可扩展性。例如，在生成层(layer2)上，不同的链可以采用不同的共识算法。CKB 是 Nervos 网络的最底层，提供最广泛和最安全的共识。然而，不同的应用对共识范围会有不同的偏好，强制所有应用都使用 CKB 共识是非常低效的选择。应用可以根据它们的实际需求选择合适的状态生成方法。只有当状态需要作为公共知识时，应用才必须提交状态给 CKB 以获得全球共识的验证。


Possible state generation methods include (but are not limited to) the following:
  <br />状态生成方法，包含但不限于以下几种
* 	Local generators on the client: Generators run directly on the client’s devices. Developers can implement the generator in any programming language.
* 	本地客户端的状态生成器：用户可以直接运行客户端生成新状态。开发者可以使用任何一种编程语言来实现这样的生成器。

* 	Web services: Users may use traditional web services to generate new states. All current web services may work with CKB in this way to gain more trust and liquidity for the generated states. For example, game companies may define in-game items as assets in CKB, the game itself functions as a web service that generates game data, which is then verified and stored in CKB.
* 	网络服务：用户可能会使用传统的网络服务来生成新状态。现有的所有网络服务都可以与 CKB 协作产生状态，以获得更多的信任和更好的流动性。例如，游戏公司可以将游戏中的物品定义为 CKB 中的资产，而游戏本身提供产生游戏数据的网络服务，这些游戏数据会在 CKB 中进行验证和存储。

* 	State channels: Two or more users may use peer-to-peer communication to generate new states.
* 	状态通道：两方或者多方用户，可能会通过点对点的通信过程来生成新状态。

* 	Generation chains: A generation chain is a blockchain that generates new states and stores them in CKB. Generation chains may be permissionless blockchains or permissioned blockchains. In each generation chain, nodes reach consensus in smaller scopes, providing better privacy and performance.
* 	生成链：生成链是一条可以生成并存储状态到 CKB 的区块链。生成链既可以是非许可链也可以是许可链。在每条生成链中，节点可以在更小范围内达成共识，并能提供更好的隐私和性能。
 
![](https://i.imgur.com/CmjICLP.png)
 Figure 1. Layered Architecture

CKB consists of a Proof-of-Work based consensus, a RISC-V instruction set based virtual machine, a state model based on cells, a state-oriented economic model, and a peer-to-peer network. The Proof-of-Work based consensus makes the CKB a public and censorship-resistant service. The combination of CKB VM and the Cell model creates a stateful Turing-complete programming model for developers, making state generation (or layer 2) on CKB practical. The CKB economic model is designed for common knowledge custody and long-term sustainability. The CKB peer-to-peer network provides secure and optimal communication between different types of nodes.
  <br />CKB 是由基于工作量证明( POW )的共识机制，基于 RISC-V 指令系统的虚拟机，基于 Cells 的状态模型，面向状态的经济模型，以及点对点网络组成。基于工作量证明的共识机制使得 CKB 能提供公共的抗审查服务。CKB 虚拟机与 Cell模型的组合，为开发者提供了一个带状态的图灵完备的编程模型。CKB 的经济模型是为公共知识的托管和长远可持续而设计的。CKB 的点对点网络为不同类型的节点提供了安全和最优的通信服务。

## **3.Consensus 共识**
CKB consensus is an improved Nakamoto consensus based on Proof-of-Work, that aims to achieve openness, correctness and high performance in distributed environments with network delay and Byzantine node faults.
Permissionless blockchains run in open networks where nodes can join and exit freely, with no liveness assumptions. These are severe problems for traditional BFT consensus algorithms to solve. Satoshi Nakamoto introduced economic incentives and probabilistic consensus to solve these problems. Nakamoto consensus in Bitcoin uses blocks as votes, which takes longer (up to 10 minutes to an hour) to confirm transactions and leads to an inferior user experience.
  <br />CKB 的共识机制是一个基于工作量证明的改良版中本聪共识机制，其设计目标是，在具有网络延迟和拜占庭节点的分布式环境中，达到开放性，正确性以及高性能。
非许可链运行在开放的网络中，节点可以自由地加入与退出，并且没有活性假设。这些是传统的拜占庭共识算法很难解决的问题。中本聪通过引入经济激励以及概率性共识解决了这个问题。在比特币的中本聪共识中，区块扮演了投票的角色，这导致交易确认时间很长(10 分钟到 1 小时)，因此用户体验较差。

CKB consensus is a Nakamoto consensus variant, which means it allows nodes to join and exit the network freely. Every node can participate in the consensus process either by mining (running a specific algorithm to find the Proof-of-Work) to produce new blocks, or by verifying new blocks are valid. CKB uses an ASIC-neutral Proof-of-Work function, with the goals of distributing tokens as evenly as possible and making the network as secure as possible.
Correctness includes eventual consistency, availability, and fairness. Eventual consistency guarantees every node sees an identical copy of state. Availability makes sure the network responds to users’ requests within a reasonable time. Fairness ensures mining nodes get fair returns for their efforts to keep the network functioning securely.
  <br />CKB 共识是一个基于中本聪共识的变种，这意味着它也允许节点能自由加入与退出。每个节点既可以通过挖矿 (运行一个特定的算法来生成工作量证明) 产生新区块，也可以通过验证区块的有效性参与共识。CKB 使用了对 ASIC 中立的工作量证明算法，以确保尽可能均匀地分发代币和维护网络安全。
正确性包括最终一致性，可用性和公平性。最终一致性确保每个节点都能看到完全相同的状态副本。可用性确保网络可以在合理的时间内响应用户的请求。公平性确保矿工的投入可以得到公平的回报，因而愿意维护网络的安全。

High performance includes transaction latency, the time between the submission of a request and the confirmation of its execution results, and transaction throughput, the number of transactions the system is capable of processing per second. Both of these measures depend on block time, which is the average time between two consecutive blocks.
Please check the CKB Consensus Paper for more details.
  <br />高性能包括交易延迟，即从提交请求到确认执行结果的时间间隔，还有交易吞吐量，也就是每秒整个系统能够处理的交易数量。这些指标的大小依赖于区块时间（连续两个区块的平均出块时间间隔）的设置。
如果想知道更多细节请参阅CKB的共识白皮书。

## **4.Programming Model 编程模型**
CKB provides a stateful Turing-complete programming model based on CKB VM and cell model.
  <br />CKB 提供了基于 CKB 虚拟机和 Cell 模型的带状态的图灵完备的编程模型。
	

| | Bitcoin | Ethereum | CKB |
|-|---------|----------|------------|
|Instruction Set|Script|EVM|RISC-V|
|Cryptographic Primitive|Opcode|Precompile|Assembly|
|Stateful|No|Yes|Yes|
|State Type|Ledger|General|General|
|State Model|UTXO|Account|Cell|
|State Verification|On-chain|On-chain|On-chain|
|State Generation|Off-chain|On-chain|Off-chain|

The CKB programming model consists of three parts:
* state generation (off-chain) 
* state verification (CKB VM)
* state storage (Cell model)

在 CKB 编程模型中包含以下三个部分
* 状态生成(链下)
* 状态验证(CKB 虚拟机)
* 状态存储(Cell 模型)

In this model, decentralized application logic is split into two parts (generation and verification), running in different places. State generation logic runs off-chain on the client side; new states are packaged into transactions and broadcasted to the entire network. CKB transactions have an inputs/outputs based structure like Bitcoin. Transaction inputs are references to previous outputs, along with proofs to unlock them. The client includes generated new states as transaction outputs, which are called cells in CKB. Cells are the primary state storage units in CKB and are assets owned by users that must follow associated application logic specified by scripts. CKB VM executes these scripts and verifies proofs included in inputs to make sure the user is permitted to use referenced cells and the state transition is valid under specified application logic. In this way, all nodes in the network verify that new states are valid and keep these states in custody.
  <br />这个模型将去中心化应用的逻辑分成了两部分(生成和验证)，并且这两部分逻辑在不同的地方执行。状态生成的逻辑在链下的客户端执行；新的状态被打包到交易中并广播到全网。CKB 交易与比特币类似，也是基于输入/输出的结构。一笔交易输入包括对一笔交易输出的引用，和一个能解锁该交易输出的证明。在客户端生成的新状态就是交易输出，在 CKB 中也被称作 Cell。Cell 是 CKB 中最基本的状态存储单元，也是用户所拥有的资产，它必须遵循特定脚本的相关应用逻辑。CKB 虚拟机执行交易输入内的证明以验证用户有权使用被引用的 Cell；执行 Cell 引用的脚本以验证状态转换符合特定的应用逻辑。通过这种方式，网络中的所有节点都能验证和托管新生成的状态。

State in CKB is a first-class citizen, states are included in transactions and blocks and synchronized directly among nodes. Although the programming model is stateful, scripts running in CKB VM are pure functions with no internal state, which makes CKB scripts deterministic, conducive to parallel execution, and easy to compose.
  <br />状态在 CKB 中是一等公民，状态被包含在交易和区块中，并且可直接在节点间同步。虽然编程模型是带状态的，但是运行在 CKB 虚拟机中的脚本却是不带状态的，这可以让 CKB 脚本具有确定性的执行结果，有利于并行执行，并且方便被其他脚本调用。


### **4.1 State Generation and Verification 状态生成与验证**

Decentralized applications on Nervos separate the generation and verification of state. While these processes occur in different places, CKB provides the additional flexibility to utilize different algorithms for state generation and verification.
  <br />在 Nervos 的去中心化应用中，状态的生成和验证是分离的。由于两个过程是在不同的地方执行，因此 CKB 获得了额外的灵活性，即状态的生成和验证可以使用不同的算法实现。

Utilizing the same algorithm on both generation and verification sides is a straightforward choice that works for general problems. In this model, the same algorithm has two implementations, one that runs off-chain in any execution environment targeted by the application, and the other one runs on-chain in CKB VM. New states are generated off-chain with this algorithm (based on previous states and user inputs), packaged as a transaction, and then broadcasted to the network. CKB nodes run this same algorithm on-chain, provide it the same previous states and user inputs, and then verify the result matches the transaction-specified outputs.
  <br />使用相同的算法生成和验证状态，是处理通用问题的最直接的选择。而在我们的模型中，同样的算法可以有两套实现，一套运行在链下，执行环境因应用而定，另外一套则运行在链上的 CKB 虚拟机中。链下通过算法生成的新状态(基于先前的状态和用户输入)，被封装成一个交易，并被广播到网络中，CKB 节点会在链上执行同样的算法（不同的实现），输入同样的前置状态和用户输入，并验证计算结果是否与交易输出吻合。

There are several advantages to this separation of state generation and validation:
  <br />状态生成与验证分离有以下几个优点:

* Deterministic transactions: Certainty of transaction execution is one of the core pursuits of decentralized applications. If transactions include only user input and new states are the result of computation on nodes (as seen in Ethereum), the transaction creator cannot be certain about the on-chain computation context, which may lead to unexpected results. In CKB, users generate new states on the client side. They can confirm the new states before broadcasting their state transition to the network. The transaction outcome is certain: either the transaction passes on-chain verification and the new state is accepted, or the transaction is deemed invalid and no state change is made to CKB (Figure 1).

* 确定性的交易执行：交易执行的确定性是去中心化应用的一个核心要求。如果交易只包含用户输入，需要经过节点的计算才能生成新状态(如以太坊)，那么交易产生者不能确定链上的计算内容，这可能会产生意料之外的结果。而在 CKB 中，用户是在客户端生成的新状态。他们在交易被广播之前就知道新状态。无论交易是否能够通过验证，这笔交易的输出都是确定的。


* Parallelism: If transactions only include user inputs and new states are generated by nodes, then nodes will not know what state is going to be accessed by the verification process, and cannot determine dependencies between transactions. In CKB, because transactions explicitly include previous states and new states, nodes can see dependencies between transactions prior to verification, and can process transactions in parallel.
* 并行处理：如果交易只包含用户输入且新状态只由节点产生，由于节点在验证过程中无法预测下一个状态，所以无法确定交易之间的依赖关系。而在 CKB 中，因为交易明确包含先前的状态和新的状态，节点可以在验证之前确定交易之间的依赖关系，从而能够并行处理交易。

* Higher resource utilization: As application logic is split and run in different places, the network can distribute computational workload more evenly across nodes and clients, and thus utilize system resources more efficiently.
* 高效的资源利用率：当应用程序的逻辑被分离并且在不同地方执行时，网络可以在节点和客户端之间更加均衡地分配计算资源，因此可以更有效率地利用系统资源。

* Flexible state generation: Even when the same algorithms are used, developers can implement generation and validation in different ways. On the client side there is the flexibility to choose the programming language that provides for better performance and fast development.
* 灵活的状态生成：即使使用相同的算法，开发者也可以用不同的方式来实现状态的生成和验证。在客户端，开发者可以灵活地选择高性能和快速开发的编程语言。

In some scenarios, state verification can utilize a different (but associated) algorithm that is much more efficient than the one used for state generation. The most typical example is seen in Bitcoin transactions: Bitcoin transaction construction consists mainly of a searching process to identify appropriate UTXOs to use, while verification is the addition of numbers and simple comparison. Other interesting examples include sorting and searching algorithms: the computational complexity for quicksort, one of the best sorting algorithms for the average case, is O(Nlog(N)), but the algorithm to verify the result is just O(N). Searching for the index of an element in a sorted array is O(log(N)) with binary search, but its verification only takes O(1). The more complex the business rules, the higher probability that there can be asymmetric generation and validation algorithms with differing computational complexity.
  <br />在某些场景中，状态验证可以使用不同(但相关)的算法，这种算法比状态生成算法更加高效。比特币交易可以被视为最典型的例子：构造比特币交易主要包含了一个搜索合适的 UTXO 的过程，而交易的验证就只有数字加减和简单的比较。其他有趣的案例包括排序和搜索算法：快速排序是平均情况下最好的排序算法之一，其计算复杂度是O(Nlog(N))，但验证排序结果却只需要 O(N)。用二分法搜索已排序数组中元素的索引，其计算复杂度是O(log(N))，但其验证算法只需要 O(1)。越是复杂的业务逻辑，就越有可能存在具有不同的计算复杂度的非对称的生成和验证算法。

System throughput can be improved by utlizing asymmetry between state generation and validation. Moving details of computation to the client side is also valuable for algorithm protection and privacy. With the advancement of technologies such as zero-knowledge proofs, we may find efficient generation and verification solutions to general problems, and CKB is a natural fit for these types of solutions.
  <br />利用状态生成和验证之间的非对称计算，我们还能进一步提升系统吞吐量。此外，将计算细节转移到客户端对于算法保护和隐私保护也很有价值。随着零知识证明等技术的进步，我们也许会发现更有效率的，对于通用问题的生成与验证解决方案，而 CKB 是这类解决方案最自然契合的选择。

We refer to programs that generate new states and create new cells as Generators. Generators run locally on the client side (off-chain). They utilize user input and existing cells as program inputs, to create new cells with new states as outputs. The inputs that Generators use and the outputs they produce together form a transaction.
  <br />我们将生成新状态以及创建新 Cells 的程序称为生成器。生成器在本地客户端运行(链下)。它们将用户输入和已有的 Cells 作为程序的输入，创建包含新状态的 Cells 作为输出。生成器使用的输入以及产生的输出一起组成了一笔交易。
![](https://i.imgur.com/ITa9cLW.png)

Figure 2. Separation of state generation and verification
  <br />图二、状态生成和验证的分离


### **4.2 Cell**
Cells are the primary state units in CKB, within them users can include arbitrary states. A cell has the following fields:
* `capacity` – Size limit of the cell. A cell’s size is the total number of bytes of all fields contained in it.
* `data` – State data stored in this cell. It could be empty, however the total bytes used by a cell (including data), must always be less than or equal to its capacity.
* `type`: State verification script.
* `lock`: Script that represents the ownership of the cell. Owners of cells can transfer cells to others.

Cell 是 CKB 中最基本的状态单元，用户可以在其中包含任意的状态。一个 Cell 由以下几个字段构成:
* `容量(capacity)`：Cell 的大小限制。一个 Cell 的大小是包含所有字段的总字节数
* `数据(data)`：状态数据储存在 Cell 中。它可以是空的，Cell 的总字节数必须总是小于或等于 Cell 的容量
* `类型脚本(type)`：验证状态的脚本
* `锁定脚本(lock)`: 代表 Cell 的所有权的脚本。只有 Cell 的所有者才能转移 Cell

A cell is an immutable object, no one can modify it after creation. Every cell can only be used once, it cannot be used as input for two different transactions. Cell ‘updates’ mark previous cells as history and create new cells with the same capacity to replace them. By constructing and sending transactions, users provide new cells with new states in them and invalidate previous cells that store old states atomically. The set of all current (or live) cells represents the latest version of all common knowledge in CKB, and the set of history (or dead) cells represents all historical versions of common knowledge.
  <br />Cell 是一个不可变的对象，自它创建后就无法修改其内容。每个 Cell 都只能被使用一次，它不能同时作为两个不同交易的输入。在 Cell 模型中，先前被引用的 Cells 会被标记为历史数据，并创建出总容量相同的新的 Cells 来替代它们。通过构建和发送交易，用户提供了包含新状态的 Cells，并能使储存旧状态的 Cells 失效。所有现存的(或者说活跃的) Cells 集合代表了 CKB 中最新版本的共同知识，而历史的(或者说废弃的) Cells 集合代表了共同知识的所有历史版本。

CKB allows users to transfer a cell’s capacity all at once, or transfer only a fraction of a cell’s capacity, which would in turn lead to more cells being created (e.g., a cell with capacity=10 can become two cells with capacity=5).
  <br />CKB 允许用户一次性转移一个 Cell 的全部容量；或者只转移一部分容量，这会导致更多的 Cells 被创建出来(例如，一个容量是 10 的 Cell，可以将容量转移到两个容量是 5 的 Cell 中)。

Two kinds of scripts (type and lock) are executed in CKB VM. CKB VM executes the type script when a cell is created in a transaction output, to guarantee the state in the cell is valid under specific rules. CKB VM executes the lock script, taking proofs as arguments, when the cell is referenced by a transaction input, to make sure the user has appropriate permissions to update or transfer the cell. If the execution of the lock script returns true, the user is allowed to transfer the cell or update its data according to validation rules that are specified by the type script.
  <br />有两种脚本(type 和 lock)会在 CKB 虚拟机中执行。当检查交易输出，生成新的 Cell 的时候，CKB 虚拟机会执行 type 脚本，以确保在 Cell 中的状态是有效的，符合特定的规则。当 Cell 作为交易输入的引用时，CKB 虚拟机会将证明作为参数执行 lock 脚本，以确保用户有适当的权限来更新或者转移 Cell。如果执行 lock 脚本返回 true，用户就可以根据 type 脚本指定的规则更新数据或者转移 Cell。

This type and lock script pair allows all kinds of possibilities, for example:
* Upgradable cryptography – Anyone can deploy useful cryptography libraries written in languages such as C or C++ and use them in type and lock scripts. In CKB VM, there are no hardcoded cryptographic primitives, users are free to choose any cryptographic signature scheme they’d like to use to sign transactions.
* Multisig – Users can easily create M-of-N multisig or more complex lock scripts.
* Lending – Cell owners can lend cells for others to use while still maintaining their ownership of the cells.

type 和 lock 脚本为各种可能性打开了方便之门，例如
* 升级加密算法——任何人都可以部署以 C 或 C++ 语言写的密码学库，并且在 type 和 lock 脚本中使用它们。在 CKB 虚拟机中，没有硬编码的密码学原语，用户可以自由地选择任何一种密码学签名方案对交易签名。
* 多重签名——用户可以轻易创建 M-N 多签脚本或者更复杂的 lock 脚本
* 租借——Cell 的拥有者可以将 Cells 租给其他人使用，并同时维持 Cells 的所有权。

The Cell model is a more generic state model compared to the UTXO or Account model. Both the UTXO and the Account model can express relationships between assets and their owners. The UTXO model defines ownership of assets (with the lock script), while the Account model defines ownership of assets by owner (with the account balance). The UTXO model makes the ledger history more clear, but its lack of generic state storage makes its already inexpressive scripts harder to use. The Account model is easy to understand and can support authorizations and identities well, but it presents challenges to processing transactions in parallel. The Cell model with lock and type scripts takes the best of both models to provide a more generic state model.
  <br />与 UTXO 或者 Account 模型相比，Cell 模型是更通用的状态模型。UTXO 和 Account 模型都可以表达资产与拥有者的关系。UTXO 模型使用 lock 脚本定义资产的所有权，而 Account 模型使用账户余额定义用户的资产所有权。UTXO 模型的账本历史更清晰，但是由于缺乏通用状态，使得其原本就表达能力不强的脚本更难使用。Account 模型非常容易理解，并且可以非常好地支持授权以及身份管理，但是它很难并发处理交易。拥有 type和 lock 脚本的 Cell 模型取这两个模型之长构建了一个更通用的状态模型。

### **4.3 VM 虚拟机**
CKB VM is a RISC-V instruction set based VM for executing type and lock scripts. It uses only standard RISC-V instructions, to maintain a standard compliant RISC-V software implementation which can embrace the broadest industrial support. CKB implements cryptographic primitives as ordinary assembly running on its VM, instead of customized instructions. It supports syscall, by which scripts can read metadata such as current transaction and general blockchain information from CKB. CKB VM defines cycles for each instruction, and provides total cycles executed during transaction verification to help miners determine transaction fees.
  <br />CKB 虚拟机是一个用来执行 type 和 lock 脚本的，基于 RISC-V 指令集的虚拟机。它只使用了标准的 RISC-V 指令集，维护了一个符合标准的 RISC-V 软件实现，因而能够获得最广泛的工业支持。运行在虚拟机上的密码学原语，其实现和部署像普通脚本一样，而不是作为虚拟机自定义的指令。CKB 虚拟机支持系统调用，脚本可以通过系统调用读取 CKB 上的当前交易以及一般的区块链信息等元数据。CKB 虚拟机定义了每条指令的周期，在矿工执行交易验证时会提供总执行周期，以帮助矿工确定交易费。

Existing blockchains hardcode cryptographic primitives in the protocol. For example, Bitcoin has special cryptographic opcodes such as OP_CHECK*, and Ethereum uses special ‘precompiled’ contracts located at a special address (e.g. 0000000000000000000000000000000000000001) to support cryptographic operations such as ecrecover. To add new cryptographic primitives to these blockchains, we can only soft-fork (as Bitcoin re-uses opcodes to support new primitives) or hard-fork.
  <br />现存的区块链项目都是将密码学原语写死在协议中。例如，比特币有特殊的加密操作码(opcodes)，比如 OP_CHECK，而以太坊则是使用特殊的预编译合约存放在一个特殊地址中(例如0000000000000000000000000000000000000001)，来支持诸如 ecrecover 这样的加密操作。在这些区块链项目中，为了增加新的密码学原语，我们只能采用软分叉(例如比特币复用 opcodes 来支持新的密码学原语)或者硬分叉的方式实施。

CKB VM is a crypto-agnostic virtual machine. There are no special cryptographic instructions hardcoded in CKB VM. New cryptographic primitives can always be deployed and used by scripts like an ordinary library. Being a RISC-V standard compliant implementation means existing cryptographic libraries written in C or other languages can be easily ported to CKB VM and used by cell scripts. CKB even implements the default hash function and public-key cryptography used in transaction verification this way. Being crypto-agnostic allows decentralized application developers on Nervos to use any new cryptography (such as Schnorr signatures, BLS signatures, and zkSNARKs/zkSTARKs) they’d like without affecting other users, and allows CKB users to keep their assets secure even in the post-quantum era.
  <br />CKB 虚拟机是一个与密码学操作无关的虚拟机。没有任何密码学指令写死在 CKB 虚拟机中。我们总能像普通脚本那样部署和使用新的密码学原语。CKB 虚拟机作为一个符合 RISC-V 标准的软件的一个好处是，现有的以 C语言或者其他语言实现的密码学库，都可以轻易地移植到 CKB 虚拟机上，并被 Cell 脚本所调用。CKB 在交易验证中默认的哈希算法和公钥加密算法就是用这样的方式实现的。CKB 虚拟机的与密码学操作无关的特性，可以允许 Dapp 开发者在不影响其他用户的情况下，在 Nervos 上使用任何新的密码学技术(例如 Schnorr 签名, BLS 签名, 和 zkSNARKs/zkSTARKs)，还可以在后量子时代继续保障 CKB 用户的资产安全。

CKB VM chooses a hardware targeting ISA because blockchain is hardware-like software. Though its creation is as easy as software, its upgrade is as difficult as hardware. As an ISA designed for chips, RISC-V is very stable, its core instruction set is implausible to change in the future. The ability to keep compatibility with the ecosystem without the need of a hard-fork is a key feature of a blockchain virtual machine like CKB VM. The simplicity of RISC-V also makes runtime cost modeling easy, which is crucial for transaction fee calculations.
Please check RFC 0003 for more details of CKB VM.
  <br />CKB 虚拟机之所以选择一个面向硬件的指令集架构，是因为区块链是一个类似硬件的软件。尽管开发一条新链像其他软件一样容易，但更新和升级区块链却像硬件一样麻烦。而 RISC-V 是为芯片设计的指令集， 它非常稳定，其核心指令集在未来都不会发生变化。能够在不需要硬分叉的情况下保持与生态系统的兼容性是 CKB 虚拟机的一个关键特性。RISC-V 的简洁性也让运行时间成本的建模变得更容易，这对于计算交易费而言相当重要。
如果想知道更多 CKB 虚拟机的细节请参阅 RFC 0003 。

### **4.4 Transaction 交易**
Transactions express state transitions, resulting in cell transfer, update, or both. In a single transaction, users can update data in one or more cells or transfer their cells to other users. All state transitions in the transaction are atomic, they will either all succeed or all fail.
  <br />一笔交易就代表了一次状态转换，这会导致 Cell 的转移或更新，或者两者同时发生。在单笔交易中，用户可以更新一个或者多个 Cell，或者将他们的 Cell 转移给其他用户。在一笔交易中的状态转换是原子性的，他们要么全部成功，要么全部失败。

A transaction includes the following:
* deps: Dependent cell set, provides read-only cells required by transaction verification. These must be references to living cells.
* inputs: Cell references and proofs. Cell references point to live cells that are transferred or updated in the transaction. Proofs (e.g., signature) prove that the transaction creator has the permission to transfer or update the referenced cells.
* 	outputs: New cells created in this state transition.

一个交易包含以下内容:
* `依赖(deps)`：依赖(deps)：依赖的 Cell 集合，提供了验证交易所需的只读 Cells，这些 Cell 引用必须是活跃的。
* `输入(inputs)`：Cell 引用和 Proofs。Cell 引用指向了在交易中将被转移或者更新的活跃的 Cell。Proofs(例如签名)则用来证明创建交易者有权转移或更新被引用的 Cells。
* `输出(outputs)`：在状态转换中产生的新 Cells。


The design of the CKB cell model and transactions is friendly to light clients. Since all the states are in blocks, block synchronization also accomplishes state synchronization. Light clients only need to synchronize blocks and do not need additional state synchronization or state transition computation. If only events were stored in blocks, full nodes would be required for state synchronization. State synchronization can be difficult across large networks because there are weak incentives to synchronize. This is different from block synchronization, in which miners are incentivized to broadcast blocks as widely as possible. With no need for extra state synchronization, the protocol makes light nodes and full nodes more equal peers, leading to a more robust and decentralized system.
  <br />CKB 的 Cell 模型设计和交易设计对轻客户端非常友好。因为所有的状态都保存在区块中，同步区块的同时也完成了状态的同步。轻客户端只需要同步区块而不需要额外同步状态或计算状态转换。如果区块中只存储了事件，那么轻客户端在同步状态时就需要全节点的支持。这可能导致在大型网络中的状态同步非常困难，因为系统对同步的激励太少了。这和区块同步非常不同，矿工会尽可能广泛地传播区块以获得奖励。由于不需要额外的状态同步，CKB 协议使得轻节点和全节点更加对等，从而能够建立一个更健壮和更去中心化的系统。
![](https://i.imgur.com/38ubs57.png)

 Figure 3. Transaction Parallelism and Conflict Detection
The deps and inputs in CKB transactions make it easier for nodes to determine transaction dependencies and perform parallel transaction processing (Figure 4). Different types of cells can be mixed and included in a single transaction to achieve atomic operation across types.

  <br />图3. 交易并行和冲突检测
CKB 交易中的依赖和输入的设计，使节点更容易确定交易的依赖，以并行处理交易(图 4)。单笔交易中可以包含不同类型的 Cells，以实现跨类型的原子操作。

## **5. Economic Model 经济模型**
A well-designed economic model should incentivize all participants to contribute to the success of the crypto-economy and maximize the utility of the blockchain.
The CKB economic model is designed to motivate users, developers and node operators to work toward the common goal of common knowledge custody. The subject of the CKB economic model is state instead of computation, by using cell capacity and transaction fees as incentives for stakeholders.
  <br />一个设计良好的经济模型应该激励所有参与者做出贡献，以实现加密经济的成功以及最大化区块链的使用。
CKB 经济模型的设计目标是，激励用户、开发者以及节点运营者一起为共同知识的安全托管努力。CKB 经济模型的主题是状态，而不是计算，使用 Cell 容量和交易费作为对 stakeholders 的奖励。
### **5.1 State Cost and Cell Capacity 状态成本与Cell容量**
The creation and storage of states on the CKB incur costs. The creation of new states needs to be verified by full nodes (which incur computational costs), and the storage of states requires full nodes to provide disk space on an ongoing basis. Current permissionless blockchains only charge one-time transaction fees, but allow states to be stored on all full nodes, occupying storage space indefinitely.
  <br />在 CKB 上的产生和存储状态需要付出成本。新状态需要被全节点验证(要付出计算成本)，而状态的存储也需要全节点持续提供磁盘空间。现存的非许可链只收取一次性的交易费，却允许状态永久地占用所有全节点的存储空间。

In CKB, cells are basic storage units of state. A cell owner can use the cell to store state himself or lend it out to others. Because a cell’s capacity can only be utilized by one user at a time, an owner utilizing the capacity himself would give up the opportunity to earn interest by lending the capacity out (either to CKB or to other users). With this opportunity cost, users pay for storage with a cost that is proportional to both space and time – the larger the capacity and the longer time they occupy it, the higher opportunity cost they incur. The advantage of CKB’s implicit state cost model, when compared to an upfront payment model (such as storage rent discussed in the Ethereum community), is that it avoids the problem that upfront payments could be used up and the system would have to recycle the state and break any applications or contracts depend on it.
  <br />在 CKB 中，Cell 是最基本的状态储存单元。Cell 的拥有者可以使用 Cell 储存自己的状态，也可以借给其他人。因为一个 Cell 的容量在同一时间只能被一个人使用，拥有者如果自己使用这些容量，就要放弃将这些容量借出(换成 CKB 或者借给其他用户)而获利的机会。在这个机会成本下，使用者为了存储空间所付出的成本会同时与时间和空间成正比--占用容量越大、占用时间越长，他们要付出的机会成本就越高。与预付模型(例如在以太坊社区讨论的存储租金)相比，CKB 隐含的状态成本模型可以避免预付金被用光，进而避免因系统必须回收状态导致其他依赖该状态的合约或者应用被破坏。

Cell metadata (capacity, type and lock) are states, which will occupy users’ cell capacity and incur a state cost as well. This meta cost would incentivize users to create fewer cells when possible, increasing capacity efficiency.
  <br />Cell的元数据(capacity, type and lock)也是状态，也要占据用户的 Cell 容量，因而也要付出状态成本。这些元数据成本会激励用户尽可能少地产生 Cell，并提高储存效率。


### **5.2 Computation Cost and Transaction Fees 计算成本和交易费用**
Updating a cell’s data or transferring cell ownership incurs transaction fees. Miners can set the transaction fee level that they are willing to accept based on CKB VM cycles used and state changes in transaction verification, allowing the market to determine transaction fees. With the programming model described above, cell owners can also pay transaction fees on behalf of their users.
  <br />更新 Cell 的数据或者转移 Cell 的所有权都会产生交易费。矿工可以基于 CKB 虚拟机在验证交易时消耗的指令周期和状态变化，去设定他们愿意接受的交易费率，市场会决定实际的交易费用。通过上述的编程模型，Cell 的拥有者还可以代替他们的用户支付交易费。

As cell capacity is the only native asset in CKB, it is the most convenient asset users can use to pay transaction fees. However, users can also use any other user-defined assets as long as miners accept them; there is no hard-coded payment method in CKB transactions. This is allowed in CKB because its economic model and native asset do not center on computation, but states. Although cell capacity can be used as a means of paying transaction fees, its primary function is secure common knowledge storage, which can store state and hold it long-term. Payment method competition in the fee market does not compromise its value.
  <br />因为 Cell 容量是 CKB 中唯一的原生资产，因此用户用它来支付交易费是最方便的。然而，只要能被矿工接受，用户也可以使用任何自定义的资产作为交易费；在 CKB 交易中没有定死支付方式。之所以能够采用灵活的支付方式，正是因为经济模型和原生资产的设计并非以计算为中心，而是以状态为中心。虽然 Cell 容量可以作为一种支付交易费的方式，但是它的首要功能还是确保共同知识的储存安全，也就是可以储存和长期保存状态。通过自由市场竞争支付方式并不会损害它的价值。

Restricting the transaction fee payment method to a blockchain’s native asset is a significant obstacle preventing blockchains’ mass adoption. This requires users to acquire native assets before using any of the blockchain’s services, raising the barrier of entry for new users. By allowing cell owners to pay fees on behalf of their users and allowing payment with any user-defined assets, CKB can provide a better experience to users and wider choices of business models for developers.
Please check the Nervos CKB Economic Paper for details of the economic model.
  <br />将交易费的支付方法限定为区块链的原生资产，是阻碍区块链大规模应用的重要障碍。这要求用户在使用任何区块链服务前就必须获得这种原生资产，这提高了新用户的进入门槛。通过允许 Cell 的拥有者来代替用户支付费用，以及允许用各种自定义的资产付费，CKB 可以提供更好的用户体验，并为开发者提供更广泛的商业模型选择。

请查阅 Nervos 的 CKB 经济白皮书来了解更多经济模型的细节。

## **6. Network 网络**
We can categorize CKB nodes into three types:
我们可以将 CKB 节点分为三类
* Mining Node: They participate in the CKB consensus process. Mining nodes collect new transactions, package them into blocks and produce new blocks when they have found a Proof-of-Work. Mining nodes do not have to store the entire transaction history, only the current cell set.
* 挖矿节点：他们参与 CKB 共识过程。挖矿节点收集新的交易，把他们打包到区块中，并在找到工作量证明时产生新的区块。挖矿节点不需要储存全部的交易历史，只需要储存当前活跃的 Cell 集合。

* Full Node: They verify new blocks and transactions, relay blocks and transactions, and select the chain fork on which they agree. Full nodes are the verifiers of the network.
* 全节点：他们需要验证新的区块和交易，转发区块和交易，以及选择他们认可的链分叉。全节点是网络中的验证者。

* Light Node: They trust full nodes, only subscribe and store a subset of cells that they are concerned with. They use minimal resources. Users increasingly rely on mobile devices and mobile apps to access the Internet, the light node is designed to run on mobile devices.
* 轻节点:他们信任全节点，只订阅和存储和他们相关的 Cells 子集。他们只需要消耗最小的资源。当前，用户越来越多的依赖移动设备和移动 app，轻节点正是为移动设备而设计的。


Uniform blockchain networks (in which each node has the same role and performs the same function) are currently facing severe challenges. Full nodes validate all blocks and transaction data, requiring minimum external trust, but they incur a higher cost and are inconvenient to run. Light clients trade minimal trust for a substantial cost reduction on transaction verification, leading to a much better user experience. In a mature crypto-economy network, the largest group of nodes would be light nodes, followed by full nodes and mining nodes. Because light nodes depend on full nodes for state and state verification, a large number of light nodes would require a large number of full nodes to serve them. With CKB’s economic model, both computation and storage resources required by a full node can be kept at a reasonable level, and the barriers to running a full node low, leading to a large group of service providers for light nodes and a highly decentralized network.
  <br />现在，单一的区块链网络(在其中每个节点都扮演相同的角色并表现出相同的功能)正面临着严酷的挑战。全节点验证所有的区块和交易数据，依赖最少的外部信任，但却需要承担较高的运行成本。轻客户端通过牺牲少量的信任，大幅降低了交易验证的成本，进而带来更好的用户体验。在一个成熟的加密经济网络中，最多的节点会是轻节点，然后才是全节点和挖矿节点。因为轻节点在获取和验证状态上依赖全节点，数量庞大的轻节点将会需要许多全节点为他们服务。在 CKB 的经济模型中，运行全节点所需要的计算和储存资源可以维持在一个合理的水平，由于运行全节点的门槛很低，会出现大量为轻节点服务的全节点，使得网络更加去中心化。


## **7. Summary 结论**
We envision a layered crypto-economy and CKB is its base layer. CKB is the decentralized trust root of this crypto-economy, it ensures the security of the trustless activities of the upper layers. It’s a common knowledge custody network, in which states are verified by global consensus and stored in a highly available peer-to-peer network. CKB is designed from scratch to meet the needs of a layered architecture, and its design focuses on states rather than computation. In CKB, users and developers can define, issue, transfer and store crypto-assets, they can also create digital identities and utilize these identities in the crypto-economy. Only our imagination is the bounds of its use.
  <br />我们设想了一个分层的加密经济，CKB 是它的基础层。CKB 是这个加密经济的去中心化信任之锚，它保障了上层的去信任活动的安全性。它是一个共同知识的托管网络，其中的状态为全球共识所验证，并且储存在高度可用的点对点网络中。CKB 是一个从零开始，为多层架构所设计的公有非许可链，它主要关注状态，而非计算。在 CKB 中，用户和开发者可以定义、发行、转移和储存加密资产，他们也可以创造数字身份，并在加密经济中使用这个身份。总之，限制 CKB 使用边界的只是我们的想象力。

## **8. References 学术引用**
Satoshi Nakamoto, “Bitcoin A Peer-to-Peer Electronic Cash System”, 2008
Vitalik Buterin, “Ethereum A Next-Generation Smart Contract and Decentralized Application Platform”, 2014


## **9. Appendix**
Common Knowledge is the knowledge that’s accepted by everyone in a community. Participants in the community not only accept the knowledge themselves but know that others in the community also accept the knowledge.
  <br />共同知识是指社区中的所有人都认同的知识。社区参与者不仅认同知识本身，还知道社区中的其他人也认同这个知识。

In the past, common knowledge was scattered across individual’s minds, and its formation required repeated communication and confirmation. Today, with the advancement of cryptography and distributed ledger technology, algorithms and machines are replacing humans as the medium for the formation and storage of common knowledge. Every piece of data in the blockchain, including digital assets and smart contracts, is a piece of common knowledge.
  <br />在过去，共同知识散落在每个人的头脑中，它的形成需要经过反复的沟通和确认。今天，随着密码学以及去中心化账本技术的进步，算法以及机器正在取代人们成为产生和存储共同知识的媒介。每一条区块链中的数据，包括数字资产和智能合约，都是一条共同知识。

Blockchains are common knowledge bases. Participating in a blockchain network implies accepting and helping validate the common knowledge contained in it. Blockchains store transactions with their proofs, users can trust the validity of these transactions and know other users trust it too.
  <br />区块链是共同知识库。参与区块链网络，意味着认可以及帮助验证其中所含的共同知识。区块链存储交易和证明，用户信任这些交易的有效性并且知道其他人也信任它们。

The various ways in which the knowledge on which people base their plan is communicated to them is the crucial problem for any theory explaining the economic process, and the problem of what is the best way to utilizing knowledge initially dispersed among all the people is at least one of the main problems of economic policy – or of designing an efficient economic system.
  <br />这句话
  <br />*- The Use of Knowledge in Society, Friedrich A. Hayek, 1945*

