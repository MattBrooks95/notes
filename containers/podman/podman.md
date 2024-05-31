I'm not knowledgeable at containers at all so take what I say here with a grain of salt.

## Images
`$ podman image create . --tag <container_name>` to create a container that can be referenced by `<container_name>` called the 'tag'

In the podman manual, it says "containerfile" which I guess means the same thing as "Dockerfile", and is trying to say that containerization is more than just Docker

`$ podman image list` to list the images that you have built already.

## Containers
Containers are created from [images](#Images)

The image is like the template for a way that a software service could be created, but cannot be used. A container is taking an image and applying to it configurations and sharing data into it that turns the data into something that can actually be ran and used.

## Gotchas
`$ podman run ...` will try to create and run a new container from an image, this is not how you start an image that has already been built

use `$ podman container inspect <container-name>` to see detailed information about the container, like what command it is set up to run when started
