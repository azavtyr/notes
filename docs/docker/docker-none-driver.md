# Docker None Driver

None driver completely isolates a container from the host and other containers.

## Create a container connected to none

```sh
docker run -itd --rm --network none alpine
```
