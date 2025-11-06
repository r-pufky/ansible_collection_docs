
# Troubleshooting

!!! info ""
    See [molecule](../molecule/testing.md) for specific molecule
    troubleshooting.

## Driver podman does not provide a schema
Podman driver does have a schema and will always generate a warning.

??? danger "Error"
    ``` log
    WARNING Driver podman does not provide a schema.
    ```
Reference:

* https://github.com/ansible/molecule/discussions/4108

## Podman requires 'tmpfs' as dict not list
Molecule documentation refers to using **tmpfs** as a list. Podman **only**
accepts dictionaries.

molecule.yml
``` yaml
platforms:
  - name: 'tmpfs as a dict'
    image: ghcr.io/hifis-net/debian-systemd:13
    tmpfs:
      /tmp: 'rw,size=787448k,mode=1777'

platforms:
  - name: 'tmpfs not needed with systemd always'
    image: 'ghcr.io/hifis-net/debian-systemd:13'
    systemd: 'always'  # tmpfs not need if 'always'.
```
Reference:

* https://docs.ansible.com/ansible/latest/collections/containers/podman/podman_container_module.html#parameter-tmpfs

* https://github.com/ansible/molecule/issues/4140

## Podman database static dir mis-match
Podman will not migrate things out of `/home` to prevent existing pod breakage.

??? danger "Error"
    ``` log
    Error: database static dir ".../graph/libpod" does not match our static dir ".../graph/libpod": database configuration mismatch
    ```

Reset podman and migrate
``` bash
podman system reset
podman system migrate
```
Reference:

* https://github.com/containers/podman/pull/20874

## Cannot use `ping` from container
Some containers services require ping.

Set system-wide:

/etc/sysctl.d/net_ipv4_ping_group_range (1)
{ .annotate }

1. 0755 root:root


``` bash
net.ipv4.ping_group_range=0 $MAX_UID
```

Alternatively set during container execution:

/etc/containers/containers.conf (1)
{ .annotate }

1. 0644 root:root`

``` ini
default_sysctls = [
  "net.ipv4.ping_group_range=0 2147483647",
]
```
Reference:

* https://github.com/containers/podman/blob/main/troubleshooting.md#5-rootless-containers-cannot-ping-hosts
