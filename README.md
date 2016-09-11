# docker-vpnc

[![Docker Repository on Quay.io](https://quay.io/repository/azavea/vpnc/status "Docker Repository on Quay.io")](https://quay.io/repository/azavea/vpnc)
[![Apache V2 License](http://img.shields.io/badge/license-Apache%20V2-blue.svg)](https://github.com/hectcastro/docker-sneaker/blob/develop/LICENSE)

A `Dockerfile` based off of [`phusion/baseimage-docker`](https://github.com/phusion/baseimage-docker) that establishes a VPN connection with [`vpnc`](https://www.unix-ag.uni-kl.de/~massar/vpnc/).

## Environment Variables

- `VPNC_GATEWAY`: IP/name of your IPSec gateway
- `VPNC_ID`: Group name
- `VPNC_SECRET`: Group password
- `VPNC_USERNAME`: XAUTH username
- `VPNC_PASSWORD`: XAUTH password

## Usage

First, ensure that all of the environment variables above exist in a file:

```bash
$ cat > .env <<EOF
VPNC_GATEWAY=1.2.3.4
VPNC_ID=joker-group
VPNC_SECRET=joker-secret
VPNC_USERNAME=joker
VPNC_PASSWORD=joker-password
EOF
```

**Note**: You can also use the `-e` option to `docker run`.

Next, build the container:

```bash
$ docker build -t azavea/vpnc .
```

Lastly, run the container, and then ask [ipify](https://www.ipify.org/) what your external IP address is. It should return the IP address of your VPN endpoint.

```bash
$ docker run --rm -ti --privileged --env-file .env --dns 8.8.8.8 \
    azavea/vpnc /sbin/my_init --quiet -- \
    /bin/sh -c "sleep 5 && curl 'https://api.ipify.org?format=json'"
VPNC started in foreground...
{"ip":"216.158.51.82"}
$ curl 'https://api.ipify.org?format=json'
{"ip":"52.2.53.130"}
```

**Option Explanations**

- `--rm`: Removes the container after it's done executing
- `--privileged`: Allows the container to create and make use of the `tun` device
- `--env-file`: Loads up the contents of `.env` into the container's environment
- `--dns`: Make use of Google's DNS servers for name resolution within the container
- `/sbin/my_init`: The init system provided by `phusion/baseimage-docker`

Everything after `--` is the command we want to run within the container, in addition to the services managed by `my_init.`

**Note**: If you get an error like the one below, it is a [known bug](https://bugs.launchpad.net/ubuntu/+source/vpnc/+bug/228365) with `vpnc`:

```
select: Interrupted system call
terminated by signal: 15
```
