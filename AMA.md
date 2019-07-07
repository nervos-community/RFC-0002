
## [AMA] Jan Xie and Kevin Wang, Founders of Nervos, and Xuejie Xiao, Core Developer of Nervos


Nervos was invited to answer questions during a private AMA on Telegram. This is an edited transcript of the conversation. 

 **Group**: Alright everyone, let's do this—please give a warm welcome to Jan, Kevin, and Xuejie of Nervos! As a reminder for everyone participating, please keep the discussion respectful at all times. Could you start off by giving us a brief bio touching on your backgrounds as well as how you got started in crypto? And then a short overview of Nervos, how the idea came to be, and how it’s going so far? We’ll then be off to the races with questions.

**Nervos (Jan Xie)**: Hi, all. Thanks for inviting us to do this. 

**Nervos (Kevin Wang)**: Hello everyone, my name is Kevin Wang and I'm one of the co-founders of the Nervos Network. I have an engineering background and started my career working on big data solutions at the IBM Silicon Valley Lab. Before Nervos, I co-founded an online software engineering education platform called Launch School. I got into Crypto around 2012 after coming across Bitcoin and reading the whitepaper, and have been hooked ever since.



Nervos Network is a layered network built to support the crypto-economy. The layer 1 protocol of the Nervos Network is the Common Knowledge Base (CKB), an open, public and Proof of Work based blockchain. The CKB is not optimized for transaction throughput or performance, but like Bitcoin is designed to be maximally secure and censorship-resistant to serve as a decentralized custodian of value and crypto-assets. It includes a crypto-economic design not just to facilitate transactions, but also for long-term value preservation.



Nervos Network layer 2 protocols leverage the security of the Common Knowledge Base and provide unlimited scalability and allow tradeoffs for application-specific concerns such as privacy and finality.

**Nervos (Jan Xie)**: I started my blockchain adventure in 2014 as the chief architect for Yunbi (first to list Ethereum in China, once largest exchange in 2018) and built its core trading engine, then I cofounded the largest Ethereum community in China - EthFans and a blockchain notary company. Then I joined Ethereum core research team and worked on Casper/sharding prototypes in 2016. Also in 2016 I founded Cryptape, a startup focusing on blockchain protocol design and blockchain engineering. Since then we have delivered a wide range of successful projects such as high-performance permissioned blockchain CITA, PoCs for financial institutions and mining pool.

**Nervos (Xuejie Xiao)**: Hey I'm Xuejie Xiao. Thanks for having us. I’m an engineer working on Nervos CKB. I'm also the main developer of CKB VM. I spent years working with technologies such as emscripten and the later emerged WASM. I basically witnessed the whole trip from the original emscripten, to asm.js, then to current WASM era.

While I’ve got to know bitcoin for quite some time, my real experience in the blockchain space didn’t start until 2017, where I worked on wallets and mining pools, later I joined Nervos, and found out blockchain VM is a space where I can combine my past experience with personal interest best 😛

**Group**: Thanks guys - let's move onto questions! For starters, I was hoping you guys could drill into the main differentiation Nervos has with Ethereum. Some investors in this room haven't had time to read the documents you sent over but are obviously very familiar with some of the benefits/challenges of ETH.

**Group**: Somewhat related: It seems smart contracting platforms generally want to move off PoW (ETH) or are pure PoS/dPoS (everyone else). Where does Nervous see the demand for a PoW smart contract platform coming from?

