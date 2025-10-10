# Molecule Setup
Prerequisite:
* [ansible environment](../ansible/README.md)
* [Podman](../podman/README.md) - Primary test framework.
* [Vagrant](../vagrant/README.md) - Secondary test framework. Explicit reasons
for using this test framework must be clearly documented.

## Create Test
Created in current working directory.

``` bash
molecule init scenario --driver-name=podman  # 'default' Podman driver.
molecule init scenario test --driver-name=vagrant  # 'test' Vagrant driver.
```

Directory layout (as directories or `yml` files):
```
├── dependency
├── lint
├── cleanup
├── destroy
├── syntax
├── create
├── prepare  # Bring host to testable status.
├── converge  # Required - apply role to test.
├── idempotence
├── side_effect
├── verify  # Assert role is correct.
├── cleanup
╰── destroy
```
[Reference](https://ansible.readthedocs.io/projects/molecule/getting-started/#inspecting-the-moleculeyml)

[Reference](https://sbarnea.com/slides/molecule/#/13)
