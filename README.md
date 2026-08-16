# Ayu Newrage

A dark VS Code theme that puts [ayu Mirage](https://marketplace.visualstudio.com/items?itemName=teabyii.ayu)
syntax colors on [Mayukai Mirage](https://marketplace.visualstudio.com/items?itemName=GulajavaMinistudio.mayukaithemevsc)
chrome.

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
which put inactive tabs at 3.6:1 against the strip. Every tab state is now white:

| | ratio |
|---|---|
| `tab.activeForeground` `#ffffff` on `#191e2a` | 16.66:1 |
| `tab.inactiveForeground` `#ffffff` on `#1f2430` | 15.52:1 |
| `tab.unfocusedActiveForeground` `#ffffff` | 16.66:1 |
| `tab.unfocusedInactiveForeground` `#ffffff` | 15.52:1 |
| `tab.selectedForeground` `#ffffff` on `#232834` | 14.74:1 |

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
