# Major Release Guide

## Decision: Distro versions are too disparate
Major release versions have too many changes to realistically track both,
especially after migrating to the new platform. Always leave artifacts so those
who wish to stay behind may handle the cost of staying behind themselves.

Only maintain active development for current Debian release; users using older
releases are welcome to download the specific branch and maintain themselves.

## Create Old Major Branch
Assumes branching **12.x** and starting **13.x** development.

1. Commit all open changes (or stash).
2. Update and commit `README.md`.
   ```
   ### Releases
   Major release versions track Debian release versions:

   * **[13.x.x](https://github.com/r-pufky/ansible_apt)**: 13 Trixie.
   * **[12.x.x](https://github.com/r-pufky/ansible_apt/tree/12.x)**: 12 Bookworm.
   ```

   Commit Message:
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
     * debian-systemd:12 -> debian-systemd:13
     * debian/bookworm64 -> debian/trixie64
     * debian-12- -> debian-
   * grep bookworm -> trixie
   * grep 12.x -> 13.x
   * grep fail:
   * grep debug:
   * grep msg:
   * resolve any TODO's
   * Update **vars/main.yml**
     * dates, packages, etc.
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
   * Update **meta/main.yml**. It is *OK* to aggressively deprecate supported
     OS's based on this release model.
   * Update **galaxy.yml**.
   * Update **changelogs/changelog.yaml**

### Releases
Roles may override default format. Check **var/main.yml**. Default to schematic
versioning; each signifier may inherit versioning scheme used for underlying
system.

#### Standard release format
* Role: **{OS}-{SERVICE}-{ROLE}**
* Collection: **{OS}-{MAJOR}-{MINOR}**

`12.0.0-2.0.3-1.0.0`
* 12.0.0 - Debian 12 (bookworm).
* 2.0.3 - Service/app version.
* 1.0.0 - Role version.

`12.1.1`
* 12 - Collection compatible with Debian 12 (bookworm).
* 1 - Collection major version 1.
* 1 - Collection minor version 1.

#### Build release artifact
``` bash
ansible-galaxy collection build
```
Sanity check size/contents for unwanted inclusions.

#### Create new release on GitHub using tag
* Generate release notes.
* Use galaxy.yml for commit template. See previous releases.

#### Upload new artifact to galaxy.ansible.com
