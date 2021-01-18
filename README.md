# cAdvisor ARM build

cAdvisor docker image build for ARM devices (for example: Raspberry PI).

This package is based on official [google/cadvisor](https://github.com/google/cadvisor)

[![](https://github.com/dcorto/cadvisor-arm/workflows/Docker%20Build%20&%20Publish/badge.svg)](https://github.com/dcorto/cadvisor-arm/actions)

[![](https://img.shields.io/docker/image-size/thedavis/cadvisor-arm/0.37.0)](#)
[![](https://img.shields.io/docker/pulls/thedavis/cadvisor-arm)](#)

## Content

* [How it works](#how-it-works)
* [How to use](#how-to-use)
  * [Docker](#with-docker)
  * [Custom build](#custom-build)

## How it works

This package compile official [google/cadvisor](https://github.com/google/cadvisor) package on Raspberry PI with `arm32v7/golang` docker image and build [google/cadvisor](https://github.com/google/cadvisor) as `arm32v7/debian` image.

## How to use

### With Docker

#### Supported tags and respective `Dockerfile` links

**NOTE:** Tag corresponds to the version of cAdvisor

* `0.37.0`, `0.37`, `latest` - [(Dockerfile)](https://github.com/dcorto/cadvisor-arm/blob/v0.37.0/Dockerfile)
* `0.36.0`, `0.36` - [(Dockerfile)](https://github.com/dcorto/cadvisor-arm/blob/v0.36.0/Dockerfile)
* `0.35.0`, `0.35` - [(Dockerfile)](https://github.com/dcorto/cadvisor-arm/blob/v0.35.0/Dockerfile)

The best (and recommended) way how to use this package is as [Docker image](https://hub.docker.com/repository/docker/thedavis/cadvisor-arm).

```shellscript
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  thedavis/cadvisor-arm:latest
```

I trying update build of this package as soon as possible for each [google/cadvisor](https://github.com/google/cadvisor) update, but when you need more actual version I recommend you use custom build.

### Docker Compose Example

```yml
version: '3'

services:
  cadvisor:
    image: thedavis/cadvisor-arm
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

```shellscript
git clone git@github.com:dcorto/cadvisor-arm.git

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
