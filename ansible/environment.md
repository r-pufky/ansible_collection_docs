# Setup Ansible Environment

## Create Environment and Install Packages

#### Create python virtual environment, install Ansible and Molecule
``` bash
python3 -m venv /var/venv/{REPO}
source /var/venv/{REPO}/bin/activate
pip install --upgrade pip
pip install --upgrade setuptools
pip install ansible
pip install argcomplete
# Install older ansible-compat layer until issue is resolved.
# Reference:
# * https://github.com/ansible/ansible-lint/issues/4533
# pip install ansible-lint
# pip install molecule
pip uninstall -y ansible-compat ansible-lint molecule
pip install "ansible-compat==24.10.0" ansible-lint molecule
```
* `{REPO}` refers to bare repository name, e.g. **ansible_collection_srv**.

[Reference](https://docs.ansible.com/ansible/2.9/installation_guide/intro_installation.html)

## Set alternative `.ansible` role cache location
A recent change in Ansible now creates an `.ansible` directory in each
directory ansible is run; building and copying all collections/roles
recursively into it potentially creating multi-GB PER directory. Leads to
extremely slow and noisy `yamllint` and `ansible-lint` usage.

#### /etc/fstab
``` bash
tmpfs  /tmp/ansible-cache  tmpfs  noatime,mode=1777  0 0
```

#### Mount new directory
``` bash
mount -o remount -a
```

#### Symlink all directories to cache
``` bash
find . -type d -name '.ansible' -exec rm -rf "{}" && ln -s /tmp/ansible-cache "{}" \;
```

[Reference](https://github.com/ansible/ansible-lint/issues/4533)

## Set alternative container storage location (optional)
Developing ansible will thrash disk especially when running playbooks and
testing with molecule. Relocate high-use directories to a disk that can handle
high wear. Prefer to config change as this enables vanilla use in production.

``` bash
# Delete or move existing cache data.
ln -s /mnt/cache/.ansible_async ${HOME}/.ansible_async
ln -s /mnt/cache/.ansible ${HOME}/.ansible
ln -s /mnt/cache/cache/ansible-compat ${HOME}/.cache/ansible-compat
ln -s /mnt/cache/cache/molecule ${HOME}/.cache/molecule
```
* Consider moving and linking entire `.cache` directory.
