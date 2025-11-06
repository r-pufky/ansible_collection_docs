# Major Release Guide

!!! tip "Decision: Distro versions are too disparate"
    Major release versions have too many changes to realistically track both,
    especially after migrating to a new platform. Always leave artifacts so
    those who wish to stay behind may handle the cost of staying behind
    themselves.

    Only maintain active development for current Debian release; users using
    older releases are welcome to download the specific branch and maintain
    themselves.

## Create Old Major Branch
Assumes branching **12.x** and starting **13.x** development.

1. Commit all open changes (or stash).
2. Update `README.md`.
    ``` md
    ### Releases
    Major release versions track Debian release versions:

    * **[13.x.x](https://github.com/r-pufky/ansible_apt)**: 13 Trixie.
    * **[12.x.x](https://github.com/r-pufky/ansible_apt/tree/12.x)**: 12 Bookworm.
    ```
3. Commit
    ``` md
    Final Debian 12.x release.

    Update README.md for Distro releases.
    ```
3. Create Old Major Branch and Push
    ``` bash
    git tag 12.x.x  # Last version for commit above.
    git push && git push --tags
    git checkout -b 12.x  # {ID} if a specific commit is needed.
    git push -u origin 12.x
    git checkout main
    ```
4. Update known settings to new version
    * Test images
        * `debian-systemd:12` ➔ `debian-systemd:13`
        * `debian/bookworm64` ➔ `inception-of-things/debian-trixie`
        * `debian-12-` ➔ `debian-`
    * Entire role
      ``` bash
      grep bookworm # Replace with trixie.
      grep 12.x # Replace with 13.x.
      grep fail:  # Update formatting.
      grep debug:  # Update formatting.
      grep msg:  # Update formatting.
      grep -ri todo  # Resolve any open TODO's.
      grep -ri noqa  # Minimize NOQA's for accepted ansible-lint disables.
      ```
    * Update **vars/main.yml** (dates, packages, etc.).
    * Fix any code warts.
    * Update docstring formats.
    * Implement MAJOR role changes.
    * Check updated packages for new variables.
    * Diff links (esp distro versions) for added/removed items, updated links.
      ``` bash
      meld \
        <(curl -s https://forgejo.org/docs/v11.0/admin/config-cheat-sheet/)
        <(curl -s https://forgejo.org/docs/v10.0/admin/config-cheat-sheet/)
      ```
    * Update **meta/main.yml**.

        !!! tip "Decision: Use aggressive deprecation"
            Remove previous versions when in-depth workarounds are required.
            Previous OS support encoded in historical build artifacts.

    * Update **galaxy.yml**.
    * Update **changelogs/changelog.yaml**

        Truncate changelog and use `ancestor: {LAST VERSION}` to link to
        previous version changelog.

## Versioning
Default to schematic versioning - each signifier may inherit versioning scheme
used for underlying system.

??? abstract "Collection: **{OS}-{MAJOR}-{MINOR}**"
    **12.1.1**

    * 12 - Debian 12 Bookworm (breaking changes).
    * 1 - Collection major version 1 (breaking changes).
    * 1 - Collection minor version 1 (non-breaking changes).

    On a new debian release:

    * Create a hard branch for release (with codename).
    * Main becomes the new Debian release.
    * Mark branch release as no longer supported (after migration is completed).

??? abstract "Role: **{OS}-{SERVICE}-{ROLE}**"
    **12-2.0.3-1.0.0**

    * 12 - Debian 12 (bookworm).
    * 2.0.3 - Service/app version.
    * 1.0.0 - Role version.


    On a new debian release:

    * Create a hard branch for release (with codename).
    * Main becomes the new Debian release.
    * Mark branch release as no longer supported (after migration is completed).

    Roles may override default format.

## Build
``` bash
ansible-galaxy collection build
```
Sanity check size/contents for unwanted inclusions.

## Release

1. Upload new artifact to https://galaxy.ansible.com

    Confirm that collection markdown is formatted correctly (Ansible uses a
    subset of GFM to render documents and may differ from Github). Rebuild and
    re-upload as needed.

2. Create github release.

    * Use **galaxy.yml** for commit template. See previous releases.

## Release Order
Collections depend on lower level dependencies. Release in this order:

1. data
2. lib
3. deb
4. srv
5. arr
6. game
7. docs
