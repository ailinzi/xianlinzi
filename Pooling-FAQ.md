# General FAQ

## When will Chia official pooling software be released?
The Chia pool reference code will be released to Testnet by end of May, 2021. Afterwards, pool operators will need time to adapt their pooling code to use Chia's method to calculate farmer's share, collect to pool wallet, and distribute XCH to pool participants. For non-developers, reference code is just that, a reference to use when building your own solution. It is not a turn-key solution someone can immediately deploy and run without the right skillset, time, and effort to make the modifications needed for your own use.

## Will I need to replot to use the official pooling protocol?
Yes. Anyone who wants to join a pool will need to create new K32 portable plots. This new plot format allows you to switch between pools and self-pooling with a cooldown of 30 minutes between each switch. Our recommendation is to slowly replace your existing plots with portable plots one by one, so you still have a chance to win XCH while you convert to all portable plots.

## Can I farm with both OG (original) plots and portable plots?
Yes. The farmer will support both OG plots and portable plots on one machine.

## When can I start creating portable plots?
Support for portable plots will be released on or before May 31, 2021.

## How can I start my own pool?
If you have experience writing pool server code for another crypto, adapting that pool code with Chia's reference pool code will be straight forward. We only recommend people who have good OPSEC experience to run public pool servers. All pools will be targeted by hackers due to the profitability of XCH currently.

## Where can I find a list of Chia pools?
A crypto community site lists all upcoming Chia pools: https://miningpoolstats.stream/chia

## Can I advertise my pool in Keybase?
You can only advertise your pool in Keybase @chia_network.public#pools once a day. If you're spammy, mods will warn you and then ban you if you persist.

## Why shouldn't I join Hpool?
Hpool has created their own version of Chia client that has no source code released with it. There is no telling what kind of malicious activity that client can do. Chia Network Inc discourages everyone from joining any pool that requires custom clients.

## Why doesn't Chia run their own official pool?
To run a good pool takes a lot of effort, and it's not Chia Network Inc core business. There's a strong ecosystem of pool developers and operators who would love to take on this challenge, so it just makes sense to allow the community to run pools.

## Can I name my pool chiapool.com?
We are not going to allow pools to use "Chia" as the first word or its equivalent (the chia pool). You can say things like "a Chia pool" though that will probably need a free and easy to get license.

## If a pool gets 51% of netspace, can they take over the network?
No, Chia's pooling protocol is designed where the blocks are farmed by individual farmer, but the pooling rewards go to the pool operator's wallet. This ensures that even if a pool has 51% netspace, they would also need to control 51% of the farmer nodes to do any malicious activity. This will be very difficult unless 51% of farmers downloaded a malicious Chia client.

## I have more questions, where do I ask?
Join our dedicated Keybase room: @chia_network.public#pools

Friendly reminder: do NOT at `@` or Direct Message (DM) developers or mods. Just post your questions in Keybase and we will answer when we have a moment.

# Technical FAQ

## Why isn't the pool reference code working?
Currently the Pool Reference Code isn't fully functional, as there are a lot pieces that need to be checked in. Devs are being transparent in providing early preview of what's going on. While all the pieces are being built out and you see progress, please be patient.

We feel being able to give you early access allows you to start framing your thinking about the work that needs to be done. It's likely the code will change many times before it becomes 1.0.

## What programming language is the reference pool code written in?
Python

## How hard is it to adapt Chia's reference pool code to my pool code?
If you've written pool code before, the reference pool code will be easy to understand. It's just replacing PoW concepts with Chia's method of evaluating each farmer's participation via PoST and adapting collection and distribution of XCH using Chia's smart contracts.

## I am a programmer, but never wrote pool code, will I be able to run a pool with Chia's reference pool code?
If it's your first time writing pool code, we recommend you look at established BTC or ETH pools source code and features they provide users. You are likely going to compete with big time pool operators from those crypto communities who will provide feature rich pools for Chia on day one. Examples of features: leaderboards, wallet explorer, random prizes, tiered pool fees, etc.

## How does one calculate a farmer's netspace?
Farmers will be sending partial proofs (proofs with lower difficulty than the blockchain) to prove netspace. We expect the farmer to send a partial proof based on a current signage point to the pool server every 5 minutes (300 proofs a day) within a 25 second window. The pool protocol will allow pools to set a minimum difficulty and a time window for farmers' partial proof submissions. Farmers will be able to pick a difficulty that lets them submit the least number of proofs that prove their netspace.

## How does difficulty affect farmer's nestpace calculation?
Difficulty is linear. Imagine this scenario: Obtaining 10 proofs a day with difficulty 1 for a k32, is equivalent to obtaining 1 proof a day with difficulty 10. As a pool server, you prefer to receive 1 proof a day per K32 with difficulty 10. This is why we allow pool servers to set a minimum difficulty level to reduce the number of proofs each farmer needs to send to prove their netspace.

## How do you identify the farmer that submitted partial proofs?
The farmer will provide their singleton_genesis which is the ID of that farmer's pool group. They will also provide their Farmer Public Key, so the pool server can validate the proof and verify the farmer signed it properly.

## Will pool servers need to keep track of all farmers and their share of rewards?
Yes, the pool operator will need to write code to keep track of all farmers and their share of rewards. Chia's pool protocol assumes no registration is needed to join a pool, so every singelton_genesis that submits a valid partial proof needs to be tracked by the pool server.

