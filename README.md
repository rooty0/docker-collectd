# collectd docker image

## Description

collectd is a daemon which collects system performance statistics periodically
and provides mechanisms to store the values in a variety of ways, for example 
in RRD files

This image allows you to run collectd in a completelly containerized
environment

Source: https://github.com/rooty0/docker-collectd

This is fork of https://github.com/fr3nd/docker-collectd which is seems like dead

Each tag version represents collectd version

Can't find the version or module you need? Make a PR or open a ticket! If you'd like to share your PR please
make sure to run `docker build .` first locally so you know it works

## How to use this image

Run collectd with the default configuration:

```
docker run \
  --privileged \
  -v /proc:/mnt/proc:ro \
  rooty0/collectd
```

Run collectd with a custom configuration stored in /etc/collect

```
docker run \
  --privileged \
  -v /etc/collectd:/etc/collectd:ro \
  -v /proc:/mnt/proc:ro \
  rooty0/collectd
```

Docker compose example:
```
version: '2.1'

services:

  collectd:
    image: rooty0/collectd
    restart: always
    privileged: true
    #  https://docs.docker.com/compose/compose-file/#host-or-none
    #  https://docs.docker.com/compose/compose-file/#network_mode
    network_mode: "host"
    volumes:
      - /proc:/mnt/proc:ro
      - ./volume/etc/collectd/collectd.conf:/etc/collectd/collectd.conf
```

## FAQ

### Do you need to run the container as privileged?

Yes. Collectd needs access to the parent host's /proc filesystem to get
statistics. It's possible to run collectd without passing the parent host's
/proc filesystem without running the container as privileged, but the metrics
would not be acurate.