**Nervos (Kevin Wang)**: On the status of the project: We started engineering early last year and have just launched our Testnet last month. We expect our mainnet to launch by the end of this year. Outside of Engineering, we’ve published our crypto-economics design as well. All high level protocol designs are also published via the RFC process [here](https://github.com/nervosnetwork/rfcs/tree/master/rfcs). 

**Nervos (Jan Xie)**: Either PoW or PoS can be used in a smart contract platform, the reason we choose PoW is because we're building a layer 1 blockchain, which must be decentralized and neutral like Internet. This is a large topic and I’ll skip the technical part such as long-range attack (some teams are trying to solve that with VDF but I’m not sure VDF itself is a good idea or not) just recommend our researcher Ren Zhang’s recent [talk](https://www.youtube.com/watch?v=gxFm1QieUdE). 

**Group**: Thanks guys for doing the AMA, my first question is related to Nervos vision to limit the global state of the protocol. As the global state is maintained by full nodes, do you think of mechanisms to reward full nodes?

**Nervos (Kevin Wang)**: Ethereum and mostly all other smart contract platforms (EOS, Dfinity etc) are built to be decentralized computation platforms. Users of this type of platforms care most about the cost of transactions, therefore it's important to have high supply of transactions (TPS) to drive down cost. This is the reason why nearly all other smart contract platforms are pursuing novel consensus algorithms and/or sharding for scalability. However, there's an inherent conflict between optimizing for TPS and optimizing for SoV, where the dominant use case is not to transact (for example, more than 50% of Bitcoin UTXOs have coinage > 1 year), but to keep assets safe. Our Common Knowledge Base is designed with economics that's designed to be long term sustainable independent of transaction demand, have sound value capture mechanism, and provide an “inflation shelter” so that holders of platform tokens can capture network overall value. We believe all the design tradeoffs above are necessary to be a true Store of (monetary) Value / Store of (non-monetary) Assets platform.

**Nervos (Jan Xie)**: The problem with PoS that may even be bigger than the technical one is it is not as open as PoW. By ‘open’ I mean anyone in the world should be able to participate in the consensus process, there should be no limit in the protocol to stop that. It’s true for PoW but not for PoS. PoS requires you to hold some resources internal to the system to participate in the consensus. If you have to deposit before being a validator, current validators will be able to censor your deposit transaction to stop that. If you have some positive probability to create a new block by simply hold some coins, large stakeholders can always ignore your block. This is different from PoW because mining hardware and energy are resources external to the system, they can always be added if needed, new miners don’t need the permission of existing miners to participate in consensus. In PoS if the validators monopolized the system there’s no way out unless you hard fork.

**Nervos (Kevin Wang)**: Other than crypto-economics, I’d also like to point out our belief that layer 2 solutions will be on the rise and will be able to provide near unlimited scalability with minimal cost. High TPS layer 1 platforms are going to be out competed and the platforms that complement layer 2s are going to win. As layer 1s are getting more mature innovation is going to happen more on layer 2s. The layer 1 platform with best support for layer 2s is going to have the best transaction liquidity and will attract the most assets in custody. Our RISC-V based VM of the Common Knowledge Base is designed to be the most layer 2 and cross-chain friendly (DIY crypto-primitives, run other VMs on our VM, accurate and fare fee calculations, etc). 

**Nervos (Jan Xie)**: Anything that can be centralized will eventually be centralized. Stake/coins will eventually be centralized IMO. In PoW we will also see centralization, but 1. the centralization of ASIC manufacturing can be alleviated by choosing a simple PoW function which makes its ASIC design barrier low, to encourage a market with many competing ASIC manufacturers, with technology advancement like 3D printing we may decentralize the ASIC manufacture completely (I guess it needs quite a time though), and 2. Energy is naturally decentralized. With decentralized ASIC manufacturing and decentralized energy we’ll have decentralized mining eventually. I believe PoW is the best fit to layer 1 blockchain because we must keep layer 1 a decentralized and neutral network just like Internet. I think PoS will shine on layer 2 blockchains.

**Group**: This is refreshing thinking - explicitly optimizing for SoV/censorship resistance. Can you walk us through the lifecycle of an asset on Nervos? Curious about how the economics make it long-term sustainable. 

**Nervos (Kevin Wang)**: In short, our layer 1 protocol is designed not to be a transactional platform, but a platform to secure preserve assets and cryptographic common knowledge. Similar to how Bitcoin is positioned to be a SoV platform and don’t try to compete in transactional cost and efficiency. Such a platform for general purpose assets other than money is very necessary to serve as the foundation, or “value layer” or the crypto-economy.

**Nervos (Jan Xie)**: I think full nodes are rewarded by the ability to validate the history by themselves. Incentivization for archive node (who stores full history) may be needed.

**Group**: Could you give a detailed overview of your team?

**Group**: I was reading through primary/secondary issuance of CKBs. It sounds like a mix of PoW/PoS, would you agree with this characterization?

**Nervos (Kevin Wang)**: There are several requirements for a multi-asset SoV platform (Bitcoin is a single asset platform). First, the platform token has to have intrinsic value beyond paying for transactions. For all other smart assets platforms, the platform dilutes native token holders to provide security to the overall ecosystem, and token asset holders (think MKR holders on Ethereum) ride platform security for free. All platforms rely on the monetary premium to be able to support this security. It’s working for Ethereum now because its near monopoly in the smart contract space. However, it’s unclear this would be sustainable with the rise of stable coins and efficient swaps. When mass adoption comes, users would want to hold just a few perceived of SoV coins such as Bitcoin and stable coins and all paying-for-transaction is going to be abstracted away by the UI. If a smart contract platform token doesn’t have an intrinsic value other than paying for transactions, it’s going to be very difficult to keep a “monetary premium” therefore the foundation of the crypto-economics will collapse.

**Group**: What layer 2 solutions are you targeting first? Do you expect to work with interoperability platforms like Cosmos for scaling too?

**Nervos (Kevin Wang)**: Second, the platform token has to be able to capture the value of the ecosystem to raise its own security budget as the value of the assets on it appreciates - otherwise it’s going to be increasingly profitable to attack the platform’s consensus to double-spend the assts on top of it. This is analogous to how a country can raise tax to pay for military to protect its borders. When there’s no central government to send out tax men to knock on doors, this tax collection has to be very effective otherwise there’s no way to pay for the ongoing cost of the military and value in the country will be looted. We believe sales tax (tx fees) are not going to be effective, since sales (txs) can easily move elsewhere, and the most effective way to raise this security budget is property tax. The value capture mechanism here is similar to how land captures value of the economy. Our native token represents claims to the global state, whose growth is strictly bounded. Therefore as more assets are secured on the platform, the token captures the value. Going back to the “country/military” analogy, as the military becomes stronger, the platform becomes increasingly attractive to high value assets. This is one of the very few sustainable positive feedback loop and fork resistance path of building a layer 1 platform.

**Nervos (Jan Xie)**: We do research on different layer 2 solutions since they have different trade-offs - channels has low latency, plasma is almost as secure as the underlying layer 1 if done right, sidechains are very practical. We keep talking with different layer2 teams to learn their needs, it's hard to say which we target first. We are trying to build a general layer 1 blockchain that can work with different layer2 protocols. We're open to collaborate with any interoperability platforms.

**Nervos (Kevin Wang)**: Third, the platform crypto-economics including token issuance themselves have to take care of the long term token holders to be SoV for the platform itself to be multi-asset SoV. Having a hardcap has issues when the hardcap reaches and the platform would only rely on transaction fees; perpetual issuance has the problem of perpetual diluting platform token holders. We come up with a combination of base and secondary issuance, and provide an “inflation shelter” so that we’ll always have new issuance, but at the same time long term platform token holders can always retain the same percentage of network value. We believe all those are critically important to design a multi-asset SoV system. [Please see our crypto-economics design for more details](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0015-ckb-cryptoeconomics/0015-ckb-cryptoeconomics.md).

**Group**: Can you guys describe the Nervos VM?  Will there be any limitations with it that might make it difficult to work with certain L2/interoperability solutions?

**Nervos (Jan Xie)**: Nervos (Xuejie Xiao) is the main developer of CKB-VM, I'll pass this question to him

**Group**: Could you explain your “store of value” thesis? What are your fundraising plans and plans for distribution of tokens?

**Nervos (Xuejie Xiao)**: Nervos CKB VM is built on the open source RISC-V ISA. We believe that to serve a blockchain better, a flexible VM is needed. And the most flexible one we can get to, is one modeled after a real processor.

One unique aspect of CKB, is that we don't pack any cryptographic algorithms in the contract side of CKB, all the signature verification algorithms, including the official shipped one, are implemented via CKB VM as a separate contract. And if you want to use a different algorithm than what we ship, you are free to do so.

So we’d believe CKB VM is flexible enough to work with different layer2/interoperability solutions. We don’t expect there will be any roadblocks for supporting different chains. In fact, we believe working on top of CKB could make layer2/interoperability solution even easier

**Group**: A related question, how is the Cycles’ concept for computations cost compares to gas concept?

**Nervos (Xuejie Xiao)**: That’s actually one more beautiful advantage provided by RISC-V: By leveraging a real ISA designed for real processors, CKB VM is born with a natural way to express runtime costs: at physical level, each instruction will have an associated *cycle* consumption, the cycle consumed here is irrelevant to the workload performance, and is only determined by the internals of the processor. As a result, the cycles here can be added up to form the runtime cost for a program. This solution is both accurate and future proof, if more work is done, there will naturally be more cycles. It is not affected by how the VM is implemented, or how people decide to use the VM.

**Nervos (Kevin Wang)**: We strictly use PoW for Sybil resistance, but long term token holders who deposit their tokens into the NervosDAO can receive income. This is different from PoS income, as there’s no need to become validators and always online, or having to delegate to someone else. You can deposit your coins in a cold wallet and go away for 20 years. This is critical for the SoV users, because they don’t want to have to run infrastructure themselves (and potentially lose their coins being punished) nor do they want to have to trust someone else. So yes NervosDAO can provide yield, but it’s more to function as an inflation shelter and impose opportunity cost to users who do occupy the global state for preservation, and it’s not part of the consensus.

This is another reason why we believe PoS systems can’t be SoV. As a token holder of a PoS network, your choices are 1) being inflated away 2) run infrastructure 3) trust someone else and none of them are attractive from a SoV point of view. If your platform tokens are not viewed as SoV, it’s hard to function as a SoV platform for other assets.

