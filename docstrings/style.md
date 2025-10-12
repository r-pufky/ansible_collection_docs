# Style Guide

### Format Rules
  Style  | Rule
 --------|----------------------------------------------------------------
  **80** | Character width.
  **2**  | Spaces per tab.
  **2**  | Spaces - nested section.
  **4**  | Spaces - extended text from previous line.
  **1**  | Vertical space between sections.
  **.**  | All lines end in `.` for complete statement unless exactly 80.
  **#**  | Comment - all lines end in `.` unless exactly 80.
  Tense  | Always use present active tense for concise documentation.


### Naming Conventions
 Style               | Rule
---------------------|-----------------------------------------------------------
 `var_name`          | Variables are unquoted in paragraphs.
 `{ROLE}_srv_{VAR}`  | Role default variable for external service setup.
 `{ROLE}_cfg_{VAR}`  | Role default variable for internal service configuration.
 `{ROLE}_role_{VAR}` | Internal role variable use.
 `_{ROLE}_{VAR}`     | Runtime internal role variable use.
 `^$*{VAL}`          | regex expressions are allowed; use {VAL}
 `N/A`               | not applicable
 `':'`               | Always quote to disambiguate separators.


### Data Types
Abbreviated datatype
  Type               | Default
 --------------------|--------------------------------------------------
  `int`              | `#` (not empty).
  `float`            | `#.#` (not empty).
  `bool`             | `False` or `True` (not empty).
  `str`              | `''`
  `list`             | `[]`
  `dict`             | `{}`
  `{TYPE} of {TYPE}` | Outer most type first (e.g. list of str - `[]`).
  `{TYPE} or {TYPE}` | Multiple types - preferred type first.


### Docstring
Always prefer consistency in documentation and update if file is touched.
Alternative formats provided with ⋮.

#### TODO
Place anywhere where additional work is needed. No restrictions.

``` yaml
# TODO(bug): List deletion via API is currently broken. This is fixed in the
#     next PiHole release. Lists will still be managed, but cannot be deleted.
# ⋮ Good categories: bug, role, {VERSION}.
```
