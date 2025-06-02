Get Valkey CLI
`$ nix-shell -p valkey`

defaults to logging into localhost*:6379
    \* it's actually the numbers that localhost represents
`$ valkey-cli`

Become admin
`$ AUTH admin password`

List permissions for user 'default'
`$ ACL getuser default`

# 'Testing' With the CLI
You can type `$ subscribe <channel name>` to enter a little loop in the Valkey cli that will show you messages published to a channel.

The output will look like this. The 'message from the game orchestrator' message is the message that my application sent.
```
2) "updates:14a8f980-c886-4768-8979-61eee3681655"
3) "message from the game orchestrator, posting to game update channel:updates:14a8f980-c886-4768-8979-61eee3681655"
Reading messages... (press Ctrl-C to quit or any key to type command)
```

## PubSub
### Permissions
 It was giving me an error message about not having permissions to subscribe to pubsub channels. I guess recent versions of valkey/redis default to users having access to no channels by default.
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
