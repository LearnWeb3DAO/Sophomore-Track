# Providers, Signers, ABIs, and Approval Flows

There are a few topics that we wanted to provide some information on, but weren't detailed enough to deserve their own levels. This level is meant to group together a bunch of separate topics like that, and go over some things that will be helpful to keep in mind.

## Table of Contents

- [Providers and Signers](#providers-and-signers)
- [BigNumbers](#bignumbers)
- [ABI](#abi)
- [ERC20 Approval Flow](#erc20-approval-flow)

## Providers and Signers

When building interfaces to smart contracts, you will commonly come across these two terms - `Provider` and `Signer`. While you will develop a much better understanding of them when you actually start using them, we will try to provide a brief explanation of what they mean.

We know that to read or write data to the blockchain, we need to be communicating through an Ethereum node. The node contains the blockchain state, allowing us to read data. It can also broadcast transactions to miners, allowing us to write data.

Notice how the node only needs to broadcast transactions in case you want to write data to the blockchain. Since if you're just reading data that already exists, miners don't need to do anything about it, they've already done their work.

A `Provider` is an Ethereum node connection that allows you to read data from its state. You will use a `Provider` to do things like calling read-only functions in smart contracts, fetching balances of accounts, fetching transaction details, etc.

A `Signer` is an Ethereum node connection that allows you to write data to the blockchain. You will use a `Signer` to do things like calling write functions in smart contracts, transferring ETH between accounts, etc. To do so, the `Signer` needs access to a `Private Key` it can use for making transactions on behalf of an account.

Additionally, a `Signer` can do everything a `Provider` can. You can do both, reading and writing, using a `Signer`, but a `Provider` is only good for reading data.

Wallets like Metamask, by default, inject a `Provider` into your browser. Therefore, dApps can use your Metamask `Provider` to read values from the blockchain network your wallet is connected to.

However, sometimes, you want the users to make transactions, not just read data. Of course Metamask can't just go around sharing your private keys with random websites - that would be crazy. For this, Metamask also allows websites to _request_ a `Signer`. So when a dApp attempts to send a transaction to the blockchain, the Metamask window pops up asking the user to confirm the action.

<Quiz questionId="bf7ddd39-e78e-45b2-bfd6-5eaa73de33a9" />
<Quiz questionId="3cba0af8-8cea-4275-9e17-2127b6a17e16" />

## BigNumbers

When learning Solidity, we have been reading about and using `uint256` a lot. `uint256` has a range from `0` to `(2^256) - 1`. Therefore, the maximum number that can be held by the `uint256` datatype is astronomically large.

Specifically, the maximum value for `uint256` is:

`115792089237316195423570985008687907853269984665640564039457584007913129639935`

For comparison, one million is:

`1000000`

Clearly, `uint256` can hold insanely large numbers. But this presents a problem.

We usually build interfaces for smart contracts in Javascript. Javascript's `number` data type has a significantly smaller upper limit.

Specifically, the maximum value of a numeric type that Javascript can hold is just:

`9007199254740991`

That is not even close to what `uint256` can hold!

So, let's say we use Javascript to call a function on a smart contract that returns a `uint256`. If that number is greater than Javascript's maximum numeric value, which is quite definitely possible, then what happens?

Well, as it turns out, Javascript can not support that. Therefore, we have to use a special type called a `BigNumber`. Libraries used for interacting with Ethereum nodes - `ethers.js` and `web3.js` - both come with support for `BigNumber`s.

`BigNumber` is a custom class library, written in Javascript, that introduces it's own functions for mathematical functions - `add`, `sub`, `mul`, `div`, etc. `BigNumber` has a significantly larger capacity for numbers than what Javascript can natively support.

When we write code in the following levels, you will come across mathematical operations being done by calling functions like `.add()` and `.mul()` instead of the typical `+` and `*` operators that we know - this is because when we are working with `BigNumber`s we need to use it's mathematical functions.

As to what would happen if we were to try and do this with Javascript numbers, well we would very easily overflow or underflow. This means our calculations would be entirely incorrect and undefined. So, just keep this in mind.

<Quiz questionId="fc0c3825-e02b-42f1-972b-6b4212c9d848" />

## ABI

ABI stands for **A**pplication **B**inary **I**nterface. It is one of the more tricky things to understand when working with Ethereum, but we will try to explain it as best as we can.

In Freshman tutorials, and in tutorials you will come across further, you will be using `ABI`s heavily.

When Solidity code is compiled, it is compiled down to bytecode which is essentially binary. It contains no record of function names that existed in the contract, what parameters they contain, and what values they return.

However, if you want to call a Solidity function from a web app, you need a way to call the correct bytecode in the contract. To do this, you need a way to convert human-readable function names and parameters into bytecode and back.

ABIs help us achieve exactly that. When you compile your Solidity code, an ABI is automatically generated by the compiler. It contains rules and metadata about functions present in the contract, which help in doing the proper data conversion back and forth.

So, when you want to call a contract, you need its address (of course), but you also need to provide its ABI. Libraries like `ethers.js` use the ABI to encode and decode the human readable functions into bytecode and back when communicating with an Ethereum node and calling functions in smart contracts.

<Quiz questionId="0db9d7eb-d646-4c24-b584-f21ca31d14cf" />

## ERC20 Approval Flow

In the past, we learnt about `payable` functions which allow smart contracts to accept an ETH payment when a function is called. This is really useful if you want to charge users in ETH in exchange for something - for example an NFT sale.

But, what if you want to use something other than ETH for payment? What if you wanted to use your own Cryptocurrency that you deployed for payment?

Things get a little tricky here.

Since ETH is the native currency of Ethereum, and the ERC20 standard was introduced much later after Ethereum was invented, they don't behave exactly the same way. Specifically, accepting payments in ERC20 tokens is not as simple as just making a function `payable` in Solidity.

The `payable` keyword is only good for ETH payments. If you wanted to use your own ERC20 cryptocurrency, the flow to do that is a little more complicated.

First, let's think about this a little bit.

- Okay, so you cannot _send_ ERC20 tokens along with a function call, like you can do with ETH
- Perhaps the smart contract can somehow _pull_ tokens from the function caller's account?
- But that would mean I can code a smart contract that steals everyone's tokens if someone makes a transaction with my contract
- So we need a safer way to _pull_ tokens out of someone's account

Here is where the `Approve and Transfer` flow comes in.

The `ERC20` standard comes with the concept of **Allowance**.

![](https://i.imgur.com/jGlNcLH.png)

Let's try to think about this with the help of an example.

---

- Alice wants to sell her NFT
- Alice wants to accept payment for her NFT in her own cryptocurrency, AliceCoin
- Alice's NFT costs 10 AliceCoin
- Bob owns AliceCoin
- Bob wants to buy Alice's NFT
- Bob needs a way to call a function on Alice's NFT smart contract, which would take the 10 Alicecoin payment, and send him his NFT
- Since smart contracts cannot accept Alicecoin as payment directly, Alice has coded up the `ERC20 Approval and Transfer` flow in her NFT contract

---

`Alicecoin` is an ERC20 token. ERC20 comes inbuilt with a few functions that relate to the Allowance concept.

#### `approve(address spender, uint256 amount)`

This allows a user to `approve` a different address to spend up to `amount` tokens on their behalf. i.e. This function provides an Allowance to the `spender` of up to `amount`

#### `transferFrom(address from, address to, uint256 amount)`

Allows a user to transfer `amount` token from `from` to `to`.

If the user calling the function is the same as the `from` address, tokens are removed from the user's balance.

If the user is someone other than the `from` address, the `from` address must have in the past given the user allowance to spend `amount` tokens using the `approve` function.

---

Continuing with the example now:

- Bob gives Alice's NFT contract the allowance to spend up to 10 of his `Alicecoin` using the `approve` function
- Bob calls the function to purchase Alice's NFT on her NFT contract
- The purchase function internally calls `transferFrom` on `Alicecoin` and transfers 10 Alicecoin from Bob's account to Alice's account
- Since the contract was given the allowance to spend up to 10 Alicecoin by Bob earlier, this action is permitted
- Alice therefore receives her 10 Alicecoin, and Bob receives his NFT

---

Ok, so what does this mean for us?

Well, note how Bob had to give approval to the contract, so the contract could _pull_ Bob's tokens from his account.

Therefore, Bob essentially had to do two transactions to replicate the behaviour of what could be done in one transaction if payment was being accepted in ETH.

**Transaction 1** - Give allowance to the contract<br>
**Transaction 2** - Call the contract function, which internally uses the allowance to transfer Bob's tokens to a different address

So, if you were building a dApp where you needed users to pay your smart contract using an ERC20 token, you would also need to make them do both transactions. Simply calling your contract function, without first having your users provide allowance to your contract, will cause the function call to fail.

---

We will come across a use-case of this flow when building the DeFi-Exchange in the final level of Sophomore track. Since an exchange involves being able to convert one token of another, you need to call a function on the exchange smart contract which takes in one token and gives you another.

To take in your token to swap, the exchange contract requires approval to _pull_ the token from your account.

So keep this flow in mind if you're wondering why swapping on the exchange can use two transactions, instead of one.

<Quiz questionId="0b47d3bb-35ee-4b01-837c-876196b94501" />

<SubmitQuiz />
