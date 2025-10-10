# Executing Tests
Prerequisite:
* [ansible environment](../ansible/README.md)
* [Podman](../podman/README.md) - Primary test framework.
* [Vagrant](../vagrant/README.md) - Secondary test framework. Explicit reasons
for using this test framework must be clearly documented.
* [Molecule setup](setup.md).


## Execute Tests

#### Manual test steps
``` bash
molecule create
molecule converge -- -v
molecule verify
```
* `scenario-name` may be used in each step to execute non-default scenario.

#### Run through the default scenario or specified scenarios
``` bash
molecule test  # Runs default test, always destroy test containers.
molecule test --scenario-name=alt_test  # Runs 'alt_test'.
```

#### Run through all existing Molecule scenarios
``` bash
molecule test
molecule test --all
molecule test --all -- -v
molecule test --all --destroy=never  # Run all tests, keep containers.
```

#### Debug molecule state
``` bash
molecule --debug ${COMMAND}  # will enabling verbose debugging.
```
