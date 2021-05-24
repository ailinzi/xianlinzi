- [General FAQ](#general-faq)
- [Technical FAQ](#technical-faq)
- [Features Requests to Devs](#features-requests-to-devs)
- [Outstanding Questions to Devs](#outstanding-questions-to-devs)
- [Draft FAQ Items](#draft-faq-items)
***

# General FAQ

## When will Chia official pooling software be released?
The Chia pool reference code will be released to Testnet by end of May, 2021. Afterwards, pool operators will need time to adapt their pooling code to use Chia's method to calculate farmer's share, collect to pool wallet, and distribute XCH to pool participants. For non-developers, reference code is just that, a reference to use when building your own solution. It is not a turn-key solution someone can immediately deploy and run without the right skillset, time, and effort to make the modifications needed for your own use.

## Will I need to replot to use the official pooling protocol?
Yes. Anyone who wants to join a pool will need to create new K32 portable plots. This new plot format allows you to switch between pools and self-pooling with a cooldown of 30 minutes (rules set by pool) between each switch. Each switch between pools will require a transaction with a smart contract on the blockchain. Our recommendation is to slowly replace your existing plots with portable plots one by one, so you still have a chance to win XCH while you convert to all portable plots.

## Will I need pay XCH to switch pools?
Yes. It will require 1 mojo (1 trillionth of a XCH) to switch pools. The mojo is used to make a transaction with a smart contract on the blockchain. For those who don't have any XCH, you can get 100 mojos from Chia's official faucet: https://faucet.chia.net/

## Can I farm with both OG (original) plots and portable plots?
Yes. The farmer will support both OG plots and portable plots on one machine.

## When can I start creating portable plots?
Support for portable plots will be released on or before May 31, 2021.

## What is this new portable pool thing called?

It is a Plot Group NFT that you will be plotting portable plots to.

## How can I start my own pool?
If you have experience writing pool server code for another crypto, adapting that pool code with Chia's reference pool code will be straight forward. We only recommend people who have good OPSEC and business experience to run public pool servers. Depending what country you operate your pooling business, you may be subject to tax, AML and KYC laws specific to your jurisdiction. All pools will be targeted by hackers due to the profitability of XCH and you may be legally liable if you have any losses.

## Where can I find a list of Chia pools?
A crypto community site lists all upcoming Chia pools: https://miningpoolstats.stream/chia

## Can I advertise my pool in Keybase?
You can only advertise your pool in Keybase @chia_network.public#pools once a day. If you're spammy, mods will warn you and then ban you if you persist.

## Why shouldn't I join Hpool?
Hpool has created their own version of Chia client that has no source code released with it. There is no telling what kind of malicious activity that client can do. Chia Network Inc discourages everyone from joining any pool that requires custom closed source clients.

## Why doesn't Chia run their own official pool?
To run a good pool takes a lot of effort, and it's not Chia Network Inc core business. There's a strong ecosystem of pool developers and operators who would love to take on this challenge, so it just makes sense to allow the community to run pools.

## Can I name my pool chiapool.com?
We are not going to allow pools to use "Chia" as the first word or its equivalent (the chia pool). You can say things like "a Chia pool" though that will probably need a free and easy to get license.

## If a pool gets 51% of netspace, can they take over the network?
No, Chia's pooling protocol is designed where the blocks are farmed by individual farmer, but the pooling rewards go to the pool operator's wallet. This ensures that even if a pool has 51% netspace, they would also need to control ALL of the farmer nodes (with the 51% netspace) to do any malicious activity. This will be very difficult unless ALL the farmers (with the 51% netspace) downloaded the same malicious Chia client programmed by a Bram like level genius.

## I have more questions, where do I ask?
Join our dedicated Keybase room: @chia_network.public#pools

Friendly reminder: do NOT at `@` or Direct Message (DM) developers or mods. Just post your questions in Keybase and we will answer when we have a moment.

# Technical FAQ

## Why isn't the pool reference code working?
Currently the Pool Reference Code isn't fully functional, as there are a lot pieces that need to be checked in all Chia-Network repos. Devs are being transparent in providing early preview of what's going on. While all the pieces are being built out and you see progress, please be patient.

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

## Why don't we give documentation or support dev preview of pooling code?
We don't support dev preview of pooling code for a few reasons: 1) it's work in progress, and will continue to change as we get closer to launch; 2) we need devs focused on completing the 1.0 version of pooling code, and every question that they take time to answer increases the chances we will slip; 3) pooling is a very complicated set of software not meant for inexperienced coders to get involved with. The pooling code is not a turn-key solution without additional  development efforts from the pool operator, and generally speaking, if this question is relevant to you then odds are you do not have the existing experience needed to successfully (and securely) run a pool unassisted at this time.

# Features Requests to Devs

No new feature requests taken at this time.

# Outstanding Questions to Devs

willi123yao - keybase://chat/chia_network.public#pools/7667
will it be ok to run the pool on a non-default port? I presume the client needs to be able to handle that as well...
efishcent
11:35 AM
willi123yao
will it be ok to run the pool on a non-default port? I presume the client needs to be able to handle that as well...
Will add it to the list to ask Devs if it's on the roadmap. It's more will it be supported in 1.0 or post...

willi123yao - keybase://chat/chia_network.public#pools/7674
also, just a side comment that the pool reference code is pretty much all in a single file, and I have to pull the repo and apply new changes to make my code compatible. will the pool reference devs consider splitting up important functions/calls into their own files so that we can import the code as a library instead? ideally it could be broken into a few parts, such as the partial checker, payout system and absorption runner

richardmolte - keybase://chat/chia_network.public#pools/7695
Can someone tell me what the "get_recent_signage_point_or_eos" method does in the reference code?
The reason why I am asking this, is because I would like to know if it is possible to run the "process_partial" method in parallel across multiple machines or threads.
Thanks for your answer in advance :-)
EDITED
efishcent
12:44 PM
richardmolte
Can someone tell me what the "get_recent_signage_point_or_eos" method does in the reference code? The reason why I am asking this, is because I would like to know if it is possible to run the "process_partial" method in parallel across multiple machines or threads. Thanks for your answer in advance :-)
EDITED
Partial proofs submitted by farmers should match the challenge of the most recent signage point or end of slot (EOS). The get_recent_signage_point_or_eos method is to get the information needed to help validate the partial proof.
I believe running "process_partial" in parallel across multiple threads is fine and even possible across machines as long you have a way to ensure you only count each farmer's partial proof once and handle reorgs properly.
I will add this to Devs Q&A just to make sure my answers are correct

felixbrucker - keybase://chat/chia_network.public#pools/7706
Question: is it possible to identify which farmer won a block when a farmer wins a block using portable plots?

willi123yao - keybase://chat/chia_network.public#pools/7897
I do remember asking a question regarding this sometime back... which are the more important/significant libraries that needs to be ported in order to make a chia pool on another lang?

cccat - keybase://chat/chia_network.public#pools/8307
Can server.index and server.get_pool_info contains anything? Is there a danger of malicious code can be run in these HTTP responses if I code some javascript in it?

# Draft FAQ Items

Important Keybase conversations captured that needs to be converted to FAQ items. All items below will be cleaned up, this is just a place to temporarily cut and paste conversations in Keybase as a place holder:

## How do pool collect rewards?
felixbrucker
12:30 AM
We need to keep track of the latest singleton coin ID of each singleton. And every time we claim rewards with it, it will create a new ID.
What happens when a user wins a block and gets rewards credited to the singleton ph, does the coin id change at this point? ie say a new user with 0 pool rewards on his singleton ph joins, at this point id guess there is no coin id, because there are no coins. Then he wins a block, and id expect there to be a coin id of that reward (1.75xch). Then directly after that block he wins another block, before the pool could claim any rewards. Does he have two coin ids at this point, or only one with 2x 1.75 xch in it?
sorgente711
12:37 AM
It's important to distinguish between the singleton and the p2_singleton_puzzle_hash
The rewards from the blockchain go straight into the p2_singleton_puzzle_hash, this is like a temporary storage for rewards. Rewards which can only be claimed by the singleton
The singleton itself must be created beforehand, before the user starts farming
The p2_singleton puzzle and the singleton are both spent together. This creates a new singleton coin ID
farmerhoss
sorgente711
12:39 AM
But the user might have 10 rewards in his p2_singleton address
the pool can claim all 10 at the same time
(well technically each claim requires spending the singleton and creating a new incarnation of the singleton, but you can do this all within the same block)
Does that make sense? So wining blocks doesn't do anything to the singleton. It just puts the 1.75 into this treasure chest waiting to be claimed by the singleton
felixbrucker
12:47 AM
hm i dont quite get some parts yet

ok, so i think the following is correct (please correct me if its not):
- a singleton has a static genesis
- a pay to singleton puzzle hash is static as well as its derived from the singleton genesis
- pool rewards go to the (static) pay to singleton puzzle hash
- when claiming pool rewards one needs to scan the blockchain by the pay to singleton puzzle hash and list all its coins (coin ids), if the user only won a single block it will be only one coin
- the coins from above can be spent to the pools puzzle hash for later distribution, this can include all or part of the coins of a single pay to singleton puzzle hash
- to spend multiple coins from above you still need separate txs (can not be one tx)
EDITED
sorgente711
12:49 AM
- the coins from above can be spent to the pools puzzle hash for later distribution, this can include all or part of the coins of a single pay to singleton puzzle hash
^ You must spend the entire reward I think. Also, spending this also requires spending the singleton and creating a new version of that singleton.
- to spend multiple coins from above you still need separate txs (can not be one tx)

Yes, but these transactions can all be combined into one (because you can merge transactions in Chia) and they can all happen at the same time
Apart from that, your understanding is correct
You must spend the whole 1.75 whenever spending one of the coins.
felixbrucker
12:51 AM
sorgente711
You must spend the whole 1.75 whenever spending one of the coins.
alright yeah, i meant if i have two coins with each 1.75, if i need to spend both coins, or if could just spend one of them and the other at a later time, id assume this works
No, I don't, but the scannign will be basically looking at all child_coins for each singleton and then seeing which one is the new singleton
But this suggests there is a way
6:02 AM
dddroptables
Great, so anyone upon seeing the block can spend the p2_singleton_ph and it will redeem to owners. Does not need to be owner of the farmer PK or pool address PK, correct?
yeah exactly. And it has to be a coinbase reward, it can't be any coin owned by p2_singleton_ph
sorgente711
6:25 AM
Whenever one of the pool members (farmers) wins a block, then the reward can be claimed to the pool's address. Anyone can initiate the claim on the blockchain.
The, periodically, the pool will pay all of the members based on some formula

## What actions can singleton take?
There are a few things you can do to the singleton:
- Change pool (needs owner signature)
- Escape pool, this is announcing that you will change pool (needs owner signature)
- Claim rewards (does not need any signature, it goes to the specified address in the singleton)

## Variable names
I have a few questions about the terminology in the reference code. Can you explain the following terms or tell me where I can look them up?

- Puzzlehash (a different format for the receive address?)
- singleton (Smart contract, so only the pool can claim rewards?)
- singleton genesis (Unique identifier?)
- singleton_coin_id_hint
- points (Is this the current account balance in mojos?)

I know this is quiete a bit to ask, but I would really like to get a better understanding of this.
EDITED
Another thing, that I wanted to ask, If I got this right is the difficulty. I think I understand the main concept of this. However imagine this scenario:

Someone with a lot of netspace (e.g. 10 PiB) gets a high difficulty assigned. This means that his netspace is only "roughly sampled" and he might not be credited with some TiB even though he has them, right?
sorgente711
12:13 AM
richardmolte
Another thing, that I wanted to ask, If I got this right is the difficulty. I think I understand the main concept of this. However imagine this scenario: Someone with a lot of netspace (e.g. 10 PiB) gets a high difficulty assigned. This means that his netspace is only "roughly sampled" and he might not be credited with some TiB even though he has them, right?
No, the difficulty will be higher for the 10PB farmer but they will still regularly be sending proofs
the number of proofs per day will be approximately the same regardless of farmer size
Points = number of proofs submitted, weighted by diffiuclty. One k32 farms 10 points per day
singleton coin id hint is removed
singleton genesis is the unique ID of the singleton, which is a smart coin that the user controls
puzzlehash is just an address but in a different format. Addresses are human readable
Points represent the amount of farming that a farmer has done
To accumulate 1000 points you need 10 TiB farming for a day
No, a singleton is a smart coin that guarantees it's uniqueness, that it is the only coin with that unique ID.
singleton means there is only 1.

## How is difficulty set for partial proofs

The current thinking is the pool will dictate the difficulty for each farmer. The open question is the initial difficulty that should be set by the pool when it gets a new farmer that joins. There was a thought that the client suggests a difficulty. Right now we are leaning towards the default difficulty for a new farmer joining a pool will be difficulty 1 and the pool adjusts the minimum requirement for the specific farmer upwards based on the initial barrage of proofs.
