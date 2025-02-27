# cAdvisor ARM build

cAdvisor docker image build for ARM devices (for example: Raspberry PI).

This package is based on official [google/cadvisor](https://github.com/google/cadvisor)

## Content

* [How it works](#how-it-works)
* [How to use](#how-to-use)
  * [Docker](#docker)
  * [Custom build](#custom-build)

## How it works

This package compile official [google/cadvisor](https://github.com/google/cadvisor) package on Raspberry PI with `arm32v7/golang` docker image and build [google/cadvisor](https://github.com/google/cadvisor) as `arm32v6/alpine` image.

## How to use

### With Docker

#### Supported tags and respective `Dockerfile` links

**NOTE:** Tag corresponds to the version of cAdvisor

* `0.44.1`, `latest` - [(Dockerfile)](https://github.com/GuyKh/cadvisor-arm/blob/v0.44.1/Dockerfile)
* `0.44.0` - [(Dockerfile)](https://github.com/GuyKh/cadvisor-arm/blob/v0.44.0/Dockerfile)
* `0.30.2` - [(Dockerfile)](https://github.com/GuyKh/cadvisor-arm/blob/v0.30.2/Dockerfile)
* `0.29.0` - [(Dockerfile)](https://github.com/GuyKh/cadvisor-arm/blob/v0.29.0/Dockerfile)
* `0.28.3` - [(Dockerfile)](https://github.com/GuyKh/cadvisor-arm/blob/v0.28.3/Dockerfile)


The best (and recommended) way how to use this package is as [Docker image](https://hub.docker.com/r/guykhmel/cadvisor-arm/).

```shell
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  guykhmel/cadvisor-arm:latest
```

I trying update build of this package as soon as possible for each [google/cadvisor](https://github.com/google/cadvisor) update, but when you need more actual version I recommend you use custom build.

### Docker Compose Example

```yml
version: '3'

services:
  cadvisor:
    image: guykhmel/cadvisor-arm
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:rw
      - /sys:/sys
      - /var/lib/docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro
    ports:
      - 8080:8080
```

## Custom build

Or you can use custom build on your ARM (Raspberry PI) device.

```shell
git clone git@github.com:GuyKh/cadvisor-arm.git
cd cadvisor-arm
docker build -t <image name> .
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  <image name>:<image tag>
```

**IMPORTANT NOTE: Build must be only on ARM device. On x86/x64 CPU not work!**
