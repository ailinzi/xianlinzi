- [General FAQ](#general-faq)
- [Technical FAQ](#technical-faq)
- [Features Requests to Devs](#features-requests-to-devs)
- [Outstanding Questions to Devs](#outstanding-questions-to-devs)
- [Draft FAQ Items](#draft-faq-items)
***

# General FAQ

## When will Chia official pooling software be released?

Launch of pools is continuing to move forward steadily. We first need to launch it on testnet for integration testing. Afterwards, pool operators will need time to adapt their pooling code to use Chia's method to calculate farmer's share, collect to pool wallet, and distribute XCH to pool participants. For non-developers, reference code is just that, a reference to use when building your own solution. It is not a turn-key solution someone can immediately deploy and run without the right skillset, time, and effort to make the modifications needed for your own use.

## How can I participate in testnet?
testnet is specifically designed for only programmers and technical testers to participate in. When we do release Chia official pooling software to testnet, we will provide enough instructions that experienced technical testers can do it. It won't be easily accessible for the average consumer and we will not provide support.

## Will I need to replot to use the official pooling protocol?
Yes. Anyone who wants to join a pool will need to create new K32 or above portable plots. This new plot format allows you to switch between pools and self-pooling with a cooldown of ~30 minutes (100 blocks) between each switch. Each switch between pools will require a transaction with a smart contract on the blockchain. Our recommendation is to slowly replace your existing plots with portable plots one by one, so you still have a chance to win XCH while you convert to all portable plots.

## When can I start creating portable plots?
Support for portable plots will be released sometime in June on testnet. Testnet portable plots are focused on finding bugs. It is highly unlikely testnet portable plots will work in mainnet. If you are OK with high chance bugs found in testing will require to replot, you are free to try it. Otherwise we recommend you wait for mainnet launch before plotting portable plots.

## Will I need pay XCH to switch pools?
Yes. It will require 1 mojo (1 trillionth of a XCH) to switch pools. The mojo is used to make a transaction with a smart contract on the blockchain. For those who don't have any XCH, you can get 100 mojos from Chia's official faucet: https://faucet.chia.net/

## Can I farm with both OG (original) plots and portable plots?
Yes. The farmer will support both OG plots and portable plots on one machine.

## How do I assign portable plots to a pool?
First you will create a Plot NFT (devs call this singleton in their code) in the new pools tab in the GUI. When you create a new portable plot, you must assign it a specific Plot NFT (for those using CLI, this replaces the Pool Public Key `-p` with a Pool Contract Address `-c`). All plots created with the same Plot NFT can then be assigned to a pool for farming.

## How is Chia pooling different from other cryptos?
Chia has three major differences from most other crypto pooling protocol: 1) Joining pools are permissionless. You do not need to sign up to an account on a pool server before joining. 2) Farmers receive 1/8th of XCH rewards plus transaction fees, while the pool receives 7/8th of XCH rewards to redistribute (minus pool fees) amongst all pool participants. 3) The farmer with the winning proof will farm the block, not the pool server.

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
We are not going to allow pools to use "Chia" as the first word or its equivalent (the chia pool). You can say things like "a Chia pool" though that will probably need a free and easy to get license. Go to https://www.chia.net/terms/ to get more information on obtaining a license.

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

## Variable names used in pooling code
- puzzlehash: an address but in a different format. Addresses are human readable.
- singleton: a smart coin (contract) that guaranteed to be unique and controlled by the user.
- singleton_genesis: unique ID of the singleton.
- points: represent the amount of farming that a farmer has done. It is calculated by number of proofs submitted, weighted by difficulty. One k32 farms 10 points per day. To accumulate 1000 points you need 10 TiB farming for a day. This is equivalent to shares in PoW pools.

## How does one calculate a farmer's netspace?
Farmers will be sending partial proofs (proofs with lower difficulty than the blockchain) to prove netspace. We expect the farmer to send partial proofs based on a current signage point to the pool server every 5 minutes (averaging 300 proofs a day) within a 25 second window. Farmers will be able to suggest a difficulty that lets them submit the least number of proofs that prove their netspace, but Pool servers will ultimately set the minimum difficulty Farmers must send their partial proofs at.

## How does difficulty affect farmer's netspace calculation?
Difficulty is linear. Imagine this scenario: Obtaining 10 proofs a day with difficulty 1 for a k32, is equivalent to obtaining 1 proof a day with difficulty 10. As a pool server, you prefer to receive 1 proof a day per K32 with difficulty 10. This is why we allow pool servers to set a minimum difficulty level to reduce the number of proofs each farmer needs to send to prove their netspace.

## How do you identify the farmer that submitted partial proofs?
The farmer will provide their singleton_genesis which is the ID of that farmer's pool group. They will also provide their Farmer Public Key, so the pool server can validate the proof and verify the farmer signed it properly.

## Will pool servers need to keep track of all farmers and their share of rewards?
Yes, the pool operator will need to write code to keep track of all farmers and their share of rewards. Chia's pool protocol assumes no registration is needed to join a pool, so every singelton_genesis that submits a valid partial proof needs to be tracked by the pool server.

## What actions can singleton take?
There are a few things you can do to the singleton:
- Change pool (needs owner signature)
- Escape pool, this is announcing that you will change pool (needs owner signature)
- Claim rewards (does not need any signature, it goes to the specified address in the singleton)

## How do pool collect rewards?
- Farmer joins a pool, they will assign their singleton to the pool_puzzle_hash.
- When a farmer wins a block, the pool rewards will be sent to the p2_singleton_puzzle_hash.
- Pool will scan blockchain to find new rewards sent to Farmer's singletons.
- The pool will send a request to claim rewards to the winning Farmer's singleton.
- Farmer's singleton will send pool rewards XCH to pool_puzzle_hash.

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
    suggested_difficulty: uint64  # This is suggested the difficulty threshold for this account
    singleton_genesis: bytes32  # This is what identifies the farmer's account for the pool
    owner_public_key: G1Element  # Current public key specified in the singleton
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

## When is the next Zoom Technical Q&A on Pooling?
There will be a Technical Q&A with the 3 core devs responsible for official Chia pooling support next Wednesday, June 2 from 09:00-10:00 AM US Pacific Daylight Time (PDT). Please mark your calendars. Zoom link will be provided early next week.

# Features Requests to Devs

No new feature requests taken at this time.

# Outstanding Questions to Devs

felixbrucker - keybase://chat/chia_network.public#pools/10430
quick question: cant we run our own private testnet to confirm wins with a single pool plot? i have not taken a look at this scenario yet, but it should be possible to run all required components from within the chia-blockchain code, right?

zelgada - keybase://chat/chia_network.public#pools/10481
Hi @efishcent - I note that the pool-reference code is receiving the partials in pool_server.py, however- I was confused about this part in the farmer code:

In the as-is client, you can see here https://github.com/Chia-Network/chia-blockchain/blob/34caa75f99547c3dae394cefb4dfa2bc34b93dc4/chia/farmer/farmer_api.py#L176 that the pool private key used to sign the pool_target_signature, and added to the DeclareProofOfSpace message.  I note that in some branches, this code has changed.

The ms.pooling branch makes some changes.  Specifically, I see that the code has changed but the pool target signature is still using the pool private key for a signature.  https://github.com/Chia-Network/chia-blockchain/blob/4788c616165388ebae786a7f06d04fd0cf892e30/chia/farmer/farmer_api.py#L302

Will this not fail on line 295, when the farmer discovers not to have the pool private key?

Why is this even needed?

alex_fx - keybase://chat/chia_network.public#pools/10513
Client apps will need to have a place for me to register an endpoint for submitting partial Proof right? (the endpoint is an api of the pool receiving the partial proof) Can pool server specify to Client the endpoint URL?

j1mb0 - keybase://chat/chia_network.public#pools/10602
Question: In the pool server reference code there is this line: self.wallet_rpc_client.log_in_and_skip, which makes calls to "host": "https://backup.chia.net" which I gather is a key management solution for storing wallet keys. I am assuming this is not intended for production and Chia devs intend for pool operators to replace it with our own key management of choice?

alexfp - keybase://chat/pooloperators#general/81
Thank you @efishcent. I have one question regarding the pooling protocol. How Plot ID is dervied from /submitPartial data? If I'm not wrong, the Plot ID is required for the partial verification.

felixbrucker - keybase://chat/chia_network.public#pools/10763
I got a question regarding the login route / login flow: https://github.com/Chia-Network/pool-reference/blob/25708b29f09c3a65320731d9211ba413232df12f/SPECIFICATION.md#flow
it says here the pool url is used, would that be the same url that is used for partials and get info? id prefer it wasnt fixed to that, because that part would be api while the login would be handled by frontend code, well for me at least
GitHub
Chia-Network/pool-reference
Contribute to Chia-Network/pool-reference development by creating an account on GitHub. 

just to clarify, i will probbaly run the api and frontend on different subdomains
providing the login url /base url could be as simple as adding a pool info field for it

alexfp - keybase://chat/pooloperators#general/93
Also, the proof actual difficulty is calculated by 2**256/quality, right?

migueloco - keybase://chat/chia_network.public#pools/10891
I saw that several programmers presented a beta wallet that allows the plots to change the pool, we are developing a pool and we cannot find that wallet in the public repositories, how can we have access to this wallet? since we need it to test the pool. Thank you so much

cccat - keybase://chat/chia_network.public#pools/10913
A question. Is /login navigating to browser or will be display in the Chia GUI? Where did the client get login_code? Thanks!
Is login_code just in the Set-Cookies of /partials response?

# Draft FAQ Items

Important Keybase conversations captured that needs to be converted to FAQ items. All items below will be cleaned up, this is just a place to temporarily cut and paste conversations in Keybase as a place holder:

All caught up at the moment!