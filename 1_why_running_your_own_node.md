# What is Bitcoin, why would I buy it and best practices

## Topics
1. [ ] [Cryptography 101](#cryptography-101)
1. [ ] [What is Bitcoin and why would I buy it?](#what-is-bitcoin)
1. [ ] [If I do, why do I need a hardware wallet, and to run your own node](#why-do-you-need-to-run-your-own-node)
1. [ ] [Basic command line skills](#basic-command-line-skills)
1. [x] [Resources](#resources)

If you are interested in crypto-currencies, I suggest you invest time in understanding Bitcoin first, because Bitcoin is the original crypto-currency protocol and many other crypto-currencies re-use the same concepts. Having said that, once you understand the characterestics of Bitcoin and the trade-offs made by other crypto-currencies, there is a high chance that you will decide to stick with Bitcoin

## Cryptography 101

#### Public key cryptography
- Unlike symmetric cryptography that only uses private/secret keys, public key cryptography relies on public keys (pk) and private/secret key (sk)
- The secret key is a number generated randomly, that you should keep to yourself. The public key is calculated based on the secret key

#### Digital signatures
- A handwritten signature only depends on the signing party. A digital signature depends on both the signing party (or rathe, a private key owned by the signing party) and the message itself being signed using `Signature = Sign(message, sk)`. A small change in the message completely changes the digital signature, commonly a 256-bit number
- When you verify a signature against a given message and public key is valid using `Verify(message, signature, pk) = True or False`, you can feel extemely confident that:
  1. the only way someone could have produced it is if they knew the private key associated with the public key. The private key ensures that only the signer can produce the signature. It is infeasible, in practical terms, to find a valid signature if you don't know the private key because there is no strategy better than just guessing and checking random signatures using the public key pk that everyone knows, which takes stupid large amount of time (see 256-bit security below)
  2. the message signed (for instance, a Bitcoin transasction) is indeed the message used to generate the digital signature. None can copy one of your signature to forge it on another message (or Bitcoin transaction). The digital signature is only valid for that specific message. The digital signature on a Bitcoin transaction is a proof that the owner of the bitcoin on the transaction has seen the transaction and approved it

#### Cryptographic Hash functions
- A Hash function takes any kind of message or file, and outputs a string of bits with a fixed length, called hash or digest. The digest is meant to look random but it is not random; it always gives the same output for a given input. If you slightly change the input (editing on character or one pixel), the resulting digest changes completely
- Cryptograhpic hash functions are hash functions for which the resulting digest is entirely unpredictable: it is infeasibable to compute in the reverse direction. There is no better method than to guess and check. No one has ever found a way to reverse engineer the desired input by looking into the details of how the function works. There is no rigorous proof that it is hard to compute in the reverse direction and yet a huge amount of modern security depends on cryptographic hash functions (e.g., secure connections to online bank account)
- SHA256 is a cryptographic hash functions that return 256-bit digests

#### How secure is 256-bit security?
- Bitcoin security relies on digital signatures and SHA256 hash function digests are 256-bit long. As mentioned earlier, there is no better method than guess and check random values. `This would require, on average, 2^255 ~ 10^77 guesses` because (2^256 + 1) / 2 = 2^255, and the average number of tries to guess a number between 1 and n, assuming that the number we guess is chosen uniformly at random (that is witha probability 1/n) is:
```
                 __ n                                                  
                \     x             __ n                                
                /__ 1        1     \            1     n(n + 1)     n + 1 
Average(x)  =  ---------  =  -  *  /__ 1  x  =  -  *  --------  =  -----
                   n         n                  n         2          2  
```
- 2^255 = 2^31 * (2^32)^7 = (2 Bn) x (4 Bn) x (4 Bn) x (4 Bn) x (4 Bn) x (4 Bn) x (4 Bn) x (4 Bn)
- This is to be compared with the estimated number of atoms in the visible universe ~ 10^80 ~ 2^266
- Another point of comparison:
  - There are (4 Bn) x (4 Bn) seconds in about 544 Bn years (or more than 39 times the age of universe of 13.8 billion years). The Milky Way has between 100 and 400 Bn stars. Let's assume there are 4 Bn Earth-like planets in each galaxy and there are about 39 x (4 Bn) = 156 Bn galaxies in the universe.
  - A really good Graphics Processing Unit (GPU) can generate a little less than 1 Bn (10^9) hashes per second (H/s). But Bitcoin miners use Application Specific Integrated Circuits (ASICs). They are pieces of hardware that are specifically designed for running a large number of SHA-256 hashes in parallel and nothing else. The efficiency gain is 1000-fold compared to a GPU. A Bitcoin ASICs can generate about 1Tn (10^12) hashes per second (T/s).
  - Let's assume each Earth-like planets has the same population (there are about 8 Bn people on Earth) and each inhabitants has 4M ASICS running at all time. That would be (2 Bn) x (4 Bn) x (4 Bn) H/s computer power on each planet. If this computing power in the whole universe was generating hashes since the Big Bang, there would still have a 1 in 4Bn chance of finding the correct guess

## What is Bitcoin

### Summary
Bitcoin is an alternative to the banking system to make payments and store value. It consists of:
- a network protocol (a decentralized peer-to-peer network)
- a blockchain (a public ledger of transactions)
- a consensus rules (a set of rules for independently validating transactions and issuing currency)
- the Proof-of-Work algorithm (a mechanism for reaching global decentralized trustless consensus on the valid blockchain


Bitcoin is a clever system of decentralized trusteless verification based on cryptography (cryptographic hash functions and digital signatures) with:
- A public ledger that records payments (e.g., Alice pays Bob 20 sats) that is accessible to everyone (any one can make a payment, i.e., adding a transaction to the ledger)
- How are we supposed to trust that all these transactions are what the sender meant for

1. Only signed transactions are valid
  -  When you sign a transaction, the message has to include some unique id associated with that transaction so that transaction cannot send to the ledger several times

1. You never to settle up in cash as long as you have some way to prevent people from spending too much more than they take
The Bitcoin protocol prevents overspending by rejecting transactions when someone is spending more than they have on the ledger
Verifying a transaction requires the full history of transactions up to that point.
This step remove the connection between the ledger and physical cash / fiat (USD, EUR, etc).
If everyone in the world used this ledger, you could live your whole life just sending and receiving money on this ledger without ever converting to fiat money.

Exchanges of USD for BTC is independent from the Bitcoin protocol. They are more analogous to exchange USD for EUR or any other currency on the open market. 
Bitcoin is a bearer instrument. It means that amounts in BTC are payable in USD to anyone possessing the instrument (i.e., control of the BTC amount)

The ledger (a history of transactions) is the Currency. 

A centralized ledger is more efficient to run that a decentralized ledger (i.e., replicated on a large number of places) but this requires trusting a central location: who hosts the ledger? who controls the rules of adding new lines? To remove that bit of trust, we'll have everyone keep their own copy of the ledger.
When you want to make a transaction, you broadcast that into the Bitcoin network for Bitcoin nodes to hear and record on their own private ledgers (miners)

- Any digital data can be read and copied, so how do you prevent forgeries?

How can you get everyone to agree on what the right ledger is? How can you be sure that everyone else received and believes that same transaction?
How can you be sure that everyone else is recording the same transactions and in the same order?
A protocol that accept or reject transactions and in what order so that you can feel confident that anyone else in the world followin the same protocol has a personal ledger that looks the same as yours
This is the problem addressed in the original Bitcoin paper.
The Bitcoin protocol trusts whichever ledger has the most computational work put into it, using cryptographic has functions
If you use computational work as a basis for what you trust, you can make it so that fraudulent transactions and conflicting ledgers would require an infeasible amount of computation to bring about (Fraud <=> computationally infeasible)

Proof of Work
How can cryptographic hash functions prove that a particular list of transactions is associated with a large amount of computational effort. 
Imagine you have a special number that, when you add it at the end of a list of transactions, and apply SHA256 to the entire thing, the first 30 bits of the output are all zeros. How hard do you think it was to find that number?

For a random message, the probability that the hash happens to start with 30 successive zeros is 1 / 2^30, which is about 1 in a billion. Because SHA256 is a cryptographic hash function, the only only way to find a special number like that is just guessing and checking. Once you know the number, you can quickly verify that this hash really does start with 30 zeros by running SHA256 once. In other words, you can verify that they went through a large amount of work without going through that same effort yourself. This is called a Proof of Work. And importantly, all this work is intrinsincally tied to that list of transactions. If you change one of the transactions, even slightly, it would completely change the hash, so you would have to go through another billion guesses to find a new Proof of Work, a new number that makes it so that the hash of the altered list together with this new number start with 30 zeros.

Everyone is broadcasting transactions and we want a way for everyone to agree on what the correct ledger is.
The core idea behind the Bitcoin White Paper is to have everyone trust whichever ledger has the most work put into it.

The ledger is organized into blocks, where each block consists of a list of transactions, together with a proof of work, that is a special number so that the SHA256 hash of the whole block with a certain number of zeros (let's say 60 zeros).
There is a systematic way to choose the number of zeros.

A transaction is only considered valid if it is signed by the sender, a block is considered valid if it has a proof of work.
To make sure that there is a standard way to order these blocks, the block has to contain the hash of the previous block.
That way, if you change any block, or try to swap the order of two blocks, it would change the block that comes after it, which changes the next block, and so on. That would require redoing all of the work, finding a new special number for each of these blocks that makes their hashes start with 60 zeros. Because blocks are chained together, instead of calling it a ledger, this is commonly called a blockchain

Anyone can be a block creator. They'll listen for the transactions being broadcast, collect them into some block, and then do a whole bunch of work to find the special number that makes the hash of this start with 60 zeros, and once they find it,  broadcast the block they found. To reward a block creator for all this work, a special transaction (the coinbase) is added at the top of the block for an amount created out of thin air. This is the block reward. It is an exception to the rules about whether or not to accept a transaction. It does not come from anyone, so it does not have to be signed (no sender or signature). It means that the total amount of currency increases with each new block (adds to the total money supply). Creating blocks is often called mining since it requires a lot of work and it introduces new bits of currency into the economy.
Miners create blocks, broadcast those blocks and getting rewarded with new money for doing so.
From the miners' perspective, each block is a miniature lottery, where everyone is guessing numbers as fast as they can until one lucky miner find a special number that makes the hash of the block start with many zeros, and get the reward.

For everyone else who wants to use the system to make payments, you listen for new blocks being broadcasted by miners, updating your own personal copy of the blockchain

If you hear of two distinct blockchains with conflicting transaction histories, you defer to the longer one, which is the one with the most work put into it. If there is a tie, wait until you hear of an additional block that makes one longer. Although there is no cental authority, and everyone is maintaining their own copy of the blockchain, if everyone agrees to give preference to whichever blockchain has the most work put into it, there is a way to arrive at decentralized consensus.
You don't need to trust a central authority because you trust instead computational work.

Is trusting work really enough?
To see why this makes for a trustworthy system, and to understand at what point you should trust that a payment is legitiimate, it is helpful to walk through exctly what it would take to fool someone in this system. If Alice wants to fool Bob with a fraudulent block, she might try to send him one that includes her paying him, but without broadcasting that block to the rest of the network. This way, everyone else thinks that Alice never paid Bob. To do this, she'd have to a valid proof of work before all other miners, each working on their own block. And that can definitely happen. Maybe Alice just happen to win this miniature lottery before everyone else. But Bob will still be hearing broadcasts made by other miners, so to keep him believing this fraudulent block, Alice would have to do all the work herself to keep adding blocks on this special fork on Bob's blockchain that is different from what he's hearing from the rest of the miners.
As per the protocol, Bob always trusts the longest chain he knows about. Alice might be able to keep this up for a few blocks if just by chance she happens to find blocks more quickly all of the rest of the miners on the network combined.
Unless Alice has close to 50% of the computing resources among all of the miners, the probability becomes overwhelming that the blockchain that all the other miners are working on grows faster than the single fraudulent blockchain that Alice is feeding Bob. So in time, Bob will reject what he's hearing from Alice in favor of the longer chain that everyone else is working on.
That means that you shouldn't necessarily trust a new block that you hear immediately. Instead, you should wait for several new blocks to be added on top of it. If you still have not heard of any longer blockchains, you can trust that this block is part of the same chain everyone else is using. The longest chain is the consensus chain.

The main ideas of Bitcoin are:
- digital signatures
- the ledger is the currency
- decentralize
- proof of work
- blockchain

This distributed ledger system based on a proof of work is more or less how the protocol works, and how many other cryptocurrencies work.

The proof of work is to find a special number so that the hash starts with 60 zeros. The actual way the bitcoin protocol works is to periodically change that number of zeros so that it should take on average, 10 minutes, to find a block. As there are more and more miners added to the network, the challenge gets harder and harder in such a way that this miniature lottery has one winner about every 10 minutes.

Average block time:
Many newer cryptocurrencies have much shorter block time (LTC: 2.5 min, ETH: 15 seconds, XRP: 3.5 seconds).

Block rewards
All the money in Bitcoin ultimately comes from some block rewards. These reward 50 Bitcoins per block.

Block explorer
https://blockstream.info/

22:37

Bitcoin is not a company. It’s free open source software. 
Centralization of miners, of developers

Requiring from a third party to do a transaction is intrusive

Initial price discovery
Seize or inflate the supply

Digital form of payment
Decentralized (permissionless, censorship-resistant), digital scarcity (limited supply)
Programmable money, transportable over a communication channel

Electronic payment system based on cryptographic proof instead of trust, allowing any two willing parties to transact directly with each other without the need for a trusted third party. 

There are lots of things that make cryptocurrencies different from the U.S. Dollar. That they facilitate "illicit activities” is not one of them. If you want to disguise your true concern, which is that you’ll lose control over your citizens’ money, think a bit harder next time.

Why bitcoin has value?
It might be worth getting some in case it catches on (as money) - speculative aspect - Satoshi
It becomes valuable as it becomes useful, it becomes useful as it becomes valuable
-> it becomes valuable as it becomes useful (fungible), it becomes useful as more people use it, because of its limited supply, it 
Demand for a commodity drives the prices up in the short term when there is low price elasticity (it is difficult or impossible to produce more quickly, and there are no alternatives).
The natural user base and the equilibrium rate of adoption is uncertain, so the price is uncertain -> volatility
Getting value circularly (network effect)
Value increases with adoption. Price will stabilize as it’s adopted by a large percentage of its natural user base
Natural user base: fringe of the population, developed countries

Store of value and payment system are two sides of the same coin

As a thought experiment, imagine there were a base metal as scarce as gold, not useful for anything but transportable over a communication channel

Stack & hodl

https://bitcoinist.com/6-reasons-run-bitcoin-full-node/

Inflation and censorship-resistant value network
Sound money: scarcity and censorship resistance
1 BTC = 1BTC

Cannot be controlled by a single party
Store and exchange value
resilient to counterfeiting (double-spends)
trust-less
no counter party risk
global distributed consensus

Trade-offs:
- electricity-consuming Proof-of-Work (mining)
- high-latency transactions (multiple transaction confirmations)
- limited scalability (monolithic blockchain containing every transaction, ever).

the ledger is the currency
the ledger is distributed, it is replicated

Security: your ownership is secure
Privacy: you decide when and with whom you share your identity
Freedom (as in free speech): censorship resistance

It reduces the need to trust other people or group of people (governments to issue currency and manage its supply, banks to secure and settle transactions)

#### Main characterics:
- scarcity (the supply is limited and the schedule of issuance of new currency is known in advance
- censhorship resistance
- permissionless

#### Main components:
- Proof of Work: 
- Blockchain: cryptographically-chained block of transactions (distributed ledger -> replicated ledger)

Side chains
Lightning payments for small payments



#### Rebuttal to Common Criticisms: Bitcoin is ...
Criticism | Rebuttal
--- | ---
Too volatile | The Bitcoin technology is very stable with an uptime 99.985% (it was down for 15 hours - in 2010 and 2013 - over the 10+ years of Bitcoin's existence). The USD/BTC price being very volatile is a feature of its adoption being very early. The USD/BTC price can be seen as Bitcoin's expected value = USD/BTC in the long term x chances of happening. A price of $10,000 means that the market on average thinks that there is 1% chance for USD/BTC to reach $1M. The year-on-year price increase has been steady (*ADD REFERENCE*). The volatility is likely to decrease as the Bitcoin adoption life cycle moves to later phases
Just for speculation | Speculation on assets with high volatility is not uncommon, but the steady increase in the year-on-year USD/BTC price is sign that 
A bubble that will pop | Bitcoin has stood the test of time in spite the numerous [predictions](https://99bitcoins.com/bitcoin-obituaries) of its downfall. Even Satoshi Nakamoto was uncertain Bitcoin would succeed (*ADD REFERENCE*). No bubble in financial market history lasted more than 10+ years. The speculative bubble is refer to when asset prices deviate from it intrinsic value. The [tulip mania](https://en.wikipedia.org/wiki/Tulip_mania) in 1636/37 had no critical influence influence on the Dutch's republic prosperity and the whole bubble-burst cycle lasted less than 6 months
Can be replicated | Bitcoin Cash vs Bitcoin
Used by criminals | USD, EUR
Not money or currency | Early days: cash payment, now: store of value + settlement system for second layer solutions (Lightning Network, Liquid)
Consumes too much energy | Energy cannot be easily transported over long distances, so energy produced in scarcely populated areas with access of cheap energy with few consumers for it (Iceland, some parts of China, Northern Russia) are great places to mine Bitcoin.

#### Common misconceptions
Misconceptions | Responses
--- | ---
It secures transactions by solving a complex problem | That's a poor description of Proof of Work. The problem to solve is very simple. You need to guess a number that has certain properties and that takes a large amount of computational effort
Bitcoin is becoming too expensive to buy | The unit of currency in Bitcoin is sat not BTC, where 1 BTC = 100,000,000 sats. If 1 BTC is worth 100k USD, you can still purchase 1,000 sats for 1 USD (not including any fee)

#### Common terms that are misnommers
Misnommers | Correct description
--- | ---
Wallet | Key chain, as it stores private and public keys (and track amounts associated to them), not credit cards and bills
Address | Deposit ID, as they identify a certain deposit and they should never be re-used
Miners | Block creators

#### Past controversies
Controversy | Background
--- | ---
ASICs vs GPUs | ASICs vs GPUs
Big vs small blocks | Segwit hard/soft forks, Bitcoin (handle decisions)

#### Bitcoin historical events
- Jan 3
- Pizza day

#### Explanation of the Twitter memes:
Meme | Why | How
--- | --- | ----
Not your keys, not your bitcoin | bitcoins hosted on an exchange are controlled by the exchange, not you | Move your bitcoin to a hardware wallet, preferably http://usecoldcard.com
Don't trust, verify | Verify that your transactions was added to a block to prove ownership | Run your own node
Bitcoin, not blockchain | A response to the 'Blockchain, not Bitcoin' / 'Blockchain technology' / 'Distributed Ledger Technology' memes from 2016 | Blockchain is one required component in Bitcoin, but, on its own, is not necessarily permissionless, secure or censorship resistant
Bitcoin does not care | Many in the Bitcoin community have strong opinions about economics and social topics, but Bitcoin remains a censorship resistant, permissionless, secure means of payment no matter what your or other's opinions are | Learn about the Bitcoin technology and make your own opinion. Meet Bitcoin maximalists. Most are adorable in person
Stack sats | You buy or earn amount less than 1 BTC | You can buy or earn sats. 1 BTC = 100,000,000 sats

### Why do you need to run your own node

### Basic command line skills

### Resources

- Learn how to use a computer the hard way: https://missing.csail.mit.edu
- The godfather of all Bitcoin resources: https://bitcoin.page
- Mastering Bitcoin: https://github.com/bitcoinbook/bitcoinbook
- Mastering Lightning Network: https://github.com/lnbook/lnbook

### Wish list

- [ ] testnet running on the same box
- [ ] Rock64 bitcoin node
