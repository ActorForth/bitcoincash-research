# SHA-Gate

~Placeholder description of document~

## Actors

The following are actors who must be involved in any SHA-Gate style transaction between BCH and smartBCH.

* Alice - An example person transacting via SHA-Gate.
* Operators (1...3) - Facilitate receiving and dispersing funds on both networks.
* BCH-Node - The communication endpoint for the BCH main chain.
* smartBCH-Node - The communication endpoint for the smartBCH side chain.
* BCH miners - Individuals who compete to validate new blocks on the BCH main chain.
* Unexplained - A placeholder entity for mechanisms not explained in the SHA-Gate document.

## BCH to smartBCH Sequence Diagram

This diagram shows Alice initiating the sequence necessary to move BCH from the BCH main chain onto the smartBCH side chain.

```mermaid
sequenceDiagram
    participant A as Alice
    participant U as Unexplained
    participant B as BCH Node
    participant O as Operators
    participant S as smartBCH Node
    autonumber

    A->>U: Gather 3 operator public keys
    Note over U: Exact details not explained
    A->>B: Create cc_covenant transaction
    Note over A: The 3 operator public keys are used to create the covenant
    O->>B: Listen for cc_covenant transactions
    Note over O: Newly 'mined' blocks are scanned for transactions matching the cc_convenant format
    O->>S: Send funds on smartBCH network to Alice.pubkey
    Note over O: Upon finding a valid transaction...
    Note over S: Transaction details not explained
```

## smartBCH to BCH Sequence Diagram

This diagram shows Alice initiating the sequence necessary to move BCH from the smartBCH side chain onto the BCH main chain.

```mermaid
sequenceDiagram
    participant A as Alice
    participant S as smartBCH Node
    participant O as Operators
    participant B as BCH Node
    participant M as Miners
    autonumber

    A->>S: Create BCH Request
    Note over S: Transaction details not explained
    O->>S: Listen for BCH Requests
    O->>B: Activate cc_covenant f(init) using Alice.pubkey as receiver
    Note over O: Upon finding valid request...
    B->>B: Covenant ages
    Note over B: (spendable after 150 blocks)
    M->>B: Vote (yes, no) or ignore cc_covenant
    A->>B: Spend UTXO when cc_covenant conditions are met
```

## Unknowns

This is a list of things not evidently clear in the SHA-Gate article.

### BCH to smartBCH

* How does Alice acquire the 3 public keys of the Operators?
* How do operators scan BCH transactions for cc_covenants when P2SH outputs would change depending on various factors? (e.g. which operator public keys are used)
* How are Operators able to unlock funds on the smartBCH network?

### smartBCH to BCH

* What does a 'BCH request' transaction on smartBCH look like? Code example?
