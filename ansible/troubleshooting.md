# Ansible Troubleshooting

### Remove Submodule
Usually after a bad initial creation or weird sync state.

#### Remove from collection root
``` bash
rm -rfv roles/{ROLE}
git rm -r roles/{ROLE}
rm -rf .git/modules/roles/{ROLE}
git config --remove-section submodule.roles/{ROLE}
```
Submodule is fully removed and ready to be re-added.

[Reference](https://stackoverflow.com/questions/1260748/how-do-i-remove-a-submodule)

### Multiple configurations found for 'submodule.roles/{ROLE}'
**.git/config** mis-match against checked in submodules file **.gitmodules**.

#### Error
``` bash
warning: {HASH}:.gitmodules, multiple configurations found for 'submodule.roles/{ROLE}'. Skipping second one!
```

#### Ensure files are the same
Check both `.git/config` and `.gitmodules` are up to date and the same. Add to
git commit if needed.

[Reference](https://stackoverflow.com/questions/51883100/git-submodule-warning-multiple-configurations-found)
