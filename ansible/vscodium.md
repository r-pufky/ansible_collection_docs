# Setup VSCodium (VScode) for Ansible
Install [Ansible extension](https://marketplace.visualstudio.com/items?itemName=redhat.ansible)

#### ansible ➔ settings ➔ user ➔ workspace

 Option                                   | Setting
------------------------------------------|----------------------------------------------------
 ansible path                             | /var/venv/{REPO}/bin/ansible
 always use FQCN                          | ☑
 reuse terminal                           | ☑
 python interpreter path                  | /var/venv/{REPO}/bin/python
 python activatioon script                | /var/git/{REPO}/ansible.env
 ansible navigator path                   | ansible-navigator
 completion provide redirect modules      | ☑
 completion provide module option aliases | ☑
 validation                               | ☑
 validation lint                          | ☑
 validation lint path                     | ansible-lint
 container engine                         | auto
 image                                    | ghcr.io/ansible/community-ansible-dev-tools:latest
 policy                                   | missing
 redhat telemetry                         | ☐
 redhat lightspeed                        | ☐

Set for the user or the workspace.

## Set alternative container storage location (optional)
Developing ansible will thrash disk especially running ansible-lint after every
file save. Relocate high-use directories to a disk that can handle
high wear. Prefer to config change as this enables vanilla use.

``` bash
# delete or move existing cache data.
ln -s /mnt/cache/config/VSCodium ${HOME}/.config/VSCodium
```