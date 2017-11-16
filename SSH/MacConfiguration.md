# Add a key to ssh-agent permenantly with passphrase managed by keychain
Step 1 - Store the key in the keychain

Just do this once:

```sh
ssh-add -K ~/.ssh/[your-private-key]
```

Enter your key passphrase, and you won't be asked for it again.

(If you're on an earlier version of OSX (pre-Sierra), you're done, Step 2 is not required.)

Step 2 - Configure SSH to always use the keychain

It seems that OSX Sierra removed the convenient behavior of persisting your keys between logins, and the update to ssh no longer uses the keychain by default. Because of this, you will get prompted to enter the passphrase for a key after you upgrade, and again after each restart.

The solution is fairly simple, and is outlined in this github thread comment. Here's how you set it up:

Ensure you've completed Step 1 above to store the key in the keychain.
If you haven't already, create an ~/.ssh/config file. In other words, in the .ssh directory in your home dir, make a file called config.
In that .ssh/config file, add the following lines:

Host *
  UseKeychain yes
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_rsa

Change ~/.ssh/id_rsa to the actual filename of your private key. If you have other private keys in your ~.ssh directory, also add an IdentityFile line for each of them. For example, I have one additional line that reads IdentityFile ~/.ssh/id_ed25519 for a 2nd private key.

The UseKeychain yes is the key part, which tells SSH to look in your OSX keychain for the key passphrase.
That's it! Next time you load any ssh connection, it will try the private keys you've specified, and it will look for their passphrase in the OSX keychain. No passphrase typing required.