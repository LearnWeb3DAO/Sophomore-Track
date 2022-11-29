# What is Proof of Work?

Ethereum, like Bitcoin, used to use a consensus protocol called **Proof of Work (PoW)**. This allows all nodes on the Ethereum Network to agree on the current state of the blockchain, and secures the network against a variety of attacks.

> NOTE: Recently on September 15 2022, the Ethereum Merge completed and Ethereum shifted to a Proof of Stake system. We have added a new lesson to the Sophomore track (the very next one) that explains Proof of Stake, but it is still helpful to understand Proof of Work first.

## What is consensus?

Merriam-Webster defines the word 'consensus' as follows.
![](https://i.imgur.com/i70BQqF.png)

## What is a consensus protocol?

When it comes to blockchains like Ethereum, which are essentially distributed decentralized databases, the network nodes need to reach an agreement on the network's current state.

Consensus protocols help us achieve the agreement, or consensus, on the state of the network at a given point.

Although consensus protocols are not directly related to building dApps, understanding them will help you understand a lot of other concepts and build your fundamentals.

Consensus protocols are primarily economic systems that help prevent certain kinds of attacks. Theoretically, an attacker could compromise consensus by controlling 51% of the network. Consensus protocols are designed to make this '51% attack' economically unfeasible. Different mechanisms are engineered to solve this problem differently.

## What is Proof of Work?

Proof of Work is a type of consensus protocol, famously used by Bitcoin and Ethereum.

### Block Production

Under Proof of Work, miners are responsible for producing new blocks. Miners in the network compete against each other to create new blocks full of processed transactions. The winner then shares the block with the rest of the network, and earns freshly minted ETH for their hard work.

The race is won by whoever's computer can solve a computationally hard mathematical puzzle the fastest. The problem is computationally hard to solve, but very easy to verify. The solution to this problem is the 'certificate of legitimacy' we discussed in the `What is Mining?` tutorial.

<Quiz questionId="c400c10a-e01c-437e-8ad9-ff78b3fa2f4f" />

### Network Security

The network is kept secure by the fact that to gain 51% control over the network, you would need 51% of the network's computational power. However, since Proof of Work incentivizes miners with mining rewards, a lot of different groups of miners are interested in running mining nodes. Therefore, getting 51% of all computational power on a network requires **huge** investments in equipment and electrical energy - which means you're likely to end up spending more than you would earn.

<Quiz questionId="1b20b974-144b-42bd-b12b-a921cc707641" />

## Sybil Resistance

Technically speaking, proof of work is **not** a consensus protocol by itself - though it is often referred to as such for simplicity. They are actually **Sybil resistance mechanisms** and **block producer selectors** - a way to decide who is going to be the producer of the latest block.

**Sybil Resistance** measures how a protocol would do against a **Sybil attack**.

A **Sybil attack** is the problem when one user or group pretends to be many different users. Security against this type of attack is essential for a decentralized blockchain to allow miners to be rewarded based on the resources they put in, not just random selection.

Hypothetically, if we were to choose the producer of a block just by random selection instead of Proof of Work, one could execute a Sybil attack quite easily.

Suppose there are 2 miners on the network. Alice and Bob. By random selection, they should be getting roughly half of the mining rewards each. Now, Charlie comes along, but pretends to be 2 different users - Charlie and Darcy. By random selection now, Charlie would end up receiving 1/2 the mining rewards as he is pretending to be 2 different users, whereas Alice and Bob only get 1/4 each instead of 1/3 which they should.

Proof of Work protects against Sybil attacks by making miners put up a large amount of computational power as collateral, therefore having them expend a lot of electrical energy. This is done through the solving of the computation puzzle to prove that the miners are 'putting in the work'. This acts as an economic deterrent to Sybil attacks.

Since rewards are given out to successful miners, and miners become successful roughly proportional to their share of computational power on the network, it doesn't matter anymore if you're pretending to be 1 user or 2 or 100. You will get the same amount of mining rewards as your computation power stays the same.

<Quiz questionId="d20c18e6-33db-47a7-a8ca-3dd7e686b4ba" />

<Quiz questionId="4f14465c-42bb-4b7e-b1a2-8b3cfafc519b" />

## Chain Selection Rules

Occasionally, two miners will produce valid blocks roughly at the same time. This can cause different nodes in the network to include different blocks in their blockchain. The technical term for this is a **fork**.

![](https://theblockchaincafe.com/wp-content/uploads/2019/09/Types-Of-Forks-In-Blockchain-Network.png)

However, for blockchains to proceed in a stable fashion, a single contiguous chain needs to be chosen as the "correct chain" to prevent the splitting of state.

Bitcoin and Ethereum use the "longest chain" rule to do this. Whichever chain is accepted by a higher number of nodes and continues to grow longer is chosen as the 'correct chain', and the forked chain is gotten rid of.

The combination of Proof of Work and the longest chain rule is known as "Nakamoto Consensus" - named after Satoshi Nakamoto, the inventor of Bitcoin.

Occasionally, this means that transactions which get mined as part of a temporary fork may be rolled back once the fork is gotten rid of in favour of a different longer chain. This introduces the concept of **Finality**.

> The blocks which form the forked chain and end up getting deleted are called **Uncle Blocks**. The miner clearly put in work to produce that Uncle block, and likely just lost out on the mining reward due to network latencies. As such, the Ethereum network still rewards Uncle block miners with 1.75 ETH for their hard work.

<Quiz questionId="ace85e9d-6ab1-4582-b60e-e31cae973510" />

## Finality

A transaction has "finality" on Ethereum when it's part of a block that can't change.

Because miners work in a decentralized way, two valid blocks can get mined at the same time. This creates a temporary fork. Eventually, one of these chains will become the accepted chain after a subsequent block has been mined and added, making it longer.

But to complicate things further, transactions rejected on the temporary fork may have been included in the accepted chain. This means it could get reversed. So finality refers to the time you should wait before considering a transaction irreversible. For Ethereum, the recommended time is six blocks or just over 1 minute. After six blocks, you can say with relative confidence that the transaction was successful (more than 99.999% chance it will not be reverted now). You can wait longer for even greater assurances.

<Quiz questionId="ce8a0d42-8244-4e5a-b3b7-aacd56125d37" />

<Quiz questionId="1ba3c8dd-3f1e-4e8b-a670-6cfffbb9aa0d" />

## The 'Work' in Proof of Work

We've been talking about a computationally hard mathematical problem that miners need to solve to provide a certificate of legitimacy. But what does this actually mean?

Essentially, we want to prove that the miners expended energy and computational power to compute the block. And, that they did so faster than everyone else. If we can prove this, it means that the miner essentially spent money and time (in the form of energy and computation) so they could get a chance to propose a new block.

It is in the miners best interest to produce valid blocks, because if they are caught lying (which is very easy since verification of the certificate is easy) - they just wasted all that energy and computation for nothing and therefore lost their money and time. As such, solving the mathematical problem means the miner _really, really_ wants to propose the new block, and is willing to spend energy and computation to receive the reward.

The Ethereum proof-of-work protocol, **Ethash**, requires miners to go through an intense race of trial and error. The process goes as follows:

- The miner selects a group of transactions to include in a potential block
- Based on the block they create, the network has rules to choose a slice of data (roughly ~1GB in size) from the current state of the blockchain network. These rules are not particularly relevant, but you can read more about them in the Ethash docs.
- They put the dataset through a hashing function to calculate a `target` value. This `target` is a number, which is inversely proportionate to the mining difficulty. The higher the mining difficulty, the lower the `target`, and vice versa.
- Then, the miner uses brute force to try to find another random number called the `nonce`.
- Putting the combination of the dataset, target, nonce, and a couple other values through a hashing function should result in a number that is lower than the `target`.
- `HashFunction(dataset, target, nonce, ...) = a number`
- The higher the mining difficulty, the lower the `target`, and hence the harder it is to find a `nonce` which satisfies this condition.
- Miners keep using trial-and-error to find a valid value for the `nonce` which satisfies the condition. There is no formula to calculate the `nonce`.

> The mining difficulty becomes less or more difficult based on how many miners are on the network, to ensure that a block can be reliably produced roughly every ~15 seconds. If it becomes too easy and there are a lot of miners, blocks will be produced much faster than 15 seconds. Likewise, if it becomes too hard and there are not a lot of miners, blocks will take a long time to be produced. Difficulty is calculated by the network automatically.

There is no way to solve this other than brute force, as currently there does not exist a way to reverse hash functions. Therefore, to know if a certain `nonce`, when appended to the dataset results in a specific hash, the only way possible is to try random values for the `nonce` and check.

Take some time to read this section thoroughly, this is all there is to Proof of Work. It might seem quite anticlimactic, but think about what we started off wanting to prove. We wanted to prove that miners did hard work to produce this block, and it wasn't a piece of cake for them. Since they did hard work, it is in their best interest to not lie. This sort of computation puzzle does exactly that, because finding a valid `nonce` is a computationally hard problem - but verifying it is easy, so other nodes can easily verify that the miner did in fact find a valid `nonce`, which means they did in fact put in the work.

<Quiz questionId="f0459255-52fd-4624-a9d4-371276bf3d56" />

<Quiz questionId="834107bf-2308-4b77-92aa-eac914f5ab85" />

## How does Bitcoin actually work?

I recommend this video to everyone even slightly curious about the workings of a blockchain, and how blocks get validated and produced.

Watch the following video for a visual understanding of everything we just read about Proof of Work.

[![But how does Bitcoin actually work?](https://i.imgur.com/b6FKTLt.png)](https://www.youtube.com/watch?v=bBC-nXj3Ng4)

<Quiz questionId="8ffc9173-d394-46c9-b96d-b31ba64c3a63" />

## Resources

The following are recommended, but optional, readings/viewings to understand more about Proof of Work.

- [Anderson Brownworth Blockchain Mining Demo](https://andersbrownworth.com/blockchain)
- [What is Proof of Work? by Binance Academy](https://www.youtube.com/watch?v=3EUAcxhuoU4)
- [A technical explanation of Proof of Work by Khan Academy](https://www.youtube.com/watch?v=9V1bipPkCTU)
- [51% Attacks](https://en.bitcoin.it/wiki/Majority_attack)
- [Transaction Finality](https://blog.ethereum.org/2016/05/09/on-settlement-finality/)
- [Ethash](https://ethereum.org/en/developers/docs/consensus-mechanisms/pow/mining-algorithms/ethash/)

<SubmitQuiz />
