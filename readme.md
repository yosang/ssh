# Authentication
Use SSH to authenticate
- passwords can easily be hacked or brute-forced by tools and are very predictable.

## SSH client
To use SSH we need a client like [OpenSSH](https://www.openssh.org/) (included in Unix systems).
- To check if OpenSSH is in your system type `ssh -V`
- On windows we need something like `PuTTY` or `Gitbash` .

## Generating a key
The command `ssh-keygen` generates a pair of keys, the private key (has no extension) and the public key (`.pub`).
- we can also specify which algorithm we want to use with the `-t` flag.
- `ed25519` is the default algorithm when just using `ssh-keygen`.
- `rsa` is also another good one
- use `ssh-add ~/.ssh/id_rsa` when using a passphrase, so we are not asked for it everytime (if using a passphrase)

By default, keys are stored in `~/.ssh`, if this directory doesnt exist create it and give it full user permissions with `chmod 700 ~/.ssh`. Or just use `ssh-keygen` it will create the folder for you. 

When generating a key we will be prompted for location and passphrase.
- If you enter a passphrase you will be prompted for it everytime the key is used.

## authentication with ssh
To authenticate the format is `ssh -i ~/.ssh/privatekey user@server`.

However, if we got multiple keys, create a `config` file in `~/.ssh` then specify which key to use for the specific host
```
Host mycustomserver
    HostName 159.65.123.45
    User root
    IdentityFile ~/.ssh/privatekey
``` 

to authenticate use `ssh mycustomerserver` 
