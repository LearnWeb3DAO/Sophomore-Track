# What is Gas?

Gas is one of the most important and fundamental aspects of understanding the Ethereum network.

> Gas is the fuel that allows it (Ethereum) to operate, in the same way that a car needs gasoline to run.

While going through the Freshman track tutorials, you may have noticed that transactions made on the Ethereum network require the users to pay a transaction fee.

- How is this transaction fee calculated?
- How much ETH do you need to pay for a transaction?
- Why are some transactions more expensive than others?
- Why do Gas Fees exist?

The answer to these questions lie within the concept of gas.

A recent upgrade, the London Upgrade of August 2021, slightly changed how transaction fees are calculated and how gas works. For that reason, we will break this tutorial into two sections:

- Pre-London Upgrade
- Post-London Upgrade

The Pre-London Upgrade is good to understand and easier to wrap your head around initially than the Post-London Upgrade, and also provides the motivation for the upgrade.

## Gas: General Concepts

Just like how `seconds` are a unit of time, and `metres` a unit of distance, `gas` by itself is a unit of computation on the Ethereum Network.

The **gas unit** is used to measure the amount of computational effort required to execute a transaction on Ethereum. Since each transaction requires some computation resources to execute, it requires a fee, commonly called **Gas fees** or **Transaction fees**.

Gas fees are paid in Ethereum's native currency - **ether** or **ETH**. How gas fees is calculated is slightly different pre- and post-London Upgrade.

> NOTE: Generally when someone says 'Gas' - they refer to 'Gas Fees' not the unit itself. However, for the purposes of this tutorial, we will be technically correct and say 'Gas' when referring to the unit, and 'Gas Fees' when referring to the fees in Ether.

## Pre-London Upgrade

Before the London Upgrade took place, how much ether you needed to pay for a transaction was calculated using a simple formula.

`gas fees = gas spent * gas price`

- **Gas Spent** is the total amount of gas (in gas units) that were used to execute the transaction
- **Gas Price** is the amount of ether you're willing to pay per gas unit of execution

Gas prices are denominated in **gwei** - a denomination of ETH.

1 Gwei = 0.000000001 ETH

1 ETH = 10^9 Gwei

So instead of saying your gas price is 0.000000001 ETH, you can say your gas price is 1 Gwei.

> Gwei stands for Giga-Wei, which is equal to 1,000,000,000 (10^9) wei. Wei is the smallest denomination of ETH. 1 ETH = 10^18 Wei.

### Example

The cheapest transaction, in terms of amount of gas required to execute, is just the transfer of ETH from one account to another. This transaction costs 21,000 gas units.

Suppose Alice wanted to pay Bob 1 ETH. The gas cost is 21,000 gas. Assume the gas price is 200 Gwei.

Therefore, `gas fees = 21,000 * 200 = 4,200,000 Gwei = 0.0042 ETH`

So, when Alice sends the money, 1.0042 ETH will be deducted from her account, and Bob will receive 1 ETH. The 0.0042 ETH fees goes to the miner who mined the block containing Alice's transaction.

<Quiz questionId="3658c2e2-355d-4a38-a08f-d2990ec4bd79" />
<Quiz questionId="ae1ac31d-8a45-436f-99f3-275fc17c0d45" />

**You may be wondering how the gas price was set to 200 Gwei?**
How much the gas price is set to is upto the user. Transactions with higher gas price have higher priority to be included in a block, as miners receive a higher tip for mining those first. Gas price, therefore, basically works like an open auction, or a bribe to miners. Whoever is willing to pay the highest price, or highest bribe, to the miners, gets their transaction included faster than the lower priced ones.

<Quiz questionId="684baf6d-328b-4ad3-b176-38b06d750d11" />

Wallets like Metamask provide reasonable estimates for gas prices based on current network conditions for transactions to be executed - therefore most users don't need to touch the gas price values themselves. (Though, you can enable modification through Metamask settings)

### Gas Cost Calculation

When a smart contract is compiled into bytecode, before deployment to the Ethereum network, it is compiled down to OPCODES. These are simple operations that can run directly on the Ethereum Virtual Machine. You can think of them as analogous to basic operations that can run directly on your Intel or AMD CPU. These OPCODES include basic operations like `ADD`, `MUL`, `DIV`, `SUB`, `SHA3`, etc.

Each OPCODE has a fixed gas cost. The gas cost of a specific function within the smart contract is the sum of the gas costs of all it's OPCODES. [You can find a list of all OPCODES and their associated gas costs here if interested.](https://github.com/crytic/evm-opcodes)

Therefore, more complex transactions which require more OPCODES to execute end up using more gas (units) than simpler transactions like transferring ETH from one account to another.

