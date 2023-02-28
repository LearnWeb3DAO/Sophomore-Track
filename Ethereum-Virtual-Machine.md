# Ethereum Virtual Machine (EVM)

![](https://miro.medium.com/max/1024/1*tv4v0wrfVOAUmdBqgpwiMw.jpeg)

The Ethereum network exists solely for the purpose of keeping the single, continuous, uninterrupted, and immutable operation of the state machine that is the Ethereum blockchain. It is the environment in which all Ethereum accounts and smart contracts and data live. At any given block, Ethereum has one and only one globally accepted 'state'. The Ethereum Virtual Machine (EVM) is what defines the rules for computing a new valid state from block to block.

## Prerequisites

Some basic understanding of [bytes](https://en.wikipedia.org/wiki/Byte), [memory](https://en.wikipedia.org/wiki/Computer_memory), and the [stack](<https://en.wikipedia.org/wiki/Stack_(abstract_data_type)>) is required to understand the EVM.

It might also be helpful to familiarize yourself with some cryptography like [hash functions](https://en.wikipedia.org/wiki/Cryptographic_hash_function).

## Ethereum as a State Machine

Blockchains like Bitcoin are often described as 'distributed ledgers' which enable the existence of a decentralized currency using fundamental tools of cryptography.

A cryptocurrency can behave like a 'normal' currency because of the rules which govern what one can and cannot do to modify this ledger. For example, a Bitcoin address cannot spend more Bitcoin than it has previously received. These rules underpin all transactions that take place on Bitcoin, and similarly other blockchains.

While Ethereum also has its native cryptocurrency, the Ether, it also enables a much more powerful function that we have seen - Smart Contracts. For this more complex feature, we need a more powerful analogy than just 'distributed ledger'.

Instead of a distributed ledger, Ethereum can be described as a distributed [state machine](https://en.wikipedia.org/wiki/Finite-state_machine). A state machine is essentially any machine that can change from one state to another in response to certain inputs.

A simple state machine is a coin-operated [turnstile](https://i.imgur.com/Uh7m6jN.png), commonly found in subways or train stations, to prevent people from entering unless they pay using a coin or have a ticket.
![](https://i.imgur.com/66Mee9k.png)

The initial state for a turnstile is locked. In the locked state, if you keep pushing it, it remains locked. If you insert a coin, it moves to the unlocked state. If you keep inserting coins, it remains in the unlocked state. Once you push in the unlocked state (and someone passes through), it becomes locked again.

For Ethereum, the state is much more complex. It is described using a large data structure which contains all the state of the blockchain. The specific rules of how state can change from block to block is defined by the EVM.

![](https://ethereum.org/static/e8aca8381c7b3b40c44bf8882d4ab930/302a4/evm.png)

<Quiz questionId="0ccbf68c-2b00-46f7-857d-2e87a74fc409" />

<Quiz questionId="1261cc91-f40f-4938-a3c0-42b864fbbae1" />

## Ethereum State Transition

On a high level, the EVM behaves similar to a mathematical state transition function. Given the current state, and a new set of valid transactions, it produces a new state. The output is deterministic, which means that for the same input, it will always produce the same output.

```
Y(S, T) = S'
```

Given the old valid state `S`, and a new set of valid transactions `T`, the state transition function `Y` produces the new valid state `S'`.

The state in Ethereum is stored as a really large data structure called a [Merkle Patricia Trie](https://ethereum.org/en/developers/docs/data-structures-and-encoding/patricia-merkle-trie/). You do not need to understand exactly how it is structured, but if you want to, you can read the given link.

<Quiz questionId="744c27fc-dbce-4e3e-a787-ac08cd2e2fd0" />

<Quiz questionId="07331bd2-bc1b-4981-b72e-95e58e73aaf0" />

## EVM Layer

The EVM lives as a layer in the software stack of Ethereum.

![](https://i.imgur.com/QGsvKyk.png)

Ethereum nodes contain implementations of the EVM, and the EVM can then execute EVM code on it. EVM code is compiled smart contract bytecode that can be executed.

## EVM Code Generation

![](https://i.imgur.com/UfnzX63.png)

![](https://i.imgur.com/k2T5iVf.png)

<Quiz questionId="9f0a062f-205a-43b0-b12b-d241bd71a91d" />

<Quiz questionId="9b9b2c9f-bf9c-46bc-a762-497a82b86fd6" />

## EVM Instructions (OPCODES)

The EVM itself behaves as a [stack machine](https://en.wikipedia.org/wiki/Stack_machine) with a maximum depth of 1024 items on the stack. Each item in the stack is a 256-bit (32 bytes) word.

During execution, the EVM maintains a transient **memory**, as a 32 byte addressed byte array, which does not persist between transactions. The transient memory is cleared when a new transaction is being executed.

<Quiz questionId="dd67f513-69cf-423b-8507-1c6327d9e08f" />

Smart contracts, however, do maintain their own state in the blockchain. This state is also modeled as a [Merkle Patricia Trie](https://ethereum.org/en/developers/docs/data-structures-and-encoding/patricia-merkle-trie/). This is commonly refered to as the EVM **storage** during transaction execution.

The EVM has logic present that allows it to execute [EVM Opcodes](https://ethereum.org/en/developers/docs/evm/opcodes/), which perform standard operations on the stack like `XOR`, `ADD`, `AND`, `SUB`, `MUL` etc. The EVM also implements a number of blockchain-specific stack operations, such as `BALANCE` and `BLOCKHASH`.

When a smart contract is compiled into bytecode (represented in hexadecimal), it compiles down to EVM opcodes. These opcodes are what get executed on the EVM.

![](https://ethereum.org/static/9628ab90bfd02f64cf873446cbdc6c70/302a4/gas.png)

<Quiz questionId="fba38a18-183f-47e6-9070-abd3d085d87c" />

## EVM Implementations

All implementations of the EVM must adhere to the specification described in the [Ethereum Yellowpaper](https://ethereum.github.io/yellowpaper/paper.pdf). Over Ethereum's history, the EVM has undergone multiple revisions, and there now exist multiple implementations of the EVM in various programming languages.

All Ethereum clients include an EVM implementation. In addition to those, there are multiple standalone implementations as well.

### Ethereum Clients (with EVM)

- [Geth](https://geth.ethereum.org/) | Programming Language = Go
- [OpenEthereum](https://github.com/openethereum/openethereum) | Programming Language = Rust
- [Nethermind](https://nethermind.io/) | Programming Language = C# (.NET)
- [Besu](https://consensys.net/quorum/developers/) | Programming Language = Java
- [Erigon](https://github.com/ledgerwatch/erigon) | Programming Language = Go

<Quiz questionId="cf441992-ff6e-4a4f-87ce-38a21593ddbd" />

### Standalone EVM Implementations

- [Py-EVM](https://github.com/ethereum/py-evm) | Programming Language = Python
- [evmone](https://github.com/ethereum/evmone) | Programming Language = C++
- [ethereumjs-evm](https://github.com/ethereumjs/ethereumjs-monorepo) | Programming Language = Javascript
- [Enclave EVM](https://github.com/microsoft/eevm) | Programming Language = C++

<Quiz questionId="77748dc7-09c8-4288-b721-b758fb930f6e" />

## Resources

The following are recommended, but optional, readings/viewings for learning more about the EVM.

- [Ethereum EVM: Illustrated](https://takenobu-hs.github.io/downloads/ethereum_evm_illustrated.pdf)
- [EVM Opcodes](https://www.ethervm.io/)
- [Ethereum Yellowpaper](https://ethereum.github.io/yellowpaper/paper.pdf)
- [Understanding EVM](https://www.youtube.com/watch?v=RxL_1AfV7N4)
- [Merkle Patricia Trie](https://www.youtube.com/watch?v=OxofT39TJgg)

<SubmitQuiz />
