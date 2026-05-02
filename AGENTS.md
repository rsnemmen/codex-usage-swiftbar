# Repository Guidelines

## Project Structure & Module Organization
This repository unit is a single SwiftBar/xbar plugin:

- `codex_usage.5m.sh`: main script (menu output, OAuth/CLI fetching, parsing, rendering)
- `icon.png`: source icon asset; embedded into the script as base64 for SwiftBar `templateImage` output

It lives inside a larger parent folder with sibling plugins (`../claude_code`, `../poe_balance`, etc.), but changes here should stay scoped to `codex/` unless explicitly requested.

## Build, Test, and Development Commands
There is no build system; development is script-driven.

- `bash -n codex_usage.5m.sh`: syntax check
- `bash codex_usage.5m.sh`: run locally and print SwiftBar output
- `chmod +x codex_usage.5m.sh`: ensure executable bit
- `shellcheck codex_usage.5m.sh`: optional lint pass

For SwiftBar, install by placing `codex_usage.5m.sh` in your plugins directory; refresh interval comes from filename (`.5m.sh`).

## Coding Style & Naming Conventions
- Language: Bash with inline Python (`python3` stdlib only).
- Indentation: 2 spaces; no tabs.
- Quote variable expansions (`"$var"`), use `local` in functions, and prefer `$(...)` substitutions.
- Keep script sections explicit (`show_error`, fetchers, formatters, rendering).
- Naming:
  - `VAR_*` for user-configurable xbar vars
  - `PCT_*`, `RESET_*`, `COLOR_*`, `BAR_*` for computed display data
- Preserve SwiftBar output protocol: first line is menu title, `---` separates dropdown sections.
- Keep `OPENAI_ICON` as embedded base64 generated from `icon.png`, following the pattern in `../claude_code/claude_code.5m.sh`.

## Testing Guidelines
Automated tests are not configured. Validate manually before merging:

1. Run `bash -n codex_usage.5m.sh`.
2. Run `bash codex_usage.5m.sh` with normal environment.
3. Check error paths (e.g., missing network/token) still render valid SwiftBar output and end with `exit 0`.
4. Verify key modes using env overrides, for example:
   - `VAR_SOURCE=oauth bash codex_usage.5m.sh`
   - `VAR_SOURCE=cli bash codex_usage.5m.sh`
   - `VAR_SHOW_BARS=false bash codex_usage.5m.sh`

## Commit & Pull Request Guidelines
Recent history favors short imperative commit subjects (e.g., “Add Codex SwiftBar usage plugin…”). Follow:

- Imperative, present-tense subject line (`Add`, `Fix`, `Refactor`)
- Keep commits focused (prefer one plugin concern per commit)
- In PRs include: behavior summary, manual test commands run, and screenshots for menu/dropdown UI changes.

## Security & Configuration Tips
Do not commit tokens, `auth.json`, or cached files from `/tmp`. Use existing credential discovery paths (`$CODEX_HOME` / `~/.codex`) and avoid logging secrets in menu output.
