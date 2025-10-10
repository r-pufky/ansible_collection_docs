# Molecule: Vagrant with VirtualBox VMs
Prerequisite:
* [ansible environment](../environment/ansible.md)
* [virtualbox](../virtualbox/virtualbox.md)
* [vagrant](./env.md)

Vagrant VMs are **ONLY** used to test cases which cannot be tested in
containers (base OS, kernel, firmware, advanced networking, etc) these should
**never** be `default` test cases.

Created in current working directory.

## System Images Used
Debian has stopped creating images due to licensing issues.

* inception-of-things/trixie
* boxen/debian-13 (alt - currently SSH key build issues)

Alternatively a non-maintained Debian image may be created:
https://raju.dev/building-debian-13-trixie-vagrant-image/

[Reference](https://portal.cloud.hashicorp.com/vagrant/discover)

## Molecule Setup
Standard molecule setup for vagrant with virtualbox VM.

`0644 {USER}:{USER}` molecule.yml
``` yaml
---
dependency:
  name: 'galaxy'
driver:
  name: 'vagrant'
  provider:
    name: 'virtualbox'
    enable_efi: true
    config_options:
      ssh.keep_alive: true
      ssh.remote_user: 'root'  # default login user, different per VM image
    #provider_raw_config_args:
    #  - "customize [ 'modifyvm', :id, '--firmware', 'efi' ]"
    # enable_efi: true
    # both options do not seem to work? maybe an image thing.
    options:
      append_platform_to_hostname: false
provisioner:
  name: 'ansible'
  config_options:
    defaults:
      interpreter_python: 'auto_silent'  # suppress warnings
      callback_whitelist: 'profile_tasks, timer, yaml'  # output profiling info
      # To cache facts between molecule steps:
      #   fact_caching: 'jsonfile'
      #   fact_caching_connection: '/tmp/facts_cache'
      #   fact_caching_timeout: 7200
      #
      # cache a fact between steps:
      #   ansible.builtin.set_fact:
      #     cacheable: yes
      #     my_fact: "howdy, world"
      #
      # always assert cache is not expired before using and document with args:
      #   - name: 'assert fact_caching not expired'
      #     ansible.builtin.assert:
      #       that:
      #         - '_test_cached_fact is defined'
      #       fail_msg: 'fact_caching has expired; re-run prepare.'
  # inventory:  # Set all base testing configuration here.
  #   group_vars:
  #     all:
  #       setup_variables: true
  #   host_vars:
  #     {IMAGE}-{TEST}:
  #       setup_variables: true
platforms:
  - name: '{ROLE}-{IMAGE}-vm-{TEST}'  # debian-13-vm-test
    box: 'inception-of-things/trixie'
    memory: 4096
    cpus: 2
    interfaces:
      - network_name: private_network  # network_name required
        auto_config: true
        type: 'dhcp'
        # type: static
        # ip: 192.168.56.10  # default is 192.168.56.0/21
    instance_raw_config_args:
      - 'vm.network "forwarded_port", guest: 8443, host: 8443'
      - 'vm.network "forwarded_port", guest: 8080, host: 8080'
      - 'vm.network "forwarded_port", guest: 8880, host: 8880'
      - 'vm.network "forwarded_port", guest: 8443, host: 8843'
verifier:
  name: 'ansible'
lint: |
  set -e
  yamllint .
  ansible-lint .
# Disable testing steps as needed with explicit reasons to minimize warnings.
scenario:
  test_sequence:
    - 'dependency'
    - 'cleanup'
    - 'destroy'
    - 'syntax'
    - 'create'
    - 'prepare'
    - 'converge'
    - 'idempotence'
    - 'side_effect'
    - 'verify'
    - 'cleanup'
    - 'destroy'
```
[Reference](https://ansible.readthedocs.io/projects/molecule/configuration/?h=test_sequence#scenario)

[Reference](https://floatingpoint.sorint.it/blog/post/setting-up-molecule-for-testing-ansible-roles-with-vagrant-and-testinfra)

# Running Tests
Always **become** the `ssh.remote_user` when creating Molecule tests or setup
an ansible user after VM turnup to apply ansible tasks.

``` yaml
- name: 'Molecule testing step'
  hosts: 'all'
  gather_facts: false
  become: true
...
```

[Reference](#failed-to-lock-apt-for-exclusive-operation)

Vagrant driver does have a schema and will always generate a warning.
``` bash
WARNING Driver vagrant does not provide a schema.
```
[Reference](https://github.com/ansible/molecule/discussions/4108)
