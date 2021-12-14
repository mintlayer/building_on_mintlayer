#  Blockchain developer looking to build your next project? Consider *Mintlayer*. #

Mintlayer combines the best features of different chains in a novel way.

At a glance, Mintlayer offers:

- Tokenization, including [NFTs](https://en.wikipedia.org/wiki/Non-fungible_token) and confidential tokens
- Support for [WebAssembly](https://webassembly.org/) smart contracts
- security and privacy by virtue of its [UTXO](https://en.wikipedia.org/wiki/Unspent_transaction_output) system and the [Chainscript](https://docs.mintlayer.org/whitepaper/4-decentralized-finance-defi) scripting language
- Signature aggregation through [BLS](https://crypto.stanford.edu/~dabo/pubs/papers/aggreg.pdf)
- Confidential transactions
- Interoperablity with [Bitcoin](https://en.wikipedia.org/wiki/Bitcoin) and the [Lightning Network](https://lightning.network/)

## Tokenization

Minting your own token on Mintlayer is as easy as submitting a transaction. No smart contract is required!
There are three ways to create your own token:
- _RPC_:  create a token issuance transaction and submit it to a Minlayer node via RPC
- _Wallet_: use the graphical user interface provided by Minlayer's mobile or web wallets
- _Cli_: use Mintlayer's [command line interface tool](https://en.wikipedia.org/wiki/Command-line_interface)

Currently, there are three types of tokens that can be issued on Mintlayer:
- **MLS-01**: "normal" fungible tokens, akin to [ERC-20](https://erc20.tech/) tokens on Ethererum
- **MLS-02**: confidential tokens, whose transactions are not publicly available on the blockchain
- **MLS-03**: NFTs

Mintlayer's native token, MLT, is used primarily for staking. It can also be used to pay transaction fees, although transaction fees can be paid in any Mintlayer token that the block creator has decided to accept.

To learn more about tokenization on Mintlayer, see the relevant [section of the whitepaper](https://docs.mintlayer.org/whitepaper/3-tokenization-standard).

## Access Control Lists

A notable feature of tokenization on Mintlayer is its support for Access Control Lists, or ACLs. ACLs are sets of rules, determined by the token creator at the time of issuance, which restrict the ways in which the token may be transferred. For example, an ACL may blacklist particular (untrusted) addresses, impose bounds on the amount of a token transferred, or enforce time locks on tokens transferred (rendering them "unspendable" for a period of time).

ACLs aim to provide an out-of-the-box solution for companies building on Mintlayer who are subject to particular company policies or regularory legislation.For more information about ACLs, their capabilities, and their possible applications, see [this](https://docs.mintlayer.org/whitepaper/4-decentralized-finance-defi#4.3.-acl-rules-for-securities) section of the docs.

## WebAssembly smart contracts

Decentralized applications of any complexity invariably require the use of [smart contracts](https://en.wikipedia.org/wiki/Smart_contract).

By supporting WebAssembly smart contracts, Mintlayer empowers blockchain developers of all backgrounds to confidently build and deploy decentralized applications.

Support for WebAssembly smart contracts means developers can code smart contracts in any language which compiles to WebAssembly, assuming library support is available.

Mintlayer natively supports [*ink!*](https://github.com/paritytech/ink), a framerork for smart contract development in [Rust](https://www.rust-lang.org/). One of the fastest-growing growing programming languages in recent years, Rust's meteoric rise over the past few years owes to its speed, safety, rich development ecosystem, and avid community of developers. A language supporting multiple programming paradigms, Rust strikes a balance between accessibility and ease of use on the one hand, and strong memory-safety and type-safety guarantees on the other. Mintlayer's support for *ink!* will be expanded and refined over time, while the community ports our libraries to other languages it would like to see supported.

Developers versed in Ethereum will probably feel most at home writing smart contracts in [Solidity](https://soliditylang.org/). [Solang](https://github.com/hyperledger-labs/solang), a project developed by Hyperledger Labs, facilitates compiling Solidity to WebAssembly, thereby providing a way for smart contracts written in Solidity to be deployed on Mintlayer.

## Interoperability with the Bitcoin ecosystem

Mintlayer is fully interoperable with Bitcoin and the Lightning Network. In plain terms, interoperability with Bitcoin means Mintlayer supports atomic swaps with Bitcoin without the need for an intermediary. Interoperability with the Lightining network means that tokens created on Minlayer (as well as the native MLT token, of course) can be exchanged over the Lightning Network, thus improving transaction speed and reducing pollution of the Mintlayer chain. In the future, Mintlayer will implement its own [decentralized exchange](https://docs.mintlayer.org/whitepaper/5-decentralized-exchange-dex) on top of the Lightning Network.

Thus, Mintlayer aims to create a fast, scalable, secure, ecosystem for decentralized fininance built on Bitcoin. In this ecosystem, the Mintlayer chain will provide the mechanisms for token minting and smart contract deployment, while transaction-heavy workloads can be carried out on the Lightning Network.

## Security, privacy, and performance

### UTXO System 

Similarly to Bitcoin, Mintlayer uses a UTXO (Unspent Transaction Output) system for transactions. This means that there is no notion of wallet (or account) at the chain level in Mintlayer as there is on blockchains such as Ethereum, Polkadot, or Solana. Instead, the blockchain keeps a history of all transactions, where each transaction consists of a set of inputs and outputs. Every input to a transaction is the output of a previously executed transaction, and a transaction output is considered _unspent_ if it does not appear as an input in any transaction.

The absence of accounts at the chain level offers privacy-related advantages. For example, wallets can implement logic allowing an end user to generate a different destination address for each transaction, while the wallet takes care of determining the user's balance by scanning the blockchain for all unspent transaction outputs and determining which outputs belong to the user. Using a different destination address for each transaction renders it far more difficult to link transactions to a specific users.

Furthermore, the UTXO system also makes it relatively simple to _mix_ transactions. Mixing is a process in which multiple inputs from different sources feed into a single transaction with several outputs having different destinations. Although all inputs and outputs are stil publicly visible, it is not possible to tie any individual source address to outputs that ended up at a specific destination.

From a resource management perspective, allowing multiple source and destination addresses within a single transaction enables improved performance and space efficiency on the blockchain, compared to what is possible with account-based systems.

For example, suppose Alice wishes to send some tokens to both Bob and Charlie. In a UTXO-based system, we can record this as a single transaction, and thus require only one mention of Alice's signature and public key to be saved on the blockchain. In an account-based system, we would need two separate transactions (one from Alice to Bob, and another from Alice to Charlie), and therefor two mentions of Alice's signature and public key, to effect the same change in state.

### Chainscript

Mintlayer implements Chainscript, its own scripting language and a superset [Bitcoin script](https://en.bitcoin.it/wiki/Script). Much like Bitcoin script, Chainscript allows customization of spending conditions on funds transferred from one user to another, and can also be used for simple smart contracts.

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

In addition to offering simplicity, Chainscript eliminates entire classes of security issues. For example, the absence of loops in Chainscript renders [DoS (Denial of Service)](https://en.wikipedia.org/wiki/Denial-of-service_attack) attacks impossible.

Furthermore, the stack-based execution model of Chainscript ensures that the time and processing resources necessary to execute a script are proportional to the size of the script. As the maximum valid size for a script is bounded, so are the resources needed to execute it. In this way, the need for gas fees is eliminated in the case of simple (Chainscript) smart contracts.

## Signature aggregation through BLS

A significant part of every transaction in a utxo system consists of the sender's signature. For example, in Bitcoin's [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm), the signature makes up about 1/3 of the entire transaction.

Signature aggregation is a technique which dramatically reduces the space occupied by transaction signatures on the blockchain. After a validator has selected the transactions for the next block, it uses all of the transaction signatures as input to a _signature aggregation scheme_ (in Mintlayer's case, BLS) in order to compute a single "aggregated signature" (of the same size as the individual transaction signatures). Only the aggregated signature is stored in the block, and other validators can use it to verify all transactions in that block.

In this way, signature aggregation enables more transactions within a block of a given size. The advantages of this are manifold. The most direct benefit is storage space saved in the long run, which in in turn improves the speed of onboarding new nodes onto the chain. Having more transactions per block also makes it easier for any given transaction to be selected for the next block, which results in lower transaction processing times, and thus lower transaction fees.

## Confidential Transactions
Mintlayer's MLS-02 tokens provide a way for users to exchange confidential assets. These tokens mirror the functionality of MLS-01 tokens, and can be created in a similar fashion. MLS-02 transactions are an opt-in feature when a user has the desire or there is a business need for privacy.
