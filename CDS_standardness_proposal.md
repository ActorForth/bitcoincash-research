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
The purpose of this proposal is getting a commitment to eliminate the restriction that enforces scriptPubkey of an Unspent Transaction Output (UTXO) 
of format `<sig> <msg> <pubkey> OP_CHECKDATASIGVERIFY` to be considered as "non-standard".
That restriction has to be lifted so the template will become one of standard UTXO formats.


## Motivation
The primary motivation for the proposal comes from a need to build a locking scripts that are primarily based on a signature for an arbitrary message.
As of this moment, all the "non-standard" locking scripts are executed within scriptSig (unlocking part), whereas the locking part stays as a "standard" P2SH.
However, `<sig> <msg> <pubkey> OP_CHECKDATASIGVERIFY` locking script is very straighforward and moreover allows indexing and querying the UTXOs without
having to actually spend them.

### Problem example
One example of such a use case is the current inabiltity to associate SLP fungible and non-fungible tokens with each other.

Every type of SLP transaction (GENESIS, MINT, SEND) for both tokens types ([Token Type 1](https://github.com/simpleledger/slp-specifications/blob/master/slp-token-type-1.md) and 
[NFT1](https://github.com/simpleledger/slp-specifications/blob/master/slp-nft-1.md)) use `OP_RETURN` to store their token information. SLP transactions are rendered invalid if any extrenous data are added to their `OP_RETURN` information.
This makes it impossible to combine other metadata or to mix multiple token protocols within a single Bitcoin Cash transaction.
Moreover, there is no any other implied way to store arbitrary data on bitcoin cash blockchain within SLP transactions.

Problem actors: __Account1__, __Account2__  

*Steps to reproduce the problem:*

* Create an SLP defined Token Type 1 (Fungible) token (Token Type 1 GENESIS transaction) - __TokenFun__ (*N* amount owned by __Account1__)
* Create an SLP defined NFT "Group" token (NFT Group GENESIS Transaction)
* Instantiate *X* number of SLP defined NFT child tokens to *X* number of transaction outputs (one per output) (NFT Group SEND Transaction)
* Create an SLP defined NFT "Child" token (NFT Child GENESIS Transaction) - __TokenNFTChild1__ (owned by __Account2__)
* Send *N* of __TokenFun__ from __Account1__ to __Account2__ and assosiate the amount *N* of __TokenFun__ to the specific __TokenNFTChild1__

Single `OP_RETURN` is already being used by SLP defined __TokenFun__ token. This leaves no way to add metadata that creates an association between the SLP defined NTF1 and SLP defined Token Type 1.

*Proposed solution:*
Add an additional output to the transaction (following `<sig> <msg> <pubkey> OP_CHECKDATASIGVERIFY` scriptPubkey template) that points 
to the __TokenNFTChild1__ token ID in its `msg`, whose `sig` signature is done using __Account2__'s credentials, which is its GENESIS transaction hash.
OP_CHECKDATASIG is beneficial in this case as it brings the BCH compliance protocol protection by checking 
the `msg`'s correctness that other alternative approaches do *not*.


## Specification
This BIP proposes to eliminate the restriction that enforces scriptPubkey of an Unspent Transaction Output (UTXO) of 
format `<sig> <msg> <pubkey> OP_CHECKDATASIGVERIFY` to be considered as "non-standard".


## Risks



## Compatibility

This proposal is backward compatible.


## Reference implementation
* 
* 


## Copyright
<!-- This BIP is licensed under the 2-clause BSD license. -->
