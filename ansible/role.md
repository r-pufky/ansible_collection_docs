# Role Creation
Prerequisite:
* [Ansible Environment](environment.md)
* [Collection Created](collection.md)
* [GIT Submodule Settings](collection.md#enable-submodule-summaries-and-out-of-date-checks)


## Create new role

#### Add role as submodule
``` bash
git submodule add https://github.com/{USER}/{REPO} roles/{ROLE}
```
Always add roles from repository root as [submodules](../roles/submodules.md).

[Reference](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

#### Create role template files (optional)
Ansible galaxy will overwrite GIT metadata when run directly on the submodule.
Instead write a skeleton role and copy data in as-needed. See
[existing roles](https://github.com/r-pufky/ansible_paperless_ngx).

``` bash
ansible-galaxy role init --init-path /tmp example
```
[Reference](https://goetzrieger.github.io/ansible-collections/5-creating-collections/#adding-content-adding-a-custom-role)

#### .gitignore
``` yaml
.ansible/
.ansible
molecule/cache
```
https://github.com/r-pufky/ansible_paperless_ngx

## Update and Commit
Role must be committed to the repository before a new commit hash can be stored
for the collection.

``` bash
cd roles/{ROLE}
git add .
git commit
git push
git submodule update --init  # Update submodule commit hash reference.
cd {COLLECTION}
git add roles/{ROLE}  # Add updated submodule from collection root.
```
[Reference](https://git-scm.com/book/en/v2/Git-Tools-Submodules)


## Best Practices
* Always create roles explicitly designed for bare-metal installations.
* Flag protect container specific options required explicit enablement.
  * Use a collection level flag `{COLLECTION}_container_enable`.
  * Use a role level flag `{ROLE}_container_enable`.
  * As the first step in `main.yml` override role level option if collection
    value is set.

  [Reference](https://github.com/r-pufky/ansible_forgejo/blob/main/tasks/main.yml)
* Handle local and remote mounted data storage:
  * Provide option for executing task as 'root' or specified user.
  * Use **UID/GID** for those locations for remote filesystem accommodation.

  See [existing roles](https://github.com/r-pufky/ansible_paperless_ngx/blob/main/tasks/config.yml).


## References

[Role development best practices](https://docs.ansible.com/ansible/2.8/user_guide/playbooks_best_practices.html)

[Ansible magic variables](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html#magic-variables)

[Providing Default Values](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_filters.html#providing-default-values)

[`ansible-lint` rules and explanation](https://ansible.readthedocs.io/projects/lint/rules/)

[`yamllint .` rules and explanation](https://yamllint.readthedocs.io/en/stable/rules.html)

[Migrating Roles to Collections](https://docs.ansible.com/ansible/latest/dev_guide/migrating_roles.html)