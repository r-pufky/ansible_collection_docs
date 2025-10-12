# Commits
Prerequisite:
* [Ansible Environment](../README.md)

## Ensure all TODOs are valid
``` bash
grep -ri todo
```

## Ensure linting follows [guidelines](../role/linting.md)
``` bash
grep -ri yamllint  # yamllint
grep -ri noqa  # ansible-lint
```

## Lint returns clean
``` bash
yamllint .
ansible-lint .
```

[yamllint rules](https://yamllint.readthedocs.io/en/stable/)

[ansible-lint rules](https://ansible.readthedocs.io/projects/lint/rules/)

## Add collection ignore files
Add files that should not be included in the built collection; such as tests.

#### galaxy.yml
``` yaml
build_ignore:
  - .gitignore
  - changelogs/.plugin-cache.yaml
```
[Reference](https://docs.ansible.com/ansible/devel/dev_guide/developing_collections_distributing.html#ignoring-files-and-folders)

## Update collection submodule reference
Required otherwise the collection will not use the updated module on checkout.

[Update Submodule Reference](../role/README.md#update-and-commit)

## Versioning (galaxy.yml)
**{OS}-{MAJOR}-{MINOR}** per [Semantic Versioning v2](https://semver.org/)

* Major: New platform release (debian release). Use **0** until first release
         as this prevents galaxy inclusion.
* Minor: Breaking changes.
* Point: Bug fixes / updates / non-breaking changes.

Versioning follows Debian release (12 - 12 Bookworm). On a new debian release:
* Create a hard branch for release (with codename).
* Main becomes the new Debian release.
* Mark branch release as no longer supported (after migration is completed).

### Track Project Updates
* Always link to the latest release (or release page).
* Set alerts to check for new release (or push notifications) for each role.

## Build Galaxy Release

``` bash
ansible-galaxy collection build
ansible-galaxy collection publish
```
[Reference](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections_creating.html)
