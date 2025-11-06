# VSCodium (VScode)
Install [Ansible extension](https://marketplace.visualstudio.com/items?itemName=redhat.ansible)

## Configure Workspace
ansible ➔ settings ➔ user ➔ workspace

 Option                                        | Setting
-----------------------------------------------|----------------------------------------------------
 Ansible: Path                                 | /var/venv/{REPO}/bin/ansible
 Ansible: Use Fully Qualified Collection Names | ✔
 Ansible: Reuse Terminal                       | ✔
 Python: Interpreter Path                      | /var/venv/{REPO}/bin/python
 Python: Activation Script                     | /var/git/{REPO}/ansible.env
 Ansible: Navigator Path                       | ansible-navigator
 Completion: Provide Redirect Modules          | ✔
 Completion: Provide Module Option Aliases     | ✔
 Validation: Enabled                           | ✔
 Lint: Enabled                                 | ✔
 Lint: Path                                    | ansible-lint
 Execution Environment: Container Engine       | auto
 Execution Environment: Image                  | ghcr.io/ansible/community-ansible-dev-tools:latest
 Pull: Policy                                  | missing
 Telemetry: Enabled                            | ✘
 Lightspeed: Enabled                           | ✘

!!! tip ""
    Disable `ansible-lint` for documentation to prevent incorrect YAML error reporting.

    Extensions ➔ Ansible ➔ Settings ➔ Folder
    ``` yaml
    ansible.validation.lint.enabled: false
    ```

## Set alternative container storage location (optional)
Developing ansible will thrash disk especially running ansible-lint after every
file save. Relocate high-use directories to a disk that can handle
high wear. Prefer to config change as this enables vanilla use.

``` bash
# delete or move existing cache data.
ln -s /mnt/cache/config/VSCodium ${HOME}/.config/VSCodium
```