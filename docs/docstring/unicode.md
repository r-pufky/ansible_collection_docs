# Unicode Glyphs

## Text Glyphs

 Glyph | Code  | Use
------:|:------|--------------------------
 ➔     | 2794  | Menus, sub-items, links.
 ⚠     | 26a0  | Warning.
 ⓘ     | 24be  | Informational.
 🗘    | 1f5d8 | Waiting.
 ✔     | 2714  | Success / Enabled.
 ✘     | 2718  | Failure / Disabled.
 ☑     | 2611  | Checked / Enabled.
 ☐     | 2610  | Unchecked / Disabled.
 ⋮     | 22ee  | Additional context (or context menu).
 ⚙     | 2699  | Settings.
 ⌘     | 2318  | Super key.

## Alert Window Glyphs

 Glyph | Code | Use
------:|:----|----------------------
 ─     | 2500 | horizontal line.
 │     | 2502 | Vertical line.
 ╭     | 256D | Top left corner.
 ╮     | 256E | Top right corner.
 ╯     | 256F | Bottom right corner.
 ╰     | 2570 | Bottom left corner.
 ├     | 251C | Bottom left message.

## Window Alert
Windows are 70 characters wide with minimum 1 character space horizontally,
1 one vertically. Place raw (unconfined) variable dumps below with additional
vertical space:

``` yaml
╭───────────────────────────────────────────────────────────────────╮
│                                                                   │
│   Distribution is being upgraded. This will take a few minutes.   │
│                                                                   │
╰───────────────────────────────────────────────────────────────────╯

{{ var_dump }}
```

## Window Extended
[Window Alert](#window-alert) with extended information presented immediately
below. Variable information is known to be one line (not horizontally
constrained):

``` yaml
╭───────────────────────────────────────────────────────────────────╮
│                                                                   │
│   Distribution is being upgraded. This will take a few minutes.   │
│                                                                   │
├───────────────────────────────────────────────────────────────────╯
│ extended information
│
│ var: {{ var_dump }}
```
