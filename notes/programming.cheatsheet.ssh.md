---
id: dzmgixwurf0d3b3p6n4khi5
title: Ssh
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
latitude: 10.8142
longitude: 106.6438
altitude: 0
title_imported: SSH
updated_imported: '2020-04-01T03:40:42.000Z'
created_imported: '2020-04-01T03:39:23.000Z'
---

# SSH Key - Still asking for password and passphrase

## Add Identity without Keychain
There may be times in which you don't want the passphrase stored in the keychain, but don't want to have to enter the passphrase over and over again.

You can do that like this:

ssh-add ~/.ssh/id_rsa 
This will ask you for the passphrase, enter it and it will not ask again until you restart.


## Add Identity Using Keychain
As @dennis points out in the comments, to persist the passphrase through restarts by storing it in your keychain, you can use the -K option (-k for Ubuntu) when adding the identity like this:

ssh-add -K ~/.ssh/id_rsa
Once again, this will ask you for the passphrase, enter it and this time it will never ask again for this identity.