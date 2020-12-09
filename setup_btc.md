### Learn how to use a computer the hard way
- https://missing.csail.mit.edu

### How to install Ubuntu Server on VirtualBox and access it with SSH
- download Ubuntu Server LTS (20.04+)
- download VirtualBox
- VirtualBox
  - > Create: Type \ Version: Linux Ubuntu (64-bit), RAM = 4GB > Create
  - Choose disk size > Create
  - > Settings
    - > System > Processor: 2 CPU
    - > Storage > ID controller > select .ISO
    - > Network > Attached to: Bridge Adapter, Advanced > Promisciuous: allow all > OK
  - Start
  - Install Open SSH server (do not select identity)
  - access from external IP address + SSL certificate
    
### Run Bitcoin on Tor
https://www.youtube.com/watch?v=57GW5Q2jdvw

Use Tor to defend against traffic analysis and network surveillance
In order to hid your IP address, route all of your bitcoin traffi through the Tor network.
Once done, you'll only see the peers' onion instead of IP addresses
$ watch bitcoin-cli getnetworkinfo # by default, only ipv4 and ipv6 networks are reachable
$ sudo apt-get install tor
after tor is install, every time you login into Ubuntu, tor will be ON by default
$ service tor status # to check if tor is running: Started -> Tor is active
$ sudo vim /usr/share/tor/tor-service-defaults-torrc


- Add user to Tor group:
sudo adduser USER_NAME debian-tor

- Edits to bitcoin.conf:
proxy=127.0.0.1:9050
bind=127.0.0.1
onlynet=onion
dnsseed=0
dns=0
addnode=nkf5e6b7pl4jfd4a.onion
addnode=yu7sezmixhmyljn4.onion

Edits to /etc/tor/torrc:
ControlPort 9051
CookieAuthentication 1
CookieAuthFileGroupReadable 1

### Bisq
- the P2P exchange network (peer-to-peer and anonymously)
- run core + download the software from bisq.network
- $ bitcoind
- $ firefox bisq.network
- $ cd Downloads
- $ sudo dpkg -i x.deb
- start bisq, read the TOS. It will help you understand the overall trading protocol and their multisig procedure. Everything is designed to make the trading process as trustless as possible
- Bisq > Settings > Network info: make sure that we're connecting to the bitcoin network using our full node
- The offer book shows you how much bitcoin you can buy at which price
- You trade directly with individuals. We'll have access to their identity and they to ours, but NOBODY else will
- The liquidity is increasing day by day (and better for EUR)
- Once we have a trade going, we'll manage it in Portfolio
- In Funds, we'll fund our trades and watch the confirmation of transactions
  - Both participants in a trade have to lock an amount of bitcoin, as a disincentive to cheat. When the trade is completed, you get your funds back minus a small fee for the exchange
  - Bisq > Account: first, backup the seed of your wallet using pen and pencil
  (no picture or screenshot). The seed words encode your wallet, but they don't save the state of your trades. For that, you need to back up the data directory
  - Bisq > Backup: back up whenever there's a change in any of your trades; start, fund, finish. 1. Select backup location 2. Backup now. It creates your Bisq data directory at ~/.local/share/Bisq. It makes a copy for you to store. Dumping your backup files on this path will recreate the state of your account and trades in any computer.
  - 6:58'

### Set up a BTC over Tor
- https://medium.com/coinmonks/how-to-run-a-bitcoin-full-node-over-tor-on-an-ubuntu-linux-virtual-machine-bdd7e9415a70
- https://blog.lopp.net/how-to-run-bitcoin-as-a-tor-hidden-service-on-ubuntu/
- https://www.maketecheasier.com/fix-no-route-to-host-error-linux/
- https://www.linux.com/topic/networking/beginners-guide-tor-ubuntu/
- https://www.semurity.com/how-to-host-an-anonymous-website-on-tor-network/


### Setup a node
- https://en.bitcoinwiki.org/wiki/Bitcoind


### Python library
- https://github.com/petertodd/python-bitcoinlib

### Sources of information
- https://bitcoinmagazine.com/best-of-bitcoin-magazine
