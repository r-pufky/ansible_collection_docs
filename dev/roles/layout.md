# Layout
All roles in collection must use this layout to remain consistent. If unclear
see previous work in other roles and update documentation.

``` yaml
├─── .ansible          # Transient molecule testing cache (very large); do not
│                      # commit. Link to tmpfs for development and testing.
│                      # '.ansible -> /tmp/ansible-cache'.
├┬── defaults
│├┬─ main
││├─ main.yml          # Role specific configuration (users, locations, etc).
││├─ *.yml             # service specific configuration; named by service/type.
││╰─ ports.yml         # Firewall definitions, even if not used.
│╰┬─ config            # Service specific configuration (what role manages).
│ ├─ {DIR}             # Very large configurations may require breaking
│ │                    # configuration into sub directories for configuration
│ │                    # multiple dependencies or sections; follow previous
│ │                    # guidance for files. See r_pufky.deb.systemd,
│ │                    # r_pufky.deb.deluge.
│ ╰─ {FILE}            # Break into sections based on service documentation.
├─── docs              # Personal documentation and notes; these are carried
│                      # over from sphinx and are currently free-form. Use
│                      # README.md.
├─── .gitignore        # Standard across all roles. Ignore .ansible and
│                      # molecule testing caches:
│                      #   .ansible/
│                      #   .ansible
│                      #   molecule/cache
├─── handlers          # Constrained to one file, with simultaneous handlers
│                      # called using 'listen' to group together (executed in
│                      # order of) appearance. See r_pufky.deb.deluge.
├─── LICENSE           # Use GNU Affero General Public License.
├┬── meta
│╰┬─ main.yml          # Galaxy metadata, MUST specify 'role_name'. As of
│ │                    # Galaxy-2.0 dependencies are listed in the
│ │                    # {COLLECTION}/galaxy.yml. 'description' is imported to
│ │                    # the collection description on galaxy.
│ ╰─ requirements.yml  # Remove. Only used for dependencies in Galaxy-1.0. Use
│                      # {COLLECTION}/galaxy.yml
├┬── molecule          # Unit testing.
│├── cache             # Testing cache location, include downloaded files and
││                     # generated testing data. No restrictions.
│╰┬─ {TEST}            # Default tests should cover default usage. Add tests
│ │                    # for regressions, and test all possible configuration
│ │                    # setup for roles. MUST be able to rely on tests to
│ │                    # signify role/update breakage.
│ ├─ molecule.yml      # Specify explicit test scenarios and test variables.
│ ├─ prepare.yml       # Populate cache and do required instance pre-setup.
│ │                    # Live API tests must be explicitly enabled and fail
│ │                    # by default with a message on how to enable.
│ ├─ converge.yml      # Minimize complexity and execute role; test assertion
│ ├─ converge.yml      # cases here.
│ ╰─ verify.yml        # Validate test cases. Use r_pufky.lib.tests.
├─── README.md         # See pre-existing work; standardize.
├┬── tasks             # In order of execution. If a step contains minimal
││                     # tasks, it is OK to role those tasks into main.yml or
││                     # the previous step, whatever makes the MOST sense.
││                     # Prefer main.yml.
│├── annotate.yml      # Annotate user/role data for standardized validation
││                     # consumption, and rendering. Should happen before
││                     # asserts and prep phase. User provided input should all
││                     # be parsed in this step.
││                     # * Parse and process user input; generating dynamic
││                     #   role variables and annotating data:
││                     #     * {ROLE}_{VAR} for all role, user data.
││                     #     * _{ROLE}_{VAR} for all dynamic role variables,
││                     #       generates, exports.
││                     # * Sanitize paths and build data structures as needed.
││                     # * Create base installation paths IF required based on
││                     #   package installation requirements.
││                     # * Create user and obtain system UID/GID for network
││                     #   mounts (use instead of specified user/group name).
││                     # See r_pufky.deb.paperless_ngx.
│├── assert.yml        # Pre-role execution assertions. Always describe exactly
││                     # what failed and how to fix it. Sanity check:
││                     # * Version.
││                     # * Data paths are not in the installed path.
│├── prep.yml          # Prepare role for execution (future steps should
││                     # execute tasks without additional munging of data):
││                     # * Parse and process user input; generating dynamic
││                     #   role variables and annotating data:
││                     #     * _{ROLE}_{VAR} for all role variables, generates,
││                     #       exports.
││                     #     * __{ROLE}_{VAR} for temporary role variables used
││                     #       in that file only.
││                     # * Sanitize paths and build data structures as needed.
││                     # * Create base installation paths IF required based on
││                     #   package installation requirements.
││                     # * Create user and obtain system UID/GID for network
││                     #   mounts (use instead of specified user/group name).
││                     # * Docstring should include general approach for prep
││                     #   and specific gotchas to look out for.
││                     # * Install any required packages for prep to execute.
││                     # * STOP any running services related to the role; must
││                     #   assume service may not exist yet. Exceptions must
││                     #   be documented as to why the service is not stopped
││                     #   before role execution.
││                     # * Minimally include references to main project.
│├── install.yml       # Install/download/build service packages/dependencies.
│├── config.yml        # Config installed service:
││                     # * Create required / additional directories (UID/GID).
││                     # * Manage systemd service definitions.
││                     # * Set configuration files.
│├── validate.yml      # Finish role application and validate it has completed
││                     # successfully:
││                     # * Start services and flush if needed.
││                     # * Query services are running and active (or the
││                     #   expected state of the service).
││                     # * Cleanup any files as needed.
││                     # * Write status message.
│╰── {FILE}.yml        # Additional files may be included as needed for:
│                      # * Complex service configuration simplification.
│                      # * Loops.
│                      # * Service start, polling, and additional tasks for
│                      #   modifying services which have values in a DB, not
│                      #   in a config.
├─── templates         # Minimize logic in templates, using dynamic templates
│                      # based on data structure (primarily only formatting
│                      # output, not data manipulation). Logic should be done
│                      # in prep.yml or related files. Prefer flat layout; sub
│                      # directories are allowed as needed. See
│                      # r_pufky.srv.forgejo, r_pufky.srv.postgres,
│                      # r_pufky.srv.deluge.
╰┬── vars
 ├── main.yml          # Role configuration, including package definitions,
 │                     # default user configuration, download locations.
 ╰┬─ main              # Only use main directory if var templates are used.
  ├─ main.yml
  ╰─ templates.yml     # Templated data structures used for sanitizing user
                       # input. These should be set to default values and
                       # combined with user data to set defaults for complex
                       # data structures.
```
