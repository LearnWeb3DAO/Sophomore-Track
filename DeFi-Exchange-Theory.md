# Diving into Decentralized Exchanges (Uniswap V1)

Most of us are used to buying crypto through centralized exchanges - Binance, Coinbase, etc. Often times, we resolve to the same platforms when trading between different cryptocurrencies.

However, centralized exchanges are rife with problems. They can get hacked and lose all their user's money, or worse yet, the company behind the exchange can close up shop and run away with all the money.

This may seem extreme, but this is not fiction.

### Mt.Gox 🏔

Mt.Gox was the leading Bitcoin exchange from 2010-2014. At it's peak, it was responsible for 70% of all Bitcoin transactions. In early 2014, the company reported they were 'missing' hundreds of thousands of Bitcoin, and declared bankruptcy. Today, those lost Bitcoin are worth billions of dollars.

In the following years, Mt.Gox faced several lawsuits, some of which are still going on 8 years later. [You can read more about what happened with Mt.Gox here](https://www.investopedia.com/terms/m/mt-gox.asp#:~:text=The%20name%20Mt.,the%20largest%20shareholder%20and%20CEO.)

### QuadrigaCX 🍁

QuadrigaCX, a centralized exchange based in Canada, went under in 2018. The founder of Quadriga mysteriously 'died' and all the crypto on the platform disappeared with him. Users reported almost $200 million in lost funds.

The Ontario Securities Commission conducted thorough research into the activities of the company, and declared that the founder of Quadriga was, simply, a fraud.

[You can read more about what happened with Quadriga here](https://www.cbc.ca/news/business/osc-quadriga-gerald-cotten-1.5607990)

> Fun Fact: There's now even a [documentary on Netflix](https://www.netflix.com/title/81349029) about this incident.

---

This is not an exhaustive list by any means, but it gives you an idea. Web3 has a common saying

> Not your keys, not your coins

which means that if you don't own your private keys, and instead trust a centralized exchange to manage them for you, you don't really own your cryptocurrency coins. This is true.

<Quiz questionId="33edec62-5683-42d6-9d60-b84217eb6d17" />

### The Birth of Decentralized Exchanges 🐣

The idea of a decentralized exchange is simple - allow users to trade their crypto directly on-chain through smart contracts without giving up control of their private keys.

While it sounds simple, the reality is much more complicated. In short, decentralized exchanges are a beautiful piece of mathematics and software combined together. We hope by the end of this article you will share the same feeling.

The birth of modern decentralized exchanges was primarily led by [Uniswap](https://uniswap.org). Not only is Uniswap the leading decentralized exchange on Ethereum, it is THE leading dApp on Ethereum in general.

After Vitalik Buterin posted a blog post on [Path Independence](https://vitalik.ca/general/2017/06/22/marketmakers.html) in 2017, Hayden Adams was inspired to try to implement Vitalik's ideas in what eventually became Uniswap.

After spending over a year working on the code, Hayden finally announced and launched Uniswap in November 2018. [You can read more about the history of Uniswap in this blog post by the founder](https://uniswap.org/blog/uniswap-history).

In this article, we will attempt to go over the mathematics that allow for Uniswap to exist and function, and hope to give you an insight into why it's so amazing.

### Why is it complicated?

You might be wondering - "why can't we just recreate a centralized exchange on-chain?"

Well, you can, but it's not good enough.

Centralized exchanges typically work on an order-book system. Alice puts up a listing saying she is willing to sell 100 of 'TokenA' for 50 of 'TokenB', and the listing is added to the order book. At some point, if Bob comes along and says he wants to buy 100 of 'TokenA' for 50 of 'TokenB' - their orders are matched together, and the trade is executed.

Order-book based exchanges were attempted on Ethereum, with the most significant example being [0xProject](https://0x.org/) but due to the high gas required for all the storage and matching algorithms, it was challenging to attract users.

There was need for a new approach, a way to allow users to swap between any two tokens arbitrarily without needing an orderbook. Additionally, cookie points if users could actually earn money by using Uniswap.

<Quiz questionId="2a97df55-c6da-4308-b738-a7fe64183bc8" />

### Uniswap V1, V2, V3

As of January 2022, three versions of Uniswap have been launched.

The first version was launched in November 2018 and it allowed only swaps between ether and a token. Chained swaps were also possible to allow token-token swaps. Chained swaps would allow for a `TokenA <> TokenB` swap by first swapping one of them for ETH, and then swapping the ETH for the second token.

V2 was launched in March 2020 and it was a huge improvement of V1 that allowed direct swaps between any ERC20 tokens, as well as chained swaps between any pairs.

V3 was launched in May 2021 and it significantly improved capital efficiency, which allowed liquidity providers to remove a bigger portion of their liquidity from pools and still keep getting the same rewards (or squeeze the capital in smaller price ranges and get up to 4000x of profits).

For the purposes of this tutorial, we will be focusing on the design of Uniswap V1, and in the following level we will actually implement a slightly simplified version of it that allows swapping between ether and a token.

<Quiz questionId="f8957397-cb9e-4309-93a2-9f5973a1a322" />

<Quiz questionId="debff206-5e79-4f3c-8035-52353f0f15fe" />

### Market Makers

Uniswap is an **Automated Market Maker**. Let's try to understand what that means.

**Market Makers** are entities that provide liquidity (assets) to trading markets. In non-orderbook systems, liquidity is what allows trading to be possible. That means if you want to sell BTC to buy ETH, the exchange must have an ETH balance you can purchase from in exchange for BTC. Some trading pairs have very high liquidity (eg. BTC/ETH trading pair), but some have extremely low or no liquidity at all (eg. scam tokens, or newly created tokens).

A DEX must have enough liquidity to function and serve as an alternative to centralized exchanges.

One way to get that liquidity is that the developers (or investors) put in their own money and become market makers. However, this is not realistic as they would need a huge amount of money to provide enough liquidity for all possible trading pairs. Moreover, this hurts decentralization, as the developers/investors would hold all the power in the market.

Another way, which Uniswap implemented, was to **let anyone be a market maker** - and this is what makes Uniswap an **automated market maker**. Any user can deposit funds to a specific trading pair and add liquidity, and in exchange earn money for doing so through trading fees taken from the users.

<Quiz questionId="54d20f2e-1452-49fd-a660-93a1795f579c" />

### Functional Requirements

Considering what we have learnt, we need to allow for the following functionality **at least** to build an automated market maker:

- Anyone can add liquidity to become a liquidity provider
- Liquidity providers can remove their liquidity and get back their crypto whenever they want
- Users can swap between assets present in the trading pool, assuming there is enough liquidity
- Users are charged a small trading fees, that gets distributed amongst the liquidity providers so they can earn for providing liquidity

### XY = K

At the core of Uniswap is one math function:

`x * y = k`

Assume we have a trading pair for `ETH / LW3 Token`

**x** = reserve balance of `ETH` in the trading pool

**y** = reserve balance of `LW3 Token` in the trading pool

**k** = a constant

This formula is responsible for calculating prices, deciding how much `LW3 Token` would be received in exchange for a certain amount of `ETH`, or vice versa.

**NOTE: It doesn't matter if we use `x` to represent the reserve of `ETH` or `LW3 Token` as long as `y` represents the opposite.**

The formula states that `k` is a constant no matter what the reserves (x and y) are. Every swap made increases the reserve of either `ETH` or `LW3 Token` and decreases the reserve of the other.

Let's try to write that as a formula:

`(x + Δx) * (y - Δy) = k`

where `Δx` is the amount being provided by the user for sale, and `Δy` is the amount the user is receiving from the DEX in exchange for `Δx`.

Since `k` is a constant, we can compare the above two formulas to get:

`x * y = (x + Δx) * (y - Δy)`

Now, before a swap occurs, we know the values of `x`, `y`, and `Δx` (given by user). We are interested in calculating `Δy` - which is the amount of `ETH` or `LW3 Token` the user will receive.

We can simplify the above equation to solve for `Δy`, and we get the following formula:

`Δy = (y * Δx) / (x + Δx)`

Let's try to code this up in Solidity.

```solidity
function calculateOutputAmount(uint inputAmount, uint inputReserve, uint outputReserve) private pure returns (uint) {
    uint outputAmount = (outputReserve * inputAmount) / (inputReserve + inputAmount);
    return outputAmount;
}
```

Assume we have `100 ETH` and `200 LW3 Token` in the contract.

**What would happen if I want to swap 1 `ETH` for `LW3 Tokens`? Let's do the math.**

`inputAmount` = 1 ETH
`inputReserve` = 100 ETH
`outputReserve` = 200 LW3 Tokens

=> `outputAmount` = `1.98019802` `LW3 Tokens`

**What would happen if instead I wanted to swap 2 `LW3 Tokens` for ETH?**

`inputAmount` = 2 LW3 Tokens
`inputReserve` = 200 LW3 Tokens
`outputReserve` = 100 ETH

=> `outputAmount` = `0.9901` `ETH`

These amounts are very close to the `1:2` ratio of tokens present in the contract reserves, but slightly smaller. Why?

The product formula we use for price calculations is actually a hyperbola.

![](https://i.imgur.com/Lo9CAct.png)

The hyperbola never intersects at `x=0` or `y=0` - this means that neither of the reserves will ever be 0 just as a product of trading! **Reserves are infinite**

<Quiz questionId="7f0f3816-d7ca-42f6-ba07-03ac8cd750b4" />
<Quiz questionId="5775c92d-83bf-4d9a-a63d-131cd1ca9879" />

### Slippage

Since we don't get tokens in the exact ratio of reserves, this leads to an interesting implication of the math. The price function causes **slippage** in the price.

The bigger the amount of tokens being traded relative to their reserve values, the lower the price would be.

Let's say I wanted to try to drain out the entire pool, and sell `200 ETH`.

`inputAmount` = 200 ETH
`inputReserve` = 100 ETH
`outputReserve` = 200 LW3 Tokens

=> `outputAmount` = `133.333` LW3 Tokens

As you can see, when we're trying to drain out the pool, we're only getting close to a half of what we expect.

Some may see this as a flaw of automated market makers that follow `x*y = k`, but it's actually not. It is the same mechanism that protects pools from being completely drained. **This also aligns with the law of supply and demand: the higher the demand relative to the supply, the more costly it is to buy that supply.**

<Quiz questionId="a6b32530-5d5f-43b2-b3d9-f9213dedb77a" />

### Who sets the initial price?

When a new cryptocurrency is created, there is no liquidity for trading pairs involving that token. As such, there is no way to calculate the price for it.

Therefore, the first person adding liquidity to the pool gets to set a price. **Adding liquidity involves adding tokens from both sides of the trading pair - you cannot add liquidity for just one side**.

When the first person adds liquidity, it creates a reserve balance and sets the initial `x` and `y` values. From that point onward, we can do price calculations when swapping between tokens.

A simple implementation of the `addLiquidity` function in Solidity would look something like this:

```solidity
function addLiquidity(uint256 tokenAmount) public payable {
    IERC20 token = IERC20(tokenAddress);
    token.transferFrom(msg.sender, address(this), tokenAmount);
}
```

This function accepts ETH and a token from the user.

However, this implementation is incomplete!

A second person can come along, and add liquidity in a completely different ratio of reserves which would massively affect price calculations. We do not want to allow for such price manipulations, and we want prices on the decentralized exchange to be as close to those on centralized exchanges as possible.

So we must ensure that anyone adding additional liquidity to the pool is doing so in the same proportion as that already established in the pool. We only want to allow arbitrary ratios when the pool is completely empty.

This leads to an implementation that looks like this:

```solidity
function addLiquidity(uint tokenAmount) public payable {
    // assuming a hypothetical function
    // that returns the balance of the
    // token in the contract
    if (getReserve() == 0) {
        IERC20 token = IERC20(tokenAddress);
        token.transferFrom(msg.sender, address(this), tokenAmount);
    } else {
        uint ethReserve = address(this).balance - msg.value;
        uint tokenReserve = getReserve();
        uint proportionalTokenAmount = (msg.value * tokenReserve) / ethReserve;
        require(tokenAmount >= proportionalTokenAmount, "incorrect ratio of tokens provided");

        IERC20 token = IERC20(tokenAddress);
        token.transferFrom(msg.sender, address(this), proportionalTokenAmount);
    }
}
```

<Quiz questionId="c0b735e1-f1aa-4595-962e-626c223957ad" />

### LP Tokens

So far we have discussed how to add liquidity, and how to do price calculations for swaps. But what if a liquidity provider wants to withdraw their liquidity from the pool?

We need a way to reward the liquidity providers for their tokens, as without them other users would not have been able to perform swaps. Nobody would put tokens in a third-party contract if they are not getting something out of it.

The only good solution for this is to collect a small fee on each token swap and distribute the fees amongst the liquidity providers, based on how much liquidity they provided.

If someone provided 50% of the pool's liquidity, they should receive 50% of the fees. Makes sense.

There is a quite elegant solution to do this: **Liquidity Provider Tokens (LP Tokens)**

LP Tokens work as shares.

1. You get LP-tokens in exchange for your liquidity
2. Amount of tokens you get is proportional to your share of the liquidity in the pool
3. Fees are distributed proportional to how many LP-tokens you own
4. LP-tokens can be exchanged back for the liquidity + earned fees

But, there are additional requirements:

1. Issued shares must always be correct. When someone else deposits or removes liquidity after you, your shares should remain and maintain correct values.
2. Writing data to the chain can be expensive (gas fees) - we want to reduce the maintenance costs of LP-tokens as much as possible.

Imagine we issue a lot of tokens - say a few billion. The first time someone adds liquidity, they own 100% of the liquidity in the pool. So do we give them all few billion tokens?

This leads to the problem that when a second person adds liquidity, the shares need to be recalculated which is expensive due to gas fees.

Alternatively, if we choose to distribute only a portion of the tokens initially, we risk hitting the limit, which will also eventually force us to recalculate existing shares.

The only good solution seems to have no supply limit at all, and mint new tokens whenever new liquidity is added. This allows for infinite growth, and if we do the math carefully, we can make sure issued shares remain correct whenever liquidity is added or removed.

**So, how do we calculate the amount of LP-tokens to be minted when liquidity is added?**

Uniswap V1 calculates the amount proportionate to the ETH reserve. The following equation shows how the amount of new LP-tokens is calculated depending on how much ETH is deposited:

`amountMinted = totalAmount * (ethDeposited / ethReserve)`

We can update `addLiquidity` function to mint LP-tokens when liquidity is added:

```solidity
function addLiquidity(uint tokenAmount) public payable {
    if (getReserve() == 0) {
        ...

        uint liquidity = address(this).balance;
        _mint(msg.sender, liquidity);
    } else {
        ...
        uint liquidity = (totalSupply() * msg.value) / ethReserve;
        _mint(msg.sender, liquidity);
    }
}
```

Now we have LP-tokens, we can also use them to calculate how much underlying tokens to return when someone wants to withdraw their liquidity in exchange for their LP-tokens.

We don't need to remember how much they originally deposited. Since LP-tokens are proportional to amount of ETH deposited, we can rearrange the above formula to calculate the amount of ETH to return, and proportionately calculate the amount of tokens to return.

<Quiz questionId="0bee63fe-03ab-4319-9033-c7e97201c9cd" />
<Quiz questionId="6de13216-cf92-411a-8e73-2ccfcb5a7ca9" />

### Fees

Now to collect fees on swaps and distribute them amongst liquidity providers, we need to think about a couple of things:

- Do we collect fees in ETH or tokens?
- Do we pay rewards in ETH or tokens?
- How do we collect the fees from each swap?
- How to distribute the fees amongst all liquidity providers?

These may seem difficult questions to answer, but we actually have everything we need to answer them.

1. Traders are already sending ether/tokens to the contract. Instead of asking for an explicit fee, we can just deduct some amount from the ether/tokens they are sending.
2. We can just add the fees to the reserve balance. This means, over time, the reserves will grow!
3. We can collect fees in the currency of the asset being deposited by the trader. Liquidity providers thus get a balanced amount of ether and tokens proportional to their share of LP-tokens.

Uniswap takes a 0.03% fees from each swap. Let's say we take 1% to keep things simple. Adding fees to the contract is as simple as making a few edits to our price calculation formula:

<Quiz questionId="516bc6b9-4c71-421a-896c-464980a0e388" />

We had `outputAmount = (outputReserve * inputAmount) / (inputReserve + inputAmount)`

Now,

`outputAmountWithFees = 0.99 * outputAmount`

But, Solidity does not support floating point operations. So for Solidity we rewrite the formula as such:

`outputAmountWithFees = (outputAmount * 99) / 100`

<Quiz questionId="f37ec27c-2ff0-4fbf-9563-b9de9a384ec3" />

### Congratulations!

This was a big tutorial with a lot of condensed information. Congratulations on making it this far!

While the math and the ideas can be a little tricky to initially understand, we hope that going by going through the material and asking questions on Discord you can develop an appreciation for how beautifully architected all of this is.

See you in the next level where we will actually implement the full contract, along with a website, for the DEX.

<SubmitQuiz />
