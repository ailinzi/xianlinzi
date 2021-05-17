# General FAQ

## When will Chia official pooling software be released?
The Chia pool reference code will be released to Testnet by end of May, 2021. Afterwards, pool operators will need time to adapt their pooling code to use Chia's method to calculate farmer's share, collect from pool wallet, and distribute XCH to pool participants. For non-developers, reference code is just that, a reference to use when building your own solution. It is not a turn-key solution someone can immediately deploy and run without the right skillset, time, and effort to make the modifications needed for your own use.

## Will I need to replot to use the official pooling protocol?
Yes. Anyone who wants to join a pool will need to create new K32 portable plots. This new plot format allows you to switch between pools and self-pooling with a cooldown of 30 minutes between each switch. Our recommendation is to slowly replace your existing plots with portable plots one by one, so you still have a chance to win XCH while you convert to all portable plots.

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
Farmers will be sending partial proofs (proofs with lower difficulty than the blockchain) to prove netspace. We expect the farmer to send a partial proof based on a current signage point to the pool server every 5 minutes (300 proofs a day) within a 25 second window. The pool protocol will allow pools to set a minimum difficulty, a maximum difficulty, and a time window for farmers' partial proof submissions. Farmers will be able to pick a difficulty that lets them submit the least number of proofs that prove their netspace.

## How does difficulty affect farmer's nestpace calculation?
Difficulty is linear. Imagine this scenario: Obtaining 1000 proofs with difficulty 1, is equivalent to obtaining 100 proofs with difficulty 10. As a pool server, you prefer to receive 100 proofs with difficulty 10. This is why we allow pool servers to set a minimum difficulty level to reduce the number of proofs each farmer needs to send to prove their netspace.

## How do you identify the farmer that submitted partial proofs?
The farmer will provide their singleton_genesis which is the ID of that farmer's pool group

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

# Outstanding Questions to Devs

felixbrucker, raffling - whats the reasoning behind a max difficulty?

felixbrucker - Do pools need to run the clvm in their own code? if i as a pool can just call a full node api method to run the required clvm instead of doing it by foot like it is done here that works for me

willi123yao - pay to singleton stuff doesn't seem ready yet. How will that work?

felixbrucker - what is the reasoning behind always providing points_balance and difficulty even in errors like this one https://github.com/Chia-Network/pool-reference/blob/147f9361be1353cf71062d5b2df673eb384c5f66/pool.py#L266-L272 ?

dddroptables - taking a look at the reference pool and i'm a little confused about the usage of owner_public_key vs singleton_genesis. Seems owner_public_key is the farmer's public key, which should uniquely identify a farmer's account. singleton_genesis also seems to identify a pool group. Which one should be used to identify a farmer, seems they are both used in various places? Can either of these change for a farmer without expecting an account reset?

dddroptables - also seems like rewards_target can be provided for each partial submission. the reference pool looks like it's just overwriting each time for that farmer. is this just meant to be a way for farmers to change their payout address on the fly? or supposed to aggregate payouts for each individual partial to different rewards_target? (the latter seems a bit overly complex, the former seems like a potential security issue with the protocol imo)

raffling - In the reference code, it's storing the last difficulty the farmer sent every time. What's the motivation behind that
