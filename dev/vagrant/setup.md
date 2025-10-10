# Setup Vagrant Environment
Prerequisite:
* [ansible environment](../environment/ansible.md)
* [virtualbox](../virtualbox/virtualbox.md)


## Install Packages

``` bash
source {VENV}/bin/activate
pamac install vagrant
pip install molecule-plugins[vagrant]
```
[Reference](https://wiki.archlinux.org/title/Vagrant)

### Set alternative storage location (optional)
Developing on vagrant will thrash disk especially when running molecule with
large (multi-GB) reads and writes. Relocate high-use directories to a disk that
can handle high wear. Prefer to config change as this enables quick use without
configuration changes.

Move and link user directories to an alternative location:
``` bash
# delete or move existing cache data.
ls -s /mnt/cache/local/share/containers ${HOME}/.local/share/containers
```
* Consider moving and linking entire `.cache` directory.
