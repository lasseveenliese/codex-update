# codex-update

Englische Version: `README.md`

Ein kleines macOS-Terminal-Tool, das:

- die lokal installierte `codex`-Version ausliest,
- sie mit der neuesten Version aus `npm` vergleicht,
- optional `@openai/codex` aktualisiert.

## Voraussetzungen

- macOS mit `zsh`
- `npm` im `PATH`
- `codex` im `PATH`

Prüfen:

```bash
command -v codex
command -v npm
```

## Nutzung

Aus diesem Repository:

```bash
chmod +x ./codex-update
./codex-update
```

Direkt ohne Rückfrage aktualisieren:

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
curl -fsSL "https://raw.githubusercontent.com/lasseveenliese/codex-update/main/codex-update?ts=$(date +%s)" -o "$HOME/bin/codex-update"
chmod +x "$HOME/bin/codex-update"
```

3. Sicherstellen, dass `~/bin` im `PATH` ist:

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

1. Neueste Version von GitHub laden:

```bash
curl -fsSL "https://raw.githubusercontent.com/lasseveenliese/codex-update/main/codex-update?ts=$(date +%s)" -o "$HOME/bin/codex-update"
```

2. Ausführbar machen:

```bash
chmod +x "$HOME/bin/codex-update"
```

3. Aktualisierung prüfen:

```bash
codex-update --help
```

## Skriptablauf

1. Prüft benötigte Tools (`codex`, `npm`, `grep`, `head`, `tr`)
2. Liest die installierte Version via `codex --version`
3. Liest die neueste Version via `npm view @openai/codex version`
4. Vergleicht beide Versionen
5. Führt bei verfügbarem Update `npm i -g @openai/codex` aus (mit Rückfrage oder `-y`)

## Rückgabecodes

- `0`: erfolgreich / bereits aktuell
- `1`: Update abgelehnt oder ungültige Eingabe bei der Rückfrage
- `2`: Fehler (z. B. fehlende Abhängigkeit oder Parse-Fehler)

## Hinweise

- Das Update verwendet eine globale npm-Installation und kann je nach npm-Konfiguration erhöhte Rechte benötigen.
- In nicht-interaktiven Shells ohne `-y` beendet sich das Skript mit Fehler, um hängende Jobs zu vermeiden.

## Deinstallation

### Benutzerlokal

```bash
rm -f "$HOME/bin/codex-update"
```
