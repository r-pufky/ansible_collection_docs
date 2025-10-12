# Collection Testing
Prerequisite:
* [Ansible Environment](../README.md)

Use community references for examples on sepcific implementations:
[Filter Plugins](https://github.com/ansible-collections/community.general/blob/main/tests/unit/plugins/filter/test_json_patch.py)
[Integration Tests](https://github.com/ansible-collections/community.general/blob/main/tests/integration/targets/filter_dict/tasks/main.yml)

## Build and Install Local Collection
The collection must be built and installed locally to be tested.

Package built using semantic version from `galaxy.yml` and installed to local
collections cache
`~/.ansible/collections/ansible_collections/{USER}/{COLLECTION}`.

``` bash
ansible-galaxy collection build -f
ansible-galaxy collection install -f
```
* `--force`: force overwrite existing files (useful for repeated testing).

Collection **will** need to be rebuilt if changes (files or tests) were made.

## Running Tests
**Any change** requires rebuilding, installing **AND** re-entering directory as
inodes change.

Alway [build and install](#build-and-install-local-collection) before running
tests.

``` bash
# Always re-enter between installs - cd ~; cd -
cd ~/.ansible/collections/ansible_collections/{USER}/{COLLECTION}
ansible-test {unit,integration,sanity}
```
[Reference](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections_testing.html)

[Reference](https://docs.ansible.com/ansible/latest/dev_guide/testing_running_locally.html#testing-running-locally)

[Reference](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections_testing.html#testing-collections)

[Reference](https://medium.com/@andrewjamesdawes/run-ansible-test-integration-tests-locally-on-docker-and-over-ssh-d8d658ba118b)


## Troubleshooting

### Testing in roles uses old files
Collection cache is outdated. Clear the static collection cache to force
rebuild with update versions, or link to development repository.

Clear build cache:
``` bash
rm -rfv ~/.ansible/collections/ansible_collections/r_pufky/srv
```


### WARNING: Unable to determine context for the following test targets
A controller and a target were not specified when running the test.

#### Error
``` bash
WARNING: Unable to determine context for the following test targets, they will
be run on the target host: {MODULE}, {MODULE}, they will be run on the target
host: {MODULE}, {MODULE}
```

This is OK for tests that are run on localhost with NO impact on system (e.g.
filters).


### Only Localhost is Available
Host inventory not detected when running collection tests.

#### Error
``` bash
ansible-playbook roles/hello_motd/tests/playbook/test_hello_motd.yml
...
  [WARNING]: No inventory was parsed, only implicit localhost is available
  [WARNING]: provided hosts list is empty, only localhost is available. Note
  that the implicit localhost does not match 'all'
```

#### Explicitly specify inventory to use

``` bash
ansible-playbook -i tests/inventory tests/playbook/test_hello_motd.yml
```
`--ask-become-pass` will be needed if sudo used or use GPG key injection.


### Fixing GPLv3 License Missing
Community distributions assumes GPLv3.

#### Explicitly ignore if different license.
``` bash
mkdir -p tests/sanity
touch tests/sanity/ignore-{ANSIBLE VERSION}.txt

touch tests/sanity/ignore-2.16.txt
```
An ignore file is required for each specific **version** of ansible.

**Order Matters**: if ignores are throwing warnings, check ordering and make
sure they are ordered correctly. Proper ignores will **not** generate warnings.

Only certain ignores are explicitly allowed; others will always throw
ansible-lint errors even if ignored.
```
plugins/modules/demo_hello.py compile-2.7!skip
plugins/modules/demo_hello.py import-2.7!skip
plugins/modules/demo_hello.py compile-3.6!skip
plugins/modules/demo_hello.py import-3.6!skip
plugins/modules/demo_hello.py compile-3.7!skip
plugins/modules/demo_hello.py import-3.7!skip
plugins/modules/demo_hello.py compile-3.8!skip
plugins/modules/demo_hello.py import-3.8!skip
plugins/modules/demo_hello.py compile-3.9!skip
plugins/modules/demo_hello.py import-3.9!skip
plugins/modules/demo_hello.py compile-3.10!skip
plugins/modules/demo_hello.py import-3.10!skip
plugins/modules/demo_hello.py compile-3.12!skip
plugins/modules/demo_hello.py import-3.12!skip
plugins/modules/demo_hello.py validate-modules:missing-gplv3-license
```

[GPLv3 License Expectation](https://github.com/ansible/ansible/issues/67032)

[Sanity Ignore](https://docs.ansible.com/ansible/latest/dev_guide/testing/sanity/ignores.html)

[Sanity Validate](https://docs.ansible.com/ansible/latest/dev_guide/testing/sanity/validate-modules.html)

[Sanity Lint Rules](https://ansible.readthedocs.io/projects/lint/rules/sanity/)
