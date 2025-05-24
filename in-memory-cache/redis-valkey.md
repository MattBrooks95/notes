Get Valkey CLI
`$ nix-shell -p valkey`

defaults to logging into localhost*:6379
    \* it's actually the numbers that localhost represents
`$ valkey-cli`

Become admin
`$ AUTH admin password`

List permissions for user 'default'
`$ ACL getuser default`

## PubSub
### Permissions
Add pub sub channel permissions
`$ ACL setuser default &*

 Here you can see that "channels" (the key) was followed by `""` (the value).
 After I used "setuser default &*", you can see that the value was set to &*, all channels.
 ```
 9) "channels"
10) ""
11) "selectors"
12) (empty array)
127.0.0.1:6379> ACL setuser default +&*
(error) ERR Error in ACL SETUSER modifier '+&*': Unknown command or category name in ACL
127.0.0.1:6379> ACL setuser default &*
OK
127.0.0.1:6379> ACL GETUSER default
 1) "flags"
 2) 1) "on"
    2) "sanitize-payload"
 3) "passwords"
 4) 1) "5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8"
 5) "commands"
 6) "-@all +@connection +@hash +@pubsub"
 7) "keys"
 8) ""
 9) "channels"
10) "&*"
11) "selectors"
12) (empty array)
```
