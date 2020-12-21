### Steps
1. Install VirtualBox
1. Create an Ubuntu LTS Virtual Machine (VM) on VirtualBox
1. (Optional) Access the VM via SSH from another computer
1. Install Bitcoin on Ubuntu
1. Install and configure Tor
1. Bitcoin Initial Block Download (IBD)
1. Confirm Bitcoin is up and running

Todo:
- [ ] not done yet
- [x] already done

### Resources
- Coinkite: https://www.youtube.com/channel/UCqMPBcyg_wemgvC1jDI3EIw/videos
- Keep It Simple Bitcoin: https://www.keepitsimplebitcoin.com/

### How to install Ubuntu Server on VirtualBox and access it with SSH
- download Ubuntu Server LTS (20.04+) - ubuntu-20.04.1-live-server-amd64.iso (973.2 MB)
- download VirtualBox - VirtualBox-6.1.16-140961-OSX.dmg (134.6 MB)
- VirtualBox
  - > Create: Type \ Version: Linux Ubuntu (64-bit), RAM = 4GB > Create
  - Choose disk size > Create
  - Settings: System > Processor: 2 CPU, Storage > ID controller > select .ISO file
  - Settings: Network > Attached to: Bridge Adapter, Advanced > Promisciuous: allow all > OK
  - Start
  - Install Open SSH server (do not select identity)
  - access from external IP address + SSL certificate
- How to access Ubuntu server running in VirtualBox from outside - Networking adapters:
  - NAT (Network Address Translation): the guest's network adapter's IP is in a virtual subnet which includes the guest and the host VirtualBox application which acts as a gateway. The guest can access the Internet but the guest cannot be accessed from anywhere. Use: browse the Web, download files and view e-mails inside the guest
  - Bridged networking: VirtualBox connects to one of your installed network cards and exchange network packets directly, circumventing your host operating system's network stack. Use: network simulations and running servers in a guest
  - Host-only networking: network containing the host and a set of virtual machines, without the need for the host's physical network interface
  - If you need to access your guest from outside, you need to configure bridged networking, which will give your guest its own IP in your local network. The configuration is done in VirtualBox settings, not in the guest OS. To be able to access the server from outside your LAN (e.g., your mobile), after configuring the networking you additionally will need to set up port forwarding on your DSL modem: VirtualBox > Settings > Network > Attached to: Bridged network, and select desired host interface from the list at the bottom of the page, which contains the physical network interfaces of your systems (en1: AirPort for the Mac wireless interface or en0: Ethernet for the network cable interface)
  
### Access Ubuntu server running in VirtualBox from an outside network
- set it with Bridget network adapter so it has its own IP and you can address your Ubuntu server from inside your own network
- $ curl ifconfig.me returns the outside IP
- regardless of whether the server is a real or a virtual machine, you need to forward the appropriate port(s) across your LAN router, i.e., translate from the public IP you obtained from https://ifconfig.me to the server's private IP. You will need to refer to your router's documentation to see how to do that for your specific 


Increase virtual disk to full size
- https://manjaro.site/how-to-extend-lvm-disk-on-ubuntu-20-04/
- `$ sudo lvm lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv`
- `$ sudo resize2fs -p /dev/mapper/ubuntu--vg-ubuntu--lv`
- `$ sudo df -h`

### Run Bitcoin on Tor

> In order to defend against traffic analysis and network surveillance, hide your IP address and route all of your Bitcoin traffic through the Tor network. Once done, you'll only see the peers' onion instead of IP addresses

1. Install tor: `$ sudo apt-get install tor`
1. To check if tor is running (active), run: `$ service tor status`. After tor is installed, tor will be ON by default after each login
1. Check the default tor configuration file: `$ sudo vim /usr/share/tor/tor-service-defaults-torrc`
```sh
DataDirectory /var/lib/tor
PidFile /run/tor/tor.pid
RunAsDaemon 1
User debian-tor

ControlSocket /run/tor/control GroupWritable RelaxDirModeCheck
ControlSocketsGroupWritable 1
SocksPort unix:/run/tor/socks WorldWritable
SocksPort 9050

CookieAuthentication 1
CookieAuthFileGroupReadable 1
CookieAuthFile /run/tor/control.authcookie

Log notice syslog
ControlPort 9051
```
4. Check the tor group name to be as above (debian-tor): `$ cat /etc/group`. It shows `debian-tor:x:118:` if there is no user in that group, `debian-tor:x:118:USER_NAME` otherwise
1. Check your identity: `$ who`
1. List the group of which you are a member: `$ id USER_NAME`
1. Add user to Tor group: `$ sudo adduser USER_NAME debian-tor`
1. Confirm the creation by running (1) and (3) again
1. Edit ~/.bitcoin/bitcoin.conf: `$ vim ~/.bitcoin/bitcoin.conf`
```sh
# [User access to the blockchain]
txindex=1
# [Tor configuration]
proxy=127.0.0.1:9050
bind=127.0.0.1
onlynet=onion
# disable node's DNS lookup to query for peer addresses
dnsseed=0
dns=0
# Tor fallback nodes - can be updated once you have more peers
# BlueMatt's node
addnode=nkf5e6b7pl4jfd4a.onion
addnode=yu7sezmixhmyljn4.onion
```
10. If file /etc/tor/torrc does not exist, create it: `$ sudo touch /etc/tor/torrc`
1. Add the following lines to /etc/tor/torrc if not already there: `$ sudo vim /etc/tor/torrc`
```sh
ControlPort 9051
CookieAuthentication 1
CookieAuthFileGroupReadable 1
```
12. Stop bitcoind if it is running: `$ bitcoin-cli stop`
1. Reboot your machine: `$ reboot`
1. After the reboot, check the logs: `$ tail -f ~/.bitcoin/debug.log`
1. In another terminal, start bitcoin: `$ bitoind`
1. Check all the new nodes: `$ grep "New outbound peer connected" ~/.bitcoin/debug.log`
1. Find out own onion address: `$ grep "tor: Got service ID" ~/.bitcoin/debug.log`, e.g., a7zzgota55omnenz.onion:8333
1. Get network info: `$ watch bitcoin-cli getnetworkinfo`
1. Run: $ `firefox https://bitnodes.io` and copy and paste your onion address (unreachable until the initial block load is complete?)
1. Get list of peer nodes: `$ watch 'bitcoin-cli getpeerinfo | grep onion'`
1. Check progress of the Initial Block Download: `$ watch -n1 'bitcoin-cli getblockchaininfo | grep verificationprogress'`
1. `$ watch bitcoin-cli getnetworkinfo`
1. Confirm the the Initial Block Download (IBD) is complete: `bitcoin-cli getblockchaininfo | grep initialblockdownload`. It is set to `false` when the IBD is complete
1. Find your onion address: `bitcoin-cli getnetworkinfo | grep onion`
1. Check that your node is reachable at: https://bitnodes.io/

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
  - 6:58' (https://www.youtube.com/watch?v=OpmDF6EbiRk)

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
