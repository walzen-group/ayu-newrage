# Ayu Newrage

A dark VS Code theme that puts [ayu Mirage](https://marketplace.visualstudio.com/items?itemName=teabyii.ayu)
syntax colors on [Mayukai Mirage](https://marketplace.visualstudio.com/items?itemName=GulajavaMinistudio.mayukaithemevsc)
chrome.

Built to hold up under VS Code's modern UI. That rendering derives parts of the
workbench from theme colors instead of reading them directly, and a palette tuned
for the classic chrome can come out washed out or muddy once those formulas are
applied. Ayu Mirage's look is kept intact; the contrast underneath it is raised
so both rendering paths stay readable. See
[Tabs under the modern UI](#tabs-under-the-modern-ui) for where that matters most.

## What comes from where

Mayukai Mirage supplies the whole workbench: editor foreground `#dadbc0`, its
opaque grey selection `#3d424d`, activity bar, tabs, title bar and status bar.

ayu Mirage supplies everything inside the code:

| | |
|---|---|
| `tokenColors` | 65 rules, ayu's set replacing Mayukai's 92 |
| `semanticTokenColors` | ayu's 21 rules; semantic highlighting is on (Mayukai ships none) |
| git decorations | ayu's semantics: modified `#80bfff`, untracked `#87d96c`, deleted `#f27983` — Mayukai had modified green and untracked red |
| `editorCursor.foreground` | `#ffcc66` gold, replacing Mayukai's red `#ff4057` |
| `terminalCursor.foreground` | `#ffcc66`, matching the editor cursor |
| `editorMultiCursor.*` | `#ffcc66` |
| 16 ANSI terminal colors | ayu's palette, so terminal output matches the syntax colors |
| `button.*` | gold `#ffcc66` on `#735923`, replacing Mayukai's hotter orange `#ffb454` |
| `editorInlayHint.foreground` | `#cccac280` — Mayukai leaves this unset |

## Deviations from both sources

**One flat editor surface at `#191e2a`.** `terminal.background` (was `#1b1c24`),
`panel.background` (was `#1b1c24`), `debugConsole.background` (was unset),
`tab.activeBackground` (was `#232834`) and `editorGroup.emptyBackground` (was
`#232834`) all moved onto the editor background. The active tab is set apart by
its gold top border rather than by a background shift, the way ayu Mirage does it.

**Tab strip contrast.** Mayukai's tab foregrounds ran from `#707a8c` to `#dadbc0`,
which put inactive tabs at 3.6:1 against the strip — below the 4.5:1 that WCAG AA
asks of body text. Every tab state is now white, and every one clears AAA:

| | on | ratio | |
|---|---|---|---|
| `tab.activeForeground` `#ffffff` | `#191e2a` | 16.66:1 | AAA |
| `tab.inactiveForeground` `#ffffff` | `#1f2430` | 15.52:1 | AAA |
| `tab.unfocusedActiveForeground` `#ffffff` | `#191e2a` | 16.66:1 | AAA |
| `tab.unfocusedInactiveForeground` `#ffffff` | `#1f2430` | 15.52:1 | AAA |
| `tab.selectedForeground` `#ffffff` | `#232834` | 14.74:1 | AAA |

Active and inactive tabs are told apart by the gold top border and the
background step from `#191e2a` to `#1f2430`, not by dimming the label. Dimming is
what put Mayukai's inactive tabs below AA in the first place.

## Tabs under the modern UI

The table above is what the `tab.*` tokens ask for, and what you get with the
classic tab strip. With `workbench.experimental.modernUI` enabled, VS Code
ignores most of them. Tab labels are set in CSS with `!important`, computed from
the global `foreground` token:

| | source | with `foreground` `#adb3be` | ratio |
|---|---|---|---|
| active label | `foreground` | `#adb3be` | 5.02:1 |
| active fill | `color-mix(foreground 22%, editor.background)` | `#3a3f4b` | — |
| inactive label | `foreground` at 50% alpha | `#636974` | 3.00:1 |
| selected label | `tab.selectedForeground` | `#ffffff` | honored |

Three consequences worth knowing:

- `tab.activeForeground`, `tab.inactiveForeground` and both `unfocused*` variants
  do nothing. `tab.selectedForeground` is the one label token still read.
- The active tab's fill is derived from the same token as its text, so the two
  move together. Darkening the highlight darkens the label faster than the
  background, which lowers contrast rather than raising it.
- Inactive labels are pinned at 50% alpha, so they top out near 4.4:1 no matter
  what the theme does. `foreground` is set to `#adb3be`, matching
  `sideBar.foreground`, so tabs and the explorer tree read as one surface.

Labels carrying a git decoration are exempt from the override and keep their
decoration color. Setting `workbench.experimental.modernUI` to `false` returns
every value in the table above to the `tab.*` tokens.

**Menus.** Mayukai defines no `menu.*` colors, so `menu.foreground` inherited
`dropdown.foreground` at `#707a8c` — 3.41:1 on the menu surface. The full set is
now explicit, with `menu.foreground` `#c2c6cc` at 8.59:1.

**Remote indicator.** `statusBarItem.remoteBackground` was `#ff9940` with no
foreground set, so VS Code defaulted to white — 2.1:1, effectively unreadable.
It's now pastel `#ffb38a` with an explicit `#1f2430` foreground at 8.92:1, plus
a matching hover pair.

## Install

Copy or symlink this folder into your extensions directory, then reload the window:

```powershell
# VS Code
Copy-Item -Recurse . "$env:USERPROFILE\.vscode\extensions\ayu-newrage-1.0.0"

# Cursor
Copy-Item -Recurse . "$env:USERPROFILE\.cursor\extensions\ayu-newrage-1.0.0"
```

Then `Ctrl+Shift+P` -> **Developer: Reload Window**, `Ctrl+K Ctrl+T`, and pick **Ayu Newrage**.

To build a `.vsix` instead:

```powershell
npx @vscode/vsce package
code --install-extension ayu-newrage-1.0.0.vsix
```