## What are the API methods a pool server needs to support Chia clients?
There are two API methods that the pool HTTP server has to support: get-pool-info and submit-partial

Note: this is a work in progress draft and likely to change before 1.0 release.

```
@dataclass(frozen=True)
@streamable
class PoolInfo(Streamable):
    name: str
    logo_url: str
    minimum_difficulty: uint64
    maximum_difficulty: uint64
    relative_lock_height: uint32
    protocol_version: str
    fee: str
    description: str
    pool_puzzle_hash: bytes32


@dataclass(frozen=True)
@streamable
class PartialPayload(Streamable):
    proof_of_space: ProofOfSpace
    sp_hash: bytes32
    end_of_sub_slot: bool
    difficulty: uint64  # This is the difficulty threshold for this account, assuming SSI = 1024*5
    singleton_genesis: bytes32  # This is what identifies the farmer's account for the pool
    owner_public_key: G1Element  # Current public key specified in the singleton
    singleton_coin_id_hint: bytes32  # Some incarnation of the singleton, the later the better
    rewards_target: bytes  # The farmer can choose where to send the rewards. This can take a few minutes


@dataclass(frozen=True)
@streamable
class SubmitPartial(Streamable):
    payload: PartialPayload
    rewards_and_partial_aggregate_signature: G2Element  # Signature of rewards by singleton key, and partial by plot key


@dataclass(frozen=True)
@streamable
class RespondSubmitPartial(Streamable):
    error_code: uint16
    error_message: Optional[str]
    points_balance: uint64
    difficulty: uint64  # Current difficulty that the pool is using to give credit to this farmer
```

## Where can I see the Chia Pool Reference Code?
Note: this is a work in progress draft, not fully functional and likely to change before 1.0 release.

You can find it here: https://github.com/Chia-Network/pool-reference

# Features Requests to Devs

Features requests to devs will be reviewed and ultimately decided by @hoffmang if it is included in 1.0 or a later release of pooling software:

grintor - To allow users with different netspace be redirected to different URL to submit their proofs with different minimum difficulties. "Can we talk about the possibility of using HTTP redirects to manage server load by moving peers to API endpoints with higher or lower difficulties?
It would be nice to be able to work with very small farmers and make sure they are online by using difficulty 1, but to also deal with very large farmers without high server load
And we can't rely on the farmers to self-police by choosing to sync with the correctly sized API endpoint
so it would be very nice for the server to be able to instruct the client which to use
everyone starts at the API endpoint with difficulty 1, and as the pool sees that they are submitting work very fast, it moves them up in difficulty until it has them at a nice place where it can be sure they are online but it's not getting spammed with requests
This can be implemented by just coding the pool subscribing agent to understand what a http 301 redirect means, and honoring it"
**Under investigation on scope of work and timing of feature release**: https://trello.com/c/4UORocJL/1179-grintor-to-allow-users-with-different-netspace-be-redirected-to-different-url-to-submit-their-proofs-with-different-minimum-diff

felixbrucker - Create RPC calls in full node to process CLVM needed by reference pool code. "is there any reason this could not just be an rpc call in chia-blockchain? it feels like a lot of work to re-implement all that logic currently only avail in python, and chia-blockchain has all the required parts included anyway"
**Will be implemented as fast follow feature after Pooling 1.0 is released**: https://trello.com/c/5goJUups/1180-felixbrucker-create-rpc-calls-in-full-node-to-process-clvm-needed-by-reference-pool-code

# Outstanding Questions to Devs

willi123yao - what is the point of storing the farmer difficulty? usually for pools is that the difficulty would reset when a new connection is made, so similarly for pools it should reset the difficulty when no shares are received after a certain amount of time

serafimcloud - Do you have any CPU, RAM, DISK requirements to run a full node? (for the pool server)

marclar - what is SingletonState.relative_lock_height?

xch_pool  - another question .. I am trying to find anything that keeps this pool reference code from being stateless .. only I could find is coin_record_cache (being local LRUCache, and for example could be rewritten to use a redis or something) .. am I missing anything?

raffling - Just wanted to make 200% sure I get this right. During this cooldown after the switch announcement from the farmer the pool can still claim rewards. And the pool can essentially enforce the cooldown period on the farmer upon joining?

raffling - What's the max cooldown pools can set? Anything close over over an hour effectively completely disincentivizes farmers from switch as they'd be missing out on shares

raffling - Can I get a confirmation that during the cooldown period, the pool that was left can still claim rewards? Aka we don't need an XL VM just to scan the blockchain asap for unclaimed rewards with minimal latency? 😄

xch_pool - that's why I am eager to get an answer to my question: is there a way to actively detect users breaking the smart contract with the pool (leaving the pool)

dddroptables - To follow up on raffling's question/answer:
It sounds like if a p2_singleton_ph is spent it will use up the singleton_genesis and create a new one. Is that correct? Meaning the id of the user would change every time we get a payout in the pool based on that user winning

dddroptables - Got it. Would there be any gain to keeping track of all the singleton_coin_ids for every spend that occurs? Would we need that to do later spends if say, the same user wins again? Or we would only need to spend with the p2_singleton_puzzle_hash every time?

dddroptables - Yeah but the p2_singleton_puzzle_hash is only to claim rewards for the pool, right? Pool would then send payout to the reward_address on file for that user. So we'd need to keep track of p2_singleton_puzzle_hash for each user in case they win, and spend that to claim our cut. Is that consistent with the model being used here?

