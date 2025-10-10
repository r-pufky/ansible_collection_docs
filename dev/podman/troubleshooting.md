
# Troubleshooting
See [molecule](../molecule/testing.md) for specific molecule troubleshooting.

### Podman required 'tmpfs' as dict, not list
Molecule documentation refers to using `tmpfs` as a list. Podman **only**
accepts dictionaries.

molecule.yml
``` yaml
platforms:
  - name: tmpfs as a dict
    image: ghcr.io/hifis-net/debian-systemd:12
    tmpfs:
      /tmp: 'rw,size=787448k,mode=1777'
```

Additionally `tmpfs` is **not** needed if `systemd` is set to `always`.
``` yaml
platforms:
  - name: 'tmpfs not needed with systemd always'
    image: 'ghcr.io/hifis-net/debian-systemd:12'
    systemd: 'always'
```

[Reference](https://docs.ansible.com/ansible/latest/collections/containers/podman/podman_container_module.html#parameter-tmpfs)

[Reference](https://github.com/ansible/molecule/issues/4140)

## Podman database static dir mis-match
Podman will not migrate things out of `/home` to prevent existing pod breakage.

Reset podman and migrate
```bash
podman system reset
podman system migrate
```

```bash
Error: database static dir ".../graph/libpod" does not match our static dir ".../graph/libpod": database configuration mismatch
```

[Reference](https://github.com/containers/podman/pull/20874)

### Cannot use `ping` from container
Some containers services require ping.

Set system-wide:

`0755 root:root` /etc/sysctl.d/net_ipv4_ping_group_range
``` bash
net.ipv4.ping_group_range=0 $MAX_UID
```

Alternatively set during container execution:

`0644 root:root` /etc/containers/containers.conf
``` ini
default_sysctls = [
  "net.ipv4.ping_group_range=0 2147483647",
]
```

[Reference](https://github.com/containers/podman/blob/main/troubleshooting.md#5-rootless-containers-cannot-ping-hosts)
