# Isolated Docker as User

Rootless Docker setup. No system daemon, no root, containers run under your
unprivileged user via subuid/subgid mapping.

## Setup

Needs: `shadow` (Arch/Artix) `shadow-uidmap` (Alpine) `iptables`

> If a rootful (system) Docker daemon is running, disable it first so it doesn't
clash with the rootless one. You can also delete any Docker related packages alltogether.

```
sudo ./00reqs <user>   # root: check uidmap, configure subuid/subgid, ip_tables
./01inst               # user: official get.docker.com/rootless installer
./02start              # user: start the per-user daemon
./02stop               # user: stop it
```

## Running containers

`docker run --rm hello-world`

https://docs.docker.com/engine/security/rootless
