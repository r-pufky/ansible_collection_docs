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

#### Update Ansible Environment
A minimal (clean) environment is provided to test collection and roles against
a vanilla ansible installation; enforcing only settings which make testing
easier.

ansible.env
``` bash
###############################################################################
# Ansible Collection Test Environment Configuration
###############################################################################
# Change environment variable names based on collection name to prevent
# collisions. See: docs/environment/README.md.

# Ansible serving/caching directory for controller node (high read/writes).
export SRV_DEPLOY="${HOME}/.ansible"  # ${ANSIBLE_HOME}

# Ansible python virtual environment (high read/writes)
export SRV_VENV='/var/venv/{REPO}'

# SRV git repository root directory (high read only)
export SRV_GIT='/var/git/{REPO}'

###############################################################################
# Ansible Serve Config (2.18.1)
###############################################################################
# All configuration is done using environment variable per best practice. Check
# both core and community options when setting or changing values.
#
# Generated with:
#
#   ansible-config init -t all -f env > /tmp/ansible-env.cfg
#
# Environment options are:
# * Explicitly set
# * Not internal-only settings
# * Deprecated settings actively removed or noted with a TODO if in use
# * Use sh interpretation (0: False, 1: True; etc)
#
# Settings can be validated with (errors will be listed):
#
#   source ansible.env
#   source /run/venv/{REPO}/bin/activate
#   ansible-config --help
#   ansible-config dump -t all --only-changed

###############################################################################
# Ansible (Core) Common Options
###############################################################################
# Environment variables for core ansible release.
#
# Do not use 'ansible' user (as containers do not have this setup); also do not
# use custom SSH options as these are not used for podman.
#
# Reference:
# * https://docs.ansible.com/ansible/latest/reference_appendices/config.html

export ANSIBLE_CONFIG=''  # Do not use config files.
export ANSIBLE_DISPLAY_SKIPPED_HOSTS=0  # Do not display skipped hosts.
export ANSIBLE_DUPLICATE_YAML_DICT_KEY='error'  # Explicit duplicate errors.
export ANSIBLE_STDOUT_CALLBACK='debug'  # ansible-doc -t callback -l.
export ANSIBLE_USE_PERSISTENT_CONNECTIONS=1  # Persist SSH connections.

###############################################################################
# Ansible (Collection) Environment Options
###############################################################################
# Environment variables for non-core ansible collections.
#
# Reference:
# * https://docs.ansible.com/ansible/latest/collections/environment_variables.html#list-of-collection-env-vars

export ANSIBLE_ADMIN_USERS='root'  # Only enable root by default.
export ANSIBLE_PARAMIKO_RECORD_HOST_KEYS=0  # Ignore host keys.
export ANSIBLE_REMOTE_TMP='/tmp'  # Use RAMFS tmp, not (~/.ansible/tmp).
export ANSIBLE_SYSTEM_TMPDIRS='/tmp'  # Use RAMFS tmp, not (/var/tmp).

###############################################################################
# Activate Ansible Virtual Environment
###############################################################################
source "${SRV_VENV}/bin/activate"
```

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
