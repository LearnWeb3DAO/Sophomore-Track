# What is Mining?

Mining is the process that helps create new blocks of transactions to be added to the Ethereum blockchain network. It also helps keep the network secure from attacks.

Just like Bitcoin, Ethereum was using a Proof-of-Work (PoW) consensus mechanism, which is heavily reliant on mining. Ethereum miners use their time and computation power to process new incoming transactions and produce new blocks.

> Ethereum Merge (The Merge was executed on September 15, 2022) it replaced Proof of Work with Proof of Stake, which significantly changed how new blocks are produced and reducing energy consumption by ~99.95%. We will talk about [The merge](https://ethereum.org/en/upgrades/merge/) later.

<Quiz questionId="6ad2fd8e-0241-4c0a-a66e-2b01116e4a9d" />

## Why do miners exist?

Most people think that mining is simply a way of adding new transactions and creating new coins. But, we mentioned above, that mining also helps keep the network secure. Why is that?

In decentralized systems like Ethereum, we need to ensure that everyone in the world agrees on the ordering of transactions.

For example, if Alice sent Bob 1 ETH, which he further sent to Charlie, the order of the transactions NEEDS to be as follows

Alice --> Bob 1 ETH

Bob --> Charlie 1 ETH

Any other transaction order will not work, and it is important everyone agrees to this.

Miners, therefore, are also responsible for validating cryptocurrency transactions on the blockchain network and adding them to the ledger. Importantly, this process prevents the [double-spending](https://en.wikipedia.org/wiki/Double-spending) of digital currency.

<Quiz questionId="9ab9ffd1-76ad-4ecf-9934-87d7991e9f4a" />

## Why would anyone become a miner?

Just like physical currencies, when someone executes a transaction, the state of the blockchain must be updated to reflect the debit/credit of each account involved.

Digital platforms however are prone to attacks, and can be manipulated. Miners therefore not only propose new blocks and validate all transactions, they also need to produce a certificate of legitimacy that all transactions in their proposed block are valid.

Producing this certificate is a computationally hard and complex challenge. This is what all of Proof of Work is about. We will go more in depth about this in a later tutorial, but for now, just know that producing this certificate is quite hard.

But, given a certificate of legitimacy, it is extremely easy for a third party to verify if it is valid or not.

For the miners hard work in producing these certificates and executing transactions, they are rewarded in the form of new coins. In Ethereum, when a miner successfully mines a block, they receive 2 ETH for their hard work. Ethereum started with a block reward of 5 ETH, but this was lowered to 3 ETH with the Byzantium upgrade in 2017, and then again down to 2 ETH with the Constantinople upgrade in 2019.

Since decentralized systems lack a central authoirity, the mining process is extremely crucial to the safety and validity of the network. Therefore, the mining reward acts as incentive to participate in the transaction validation process.

<Quiz questionId="b18c52d4-dbe2-4dda-b6cf-a8fb6d07fa36" />

## Who can become a miner?

Technically speaking, anyone can become a miner on Ethereum by running the Ethereum node software. However, not everyone can mine Ethereum _profitably_.

In most cases, miners purchase specialized hardware to mine profitably. It is unlikely for average computers to earn enough mining rewards to cover the associated electrical and hardware costs for mining.

<Quiz questionId="91b88ca4-ce1e-4cd8-8736-857e62867eff" />

### Costs of Mining

- Costs of hardware necessary to build and maintain the hardware
- Electrical costs of powering the mining rig
- Potential cost of equipment to support the mining rig (coolers, ventilators, energy monitors, electrical wiring, solar farms, etc.)

To explore mining profitability, you can use a calculator [like this](https://etherscan.io/ether-mining-calculator)

<Quiz questionId="e40e5d82-8c64-4fa9-9d0f-39ce807f19b3" />

## How are Ethereum transactions mined?

1. A user signs a transaction request from their account
2. The transaction is broadcasted to the Ethereum network through a node
3. Upon hearing of the new transaction, each node in the network adds the transaction to their local mempool (memory pool). The mempool is a list of transactions that nodes have heard about but have not yet been included as part of a block.
4. At some point, a miner groups a lot of transactions into a potential block, in a way that maximizes transaction fees they earn while staying under the block gas limits. Then,
   1. The miner verifies the validity of each transaction. This involves things like checking that no one is trying to transfer ether they don't own, or that the signature is invalid, or the request is malformed, etc.
   2. The miner then executes the transaction on their local copy of the Ethereum Virtual Machine (EVM). It awards the transaction fees for each such request to their own account.
   3. The miner begins the process of creating the Proof-of-Work certificate of legitimacy for the entire block, after all transactions in the block have been verified and executed on their local EVM.
5. Eventually, the miner finishes producing the certificate of legitimacy for the block, which includes the user's transaction. The miner then broadcasts the completed block to the world, which includes the certificate and the new state information for the blockchain.
6. Other nodes hear about the new block. They verify the certificate (which is easy to do), execute all transactions in the block on their local copies of the EVM, and verify that their EVM state matches the one proposed by the miner.
7. Once other nodes have verified everything proposed by the miner, they add this block to their blockchain node, and accept the new EVM state as "the current state of the blockchain".
8. Each node removes the transactions included within the block from their local mempool.
9. This process repeats.

<Quiz questionId="1f3de78a-6926-42cd-9577-95c3faba5f36" />

## Different methods of Mining

In the technology's early days, when it was quite easy to become a miner, most miners used CPU-based mining running on their desktops and laptops. However, CPU-mining is quite slow and impractical in today's age because it can take months to earn even a tiny amount of mining rewards, while constantly running your CPU at 100% of it's capacity and paying for high electrical costs.

GPU-based mining is the most common approach these days. This approach can maximize the computation power by bringing together multiple GPUs under a single mining rig, putting them all to work in parallel thereby increasing overall capacity and throughput of the one rig.

Application-Specific Integrated Circuit (ASIC) mining is also quite popular. These are hardware chips designed specifically to mine Ethereum. You can purchase ASICs online for most popular blockchain networks. However, they are expensive, and as more and more miners join the network and mining becomes harder, the hardware becomes obsolete quickly. A brand new ASIC would usually only last 1-2 years max.

Another option that is growing these days is cloud mining, which allows individual miners to leverage the power of dedicated mining facilities, without owning any hardware.

<Quiz questionId="a5cbe921-a963-4f93-b065-6fdfdbae0c0f" />

<Quiz questionId="a99fe844-fc86-436b-b0a8-fe8d54deef6d" />

## Mining Pools

Most individual miners do not have enough resources to set up huge mining facilities with thousands of GPUs and ASICs. Mining pools allow miners to combine their computational resources in order to increase their chances of mining blocks. If anyone in the mining pool succeeds at mining a block, the reward is distributed proportionately to everyone in the pool based on their computational power they contribute to the pool.

<Quiz questionId="83c916b8-b7ba-4913-a308-7c748a3a1f1b" />

## Visual Demo of Mining

Watch this video to get a visual demo of the mining process.

[![Visual Demo of Mining](https://i.imgur.com/ceWN69r.png)](https://www.youtube.com/watch?v=zcX7OJ-L8XQ "Visual Demo of Mining")

<Quiz questionId="a604786d-175c-4396-b251-e5e7f76e7371" />

## Resources

The following are additional recommended, but optional, readings and viewings to learn more about mining:

- [What does it mean to mine Ethereum?](https://docs.ethhub.io/using-ethereum/mining/)
- [Top Ethereum Miners](https://etherscan.io/stat/miner?range=7&blocktype=blocks)
- [Etherscan Mining Calculator](https://etherscan.io/ether-mining-calculator)

<SubmitQuiz />
