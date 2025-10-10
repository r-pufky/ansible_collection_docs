# Molecule Troubleshooting
See [Podman](../podman/troubleshooting.md) and [Vagrant](../vagrant/troubleshooting.md) for framework specific troubleshooting.

### YAML files reverted on test execution
File linking or caching issue with IDE or Molecule.

Suspected causes:
* SSHFS connection drops.
* Rebooting while molecule testing instances are running.
* IDE environment using stale file handle.

#### Clear all caches and re-open affected file.
1. **CLOSE** the file with the issue in the editor and reopen/save. Re-run
   tests.

2. Remove all molecule test state from all tested roles. Re-run tests.

   ``` bash
   for x in $(ls -1); do pushd ${x} && molecule destroy --all && popd; done
   ```

   Clean out all ansible caches
   ``` bash
   molecule destroy --all
   molecule reset
   rm -rfv ~/.cache/molecule/*
   rm -rfv /tmp/ansible-tmp*
   rm -rfv /tmp/ansible-cache/*
   rm -rfv ~/.ansible/*
   rm -rfv ~/.ansible_async/*
   ```

### ERROR! Could not find or access
Molecule uses galaxy roles as dependencies when testing.

#### Error

``` bash
ERROR! Could not find or access '.../ansible_collection_srv/roles/{ROLE}/molecule/{TEST}/{TASK_FROM_FILE}.yml on the Ansible Controller.

fatal: [{INSTANCE}]: FAILED! => {
    "changed": false,
    "include": "{TASK_FROM_FILE}.yml",
    "reason": "Could not find or access '/var/git/ansible_collection_srv/roles/debian/molecule/network/global.yml' on the Ansible Controller."
}
```

#### Use FQCN
``` yaml
- name: 'include full role'
  ansible.builtin.include_role:
    name: 'r_pufky.deb.os'
```

#### Use tasks_from when no other tasks are sourced within
``` yaml
- name: 'include task from role if there are no additional include_roles'
  ansible.builtin.include_role:
    name: 'r_pufky.deb.os'
    tasks_from: 'optimizations/firmware.yml'
```

[Reference](https://github.com/ansible/molecule/issues/3857)

### ERROR! the role '{ROLE}' was not found ...
Molecule uses galaxy roles as dependencies when testing.

#### Error
``` bash
ERROR! the role 'r_pufky.{COLLECTION}.{ROLE}' was not found in ...

The offending line appears to be:

      ansible.builtin.include_role:
        name: 'r_pufky.deb.apt'
              ^ here
```

#### Force install updated collection
``` bash
ansible-galaxy collection build -f
ansible-galaxy collection install {COLLECTION}-X.X.X.tar.gz -f
```

### CRITICAL Idempotence test failed because of the following tasks
Not all operations are idempotent.

#### Explicitly disable when idempotence cannot be guaranteed
molecule.yml
``` yaml
scenario:
  test_sequence:
    # - 'idempotence'  # Reason for disable.
```

### Gathering Facts failed
Leftover state from previous or interrupted test.

#### Error
``` bash
TASK [Gathering Facts] *********************************************************
fatal: [{HOST}]: UNREACHABLE! => {"changed": false, "msg": "Failed to create temporary directory. In some cases, you may have been able to authenticate and did not have permissions on the target
```

#### Destroy and re-create
``` bash
molecule destroy --all
```
* `molecule reset` is a nuclear option.
