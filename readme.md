# Isolated Docker as User

Rootless Docker setup. No system daemon, no root, containers run under your
unprivileged user via subuid/subgid mapping.

## Setup

Needs: `shadow` (Arch/Artix) `shadow-uidmap` (Alpine) `iptables`

> If a rootful (system) Docker daemon is running, disable it first so it doesn't
clash with the rootless one. You can also delete any Docker related packages alltogether.

```
sudo ./00reqs <user>   # root: uidmap, subuid/subgid, load nf_tables (fallback ip_tables) + tun
./01inst               # user: get.docker.com/rootless installer (handles logind + nft-only kernels)
./02start              # user: start the per-user daemon
./02stop               # user: stop it
```

## Running containers

Smoke test:

`docker run --rm hello-world`

Hardened defaults (drop all caps, no new privileges, read-only rootfs, tmpfs
/tmp, no network):

```
docker run --rm --security-opt no-new-privileges --cap-drop=ALL --read-only \
  --tmpfs /tmp:rw,noexec,nosuid,size=64m --network none <image> <cmd>
```

Add network only when a task needs it:

```
--network bridge --cap-add NET_RAW --cap-add NET_BIND_SERVICE
```

https://docs.docker.com/engine/security/rootless
