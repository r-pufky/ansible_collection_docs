# Vagrant Troubleshooting

### WARNING Driver vagrant does not provide a schema.
Vagrant driver **always** generates a warning - it does not have a schema.
``` bash
WARNING Driver vagrant does not provide a schema.
```
* Reference discussion lost in ansible consolidation to forum.ansible.com.

[Reference](https://github.com/ansible/molecule/discussions/4108)

### A Vagrant environment or target machine is required to run this command.
Virtualbox kernel modules are likely not loaded.

#### Error
``` bash
A Vagrant environment or target machine is required to run this
command. Run `vagrant init` to create a new Vagrant environment. Or,
get an ID of a target machine from `vagrant global-status` to run
this command on. A final option is to change to a directory with a
Vagrantfile and to try again.
```

#### Install kernel modules

``` bash
mhwd-kernel -l
pamac install linux{KERNEL}-virtualbox-host-modules
systemctl reboot
```

#### Confirm each command works
``` bash
vboxmanage --version  # Executable with modules loaded.
> 7.1.4r165100
vagrant global-status
vagrant up --provider virtualbox  # Provides details why it is not running.
```
[Reference](../virtualbox/virtualbox.md)


### VERR_CFGM_NOT_ENOUGH_SPACE
Molecule test fails during create due to vagrant path length limitation (see
underlying virtualization manager).

#### Error

``` bash
fatal: [localhost]: FAILED! => {
    "changed": false,
    "cmd": [
        "/usr/bin/vagrant",
        "up",
        "--no-provision"
    ],
    "rc": 1
}

STDERR:

### YYYY-MM-DD 20:32:26 ###
### YYYY-MM-DD 20:32:26 ###
There was an error while executing `VBoxManage`, a CLI used by Vagrant
for controlling VirtualBox. The command and stderr is shown below.

Command: ["startvm", "b3415658-8f60-4026-8375-f6ec52014db5", "--type", "headless"]

Stderr: VBoxManage: error: Failed to construct device 'ichac97' instance #0 (VERR_CFGM_NOT_ENOUGH_SPACE)
VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component ConsoleWrap, interface IConsole

MSG:

Failed to start the VM(s): See log file '.../vagrant.err'
```

#### Use shorter component names
* VM instance name in `molecule.yml`.
* Rename molecule test to shorter name.
* Extreme cases may require cloning repo with a shorter root repository name.

my_role/molecule/my_test/molecule.yml
``` yaml
platforms:
  - name: 'test-case-vm'
    box: 'inception-of-things/trixie'
    interfaces:
      - network_name: 'private_network'
```

Molecule creates a VM name combining:
* Molecule test name (`my_test`).
* Molecule image test name (`test-case-vm`).
* Image box name (`inception-of-things-trixie`).
* OS used (`debian`).
* Network (`private_network`).
* Random UUID hash (36 characters).


### Failed to lock apt for exclusive operation.
Debian virutalbox VM uses root to configure and test the instance. All Molecule
setup/teardown steps require root user.

``` yaml
- name: 'Molecule testing step'
  hosts: 'all'
  gather_facts: false
  become: true
```
[Reference](https://stackoverflow.com/questions/33563425/ansible-1-9-4-failed-to-lock-apt-for-exclusive-operation)


## Failed to start the VM(s).
Virtualbox may fail due to system constraints.

#### Error

``` bash
PLAY [Create] ******************************************************************
fatal: [localhost]: FAILED! => {
    "changed": false,
    "cmd": [
        "/usr/bin/vagrant",
        "up",
        "--no-provision"
    ],
    "rc": 1
}

STDERR:

### YYYY-MM-DD 15:10:23 ###
### YYYY-MM-DD 15:10:23 ###
### YYYY-MM-DD 15:10:23 ###
### YYYY-MM-DD 15:32:14 ###
### YYYY-MM-DD 15:32:15 ###
### YYYY-MM-DD 15:32:15 ###
The SSH connection was unexpectedly closed by the remote end. This
usually indicates that SSH within the guest machine was unable to
properly start up. Please boot the VM in GUI mode to check whether
it is booting properly.

MSG:

Failed to start the VM(s): See log file '.../vagrant.err'
```

#### Re-run test

``` bash
molecule test --scenario-name={TEST}
```


## Flaky VM
A previous molecule test likely not cleaned up properly.

#### Clean all VMs and re-run

``` bash
vagrant global-status
vagrant destroy {ID}
vagrant box list
vagrant box destroy {ID}
```
* Check GUI for additional VMs if needed.


## ERROR! couldn't resolve module/action 'vagrant'
Molecule `25.2.0` is a bad release that broke vagrant modules.

#### Error
``` bash
molecule create
> WARNING  Driver vagrant does not provide a schema.
> INFO     default scenario test matrix: dependency, create, prepare, converge
> INFO     Performing prerun with role_name_check=0...
> INFO     Running default > dependency
> WARNING  Skipping, missing the requirements file.
> WARNING  Skipping, missing the requirements file.
> INFO     Running default > create
> ERROR! couldn't resolve module/action 'vagrant'. This often indicates a misspelling, missing collection, or incorrect module path.
>
> The error appears to be in '.../molecule_plugins/vagrant/playbooks/create.yml': line 8, column 7, but may
> be elsewhere in the file depending on the exact syntax problem.
>
> The offending line appears to be:
>
>   tasks:
>     - name: Create molecule instance(s) # noqa fqcn[action]
>       ^ here
```

#### Install Molecule 25.1.0 as a temporary workaround

``` bash
pip install --force-reinstall -v 'molecule==25.1.0'
```

[Reference](https://github.com/ansible-community/molecule-plugins/issues/301)
