# Docker Image

**Docker Images** are built using a **Dockerfile** with multiple layers, each representing filesystem changes. Layers are stacked on top of each other, forming a complete image.

Layers are cached and can be reused. If layer already exists from a previous build, Docker will not create the new one, but instead will use cached layer, which significantly increases speed of the building process.

## Example of Image Layers

This Dockerfile contains four commands. Commands that modify the filesystem create a new layer.

```Dockerfile
FROM debian:latest

RUN apt update && apt install iputils-ping -y

CMD ["ping", "-c 3", "8.8.8.8"]
```

* `FROM`, and `RUN` instructions, each create a new read-only layer.
- The `CMD` instruction only modifies the image's metadata, which doesn't produce an image layer.

## Build a Docker Image

`docker image build -t my_image .`

## Writable Container Layer

When you create a new container, you add a new writable layer on top of the other layers. All changes made within a running container, such as writing new files, deleting or modifying existing ones, are written to this writable container layer. When the container is deleted, the writable layer is also deleted.

## Union Filesystem

Docker uses union filesystem to create a single unified view of all layers, which are stacked on top of each other. When the container starts, it sees a unified filesystem, which contains all of underlying layers.

When the union filesystem is created, in addition to the image layers, a directory is created specifically for the running container. This allows the container to make filesystem changes while allowing the original image layers to remain untouched. This enables you to run multiple containers from the same underlying image.

## References

* <https://docs.docker.com/get-started/docker-concepts/building-images/understanding-image-layers/>
* <https://useful.codes/understanding-image-layers-and-caching-in-docker/>
* <https://docs.docker.com/engine/storage/drivers/>