<Quiz questionId="3a867bc5-bbec-4e39-95b2-ba0e3b52257b" />

### Gas Limits

Now, you can imagine that there exist a lot of functions that are much more complicated than just sending ETH from one account to another. Those which involve loops, or randomness, or rely on user input.

For such functions, it can be hard to predict exactly the amount of gas that will be required for execution as it depends on other variables.

As such, instead of specifying the exact gas cost when deciding how much fees to pay for the transaction, you can specify an upper bound limit.

**Gas Limit** refers to the maximum amount of gas (units) you're willing to use for the transaction. This is set by the user.

Again, wallets like Metamask provide reasonable estimates.

**If your transaction uses less gas than your limit, the unspent gas is refunded to your account.**

Your wallet therefore must have `gas limit * gas price` ether to pay for gas when sending the transaction. Any unspent gas will be refunded when the transaction gets executed and mined.

**If, however, your transaction uses more gas than your limit, the transaction will fail and your gas will be gone.**

### Block Gas Limits

In addition to user specified gas limits per transaction, the Ethereum network also imposes a limit on the maximum amount of gas (units) allowed in a single block.

This is done to ensure that each block stays within an allowable range of computational cost. Since more complex transactions take longer to execute, this makes sure that nodes don't start falling out of sync with the rest of the network due to increased computational complexity.

## Post-London Upgrade

On August 5th, 2021 - the London Upgrade was implemented on the Ethereum network. This upgrade primarily introduced three benefits:

- Better gas fees estimations
- Quicker transaction inclusion
- Burning a percentage of ETH being used as transaction fees

For the purposes of this article, we are primarily interested in the first two points.

<Quiz questionId="98264d20-85a0-48e2-b1fa-a435814bee79" />

Prior to the London Upgrade, wallets like Metamask would provide estimates for gas prices based on past network activity. Every wallet used their own methodology to do so. Metamask, specifically, scanned the last 1000 blocks on Ethereum, and predicted the gas price for your transaction.

Starting with the London Upgrade however, every block was set to have a **base gas price fees**. This was the **minimum** price per unit of gas for getting your transaction included within this block. This was calculated natively by the network based on the demand for block space. These base fees would go on to be burnt by the Ethereum network, therefore forever getting rid of that ETH to offset issuance. Since Ethereum does not have an overall maximum supply (unlike Bitcoin, which has a maximum supply of 21M Bitcoins), the burn helps the ETH supply reach an equilibrium by not inflating it infinitely.

In addition to the base fees, the concept of **tipping (priority fees)** was introduced. As the base fees got burnt, the tip was there to compensate miners for executing and propogating user transactions. This is again set by most wallets automatically, though you can choose to set this manually. Higher tip transactions tend to get higher priority.

With this upgrade, the formula to calculate gas fees changed to the following:

`gas fees = gas spent * (base fees + priority fees)`

<Quiz questionId="5608fd5f-9f59-410c-a556-01408de738b0" />

### Example

Going back to the earlier example, if Alice had to pay Bob 1 ETH, the gas cost (in units) is 21,000. Suppose the base fees is 100 Gwei, and Alice decides to include a tip of 10 Gwei.

`total gas fees = 21,000 * (100 Gwei + 10 Gwei) = 2,310,000 Gwei = 0.00231 ETH`

<Quiz questionId="eeb05397-e992-4e06-ac66-e87001c9d3cb" />

### Variable Block Sizes

Prior to the London Upgrade, the block gas limit was constant for all blocks. Each block had a maximum capacity of 15M gas. In times of high demand, this resulted in bad user experience, as blocks were operating at full capacity, and users had to wait for the demand to reduce to get included in a block.

The upgrade introduced variable size blocks to Ethereum. Each block now has a **target** gas limit of **15M gas**, but the size can increase or decrease along with network demand, up until a maximum of **30M gas**.

<Quiz questionId="857488bf-5f85-4fd5-b6ff-4686799f69f2" />

On average, the network achieves equilibrium around 15M gas by modifying the block size and base fees.

**If the block gas is greater than the 15M target, the base fees for the next block is increased. Similarly, if the block gas is smaller than the 15M target, the base fees for the next block is decreased.** The amount by which the base fees gets adjusted is dependent on how far the block gas was from the 15M target.

Take some time to read the last couple of paragraphs and fully grasp them - it is pretty fascinating stuff, but can be a little tricky to wrap your head around.

### Variable Base Fees

Let's look at what happens to the base fees in times of high network demand.

The base fees is increased by a maximum of 12.5% per block if the target 15M gas is exceeded. This exponential growth makes it financially non-viable for block gas to remain high indefinitely, therefore allowing nodes to stay synced with the network and not constantly be executing 30M gas blocks.

