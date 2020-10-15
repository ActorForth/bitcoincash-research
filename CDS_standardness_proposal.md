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
However, `<sig> <msg> <pubkey> OP_CHECKDATASIGVERIFY` locking script is very straighforward and moreover allows indexing and quering the UTXOs without
having to actually spend them.


### Problem example



## Specification
This BIP proposes 



## Risks



## Compatibility

This proposal is backward compatible.


## Reference implementation
* 
* 


## Copyright
<!-- This BIP is licensed under the 2-clause BSD license. -->
