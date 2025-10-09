# Release Guide

* Update `galaxy.yml`
* Update `changelogs/changelog.yaml`

Remove cached ansible collections, commit (with signature),
``` bash
find . -type d -name '.ansible' -exec rm -rfv {} \;  # If needed.
git commit -S
git tag {VERSION} {COMMIT}
git push
git push --tags
```

Build release artifact. Sanity check size/contents for unwanted inclusions.
``` bash
ansible-galaxy collection build
```

Create new release on GitHub using tag:
* Generate release notes (right of tag)
* Use galaxy.yml for commit template. See previous releases.
* Manually attach build artifact to tagged release


###### NOTES ######
## New Debian Release
TODO(docs): merge, format, update.

Only maintain active development for current Debian release; users using older
releases are welcome to download the specific branch and maintain themselves.

Decision: Distro versions are too disparate - Major release versions have too
    many changes to realistically track both, especially after migrating to the
    new platform. Always leave artifacts so those who wish to stay behind may
    handle the cost of staying behind themselves.

For all collections, roles:

* Ensure all changes are checked in and committed
* Update and commit README.md with link to branch, new main
* git checkout -b 12.x
* git push -u origin 12.x
* git checkout main
* Continue work on new debian platform


### TODO

Bookworm -> trixie

Python 3.11 errors -- likely /tmp/ansible-cache is caching old modules clear.
  rm -rfv /tmp/ansible-cache


* update README.md and commit


### Releases
Major release versions track Debian release versions:

* **[13.x.x](https://github.com/r-pufky/ansible_apt)**: 13 Trixie.
* **[12.x.x](https://github.com/r-pufky/ansible_apt/tree/12.x)**: 12 Bookworm.



  Final Debian 12.x release.

  Update README.md for Distro releases.

* branch
  git tag 12.0.0
  git push && git push --tags
  git checkout -b 12.x  (git checkout -b 12.x {ID})
  git push -u origin 12.x
  git checkout main

* Update testing images
  debian-systemd:12 -> debian-systemd:13
  debian/bookworm64 -> debian/trixie64
  debian-12- -> debian-

* grep bookworm -> trixie
* grep 12.x -> 13.x
* resolve any todo's
* Update vars/main.yml
  * dates, packages, etc.
* Fix any warts that are known or don't like
* UPODATE DOCSTRING FORMATES ESPECIALLY FOR OLDER ROLE
* Major role changes may be applied here (new/removed vars, process changes, etc.)
* Check updated packages for new variables
* Diff links (esp distro versions) for added/removed items, updated links.
  wdiff 1  2

  meld \
    <(curl -s https://forgejo.org/docs/v11.0/admin/config-cheat-sheet/)
    <(curl -s https://forgejo.org/docs/v10.0/admin/config-cheat-sheet/)

* Update alert messaging:
  * Use box for fixed-lenght alert header, then list variables below

╭───────────────────────────────────────────────────────────────────╮
│                                                                   │
│   Distribution is being upgraded. This will take a few minutes.   │
│                                                                   │
╰───────────────────────────────────────────────────────────────────╯

╭───────────────────────────────────────────────────────────────────╮
│                                                                   │
│   Distribution is being upgraded. This will take a few minutes.   │
│                                                                   │
├───────────────────────────────────────────────────────────────────╯
│ var: value
│ var: value
│
│ some other value.


* commit with tag 13.0.0

Migrate to Debian 13.x (Trixie).

Migrate to Debian 13.x (Trixie).


ansible_collection_deb -- currently in 12.x -- updates to roles need to be
updated in main, NOT 12.x.
- unstage 12.x changes AFTER committing main
- stage updates to main



* PACKAGE VERSIONS TRACK DEBIAN LIKE COLLECTIONS
* INDEPENDENT VERSION TRACK PACKAGE LIKE PLEX.