### Source
* https://missing.csail.mit.edu/

### Missing CS course # 5: command-line environment

* job control (killing, pausing or backgrouding a process)

* terminal multiplexers

* aliases

* dotfiles
  * plain-text configuration files, that begins with a dot
  * shell dotfiles: https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html
    * bash - ~/.bashrc, ~/.bash_profile (e.g., `export PATH="$PATH:/path/to/program/bin"`)
    * git - ~/.gitconfig
    * vim - ~/.vimrc and the ~/.vim folder
    * ssh - ~/.ssh/config
    * tmux - ~/.tmux.conf
  * organization: in their own folder, under version control, and symlinked into place using a script for:
    * easy installation on a new machine
    * change tracking, portability and synchronization across several machines
  * good resources: https://github.com/mathiasbynens/dotfiles and https://dotfiles.github.io/

* portabily

* remote machines
  * to ssh into a server you execute a command as follows: `$ ssh a_user@a_url_or_an_ip_address`
  * executing commands directly: `ssh foobar@server ls | grep PATTERN` will execute ls remotely and grep locally
  * SSH keys:
    * exploit public-key cryptography to prove to the server that the client owns the secret private key without revealing the key, so you do not need to reenter your password every time
    * the private key (often `~/.ssh/id_rsa` and more recently `~/.ssh/id_ed25519`) is effectively your password, so treat it like so.
    * key generation
      * to generate a pair you can run ssh-keygen: `ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519`
      * you should choose a passphrase, to avoid someone who gets hold of your private key to access authorized servers
      * use ssh-agent or gpg-agent so you do not have to type your passphrase every time
      * to check if you have a passphrase and validate it you can run ssh-keygen -y -f /path/to/key.
  * key based authentication
    * ssh will look into .ssh/authorized_keys to determine which clients it should let in
    * to copy a public key over you can use: `cat .ssh/id_ed25519.pub | ssh foobar@remote 'cat >> ~/.ssh/authorized_keys'`
    * or `ssh-copy-id -i .ssh/id_ed25519.pub foobar@remote` when available
  
