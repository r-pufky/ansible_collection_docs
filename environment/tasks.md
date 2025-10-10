# TODO(docs): placeholder for tasks

Until bug is fixed do one of the following:

with https://ansible.readthedocs.io/projects/molecule/configuration/#molecule.config.Config



##### Long running tasks must notify user (use 🗘)
Alway message user with working message for long tasks.

``` yaml
- name: 'Dist upgrade | updating 🗘'
  ansible.builtin.debug:
    msg: |
      🗘 Upgrading distribution. This may take a few minutes.
```

#### Name Directives
Use nested pipes to specify testing stage and description. Additional pipe for
task subdirectory or file may be used if needed for additional clarification.

`0644 {USER}:{USER}` converge.yml
``` yaml
- name: '{TEST} | Converge | run locales.yml'
  ansible.builtin.include_role:
    name: 'r_pufky.deb.os'
    tasks_from: 'locales.yml'
```