Just to stretch this questions a bit further - we believe a layer 1 smart contract platform has to be SoV. The crypto-economy has to build on a sound foundation and asset security has to be protected before optimization for tx efficiency.

**Group**: Thanks for the clarification, wouldn't you worry that in the long run (20+ years) you will run into the problem of low incentive to do the PoW/Validation job (similar to Bitcoin)? This can be even made worse by inactive holders being rewarded without the need to perform work

**Group**: Is there any performance advantage of using RISC-V over EVM or WASM?

**Nervos (Xuejie Xiao)**: In terms of performance, we do want to point out a separate point here: you might have seen benchmark numbers for WASM VMs, some of which might be very promising. But please keep in mind, that not all WASM VMs can be used in the blockchain space. In fact, most, if not all, of the promising WASM benchmark numbers are from V8 or SpiderMonkey, which CANNOT be used as blockchain VMs. A blockchain VM must be deterministic, meaning the VM must give the same result in all environments given the same input. This is not a property shared by modern JS engines such as V8 or SpiderMonkey. A secure WASM VM for blockchains must satisfy certain properties including determinism, and we wonder if one can make a WASM VM that is both as fast as V8, and secure enough for blockchain usage.

As for CKB VM which uses RISC-V itself, our latest code can already reach the same level as speed compared to a sophisticated WASM VM such as the one used in v8(but keep in mind, the WASM VM in v8 cannot be directly used in a blockchain project yet). And we are already within the same order of magnitude compared with native code.

