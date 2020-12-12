### Topics
1. [ ] [Cryptography 101](#cryptography-101)
1. [ ] [What is Bitcoin](#what-is-bitcoin)
1. [ ] [Why do you need to run your own node](#why-do-you-need-to-run-your-own-node)
1. [ ] [Basic command line skills](#basic-command-line-skills)
1. [x] [Resources](#resources)

If you are interested in crypto-currencies, I suggest you invest time in understanding Bitcoin first, because Bitcoin is the original crypto-currency protocol and many other crypto-currencies re-use the same concepts. Having said that, once you understand the characterestics of Bitcoin and the trade-offs made by other crypto-currencies, there is a high chance that you will decide to stick with Bitcoin

### Cryptography 101

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
- Bitcoin signatures and SHA256 hash function digests are 256-bit long. If you want to find a message's SHA256 digest or message's 256-bit signature, there is no better method than guess and check random values. This would require, on average, 2^255 guesses.
- Average number of guesses made to guess a number between 1 and n, assuming n is chosen uniformly at random (that is with probability 1/n) = Sum(a=1 to n)(1/n x a) = 1/n x Sum(a=1 to n)(a) = (1/n)(n)(n+1)/2 = (n+1)/2
- Sum(1 to 100) = (100+1) + (99+2) + ... + (51+50) = (n+1)(n/2)
- Number of atom in the visible universe ~ 10^80 ~ 2^266
- 2^256 = (2^32)^8 = (4 Bn)^8 ~ 10^77
- 2^256 = (4 Bn) x (4 Bn) x (4 Bn) x (4 Bn) x (4 Bn) x (4 Bn) x (4 Bn) x (4 Bn)
  - A really good Graphics Processing Unit (GPU) on a computer can run a cryptographic hash function and generate a little less than 1 Bn hashes per second. The first 4 Bn can represent the number of hashes per second per computer (H/s)
  - Imagine 4Bn computers (Google is estimated to have about single digit millions of servers -> 4Bn servers ~ 1 kilo Google worth of computing power
  - There are less than 8 Bn people on Earth. Imagine giving a little over half of every individual on Earth their own personal KiloGoogle
  - Imagine 4Bn copies of this Earth (compared to 100 to 400 billion starts in the Milky Way) ~ 1% of every star in the galaxy
  - Imagine 4 Bn copies of the Milky Way. Let's call it our GigaGalactic Super Computer
  - 4 Bn seconds is about 126.8 years x 4 Bn = 507 Bn years (which is about 37 times the age of the universe)
  - You would still only have a 1 in 4 Bn chance of finding the correct guess
  - Total Bitcoin mining power ~ 5 Bn Bn H/s (1/3 of our KiloGoogle). Bitcoin miners use Application Specific Integrated Circuits. These are pieces of hardware that are a thousand times better than a GPU (Tn H/s) and are specifically designed for Bitcoin mining, for running a bunch of SHA-256 hashes in parallel and nothing else. There is a lot of efficiency gains to be had when you throw out the need for general computation and design your integrated circuits for one and only one task.

```
                 __ n                                                  
                \     x                                                
                /__ 1        1      __ n       1     n(n + 1)     n + 1
Average(x)  =  ---------  =  -  *  \     x  =  -  *  --------  =  -----
                   n         n     /__ 1       n         2          2  
```

### What is Bitcoin
- The Bitcoin protocol is an alternative to the banking system to make payment transactions and store value
- A clever system of decentralized trusteless verification based on cryptography:
  - Cryptographic hash functions
  - Digital signatures
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
Cryptographic hash functions can prove that a particular list of transactions is associated with a large amount of computational effort. 







======

What is Bitcoin?

Bitcoin is not a company. It’s free open source software. 
You can buy a fraction of 1 bitcoin.
Why would I buy bitcoin?

Centralization of miners, of developers


Requiring from a third party to do a transaction is intrusive

Initial price discovery
Seize or inflate the supply


Digital form of payment
Decentralized (censorship resistant), digital scarcity (limited supply)
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
Run your own node
Not your keys, not your sats

Full node
https://bitcoinist.com/6-reasons-run-bitcoin-full-node/


Inflation and censorship-resistant value network
Sound money: scarcity and censorship resistance
1 BTC = 1BTC

Cannot be controlled by a sIngle party

Bitcoin is not tied to anything, neither is USD or EUR, and how big is your position?

Store and exchange value
resilient to counterfeiting (double-spends)
trust-less
no counter party risk
global distributed consensus

Trade-offs:
* electricity-consuming proof-of-work (mining)
* high-latency transactions (multiple transaction confirmations)
* limited scalability (monolithic blockchain containing every transaction, ever).

the ledger is the currency
the ledger is not distributed, it is replicated

Public conversations matter. They shape social norms.

Security
Privacy
Freedom (as in free speech)

Build your own ISP (Internet mesh network)
BlueMat
Purchase an IP block
Multi-homed

Router and data
At least 2 Internet connections

Full-sovereignty project:
- land, solar panel
- own ISP

Running your own node:
Security: fixed supply
Do not trust verify

Security
Privacy
Freedom: censorship resistance
 Reduces the need to trust other people of group of people (governments, organizations)

#### Main characterics:
- censhorship resistance
- permissionless

#### Main components:
- Proof of Work: 
- Blockchain: cryptographically-chained block of transactions (distributed ledger -> replicated ledger)

Side chains
Lightning payments for small payments

Testnet box

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

#### Common terms that are misnommers
Misnommers | Correct description
--- | ---
Wallet | Key chain, as it stores private and public keys (and track amounts associated to them), not credit cards and bills
Address | Deposit ID, as they identify a certain deposit and they should never be re-used

#### Past controversies
Controversy | Background
--- | ---
ASICs vs GPUs | ASICs vs GPUs
Big vs small blocks | Segwit hard/soft forks, Bitcoin (handle decisions)

#### Explanation of the Twitter memes:
Meme | Why | How
--- | --- | ----
Not your keys, not your bitcoin | bitcoins hosted on an exchange are controlled by the exchange, not you | Move your bitcoin to a hardware wallet, preferably http://usecoldcard.com
Don't trust, verify | Verify that your transactions was added to a block to prove ownership | Run your own node
Bitcoin, not blockchain | A response to the 'Blockchain, not Bitcoin' / 'Blockchain technology' / 'Distributed Ledger Technology' memes from 2016 | Blockchain is one required component in Bitcoin, but, on its own, is not necessarily permissionless, secure or censorship resistant
Bitcoin does not care | Many in the Bitcoin community have strong opinions about economics and social topics, but Bitcoin remains a censorship resistant, permissionless, secure means of payment no matter what your or other's opinions are | Learn about the Bitcoin technology and make your own opinion. Meet Bitcoin maximalists. Most are adorable in person

### Why do you need to run your own node

### Basic command line skills

### Resources

- Learn how to use a computer the hard way: https://missing.csail.mit.edu
- The godfather of all Bitcoin resources: https://bitcoin.page
- Mastering Bitcoin: https://github.com/bitcoinbook/bitcoinbook
- Mastering Lightning Network: https://github.com/lnbook/lnbook
