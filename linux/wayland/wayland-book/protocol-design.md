* Wayland strives for `atomicity`
    * goal is "every frame is perfect"
    * this perfection is achieved by updating interfaces transactionally
    * this helps avoid issues like screen tearing
    * changes requested to an interface over the wire are `queued` and are not actually acted upon until a `commit` message is sent. Then, the queued up changes are applied all at once
* the Wayland protocol is asynchronous
    * messages are guaranteed to arrive in the order they were sent, but only with respect to one sender
    * resources have lifetimes that are communicated over the protocol, and clients must be careful to handle messages that were meant to affect a resource that it had been destroyed
    * the book also says something about the client and the server having to agree that a resource was freed, I'm not sure what that means yet

## Versioning
* the book talks about the rules that apply to `stable` and `unstable` protocols
* it says that `stable` and `unstable` both only allow for backwards-compatible changes, but when a protocol goes from unstable to unstable it is allowed one breaking change. The intention is that protocols can have an incubation period where they can iterate and be improved upon before cementing them as a grown-up protocol that plans on being used for a long time

## Primitives
* `wayland-util.h` defines structs, helper functions and macros that create primitives for Wayland applications
    * the book author claims that this file is well-documented in the source code and suggests reading it

## wayland-scanner
* the only binary that comes with the Wayland package
* generates C headers and glue code from Wayland XML files
* the created headers are `wayland-client-protocol.h` and `wayland-server-protocol.h` 
    * however, instead of using these directly, you will most often use `wayland-client.h` and `wayland-server.h` instead

## Resources
* an `object` is something that holds state and is known to the client and the server
    * communications over the wire protocol are used to update the state & keep the client & server in sync with eachother
* on the server, objects are abstracted through `wl_resource`
    * the server must track which resource is owned by which client

## Interfaces and Listeners
* the `wayland-scanner` generates the code for interfaces and listeners, and code to glue them to the lower level protocols
* wayland clients listens for events and sends requests
* the wayland server listens for requests and sends events