We don’t have comparison with EVMs yet since EVM is much less powerful than CKB VM, and we cannot build the benchmark we use on top of EVM.

**Nervos (Xuejie Xiao)**: There’s also one additional advantage for CKB VM here: since CKB VM uses RISC-V, which is an ISA for real processors, nothing prevents one from using real high performance RISC-V CPUs in case where higher TPS is required (such as a mining pool or an exchange). This is something that an EVM or WASM solution cannot compete with

**Group**: What is your go-to-market strategy (i.e. partnerships and business development)?

**Nervos (Kevin Wang)**: The secondary issuance is ongoing, this is critical so that we don’t run into incentive compatibility problems when token issuance stops, such as the fee sniping issues that’ll inevitably come up in Bitcoin. In terms of compensating for miners, we have to look at this from the point of view of the fundamental value proposition of our platform to our users. Again, it’s not to pay for transactions, but to secure preserve assets and pay for their occupied state. With the secondary issuance, this is done with the equivalent of state rent income collected through targeted inflation. As long as there’s need for a best-in-class asset preservation platform, there’ll be demand for occupying global state which will raise the fiat price of our tokens and miners can get income from this income even there’s no transactions.

**Group**: How does Ethereum's state rent compare to your solution?

**Nervos (Kevin Wang)**: In the recent years we’ve seen crypto adoption moving to more friendly jurisdictions and many of them are in Asia. We have a large community in China and is one of the most anticipated upcoming blockchains in Asia. Our team has deep roots in the Bitcoin and Ethereum communities in China and have lots of connections in the ecosystem such as wallets, miners, mining pools, developers, exchanges etc. We’ll formalize and announce those partnerships leading up to the mainnet launch.

**Group**: Having raised a large amount of funding to build Nervos, can you describe what your process has been like to identify/recruit/hire talented blockchain engineers? What was that experience been like?

**Nervos (Kevin Wang)**: Though public blockchain communities are inherently decentralized, we do expect the core team to play a critical role to bootstrap adoption. We have strong ties to traditional finance (have partnerships and worked on prototypes at some of the world’s largest financial institutions) and connections to startups and small/medium businesses through Cryptape, our service company that worked on blockchain adoption in China for the last 3 years. There’s a long list of companies that have talked to us and wanted our help to build blockchain based solutions, and have been waiting for our mainnet launch. In particular, we have a few high profile partnerships in the pipeline, but not ready to publicly disclose them yet.

