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
The purpose of this proposal is getting a commitment to eliminate the restriction that enforces to have one `OP_RETURN` output within a single transaction.
That restriction has to be replaced with the one enforcing the total maximum size across all the `OP_RETURN` outputs within a transaction.


## Motivation
The primary motivation for the proposal comes from a need to extend the ability for many protocols, that are based on `OP_RETURN` outputs, (e.g. SLP)
to have an ability to collaborate. Additionally, there's currently no ability to use `OP_RETURN` outputs for one's private purpose within transactions that also 
include the outputs of `OP_RETURN`-based protocols (e.g. SLP).  


### Problem example
One example of such a limitation would be a way to assosiate SLP Non-fungible tokens with SLP fungible ones.  
Every kind of SLP transaction (GENESIS, MINT, SEND) for both types of tokens ([Token Type 1](https://github.com/simpleledger/slp-specifications/blob/master/slp-token-type-1.md),
[NFT1](https://github.com/simpleledger/slp-specifications/blob/master/slp-nft-1.md)) uses `OP_RETURN`. 
This makes it impossible to combine multiple of those within a single bitcoin cash transaction. 
Moreover, there is no any other implied way to store arbitrary data on bitcoin cash blockchain within SLP transactions.


Problem actors: __Account1__, __Account2__  

Steps to reproduce the problem: 
* Create a SLP defined Token Type 1 (Fungible) token (Token Type 1 GENESIS transaction) - __TokenFun__ (N amount owned by __Account1__)
* Create a SLP defined NFT "Group" token (NFT Group GENESIS Transaction)
* Mint a SLP defined NFT "Group" token (NFT Group MINT Transaction)
* Create a SLP defined NFT "Child" token (NFT Child GENESIS Transaction) - __TokenNFTChild1__ (owned by Account2)
* Send N of __TokenFun__ from __Account1__ to __Account2__ and assosiate the amount N of __TokenFun__ to the specific __TokenNFTChild1__

OP_RETURN has already been taken by SLP defined __TokenFun__ token.

Proposed solution:
Add an additional OP_RETURN output that points to the __TokenNFTChild1__ token ID, which is its GENESIS transaction hash.


## Specification

This BIP proposes a 



## Risks



## Compatibility

This proposal is backward compatible.


## Reference implementation
* 
* 


## Copyright
<!-- This BIP is licensed under the 2-clause BSD license. -->


## References
1. [BUIP140: Multiple OP_RETURN with shared size limit](https://bitco.in/forum/threads/buip140-multiple-op_return-with-shared-size-limit.24952/) *by Jonathan Silverblood*
2. [BUIP149: Delimited OP_RETURNs](https://bitco.in/forum/threads/buip149-delimited-op_returns.26362/#post-111375) *by Jonathan Silverblood*
