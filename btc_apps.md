## BTC-based applications

- Discrete log contracts
  - Based on ECDSA adaptor signatures on layer 1 (possibly on LN in the future):
    - Two parties send funds to a multi-signature address
    - In order to settle the transaction, an oracle signs the contract with a signature that corresponds to the hash of the winning outcome (e.g., specific strings)
    - The person with the hash that corresponds with the oracle’s signature can then withdraw the funds from the contract
  - Oracle: https://twitter.com/outcomeobserver
  - Actors: Suredbits, Crypto Garage, Atomic Loans, Square Crypto-funded Loyd Fornier, Chaincode Labs developer Antoine Riard
  - Use cases:
    - betting (elections, sports)
    - hedging (e.g., forward contracts) or synthetic assets (synthetic gold in BTC-denominated terms)
   - Articles:
      - [ ] https://medium.com/@gertjaap/discreet-log-contracts-invisible-smart-contracts-on-the-bitcoin-blockchain-cc8afbdbf0db