**Group**: Can you explain how it will be possible to double-spend assets on top of other smart contract platforms and how Nervos mitigates this attack?

**Nervos (Kevin Wang)**: It’s helpful that our founders are all experienced engineers and we’re very visible in the crypto engineering scene in China. Early members of the team are almost exclusively personally recruited from who we know are the top engineers. As we’re building up the community in China and Asia and events in particular (we’ve organized and attended nearly 100 meetups worldwide, with most of them in China), we’ve seen experienced engineers take interests in our projects and come to us to talk about opportunities. So what worked best for us is the community based approached and deeply technical events and online discussions that appeal to experienced engineers who’re seeking work in this space.

**Group**: How will you get Ethereum, Tezos, and other smart contract developers to leave those platforms and build on top of Nervos?

**Group**: As a follow up to that, do you think blockchain engineering is a field in itself, or can a top engineer learn the blockchain-specific aspects fairly quickly and make the transition?

**Nervos (Kevin Wang)**: Consider a platform where the platform market-cap is 10 million, but has an asset (say Maker) that’s worth 100 million. In this case, attackers can attack the base platform to double spend the Maker asset. This is where the security budget of the platform itself imposes a ceiling to the value of its assets running on top. To avoid this, there has to be a clear value capture path such that demand to the assets on the platform generates demand to the native platform tokens. Purely relying on being a good TPS platform and hoping token holders will volunteer to hold your tokens for long term therefore having a monetary premium is not sustainable, because nearly everyone else is trying to do the same, including your own forks. I talked about this in more details [here](https://medium.com/nervosnetwork/smart-contract-platforms-have-to-be-store-of-value-323745fac0a5). 

**Nervos (Jan Xie)**: Ethereum's state rent is much more complex, because: 1. Ethereum's asset model is based on shared state - all erc20 records are pooled together in a single account, this raises a problem for rent because the state ownership is not clear, who should pay the rent for erc20 contract? or all the token holders should pay it collectively? In CKB state is a first-class citizen has clear ownership specified, scripts are pure functions that has no internal state, CKB users own their assets completely, because both asset record and its storage are owned by user, it's like you not only own the house but also the land it's built on; 2. Rent model requires users to pay explicitly which is not user friendly, what happens if the rent is no longer paid or just not being paid in time? Such things will make it even harder to audit the security of contracts because now you need to consider if your dependent contracts paid enough rent. Mechanism like 'resurrection' will only make things more complicated. Paying the rent implicitly by inflation make things much simpler.

**Nervos (Xuejie Xiao)**: I think top engineers can be adapted to any fields as they want. Our CKB team contain many top engineers, or even former CTOs from other fields. Given a short period of time, they can all transition into blockchain minded developers and deliver awesome products. In fact if you think about it, blockchains have a lot in common with other software projects, especially distributed systems. So the knowledge one gained from a different field might very likely still be useful in the blockchain space

**Nervos (Kevin Wang)**: First, our VM is very low level, which makes it much easier to run other VMs (such as EVM and the VMs of other platforms) on top of our VM - all you need is a compiler to compile their VMs down to RISC-V ISA, which has wide toolchain support (gcc, llvm) in the industry. This makes it very easy for other platform applications to migrate to Nervos. Secondly, we’ll run a large community grants program, as well as operating a long term treasury to encourage adoption. Thirdly, we’re building best-in-class for layer 2 solutions, and expect to have the most transaction liquidity (best access to layer 2 tech). Assets will flow naturally to where the best tx liquidity is.

**Nervos (Jan Xie)**: CKB's programming model gives developers more freedom. We also care about developers who are not in blockchain space today. CKB-VM supports all programming language that can be compiled by gcc which is an advantage here. We even have a demo showing how to use ruby to write smart contract on ckb (ruby smart contract running in a ruby interpreter running in CKB-VM)

**Group**: Alright guys, we're running up on time. Thanks for joining us! How can people stay up to date with Nervos progress and get in touch?

**Nervos (Kevin Wang)**: Please follow our project on twitter and telegram (they’re pretty easy to google) for official announcements.  Thanks everyone, it’s been really great chatting!

**Nervos (Xuejie Xiao)**: Thanks a lot, this has been super fun!

**Nervos (Jan Xie)**: Thanks everyone!
