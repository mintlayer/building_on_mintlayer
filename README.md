#  Blockchain developer looking to build your next project? Consider *Mintlayer*. #

Mintlayer combines combines the best features of different chains in a novel way.

At a glance, Mintlayer offers:

- Native tokenization, including NFTs and confidential tokens
- Support for WebAssembly smart contracts
- Formidable security and privacy features, by virtue of its UTXO system and the Chainscript scripting language
- Signature aggregation through BLS
- Confidential transactions
- Fully interoperablity with bitcoin and the lightning network

## Native tokenization

Mintlayer's native token, MLT, is used for staking and for paying transaction fees.

In addition, minting your own token on Mintlayer is as easy as submitting a transaction. No smart contract is required!
There are three ways to create your own token:
- _RPC_:  create a token issuance transaction and submit it to a Minlayer node via RPC
- _Wallet_: use the graphical user interface provided by Minlayer's mobile or web wallets
- _Cli_: use Mintlayer's command line interface tool

Currently, there are three types of tokens that can be issued on Mintlayer:
- **MLS-01**: "normal" tokens, akin to ERC-20 tokens on Ethererum
- **MLS-02**: confidential tokens, whose transactions are not publicly available on the blockchain
- **MLS-03**: NFTs

## WebAssembly smart contracts

Decentralized applications of any complexity invariably require the use of smart contracts.

By supporting WebAssembly smart contracts, Mintlayer empowers blockchain developers of all backgrounds to confidently build and deploy decentralized applications.

Support for WebAssembly smart contracts means developers can code smart contracts in any language which compiles to WebAssembly.