![](https://i.imgur.com/q5fEwNn.png)

In this example, Block 2 saw the maximum increase possible from the target 15M to 30M. As a result, the base fees for block 3 was increased by 12.5% from 100 Gwei to 112.5 Gwei.

Similarly, since block 3 also hit the maximum limit of 30M gas, which is the maximum possible distance from the target, the base fees for block 4 was again increased by 12.5% to 126.6 Gwei. And so on...

This kept happening, and by block 8, the base fees was 202.7 Gwei. A 102.7% increase from 7 blocks ago! By block 100, base fees was 10302608.6 Gwei - this is insane (and also unrealistic). **This means that a simple ETH transfer at block 100 will cost you (21000 \* 10302608.6 Gwei) = 216 ETH.**

Due to this exponential increase in base fees, it can be noted that it is extremely unlikely to see extended spikes of full blocks.

It should be noted that the base fee is also decreased by a maximum of 12.5%, helping the spikes return to equilibrium after traffic begins to slow down.

### Better Gas Estimation

Relative to the Pre-London Upgrade mechanics, this base fee mechanism change allowed fee prediction to be much more reliable. Following the above table, to create a transaction in block number 9, the wallet can let the user know with 100% certainty that the **maximum base fees** to be added to the next block is `current base fees (base fees of prev block) * 112.5%` = `202.8 * 112.5/100` or `228.1 Gwei`. Conversely, the **minimum base fees** could be determined knowing a decrease could only be 12.5%: `current base fees (base fees of prev block) * 87.5%` = `202.8 * 87.5/100` or `177.45 Gwei`.

Therefore, wallets now know a minimum and maximum range of base fees to provide to the user when supplying estimations. The minimum is the `current base fees * 87.5%`, and the maximum is the `current base fees * 112.5%` The user can then just adjust the tip, which is usually a fraction of the base fees, for the miner.

<Quiz questionId="af9eef82-6a4b-49b6-be31-67fb539e68bd" />

## Why does Gas exist?

Gas fees help keep the Ethereum network secure. By requiring a fee for every computation executed on the network, bad actors are prevented from spamming the network.

In order to avoid accidental or malicious infinite loops in smart contracts, which would cause all Ethereum nodes to forever be stuck, gas limits on transactions set a limit to how much computation a transaction can use.

Code like this will use up all the gas provided, upto the limit, and then the transaction will fail:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Gas {
    uint public i = 0;

    // Using up all of the gas that you send causes your transaction to fail.
    // State changes are undone.
    // Gas spent are not refunded.
    function forever() public {
        // Here we run a loop until all of the gas are spent
        // and the transaction fails
        while (true) {
            i += 1;
        }
    }
}
```

The fundamental unit to make all of this possible is Gas.

<Quiz questionId="e1a8370e-558b-482b-a46e-6f1d1ce20595" />

## Reducing Gas Fees

High fees on Ethereum is a hot topic these days. The Ethereum community has solemlny swore to not hurt the decentralization or security of the network. As such, tradeoffs were made in favour of security which leads to the Ethereum network currently having higher transaction fees than other blockchains such as Solana, which made the tradeoff in favour of lower fees at the expense of security and decentralization.

Ethereum's fundamental goal is to be a **highly secure** and **highly decentralized** blockchain network capable of executing smart contracts.

But, none of that matters if users have to keep spending hundreds of dollars to move one dollar around.

As such, there are a lot of things being worked on, and some already available, to allow for a reduction in gas fees and improve the user experience.

Primarily, the network upgrades that will be offered by Ethereum 2.0 (also known as Eth2) will ultimately address some of the gas issues, which in turn will enable the network to process thousands of transactions per second and scale globally.

In addition, a lot of work is being done on Layer 2 Scaling. We will learn much more in depth about Layer 2 Scaling and Layer 2 platforms later on, but essentially they are networks which shift the heavy computation aspects of smart contracts somewhere else, and use the Ethereum mainnet as a final settlement layer, thereby inheriting the security and decentralization benefits of Ethereum, and maintaining low gas fees and high transaction speed for users.

## Resources

The following resources are recommended, but optional, readings/viewings to learn more about gas:

- [Video: Ethereum Gas Explained](https://www.youtube.com/watch?v=AJvzNICwcwc)
- [The London Upgrade](https://ethereum.org/en/history/#london)
- [Gas optimizations in smart contracts](https://medium.com/coinmonks/8-ways-of-reducing-the-gas-consumption-of-your-smart-contracts-9a506b339c0a)
- [More on Layer 2 Scaling](https://ethereum.org/en/developers/docs/scaling/layer-2-rollups/)

<SubmitQuiz />
