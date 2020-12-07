# Tunnelto Proxy

## Motivation

When [`tunnelto`](https://github.com/agrinman/tunnelto) starts up, it listens to two ports

1. Control port, used to control the tunnel
2. Tunnel port, used to forward incoming traffic through the tunnel

However, as `tunnelto` may be running on a host which only allows one port exposed (e.g. Heroku), it is useful to use a proxy to multiplex incoming traffic on that one port and route it between the control and tunnel ports.

Since tunnel control traffic is expected at the root domain, while tunnel traffic is expected at subdomains, host-based routing using HAProxy is used for the implementation here.

## Usage

### 1. Build `tunnelto_server`

1. From the root of this repository, run `git clone https://github.com/agrinman/tunnelto`.

2. `cd tunnelto; sh musl_build.sh`

### 2. Build and run Docker container

From the root of this repository, run `cd docker; docker-compose up`

### 3. Enjoy!

Try out these local curl commands and observe the logs from `docker-compose` to verify routing:

- `curl -v http://localhost:8080` (root domain; routes to `tunnelto` control port)
- `curl -v -H 'Host: localhost2' http://localhost:8080` (alternative root domain; routes to `tunnelto` control port)
- `curl -v -H 'Host: sub.localhost' http://localhost:8080` (not a root domain; routes to `tunnelto` tunnel port)
