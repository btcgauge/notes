# BTC-based applications

### Discrete log contracts (DLCs)
- Contracts that use one or several oracle signatures for enforcement
- Take less block space and more private when using adaptor signatures: the adaptor points on the oracle's expected signature. Each party gives each other invalid transaction signatures that can only be made valid in conjunction with the oracle signature
  
- Based on ECDSA adaptor signatures on layer 1 (possibly on LN in the future):
  - Two parties send funds to a multi-signature address
  - In order to settle the transaction, an oracle signs the contract with a signature that corresponds to the hash of the winning outcome (e.g., specific strings)
  - The person with the hash that corresponds with the oracle’s signature can then withdraw the funds from the contract
  - Actors: Suredbits, Crypto Garage, Atomic Loans, Square Crypto-funded Loyd Fornier, Chaincode Labs developer Antoine Riard
  - Major implementations on [bitcoin-s](https://github.com/bitcoin-s/bitcoin-s/tree/adaptor-dlc/dlc/src/main/scala/org/bitcoins/dlc), [NDLC](https://github.com/dgarage/NDLC), [rust-dlc](https://github.com/p2pderivatives/rust-dlc), [cfd-dlc](https://github.com/p2pderivatives/cfd-dlc)
- Use cases:
  - betting (elections, sports) - 1st use case: [bet on 2020 US elections results](https://twitter.com/NicolasDorier/status/1303356212705030144) between Christian Stewart (Suredbits) and Nicolas Dorier (BTCPayServer) using [bitcoin-s], a JVM toolkit](https://github.com/bitcoin-s/bitcoin-s) and [this oracle](https://twitter.com/outcomeobserver)
  - hedging (e.g., forward contracts) or synthetic assets (synthetic gold in BTC-denominated terms)
  - DLC on the Lightning Network using Taproot
   - does not require parties to have a channel open with each other
   - requires barrier escrows which is not yet implemented
   - distribute trust between multiple oracles (no single point of failure)
- Articles:
  - Original proposal: https://adiabat.github.io/dlc.pdf
  - [ ] https://medium.com/@gertjaap/discreet-log-contracts-invisible-smart-contracts-on-the-bitcoin-blockchain-cc8afbdbf0db
  - [ ] https://suredbits.com/category/discreet-log-contracts/
  - [ ] [DLC specifications](https://suredbits.com/category/discreet-log-contracts)
  
 ### Bitcoin-related projects
 - Bitcoin Optech: https://bitcoinops.org/
 - Bisq: https://bisq.network/
 - Block explorer: https://blockstream.info/
 - DIY hardware wallet: https://github.com/cryptoadvance/specter-diy
 - C cryptographic library: https://github.com/ElementsProject/secp256k1-zkp/blob/secp256k1-zkp/README.md
 - Taproot activation: https://taprootactivation.com/
 - https://en.bitcoin.it/wiki/Promotional_graphics
 - https://github.com/cryptoadvance/specter-desktop
 - https://github.com/esotericnonsense/bitcoind-ncurses2
 - https://github.com/petertodd/python-bitcoinlib
  
 ### Bitcoin security guides
 - https://bitcoinsecurity.guide/
 - https://btcguide.github.io/
 
 ### Self-sovereignty
 - Coinbase supports withdrawing from bech32 - bc1 addresses
 
 ### Taproot
 - Taproot activation: https://taprootactivation.com
 - Tapscript: https://bitcoinops.org/en/topics/tapscript/
 
 ### Multi-signatures
 - Multi-signatures: n of n
 - Threshold signatures: k of n (k <n)
 - MuSig2: https://medium.com/blockstream/musig2-simple-two-round-schnorr-multisignatures-bf9582e99295
 
 ### Coinjoin markets
 - https://github.com/JoinMarket-Org
 - https://www.keepitsimplebitcoin.com/joinmarket
 - https://www.youtube.com/watch?v=9COdlsh4inY&list=PLZKkuPrgFw0bnVJDyzMGHDJ965mlw5Qxg&index=7
 
 ### Miniscript
 - https://github.com/sipa/miniscript
 
