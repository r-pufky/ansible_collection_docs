# Task Docstring (remove irrelevant sections)
Prerequisite:
* [Style](style.md)

``` yaml
###############################################################################
# Capped Header (Context)
###############################################################################
# Immediate start description of task set.
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
# Args:
#   db: Postgres DB to manage.
# ⋮Explicitly for block/loop idioms to define what variables are required for
# ⋮task execution.
#
# Generates:
#   _paperless_ngx: dict - Role runtime specific config.
#   _paperless_ngx_map: list of dict - Annotated config map.
#   _paperless_ngx_order: list of str - Config section order.
# ⋮Runtime variables that may be consumed in other role task files.
#
# Exports:
#   _sqlite_sql_results: dict - User consumable return values from execution.
# ⋮Exported variables end-users may consume. Must update README.md.
# ⋮
# ⋮README.md:
# ⋮ ### Generated Variables
# ⋮ After successful execution the following variables are available for
# ⋮ further manipulation during the same play (standard role variable scope):
# ⋮
# ⋮  Variable            | type | Description
# ⋮ ---------------------|------|-----------------------------------------
# ⋮  _sqlite_sql_results | dict | registered return results from command.
#
# Reference:
# * https://manpages.debian.org/bookworm/openssh-client/ssh_config.5.en.html
```
