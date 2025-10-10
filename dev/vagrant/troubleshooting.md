# Vagrant Troubleshooting

### A Vagrant environment or target machine is required to run this command.
Virtualbox kernel modules are likely not loaded.

``` bash
A Vagrant environment or target machine is required to run this
command. Run `vagrant init` to create a new Vagrant environment. Or,
get an ID of a target machine from `vagrant global-status` to run
this command on. A final option is to change to a directory with a
Vagrantfile and to try again.
```

``` bash
pamac install linux$(mhwd-kernel -l)-virtualbox-host-modules  # for current kernel
reboot
```

After reboot confirm that each command works:
``` bash
vboxmanage --version  # should be executable, with modules loaded
> 7.1.4r165100
vagrant global-status
vagrant up --provider virtualbox  # will detail why it is not running.
```
[Reference](../environment/virtualbox.md)

### Molecule test fails during create
Vagrant VM name/path length limitation. Molecule creates a VM name combining
test, name, box, OS, network, and a random hash; which leads to excessively
long VM names (see underlying virtualization manager).

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

Failed to start the VM(s): See log file '/mnt/fast/cache/mono/cache/molecule/network/interfaces_resolv/vagrant.err'
```

Use shorter component names:
* VM instance name in `molecule.yml`.
* Rename molecule test to shorter name.
* Extreme cases may require cloning repo with a shorter root repository name.

## Failed to lock apt for exclusive operation.
Debian virutalbox VM uses root to configure and test the instance. All Molecule
setup/teardown steps require root user.

``` yaml
- name: 'Molecule testing step'
  hosts: 'all'
  gather_facts: false
  become: true
...
```
[Reference](https://stackoverflow.com/questions/33563425/ansible-1-9-4-failed-to-lock-apt-for-exclusive-operation)

## Failed to start the VM(s).
Virtualbox may fail to bring up the VM properly due to system constraints.
Re-run the test.

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

Failed to start the VM(s): See log file '.../molecule/debian/firmware/vagrant.err'
```

``` bash
molecule test --scenario-name={TEST}
```

## VM seems flaky
Vagrant uses base VM images; a previous molecule test was likely not cleaned up
properly.

Clean all VMs and re-run:
``` bash
vagrant global-status
vagrant destroy {ID}
vagrant box list
vagrant box destroy {ID}
```
* Additionally check GUI if needed

### Molecule fails with couldn't resolve module/action 'vagrant'.
Molecule `25.2.0` is a bad release that broke vagrant modules. Temporary
workaround while fixes are applied:

``` bash
pip install --force-reinstall -v 'molecule==25.1.0'
```

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
> The error appears to be in '/home/garar/.local/share/pipx/venvs/molecule-plugins/lib/python3.12/site-packages/molecule_plugins/vagrant/playbooks/create.yml': line 8, column 7, but may
> be elsewhere in the file depending on the exact syntax problem.
>
> The offending line appears to be:
>
>   tasks:
>     - name: Create molecule instance(s) # noqa fqcn[action]
>       ^ here
```

https://github.com/ansible-community/molecule-plugins/issues/301
