# Variable Docstring (remove irrelevant sections)
Prerequisite:
* [Style](style.md)

``` yaml
# One line active description of variable.
# ⋮Single line: Description (unit). Default: {VALUE}.
# ⋮Boolean values must declare what bool does (enable/disable).
#
# WARNING
# > Detailed end-user explanation of warning. **ALL** warnings are copied
# > verbatim to README.md.
#
# NOTE
# > Detailed explanation for developer of critical cases which hard to debug
# > failures will occur. Provide explanation, workarounds, and fixes.
#
# Requires role_other_option=true.
# ⋮Requires:
# ⋮* multi_option_one='value'.
# ⋮* multi_option_two=true.
#
# Versions (Schematic):
#       MAJOR: Unsafe - requires major role changes; only default MAJOR version
#              is supported. See other branches if they exist.
#       MINOR: Unsafe - Paperless has a history of introducing breaking changes
#              on minor updates (see 2.14, 2.15, 2.16). These should be
#              considered 'major' versions. See other branches if they exist.
#       PATCH: Safe - Usually requires no role updates. Some have included
#              breaking changes (2.16.0 - 2.16.2).
#   RECOMMEND: Only increment PATCH. Never use 'latest'.
# ⋮Always recommend and provide context for safe option changes.
#
# Security: CVE-2023-48795
#   CVE:
#   * https://nvd.nist.gov/vuln/detail/cve-2023-48795
#   * https://nvd.nist.gov/vuln/detail/cve-2023-46445
#   * https://nvd.nist.gov/vuln/detail/cve-2023-46446
#   * https://nvd.nist.gov/vuln/detail/cve-2024-41909
#   Decision: remove chacha20 cipher, *-etm MAC - disables attack for modern
#       systems, remove when OpenSSH 9.6 released with strict key exchange.
#   Mitigation:
#   * remove 'chacha20-poly1305@openssh.com' from client ciphers.
#   * remove '*-etm@openssh.com' from server macs.
#   Reference:
#   * https://terrapin-attack.com/#scanner
#   * https://www.openssh.com/txt/release-9.6
# ⋮Most critical security vulnerability first.
#
# Decision: Only manage owner - REST API does not provide a good way to set
#     user passwords (requires old and new passwords), etc; without directly
#     editing DB. Instead create required user so user can login.
# ⋮Explain opinionated or complex decision for implementing a specific way.
# ⋮Be clear and concise as to decision and supporting reasoning.
#
# Paragraph documentation.
# ⋮Make it unambiguously clear what variable does. Present, active tense,
# ⋮removing superfluous words.
# ⋮
# ⋮If it took more than a few seconds to understand write it down now.
#
# Special Case:
#         value: Description (Vertically align values, min indent 2 spaces).
#   other_value: Other.
# ⋮Special Case:
# ⋮* Non value based cases.
# ⋮* All paths are prepended with some_var.
# ⋮* Wildcards allowed.
#
# Values:
#            none: explicitly disable use of an authentication agent.
#              '': no default system agent (default).
#             ':': clarify colon usage or cases where colon could be confused.
#   SSH_AUTH_SOCK: get socket location from SSH_AUTH_SOCK environment variable.
#              $*: strings beginning with $ will be treated as environment
#                  variable containing the location of the socket.
#                      Values:
#                        {VALUE}: additional values for value element.
# ⋮All docstrings can be nested as needed for additional context.
# ⋮Always mark the default option with '(default)'.
#
# role_multi_complex:
#     list of dict - definitions for local configuration.
#   - config: str - name of local config ({INCLUDE}/{CONFIG}).
#     state: str - whether config should be managed or removed.
#           Additional description details here.
#
#           Reference:
#           * https://example.com
#         Values:
#           present: manage configuration
#            absent: remove configuration
#         Default: 'present'.
#     options: dict - ssh_config options to manage (per ssh_config options).
#       file: str - file location.
#       mode: str - octal file permissions.
#           Default: '0644'.
# ⋮For complex variables using condensed docstring format. Nest as needed.
# ⋮Always provide examples showing actual test data (see below).
#
# role_multi_complex:
#   - config: 'custom_ssh_config'
#     state: 'present'
#     options:
#       file: '/etc/example'
#       mode: '0644'
#
# role_multi_complex:
#   - config: 'alternative_config'
#     state: 'absent'
#   - config: 'custom_ssh_config'
#     state: 'present'
#     options:
#       file: '/etc/example'
#       mode: '0644'
#
# Variable: variable_name | type
# ⋮Defines an inline extended variable for a variable defined in another file.
# ⋮**Only** used for extremely large and complex configurations that have
# ⋮multiple overlapping, duplicated options, resulting in 5k+ line default
# ⋮values files.
# ⋮
# ⋮Extended options are defined in a separate file (with no
# ⋮actual YAML variables) and cross-reference actual configuration location.
# ⋮See r_pufky.deb.systemd defaults for example.
#
# Default: [] (un-managed).
# ⋮ Default: (description).
# ⋮   - 'long'
# ⋮   - 'defaults'
#
# Reference:
# * https://manpages.debian.org/bookworm/openssh-client/ssh_config.5.en.html
```