For example, the [*ink!*](https://github.com/paritytech/ink) framerork enables smart contract development in the Rust programming language, which has seen a meteoric rise in popularity over the past years owing to its speed, safety, and rich development ecosystem. A language supporting multiple programming paradigms, Rust strikes a balance between accessibility and ease of use on the one hand, and strong memory-safety and type-safety guarantees on the other.

Developers versed in Ethereum will probably feel most at home writing smart contracts in Solidity. [Solang](https://github.com/hyperledger-labs/solang), a project developed by Hyperledger Labs, facilitates compiling Solidity to WebAssembly, thereby providing a way for smart contracts written in Solidity to be deployed on Mintlayer.

## Full Interoperability with the Bitcoin ecosystem

- Mintlayer supports atomic swaps with Bitcoin without the need for an intermediary.
- Have Lightning support.
- Trifecta of different chains: Bitcoin, LN for fast transactions. ML for Defi. All focus on their own part. Become part of a wider ecosystem.
LN is under VERY rapid development. Horrific to keep up 
Essentially acts as a graph - shortest graph.
No path - backup - wallet controlled node

Dex -  key place where it belongs.
One of the main issues with ethereum - Dexs clogging up blocks.

Lightning: An intermediary for bitcoin - all transactions happening of Dex can happen on LN. Not clogging ML, not clogging Bitcoin. Fast, Essentially zero fees, no "junk" floating around on ML and Bitcoin chain. junk = transactions on DEXs. Avoids pushing up transaction fees on ML and bitcoin. Gas on ethereum has become a transaction fees. Was introduced with Smart contracts. Introducing gas - defined period of time. In v0.1 - not the same thing as TX fee. No distinction today.
Bitcoin doesn't have gas becasue it doesn't have smart contracts. Script contracts, just have a transaction fee.
ML is somewhere in the middle - murky: people are trying to avoid using the term gas. Strictly speaking, there will be gas for pp. Essentially that's because we're using ink.
We tend to refer a normal transaction as having a transaction fee.
We will have a free market for that transaction fee. A block creator can accept a transaction fee in any token they want.
In Eth, need to pay transaction fee in ETH
In bitcoin, BTC
In ML - can pay in anything, hoping that whoever is mining that block accepts it

In BTC - just the difference between the inputs and the outputs. You will get a suggested transaction fee.
Wallet determines based on the mempool. Strictly speaking it's a user decision.

ISSUE: How does the block creator know which tokens to accept. Will need some kind of lookup solution.

Chainlink:
They provide a bridge between their chain and Ethereum and their chain is an "oracle of prices". Can only do this with a token actually being traded. Data from, centralized, DEXs.

Two ways to do LN integration
1. Third pary node somewhere that ML nodes communicate with "Atomic swaps". Swap ML into lighting, TX will happend hashlocked on lightinig
2. Full Mintlayer node will also become a LN

LN - layer two designed purely for speed and to avoid pollution on the bitcoin chain.
LN support
Interact with LN from ML network.
Mintlayer tokens will be able to be traded on the LN

LN - quick
BTC - holds the value
ML - tokenization, DEXs, and smart contracts, DEfi

## Formidable security, privacy, and performance

### UTXO System 

Similarly to Bitcoin, Mintlayer uses a UTXO (Unspent Transaction Output) system for transactions. This means that there is no notion of wallet (or account) at the chain level in Mintlayer as there is on blockchains such as Ethereum, Polkadot, or Solana. Instead, the blockchain keeps a history of all transactions, where each transaction consists of a set of inputs and outputs. Every input to a transaction is the output of a previously executed transaction, and a transaction output is considered _unspent_ if it does not appear as an input in any transaction.

The absence of accounts at the chain level offers privacy-related advantages. For example, wallets can implement logic allowing an end user to generate a different destination address for each transaction, while the wallet takes care of determining the user's balance by scanning the blockchain for all unspent transaction outputs and determining which outputs belong to the user. Using a different destination address for each transaction renders it far more difficult to link transactions to a specific users.

Furthermore, the UTXO system also makes it relatively simple to _mix_ transactions. Mixing is a process in which multiple inputs from different sources feed into a single transaction with several outputs having different destinations. Although all inputs and outputs are stil publicly visible, it is not possible to tie any individual source address to outputs that ended up at a specific destination.

From a resource management perspective, allowing multiple source and destination addresses within a single transaction improves performance and saves space on the blockchain. (**TODO** maybe elaborate).

- More resource conservative than account based, IF you see to it.
Ben wants to send me MLT, Enrico MLT.
1 input, two output's one pk, one signature
Each entire transaction is signed
Account based - need to have two completely separate tranasactions:
Two inputs, two outputs, two public keys, two signatures

Can have a single signature, single public key for both. Less energy to validate.

Bitcoin hash:
Need to be careful about mentioning attack prevention - not used.

Key thing: by including the bitcoin block hash - makes it more expensive to attack/rewrite the chain. To attack ML you would have to attack BTC. 

### Chainscript

Mintlayer implements Chainscript, its own scripting language and a superset Bitcoin script. Much like Bitcoin script, Chainscript allows customization of spending conditions on funds transferred from one user to another, and can also be used for simple smart contracts.

For example, suppose Alice wants to send Bob some money provided he is able to produce a secret password picked by Alice. Alice wants to be able to take the funds back if Bob is unable or unwilling to produce the password within 2 days. In Chainscript, these conditions are expressed by:

```
OP_IF
  OP_SHA256 <SHA256_OF_PASSWORD> OP_EQUALVERIFY <Bob_PK> OP_CHECKSIG
OP_ELSE
  <T+2days> OP_CHECKLOCKTIMEVERIFY OP_DROP <Alice_PK> OP_CHECKSIG
OP_ENDIF

```
where `<T+2days>` where  represents the Unix timestamp two days from now.

This script can be redeemed by Bob:
```
<Bob_SIG> <PASSWORD> 1
```

or by Alice, after two days have elapsed:
```
<Alice_SIG> 0
```

In addition to offering simplicity, Chainscript eliminates entire classes of security issues. For example, the absence of loops in Chainscript renders DoS (Denial of Service) attacks impossible.

Furthermore, the stack-based execution model of Chainscript ensures that the time and processing resources necessary to execute a script are proportional to the size of the script. As the maximum valid size for a script is bounded, so are the resources needed to execute it. In this way, the need for gas fees is eliminated in the case of simple (Chainscript) smart contracts.

## Signature aggregation through BLS

A significant part of every transaction in a utxo system consists of the sender's signature. For example, in Bitcoin's ECDSA, the signature makes up about 1/3 of the entire transaction.

Signature aggregation is a technique which dramatically reduces the space occupied by transaction signatures on the blockchain. After a validator has selected the transactions for the next block, it uses all of the transaction signatures as input to a _signature aggregation scheme_ (in Mintlayer's case, BLS) in order to compute a single "aggregated signature" (of the same size as the individual transaction signatures). Only the aggregated signature is stored in the block, and other validators can use it to verify all transactions in that block.

In this way, signature aggregation enables more transactions within a block of a given size. The advantages of this are manifold. The most direct benefit is storage space saved in the long run, which in in turn improves the speed of onboarding new nodes onto the chain. Having more transactions per block also makes it easier for any given transaction to be selected for the next block, which results in lower transaction processing times, and thus lower transaction fees.

## Confidential Transactions
Mintlayer's MLS-02 tokens will provide a way for users to exchange confidential assets.
**TODO** Ben will write a short paragraph

## Access Control List
Wheen you issue a token, you can define a list of blacklisted, whitelisted addresses. Implement a solution for companies building on minlayer to implement regulatory restrictions. ACL - simplify regulatory compliance.

**TODO** Ben to ask Enrico

One of the key things:
Banking is littered with compliance, regulations, laws.
ACLs make it more viable for projects to onboard
Tokenized stocks

ML is regulated by the San Marino government. When we first launch, we have to be careful who we sell tokens to. KYC. Also KYC on any money we receive

Whitelisted addresses:
As part of creating a token, a whitelist (a specific type of transaction)

99% people will probably never use an ACL. Most tokens would never touch them. Niche bit of technology. Eventually also for pp.
