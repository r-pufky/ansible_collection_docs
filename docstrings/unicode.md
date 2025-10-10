# Unicode Glyphs

 Glyph | Code  | use
-------|-------|--------------------------
 ➔     | 2794  | Menus, sub-items, links.
 ⚠     | 26a0  | Warning.
 ⓘ     | 24be  | Informational.
 🗘    | 1f5d8 | Waiting.
 ✔     | 2714  | Success.
 ✘     | 2718  | Failure.
 ☐     | 2610  | Unchecked.
 ☑     | 2611  | Checked.
 ⋮     | 22ee  | Context menu.
 ⚙     | 2699  | Settings.
 ⌘     | 2318  | Super key.

## Alert Window Format

 Glyph | Code | use
-------|------|----------------------
 ─     | 26a0 | horizontal line.
 │     | 2502 | Vertical line.
 ╭     | 2714 | Top left corner.
 ╮     | 2718 | Top right corner.
 ╯     | 2718 | Bottom right corner.
 ╰     | 2718 | Bottom left corner.
 ├     | 2718 | Bottom left message.

### Window
Windows are 70 characters wide with minimum 1 character space horizontally,
1 one vertically. Place raw (unconfined) variable dumps below with additional
vertical space:

```
╭───────────────────────────────────────────────────────────────────╮
│                                                                   │
│   Distribution is being upgraded. This will take a few minutes.   │
│                                                                   │
╰───────────────────────────────────────────────────────────────────╯

{{ var_dump }}
```

### Window With Message
Windows with extended information presented immediately below. Variable
information is known to be one line (not horizontally constrained):

```
╭───────────────────────────────────────────────────────────────────╮
│                                                                   │
│   Distribution is being upgraded. This will take a few minutes.   │
│                                                                   │
├───────────────────────────────────────────────────────────────────╯
│ extended information
│
│ var: {{ var_dump }}
```
