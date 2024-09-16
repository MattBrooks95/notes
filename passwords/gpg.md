# GPG

`$ gpg --list-keys` lists the keys on your system

`$ gpg --send-key KEYNAME`
Publishes your public key to a default key server
Type out the whole key, as seen in the fingerprint output.
It will look like this `pub rsa3072/0x<blah blahblah> ...`, and the
`0x<blah blahblah>` part is what you need to pass to `$ gpg --send-key <key here>` as `<key here>`

`$ gpg --list-keys --fingerprint --keyid-format 0xlong <email>`
will list out the key in long form. The short form is not secure because it is not long enough to prevent collisions.
