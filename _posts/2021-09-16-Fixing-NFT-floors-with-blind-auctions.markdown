---
layout: post
title:  "Fixing NFT floor prices with blind auctions."
date:   2021-09-16 19:42:19 +0200
categories: crypto nft market-design auctions
---

This is a design proposal to solve the problem of “Index Reliability” presented by the VC firm Paradigm [here](https://www.paradigm.xyz/2021/08/floor-perps/).

In essence, the problem is that for any financial derivative contract to exist, it needs an underlying value/price to track. What is commonly done on NFT marketplaces such as OpenSea is to define the "floor price" as the lowest offered price on any item of an NFT collection and use that as the underlying price.

Unlike BTC, ETH or any other token, the liquidity of an NFT collection is limited by the edition size and the offered liquidity.

As a side effect, NFT prices are updated much less often and can even present gaps, making it hard to create derivatives with reasonable initial/maintenance margins.

In addition, "floor prices" only take the supply into account without bothering whether there is any real demand at these levels.

Let's see if we can do better.

![NFT auction](/assets/images/nft-auction.gif)

# What we seek
We want to be able to produce a NFT series Price Index.

# Why we seek
We want to do that to give people access to synthetic exposure, either long or short to NFT series.

What are our options?

### Option 1 — Current OpenSea Floor Price
That’s using a definition of a floor price equal to the lowest SELL order on the entire NFT series. There are several problems with this.

1. It relies on a single item in the series which makes the floor volatile. if you are the owner of the NFT, you have direct access to the index. Update your price up and the index moves up. Same on the way down.
2. The fact that this item is ‘trailing’ the others in the series could be for some external factors which are not representative of the series.

The cool thing though is that it’s simple. It says that if you want to know how much you have to pay to get one (possibly the worst of the series), here is the price you have to pay right now.

### Option 2 — The Realized Price
Use the realized price:

$$IndexPrice=\sum_{i=1}^{N}LastPrice_i$$

This represents the value committed in the series, either on the primary or secondary market.

#### The good parts

You have a price as soon as 1 item in the series is sold on the primary market.
You have no dependency to liquidity. Days can go without a trade and you’ll be good.
The Index price evolution is smoothed by the series’ size.
No faking the order book game. The only way to move the index is to trade an NFT of the series.

#### The bad parts

On either side of the market you can take a leveraged position on a derivative tracking the Index Price and manipulate it at profit. That is if the cost of buying the j lowest priced items in the series is less than the gain you will have on your leveraged position. You can then exit your leveraged position and sell back the j items at a price totaling less than the gain you made.

#### Variations

You can have the price expressed in USD base currency with either the ETH/USD current quote or quote at the time of the ith transaction.
The good thing here is that if the NFT series are sold in both DAI or ETH, it still works.
Note that using the current ETH/USD quote implies an update rate of the index price at a faster rate to avoid arbitrage.

### Option 3 — Blind auctions
It goes like this:

Holders of an item in the NFT series place a minimum price at which they are ready to part with their item. That is the supply S.
People who wish to acquire an item of the series place a maximum price at which they are ok for a purchase. That is the demand D.
Nobody knows what any other actor is doing. That is: The people placing an order on either side don’t know if there are other participant on their side or the other side and don’t know the prices or items that participate in the auction.
Supply and demand are matched and cleared where

$$IndexPrice=ClearingPrice=\frac{\sum_{i=1}^{j}D_i}{j}$$ if $$max(D_i)\geq min(S_i)$$

$$IndexPrice=max(D_i)$$ if $$max(D_i)\lt min(S_i)$$

$$$IndexPrice=min(S_i)$$ if there is at least 1 offer (supply side).

In picture:

![NFT auction](/assets/images/auction-result.png)

#### Notes

- If a Clearing Price is found an actual transfer of j NFT for cash occurs.
- Otherwise you get no transfer but you still get an Index Price of the maximum price somebody is willing to pay to get any NFT from the series or if no demand, the minimum of the supply, which should be equal to Option1.
- It is key, that each participant's preference is respected but the floor price is actually a unique clearing price for all the transfers.
- The fact that the auction is hidden forces all the participants to price NFTs for the personal economic value they see. It means that the floor price here is actually much more informed than if participants had access to the other participants orders.
- There’s a hedge case where nobody shows up at the auction. While I believe that people will create such a market only for items of interest (aka there is a demand), it’s possible and we could use a fallback method (Option1) or last auction’s Index Price.

#### The good things are:

- It is hard to rig because as soon as you place an order, it may get filled.
- But placing an order does not allow you to easily modify the Index Price because

1. if your price is set to the Index Price, it means it is the highest demand, hence the riskier order.
2. the demand side is open and hence anyone can actually have the upper hand either through clearing or out bidding you.
3. in the end, the only thing you can know for sure when placing such an order is that you can trade at the floor or not trade at all.

**OK, I see BID and ASK orders here, so why not just go for an order book?**

Glad you ask. Because the floor price here is in fact **a mutualized floor price** of the series. It means it can only exist **when orders are placed at the same time to be cleared together**.

If you were to use an order book, the likelihood that you could clear more than 1 order at the same time would be very low and your Index Price will look like Option1.

That’s what auctions for fixing prices are for.