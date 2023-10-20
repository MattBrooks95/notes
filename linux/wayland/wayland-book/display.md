# The Wayland Display
* implicitly exists on every connection
* interface defined by XML
* specifies a request `get_registry`, the registry is used to allocate other objects
* `libwayland` will handle the details of the display interface for you

## Client - get a display connection
* `wl_display_connect` is the most common way for clients to get a connection
    * the parameter to the function can be the name of a wayland display, but if you pass in NULL, libwayland will try to figure out the display name for you by trying to connect to the wayland sockets in `$XDG_RUNTIME_DIR/$WAYLAND_DISPLAY`

## Server - creating the display
* creating the display and associating it with a socket are separate steps, so you can setup the display as you need before clients get a chance to connect to it
