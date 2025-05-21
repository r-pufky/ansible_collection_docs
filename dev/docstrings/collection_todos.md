
# TODO(role): Fix line wrapping for lookup calls (all roles).
# TODO(role): line-length - ONLY accept for docstring URLs. Make decision and
#     document for everything else.
#     URL's OK to not split
#     docstrings can be longer if they make sense (CLI, etc)
-

move r_pufky.deb.tests to r_pufky.tests and deploy as NO DEP/PLATFORM AGNOSTIC independent role
r_pufky -> r-pufky (actual galaxy username)

Get collection role working on galaxy
- r_pufky.deb.apt works
- r-pufky.deb.apt does not
- r-pufky.apt does not
-- collection was init'd with "ansible-galaxy collection init --init-path . r_pufky.srv"

unsure why


USE r_pufky FOR THE NEW COLLECTION NAMESPACE, NOT r-pufky!!!

*ALL* roles must be in a collection to be certified ansible content.


requirements.yml bulllllshit https://github.com/ansible/ansible/issues/73347


    - name: 'Dist upgrade | verify | standard | MANUAL VERIFICATION ⚠'
      ansible.builtin.debug:
        msg: |

          ⚠ MANUAL VERIFICATION.

same with check and non check for asserts (all pass/fails)
⚠
🗘
✔
✘
- asserts only in message
- debugs - message and name

Use libraries for common operations.

Migrate to using standardized libraries for common operations and tests
within the role.

Changed:
* Migrate OS roles to r_pufky.deb collection.
* Migrate common test patterns to r_pufky.lib.tests.
* Minimize use of line-length yamllint disable.
* Standardize molecule test docstrings.


**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.deb](https://github.com/r-pufky/ansible_collection_deb) collection.
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv) collection.


TODO (pending deps)
nfs

stopped --zfs

TODO - slurp
SMELL -- use molecule/cache if appropriate.
