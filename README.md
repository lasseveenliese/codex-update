# codex-update

Mini-Terminal-Tool für macOS, das:

- die lokal installierte `codex`-Version ausliest,
- die aktuelle Version aus `npm` vergleicht,
- optional ein Update auf `@openai/codex` startet.

## Voraussetzungen

- macOS mit `zsh`
- `npm` im PATH
- `codex` im PATH

Prüfen:

```bash
command -v codex
command -v npm
```

## Nutzung

Aus dem Repo:

```bash
chmod +x ./codex-update
./codex-update
```

Direkt aktualisieren ohne Rückfrage:

```bash
./codex-update -y
```

Hilfe:

```bash
./codex-update --help
```

## Installation auf macOS

### Benutzerlokal installieren

1. Lokales Bin-Verzeichnis anlegen:

```bash
mkdir -p "$HOME/bin"
```

2. Skript direkt von GitHub in `~/bin` kopieren:

```bash
curl -fsSL "https://raw.githubusercontent.com/lasseveenliese/codex-update/main/codex-update" -o "$HOME/bin/codex-update"
chmod +x "$HOME/bin/codex-update"
```

3. Sicherstellen, dass `~/bin` im PATH ist:

```bash
grep -qxF 'export PATH="$HOME/bin:$PATH"' "$HOME/.zshrc" 2>/dev/null || echo 'export PATH="$HOME/bin:$PATH"' >> "$HOME/.zshrc"
source "$HOME/.zshrc"
```

4. Test:

```bash
codex-update --help
```

## Skript aktualisieren

Wenn `codex-update` bereits in `~/bin` installiert ist:

1. Neueste Version aus GitHub laden:

```bash
curl -fsSL "https://raw.githubusercontent.com/lasseveenliese/codex-update/main/codex-update" -o "$HOME/bin/codex-update"
```

2. Ausführbar machen:

```bash
chmod +x "$HOME/bin/codex-update"
```

3. Aktualisierung prüfen:

```bash
codex-update --help
```

## Ablauf des Skripts

1. Prüft benötigte Tools (`codex`, `npm`, `grep`, `head`, `tr`)
2. Liest installierte Version via `codex --version`
3. Liest neueste Version via `npm view @openai/codex version`
4. Vergleicht beide Versionen
5. Startet bei Bedarf `npm i -g @openai/codex` (mit Rückfrage oder `-y`)

## Rückgabecodes

- `0`: erfolgreich / bereits aktuell
- `1`: Update vom Nutzer abgelehnt oder ungültige Antwort
- `2`: Fehler (z. B. fehlende Abhängigkeit, Parse-Problem)

## Hinweise

- Das Update nutzt eine globale npm-Installation und kann je nach npm-Konfiguration Admin-Rechte benötigen.
- Bei nicht-interaktiver Ausführung ohne `-y` bricht das Skript mit Fehler ab, um hängende Jobs zu vermeiden.

## Deinstallation

### Benutzerlokal

```bash
rm -f "$HOME/bin/codex-update"
```
