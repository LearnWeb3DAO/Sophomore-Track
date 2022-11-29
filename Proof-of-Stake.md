# What is Proof of Stake?

Ethereum, unlike Bitcoin, uses a consensus protocol called **Proof of Stake (PoS)**. This allows all nodes on the Ethereum Network to agree on the current state of the blockchain, and secures the network against a variety of attacks.

> NOTE: Prior to September 15, 2022, Ethereum used to use the **Proof of Work (PoW)** consensus protocol, as Bitcoin does. A major update to the network - colloquially referred to as 'The Merge' updated the consensus protocol from PoW to PoS.

## What is consensus?

Merriam-Webster defines the word 'consensus' as follows.
![](https://i.imgur.com/i70BQqF.png)

## What is a consensus protocol?

When it comes to blockchains like Ethereum, which are essentially distributed decentralized databases, the network nodes need to reach an agreement on the network's current state. This state includes things like "What is Haardik's balance of ETH right now?". All the different nodes on the network better have the same value, otherwise it's a problem.

Consensus protocols help us achieve the agreement, or consensus, on the state of the network at a given point. Although they are not directly related to building dApps, understanding them will help you understand a lot of other concepts and build your fundamentals.

They are primarily economic systems that help prevent certain kinds of attacks. Theoretically, an attacker could compromise consensus by controlling 51% of the network. Consensus protocols are designed to make this '51% attack' economically unfeasible. Different mechanisms are engineered to solve this problem differently.

## Digging into Proof of Stake

Proof of Stake underlies the consensus mechanism used by the Ethereum network (and many other blockchains). The idea is as follows:

- Validators stake capital in the form of $ETH tokens into a smart contract on Ethereum
- The staked ETH acts as collateral and can be destroyed if the validator behaves maliciously (lies about the state, or behaves lazily i.e. doesn't respond to requests)
- The validator is then responsible for checking that new blocks being formed are valid, and occasionally creating those blocks themselves

With the stake in place, lying as a validator can cause you to lose your staked capital. Therefore, validators are financially incentivized to not lie about their actions.

### Advantages of PoS over PoW

The Proof of Stake system has a variety of improvements over Proof of Work.

- Significantly less energy usage - moving Ethereum from PoW to PoS reduced the world's energy usage by 0.2%. On the scale of the entire world, that's a LOT of energy that was saved.
- Lower barrier to entry - in PoW miners needed to have expensive, specialized hardware to produce new blocks. This is no longer the case with PoS and the hardware requirements are much more "average joe" affordable.
- Reduced centralization risk - due to the cost and specialized knowledge of setting up mining farms in PoW, there was a centralization risk associated with Ethereum being a PoW network. With PoS, since validators just need to stake some capital up front with much more reasonable hardware requirements, it should lead to much more nodes securing the network than in PoW
- Reduced ETH issuance - due to low energy requirements, less ETH is required as a reward to incentivize validators leading to an overall reduced ETH issuance that decreases inflation on the network
- Economic penalties for 51% attacks are exponentially more costly with the required staked capital up front compared to cost of hardware on PoW

### Block Production

Under Proof of Stake, validators are responsible for occasionally creating new blocks, and rest of the times validating blocks produced by other validators.

To become a validator, a user must deposit 32 ETH into the deposit contract, and run the required node software - an execution client, a consensus client, and a validator client.

> NOTE: While 32 ETH may seem like a high cost to run a validator node, it is significantly less than what miners were spending in PoW, and is also significantly less than costs of being a validator on other PoS blockchains.

Then, validators will receive new blocks from peers on the Ethereum network. The validator re-executes the transactions in that block, and confirms that the block is valid. Then they send an attestation (a vote) in favor of the block across the network. If enough votes are collected in favor of the block, the block is added to the chain, and then we rinse and repeat for the next block.

Every 12 seconds, the time of a new block on Ethereum, a validator is randomly selected to be a block proposer. If selected as a proposer, this validator is responsible for creating a new block and sending it out to the other nodes, which then can vote in favor of or against that block.

<Quiz questionId="7b6261a0-12e1-417c-953e-5bd35f474b47" />

### Network Security

Becoming a validator is a commitment towards helping secure the Ethereum network. The validator is expected to maintain sufficient hardware, internet connectivity, and uptime to participate in block proposal and validation. In return, the validator is paid rewards in ETH.

If a validator fails to meet it's commitments, they miss out on ETH rewards. By failing to participate when called upon (asked to vote on a block, for example) they fail to get the ETH reward. If they behave dishonestly or maliciously, then their stake can also be slashed leading to a financial loss and them being kicked out from the position of being a validator.

Behaving maliciously could include things like proposing multiple blocks in a single 12 second time period, or submitting false attestations during block validations. The amount of ETH that gets slashed by doing such malicious activities depends on how many _other_ validators also got slashed around the same time. This is because a single validator's internet connection going out shouldn't have the same penalty as 100 validators coordinating to act maliciously together as an attack to the network.

<Quiz questionId="511aca86-18ec-4aff-b8f1-3efde336c82f" />

### Sybil Resistance

Technically speaking, proof of stake is **not** a consensus protocol by itself - though it is often referred to as such for simplicity. It is actually a **Sybil resistance mechanism** and **block producer selector** - a way to decide who is going to be the producer of the latest block.

**Sybil Resistance** measures how a protocol would do against a **sybil attack**.

A **sybil attack** is the problem where one user or entity pretends to be many different users or entities. Security against this type of attack is essential for a decentralized blockchain to allow validators to be rewarded based on the resources they put in, not just random selection.

Hypothetically, if we were to choose the producer of a block randomly instead of Proof of Stake, one could execute a Sybil attack quite easily.

Suppose there are 2 validators on the network - Alice and Bob. By random selection, they should get 1/2 of the validator rewards each. Now Charlie comes along, and pretends to be 2 different users - Charlie and Darcy. By random selection now, Charlie would end up receiving 1/2 the validator rewards as he is pretending to be 2 different users, whereas Alice and Bob only get 1/4 each instead of 1/3 which they should.

Proof of Stake protects against Sybil attacks by making validators put up their stake as collateral, therefore having them "put their money where their mouth is". The more number of validators you want to act as, the higher stake you need to put up. If you act maliciously, there is a higher amount of collateral at risk of getting slashed. This acts as an economic deterrent to Sybil attacks.

<Quiz questionId="2a87a5ed-2aaa-42e3-9fe7-88f109c358b6" />

<Quiz questionId="f4b7dd29-3bb1-4bc8-b11c-ea6d038549b7" />

### Chain Selection Rules

Occasionally, due to internet latency issues or something, it is possible for validators to have two different views of the head (current state) of the blockchain, and therefore two different views towards what constitutes a valid new block. The technical term for this is called a **fork**.

Normally, when the network is performing optimally and honestly, there is only one block at the head of the chain, and all validators agree on that state.

<Quiz questionId="0f2b642a-ea18-4dc8-bfc8-3d34995a7e26" />

In the case however the first scenario happens, and different validators end up with different views of the head of the chain, a way to converge back to a single chain and therefore a single state is important. The "correct chain" needs to be chosen to prevent the splitting of state and a fork to continue.

To do this, Ethereum uses an algorithm called [LMD-GHOST](https://arxiv.org/pdf/2003.03052.pdf) which works to identify which fork has the greatest amount of assestations in it's history. So for example, if the chain splits into two forks at a point, and different validators believe differently, LMD-GHOST will figure out which of the two forks received a higher number of attestations i.e. was approved by more validators on the network, and then all validators will converge on resuming from that fork and say goodbye to the other one.

Occasionally, this algorithm also means that transactions which get validated as part of a temporary fork may be rolled back once the fork is gotten rid of in favour of a different fork. This introduces the concept of **Finality**.

### Finality

A transaction has "finality" on Ethereum when it's part of a block that cannot be changed, i.e. the transactions in that block cannot be rolled-back, at least not feasibly.

On PoS Ethereum, this is managed through the use of "checkpoint" blocks. Every 32 blocks, called an epoch, a checkpoint block is produced. Along with voting for individual blocks, validators also vote for pairs of checkpoint blocks every epoch.

So for example, if `Block n` was a checkpoint block, then `Block n+32` is also a checkpoint block. Validators vote for the pair `(Block n, Block n+32)` that they consider to be a valid sequence of the chain. If enough votes are collected in favor of the pair, then:

1. The earlier block (`Block n`) is marked as `finalized`
2. The later block (`Block n+32`) is marked as `justified`

> NOTE: The earlier block must have already been `justified` when it was voted for during the epoch (`Block n-32, Block n`).

<Quiz questionId="9d57e6c4-fe20-4d05-b504-e89950f873bc" />

Once a block enters `finalized` state, an attacker would need to spend a LOT of money trying to revert it. In Proof of Stake systems, it is theoretically possible to rewrite some of the history if you control 33% of the entire stake. So, to revert a `finalized` block, an attacker would need to commit to losing at least one-third of the total supply of staked ETH.

Specifically, to do that, the attacker could vote against the pair of checkpoint blocks (`Block n, Block n+32`) to never let `Block n` be finalized.

Aside from costing a massive amount of money, the consensus mechanism does have a way to solve for this. If no blocks are finalized for more than four epochs (128 blocks), the network starts slashing staked ETH of the minority which is trying to vote against the majority and not letting the vote pass, allowing the majority to regain power and finalize the chain.

<Quiz questionId="2af16baa-b6a2-4765-9ec3-a7ae68556550" />

### The Downsides

Not all is rainbows and unicorns in this world however, and with it's many advantages, PoS does have a few cons that continue to make consensus mechanisms an active field of research and innovation.

1. Proof of Stake is younger and less battle-tested than Proof of Work, which has stood the test of time ever since Bitcoin was created till date.
2. Proof of Stake protocols are much more complex to implement in code, technically, which also increases the surface area for possible bugs and issues to arise.

Due to this, many in the Ethereum community (and outside) are still actively researching and working on ways to keep improving consensus mechanisms.

## Resources

The following are recommended, but optional, readings/viewings to understand more about Proof of Stake.

- [Proof of Stake FAQ by Vitalik Buterin](https://vitalik.ca/general/2017/12/31/pos_faq.html)
- [Why PoS Matters](https://bitcoinmagazine.com/culture/what-proof-of-stake-is-and-why-it-matters-1377531463)
- [LearnWeb3's older lesson on Proof of Work](https://github.com/LearnWeb3DAO/Sophomore-Track/blob/main/Proof-of-Work.md)
