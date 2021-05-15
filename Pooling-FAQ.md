# General FAQ

## When will Chia official pooling software be released?
The Chia pool reference code will be released to Testnet by end of May, 2021. Afterwards, pool operators will need time to adapt their pooling code to use Chia's method to calculate farmer's share, collect from pool wallet, and distribute XCH to pool participants. For non-developers, reference code is just that, a reference to use when building your own solution. It is not a turn-key solution someone can immediately deploy and run without the right skillset, time, and effort to make the modifications needed for your own use.

## Will I need to replot to use the official pooling protocol?
Yes. Anyone who wants to join a pool will need to create new portable plots. This new plot format allows you to switch between pools and self-pooling with a cooldown of 30 minutes between each switch. Our recommendation is to slowly replace your existing plots with portable plots one by one, so you still have a chance to win XCH while you convert to all portable plots.

## When can I start creating portable plots?
Support for portable plots will be released on or before May, 31, 2021.

## How can I start my own pool?
If you have experience writing pool server code for another crypto, adapting that pool code with Chia's reference pool code will be straight forward. We only recommend people who have good OPSEC experience to run public pool servers. All pools will be targeted by hackers due to the profitability of XCH currently.

## Where can I find a list of Chia pools?
A crypto community site lists all upcoming Chia pools: https://miningpoolstats.stream/chia

## Why shouldn't I join Hpool?
Hpool has created their own version of Chia client that has no source code released with it. There is no telling what kind of malicious activity that client can do. Chia Network Inc discourages everyone from joining any pool that requires custom clients.

## Why doesn't Chia run their own official pool?
To run a good pool takes a lot of effort, and it's not Chia Network Inc core business. There's a strong ecosystem of pool developers and operators who would love to take on this challenge, so it just makes sense to allow the community to run pools.

## Can I name my pool chiapool.com?
We are not going to allow pools to use "Chia" as the first word or its equivalent (the chia pool). You can say things like "a Chia pool" though that will probably need a free and easy to get license.

## If a pool gets 51% of netspace, can they take over the network?
No, Chia's pooling protocol is designed where the blocks are farmed by individual farmer, but the pooling rewards go to the pool operator's wallet. This ensures that even if a pool has 51% netspace, they would also need to control 51% of the farmer nodes to do any malicious activity. This will be very difficult unless 51% of farmers downloaded a malicious Chia client.

# Technical FAQ

## What programming language is the reference pool code written in?
Python

## How hard is it to adapt Chia's reference pool code to my pool code?
If you've written pool code before, the reference pool code will be easy to understand. It's just replacing PoW concepts with Chia's method of evaluating each farmer's participation via PoST and adapting collection and distribution of XCH using Chia's smart contracts.

## I am a programmer, but never wrote pool code, will I be able to run a pool with Chia's reference pool?
If it's your first time writing pool code, we recommend you look at established BTC or ETH pools on the other features they provide users. You are likely going to compete with big time pool operators from those crypto communities who will provide feature rich pools for Chia on day one. Examples of features: leaderboards, wallet explorer, random prizes, tiered pool fees, etc.
