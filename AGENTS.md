# Night Eagle

A VS Code color theme forked from [Night Owl](https://github.com/sdras/night-owl-vscode-theme) by Sarah Drasner. Night Owl is no longer maintained and things break as VS Code evolves; Night Eagle keeps the same look while staying compatible with current VS Code.

Guiding principle: preserve Night Owl's look. When VS Code renames or replaces a color key, port the original Night Owl value to the new key rather than inventing new colors (unless the user asks for a change).

## Layout

- `themes/night-eagle-color-theme.json` — the only theme (dark, with italics). Night Owl's light and no-italics variants were dropped on purpose.
- `package.json` — minimal extension manifest; one entry in `contributes.themes`. Theme files use the `night-eagle-` prefix; the display name/label is "Night Eagle".
- `.vscode/launch.json` — F5 opens an Extension Development Host with the theme loaded. Pick it via "Preferences: Color Theme"; reload that window (Cmd+R) after editing the JSON.
- `LICENSE.md` — Night Owl's MIT license; must be kept for attribution.

## Working on the theme

- Validate with VS Code's diagnostics on the theme file (the Problems panel, or the getDiagnostics tool). It flags unknown keys, deprecated keys, and wrong types. Keep it at zero warnings.
- Values must be color strings — no `null` (omit the key instead).
- Token background colors are not supported by VS Code; only use `foreground`/`fontStyle` in `tokenColors`.
- Format with `pnpm format` (Prettier) to keep the original style (short arrays on one line).
- When looking for a fix to something that broke, check the Night Owl issue tracker first (`gh search issues --repo sdras/night-owl-vscode-theme ...`); users often report breakage and workarounds there.

## History of compatibility fixes

- Removed `null` values, unknown keys (`welcomePage.button*`, `activityBar.dropBackground`) and token scopes that had been misplaced in `colors`.
- Renamed `editorIndentGuide.background`/`activeBackground` → `...1`, `progress.background` → `progressBar.background`.
- `merge.currentHeaderBackground` must be transparent → `#5f7e9780`.
- Invalid-token rules had background colors; their former background is now the foreground.
- VS Code's modern editor tab UI ignores `tab.*` colors, which made the active tab indistinguishable (Night Owl issue #348). Fixed by adding `modernEditorTab.activeBackground`, `activeForeground` and `inactiveBackground` with Night Owl's original tab values. Other `modernEditorTab.*` keys (inactive/unfocused foregrounds) are not valid.

## Publishing

- `pnpm package` builds a `.vsix`; `pnpm release` publishes to the Marketplace (publisher `crudh`, needs `pnpm exec vsce login crudh`). Not `pnpm publish`, which targets npm; `"private": true` blocks that. Both pass `--no-dependencies` since pnpm's `node_modules` layout breaks vsce's dependency scan and a theme has no runtime deps.
- `.vscodeignore` keeps the package to README, CHANGELOG, LICENSE, package.json, the icon (`eagle-icon.png`, 256×256 PNG; the Marketplace needs a square PNG of at least 128×128, no SVG) and the theme.
- Bump `version` in package.json and add a CHANGELOG.md entry for each release.

## Conventions

- Package manager is pnpm, pinned via the `packageManager` field (Corepack). Run `pnpm install` first.
- Branch is `master`, remote `origin` → https://github.com/crudh/night-eagle.
