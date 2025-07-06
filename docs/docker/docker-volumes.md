# Docker Volumes

Docker volumes are persistent data stores for containers, providing a way to store and manage data independently of a container's lifecycle. Unlike writing data directly to a container's writable layer, volumes do not increase the container's size, making them a preferred choice for data persistence. Volumes can be mounted into multiple containers simultaneously, enabling data sharing.

## Key Behaviors

* Named vs. Anonymous Volumes: Volumes can be named (user-defined) or anonymous (Docker assigns a random unique name). Named volumes are explicitly created and reusable, while anonymous volumes are not automatically shared or reused between containers.
* Persistence: When a container is destroyed, its writable layer is lost, but data in both named and anonymous volumes persists unless the container is created with the --rm flag, which removes anonymous volumes upon container removal.
* Non-Empty Volume Mounts: If you mount a non-empty volume into a container's directory, existing files or directories in that container path are not deleted but become inaccessible, hidden by the volume's contents.
* Empty Volume Mounts: If you mount an empty volume into a container directory containing files or directories, those files are copied into the volume by default. If the specified volume does not exist, Docker creates an empty volume automatically.

## Docker Volume Commands

Here are common commands for managing Docker volumes:

### Create a Volume

```sh
docker volume create myvol
```

### Remove a Volume

```sh
docker volume rm myvol
```

### List Volumes

```sh
docker volume ls
```

### Inspect a Volume

```sh
docker volume inspect myvol
```

### Mount a Volume

```sh
docker run -it --mount type=volume,src=myvol,dst=/mount/path ubuntu
```

Or,

```sh
docker run -it -v myvol:/mount/path ubuntu
```

For an anonymous volume, omit the `src` field (with `--mount`) or the volume name (with `-v`):

```sh
docker run -it --mount type=volume,dst=/mount/path ubuntu
```

Or,

```sh
docker run -it -v /mount/path ubuntu
```

To create a container with an anonymous volume that is removed when the container is deleted:

```sh
docker run -it --rm --mount type=volume,dst=/mount/path ubuntu
```
