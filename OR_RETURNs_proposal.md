```
  BIP:
  Layer:
  Title:
  Author:
  Comments-Summary: No comments yet.
  Comments-URI:
  Status: Draft
  Type:
  Created: 2020-10-12
  License:
```

## Abstract

The purpose of this proposal is getting a commitment to eliminate the restriction that enforces having only a single `OP_RETURN` output within a transaction. It's a policy change not concensus change. 4 line in node code change e.g. /BitcoinUnlimited/src/policy/policy.cpp.
```
// only one OP_RETURN txout is permitted
// if (nDataOut > 1)
// {
//     reason = "multi-op-return";
//     return false;
// }
```
This restriction has to be replaced with one enforces the total maximum size across multiple `OP_RETURN` outputs within a transaction.

## Motivation

The primary motivation for this proposal comes from a desire to combine multiple `OP_RETURN` based protocols (such as SLP) and to add additional data to token transactions that utilize these protocols. Currently it is not possible to construct a transaction using SLP and append additional data to said transaction. Allowing for such additional data could open up new use case opportunities.

## Problem example

One example of such a use case is the current inability to associate SLP fungible and non-fungible tokens with each other.

Every type of SLP transaction (GENESIS, MINT, SEND) for both tokens types ([Token Type 1](https://github.com/simpleledger/slp-specifications/blob/master/slp-token-type-1.md) and
[NFT1](https://github.com/simpleledger/slp-specifications/blob/master/slp-nft-1.md)) use `OP_RETURN` to store their token information. SLP transactions are rendered invalid if any extraneous data are added to their `OP_RETURN` information.
This makes it impossible to combine other metadata or to mix multiple token protocols within a single Bitcoin Cash transaction.
Moreover, there is not any other implied way to store arbitrary data on bitcoin cash blockchain within SLP transactions.

Problem actors: __Account1__, __Account2__  

*Steps to reproduce the problem:*

* Create an SLP defined Token Type 1 (Fungible) token (Token Type 1 GENESIS transaction) - __TokenFund__ (*N* amount owned by __Account1__)
* Create an SLP defined NFT "Group" token (NFT Group GENESIS Transaction)
* Instantiate *X* number of SLP defined NFT child tokens to *X* number of transaction outputs (one per output) (NFT Group SEND Transaction)
* Create an SLP defined NFT "Child" token (NFT Child GENESIS Transaction) - __TokenNFTChild1__ (owned by __Account2__)
* Send *N* of __TokenFund__ from __Account1__ to __Account2__ and associate the amount *N* of __TokenFund__ to the specific __TokenNFTChild1__

Single `OP_RETURN` is already being used by SLP defined __TokenFund__ token. This leaves no way to add metadata that creates an association between the SLP defined NTF1 and SLP defined Token Type 1.

*Proposed solution:*
Add an additional `OP_RETURN` output that points to the __TokenNFTChild1__ token ID, which is its GENESIS transaction hash.

## Specification

This BIP proposes allowing multiple `OP_RETURN` outputs in a transaction, but maintaining the same size limit across all `OP_RETURN` outputs.

## Risks
Increase mem pool?

## Compatibility

This proposal is backward compatible.

## Reference implementation
This reference implementation is trying to exemplify the use case of having a multiple op_return in a single transaction output.
By remove the check single op_return from the node policy. That's mean the node have no consensus change and

### node code change
https://github.com/BitcoinUnlimited/BitcoinUnlimited/compare/release...ActorForth:feature/regtest-multi-op-return

node

### slp serve

http://128.199.203.157:12300/explorer

### bch-api
http://128.199.203.157:3000/v3/blockchain/getBlockchainInfo

## Copyright
<!-- This BIP is licensed under the 2-clause BSD license. -->

## References


1. [BUIP140: Multiple OP_RETURN with shared size limit](https://bitco.in/forum/threads/buip140-multiple-op_return-with-shared-size-limit.24952/) *by Jonathan Silverblood*
2. [BUIP149: Delimited OP_RETURNs](https://bitco.in/forum/threads/buip149-delimited-op_returns.26362/#post-111375) *by Jonathan Silverblood*
