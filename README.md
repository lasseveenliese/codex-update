# codex-update

German version: `README_GERMAN.md`

A small macOS terminal tool that:

- reads the locally installed `codex` version,
- compares it with the latest version from `npm`,
- optionally updates `@openai/codex`.

## Requirements

- macOS with `zsh`
- `npm` in `PATH`
- `codex` in `PATH`

Check:

```bash
command -v codex
command -v npm
```

## Usage

From this repository:

```bash
chmod +x ./codex-update
./codex-update
```

Update immediately without confirmation:

```bash
./codex-update -y
```

Help:

```bash
./codex-update --help
```

## Install on macOS

### Install for your user

1. Create a local bin directory:

```bash
mkdir -p "$HOME/bin"
```

2. Download the script from GitHub into `~/bin`:

```bash
curl -fsSL "https://raw.githubusercontent.com/lasseveenliese/codex-update/main/codex-update?ts=$(date +%s)" -o "$HOME/bin/codex-update"
chmod +x "$HOME/bin/codex-update"
```

3. Ensure `~/bin` is in `PATH`:

```bash
grep -qxF 'export PATH="$HOME/bin:$PATH"' "$HOME/.zshrc" 2>/dev/null || echo 'export PATH="$HOME/bin:$PATH"' >> "$HOME/.zshrc"
source "$HOME/.zshrc"
```

4. Test:

```bash
codex-update --help
```

## Update the script

If `codex-update` is already installed in `~/bin`:

1. Download the latest version from GitHub:

```bash
curl -fsSL "https://raw.githubusercontent.com/lasseveenliese/codex-update/main/codex-update?ts=$(date +%s)" -o "$HOME/bin/codex-update"
```

2. Make it executable:

```bash
chmod +x "$HOME/bin/codex-update"
```

3. Verify the update:

```bash
codex-update --help
```

## Script flow

1. Checks required tools (`codex`, `npm`, `grep`, `head`, `tr`)
2. Reads installed version via `codex --version`
3. Reads latest version via `npm view @openai/codex version`
4. Compares both versions
5. Runs `npm i -g @openai/codex` when an update is available (with prompt or `-y`)

## Exit codes

- `0`: success / already up to date
- `1`: update declined or invalid prompt input
- `2`: error (for example missing dependency or parse error)

## Notes

- The update uses a global npm installation and may require elevated permissions depending on your npm setup.
- In non-interactive shells without `-y`, the script exits with an error to avoid hanging jobs.

## Uninstall

### User local

```bash
rm -f "$HOME/bin/codex-update"
```
