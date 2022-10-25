---
layout: post
title:  "Auctions with variable supply in crypto NFT markets."
date:   2021-09-07 08:42:19 +0200
categories: crypto nft market-design auctions
---

# Overview
This paper introduces an auction design for NFT series that allows for a better equilibrium between demand and supply.

It does so by creating an auction where participants (both creators and bidders) state their preferences in terms of both PRICE and EDITION SIZE.

The design allows the creators to

- Maximize their revenues.
- Reach their maximum audience within price and edition size boundaries that they control.
- Provide a fair distribution, giving all successful bidders the same NFT price (aka floor).
- Preserve their brands, as failed NFT auctions can destroy the trust your customers place when they enter an auction for one of your series. If they do that regularly.

It also allows bidders to

- Enter auctions more safely by allowing them to place both price and edition size limits outside of which they wish not to participate. This allows them to trade within their budget but also to target an edition size minimum (below which they consider the NFT auction a failure) and maximum (after which they consider the NFTs are not scarce enough for them).
- Enter auctions more efficiently as the design lessens the gas race that can occur on fixed-size auctions.

Both creators and bidders benefit from the fact that the equilibrium found with that design is less volatile post-auction as it finds an equilibrium both in terms of price AND market size.

This is particularly important for NFTs that have utilities/constraints/interactions/upsells later on during their lifetime, allowing for a secondary market to open in a more price-stable way.

{% twitter https://twitter.com/defiprime/status/1434644596579913729 %}

Let’s go.

# The Problem
## From the point of view of the creator
Each creator has its preference regarding the NFT size they see fit:

- Some creators want to express “I created the new Mona Lisa, there shall be 3 NFTs and no more”.
- Some creators want to create an edition targeting a community/market size. These opportunities have an equilibrium at which they function. Too few is a failure and too many is a failure.
- Some creators want to optimize for REACH. This is equivalent to optimizing for a maximum number of users.
- Some creators also simply have technical constraints. For instance, generative arts algorithms could generate only max N variations. Or if the NFT is used to grant access to a Discord channel, they wish that channel to be of a manageable size.
Creators also obviously seek to maximize their revenues.

## From the point of view of the bidders
Bidders also have preferences for entering an auction.

Whether it is an art they are buying, access to a market or a key to a community, they each have

- a budget
- a desire to buy the NFT at the same price as everyone else during the auction.
- a desire to pay the smallest price possible.
- supply boundaries that they prefer:

    1. Some art collectors only deal with very scarce items, similarly to art collectors and don’t want to enter a crowded market.
    2. Some others have boundaries, for instance for in-game NFTs where they know the mechanics and they are ok to purchase something if there are enough of it, and not too many.
    3. Some others want to buy their NFTs whatever the edition size. They just like the stuff and have no price and/or edition size boundaries

# The solution
The solution is to design an auction where

### the creator specifies

- a minimum and maximum edition size. The minimum size is your kickstarter target. You may want to cancel the entire thing (and try again later with more marketing) if not enough NFTs are minted for the game to function otherwise the few people who showed up could feel scammed. The maximum edition is the hard limit of the mint.
- a minimum revenue that applies to the whole edition below which they are ready to cancel the whole thing and refund the participants.

### the bidders specify

- a minimum and a maximum NFT price. Outside this zone, they do not wish to participate.
- a minimum and maximum edition size. If the edition size is outside these boundaries, they do not wish to participate either.
## Solving multiple possible equilibrium solutions
Multiple solutions may provide the same revenue to the creator. They can then either decide on the solution they prefer either by

- time of arrival. (current gas war)
- edition size (creators choose to select either the smallest edition size or the biggest)
random choice
- Or a combination of them (first the edition size, if there are still multiple solutions, the time of arrival or a random solution).

Obviously, current gas war auctions are a special case of this auction design where they set

- No minimum revenue at the edition level.
- Hard minimum and maximum edition size, set to the same value.
- Give the users no choice but to pay for the gas war by not giving them edition size boundaries.

# Extensions
A lot of constraints can be added or removed. For instance, if your NFTs have in-game mechanisms or characteristics that require that some NFTs are present from day one, you can add constraints that allow for an auction to succeed only if some quantities are met or you can prioritize certain NFTs over others for the game to work.

# Technicalities
An algorithm may take a lo(ooooo)ong time to solve such an auction and it probably can’t run fully on chain (I’m no expert here, feel free to comment). It would probably take an on-chain with an off-chain ping-pong to work.
Last but not least, it works for fungible tokens too, helping projects find the right price and supply for an ICO/ECO.
Reach out on Twitter if you’re interested in discussing the topic in public or private, DMs are open.

Cheers,

Some Links on the subject

[Maximising Blockchain Collectible Economies — (simondlr.com)](https://blog.simondlr.com/posts/maximising-blockchain-collectible-economies)

[Exploring NFT Economies: Creators, Collectors, & Collection Sizes. — (simondlr.com)](https://blog.simondlr.com/posts/exploring-nft-economies)

# Edit
It is also possible to factor in the bidders' preference in terms of quantity of items acquired: “I want to acquire at least X items and at most Y” but as well as the percentage of ownership: “I want to acquire at least W% and at most Z% of the items”.

