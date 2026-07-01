# Authentication
Use SSH to authenticate
- passwords can easily be hacked or brute-forced by tools and are very predictable.

## SSH client
To use SSH we need a client like [OpenSSH](https://www.openssh.org/) (included in Unix systems).
- To check if OpenSSH is in your system type `ssh -V`
- On windows also includes openssh, usually im using gitbash so its available .

## Generating a key
The command `ssh-keygen` generates a pair of keys, the private key (has no extension) and the public key (`.pub`).
- we can also specify which algorithm we want to use with the `-t` flag.
- `ed25519` is the default algorithm when just using `ssh-keygen`.
- `rsa` is also another good one
- use `ssh-add ~/.ssh/id_rsa` when using a passphrase, so we are not asked for it everytime (if using a passphrase)
- use the `-b` flag to set a bit size (only for rsa algorithm, ed25519 has a fixed size).
    - when setting key size, consider: bigger number, harder to break, but requires more resources, `4096` is considered the strongest size. while `2048` is secure enough with faster performance.
    - the performance is noticed at: generation, signing, verification, on slow servers, each handshake can take some time if using `rsa` with a `4096` bit size.
    - sticking to `ed25519` is often good enough

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

# Github SSH
The concept above is fine for logging into a server, however with Github we are identifying ourselves each time we do a git operation (push, pull etc), which is not the same as a login session.

Once a key pair is generated, we can place the public key in [github settings > ssh and gpg keys](https://github.com/settings/keys).

We can then configure `config` to use a private key for github
```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
```

Now every operation to a host like `git@github.com:repo.git` will recognize the User `git` and Host `github.com` and try to authenticate using private key accordingly. 
- The `repo.git` checks wether we have access to this repo or not, which we should if the private key is in the user settings.

We can run a verbose command like `ssh -v git@github.com` to see in the logs that it finds the corrent config.
