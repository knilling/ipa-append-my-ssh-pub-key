# ipa-append-my-ssh-pub-key

## TL;DR
```
chmod +x ipa-append-my-ssh-pub-key
./ipa-append-my-ssh-pub-key
# add to `/etc/profile` or `~/.profile`, to run automatically on login
```

## The Problem
Let's say that you...
1. build a new machine as a FreeIPA client.
1. log into the new machine.
1. generate new SSH keys.
1. want to publish your new public SSH key to the FreeIPA server.

This sequence of events is annoying in one-off scenarios, and it is a major
pain if you constantly build up and tear down machines.

FreeIPA's `ipa` command line client does offer [a way to upload your public SSH key to the FreeIPA server](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/identity_management_guide/user-keys#uploading-user-keys).

However, `ipa --sshpubkey=` will not append your new SSH public key to your list
of public keys.  Instead, it will upload your new public key *and* *delete*
*all* *of* *your* *existing* *public* *SSH* *keys*.

## The Solution
Instead, what you really want to do is:
1. build a new machine as a FreeIPA client.
1. log into the new machine.
1. automatically generate new SSH keys, upon login.
1. automatically publish your new public SSH key to the FreeIPA server.
1. automatically remove any old SSH public keys form the FreeIPA
   for this user+host, if they exist.

That's where `ipa-append-my-ssh-pub-key` comes in.

This script
1. creates a new SSH keypair for you, if one doesn't exist.
1. checks to see if your current SSH public key is on the FreeIPA server.
1. uploads and *appends* your current SSH public key to your list of public keys
   on the FreeIPA server, if it is not already on the FreeIPA server.
1. uploads and *replaces* any old SSH public keys for your user+hostname, on the FreeIPA server,
   with your current SSH public key if such old public SSH keys exist.
