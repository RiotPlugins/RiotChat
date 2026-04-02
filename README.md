# RiotChat

RiotChat is a lightweight, moderation-focused chat plugin for Spigot/Paper. It provides formatted chat, mentions, a persistent ignore system, clear/announce utilities, tablist headers/footers, and a persistent mute system with database backing and tab-completion for moderation commands.

This README documents features, installation (binary/JAR), configuration, database, commands, permissions and how to report issues.

---

## Features

- Formatted chat with prefix/suffix support (LuckPerms integration)
- Mentions highlighting and processing
- Private messages (`/msg`, `/reply`)
- Persistent ignore system (database-backed)
- Clear chat with optional announcement and sound
- Server-wide announcements using MiniMessage templates
- Tablist header/footer support
- Persistent mute system with DB storage:
  - `/mute <player> [duration] [reason]`
  - `/unmute <player>`
  - Mutes stored in `riotchat_mutes` table
  - Tab-completion for `/mute` (player names + durations)
- Config-driven behavior and H2/MySQL support

---

## Installation (JAR / release repository)

This repository is intended to host the plugin releases (JAR files) and does not necessarily include the source code. To install the plugin on your server:

1. Download the release JAR from this repository's Releases page or Modrinth, SpigotMC, Hangar.
2. Copy the JAR file into your Paper/Spigot server's `plugins/` directory.
3. Start or restart the server. The plugin will generate default configuration files on first run.

If a compiled JAR is not present in the repository, check the Releases tab or the project's website for binary downloads.

---

## Configuration

Main config file (generated on first run): `config.yml` in the plugin folder. Important config keys (examples):

- `enable-chat` (boolean) — Global switch for chat features.
- `chat.separator` (string) — Separator between name and message.
- `chat.name-hover` / `chat.name-click` (boolean) — Enable hover/click behaviour on player names.
- `clearchat.lines` (int) — Number of blank lines to send for `/clearchat`.
- `announce.format` (MiniMessage block) — Template used for broadcast announcements.
- `database.type` — `H2` or `MYSQL`.
- `database.h2.file` — H2 file path (without `.mv.db`).
- `database.mysql.*` — MySQL connection settings.

Example excerpt:

```yaml
database:
  type: "H2"
  h2:
    file: "plugins/RiotChat/riotchat"

clearchat:
  lines: 120
  message: |
    <dark_gray>━━━━━━━━━━━━━━━━━━━━━━━━</dark_gray>
    <gradient:#00ffff:#00bcd4><bold>CHAT CLEARED</bold></gradient>
    <gray>The chat has been cleared by <b><cyan>%player_name%</cyan></b></gray>
    <dark_gray>━━━━━━━━━━━━━━━━━━━━━━━━</dark_gray>
```

---



## Commands

- `/msg <player> <message>` — Send a private message.
- `/reply <message>` — Reply to last private message.
- `/announce <message>` — Broadcast an announcement (permission: `riotchat.announce`).
- `/clearchat [silent]` — Clears chat; `silent` suppresses broadcast (permission: `riotchat.clearchat`).
- `/togglechat` — Toggle global chat state (permission: `riotchat.chat.toggle`).
- `/ignore <player>` — Toggle ignoring a player (permission: `riotchat.ignore`).
- `/ignorelist` — Show the players you currently ignore.
- `/mute <player> [duration] [reason]` — Mute a player (permission: `riotchat.mute`).
  - Duration examples: `10s`, `5m`, `1h`, `1d`, or `perm` for permanent.
  - The plugin provides a `MuteTabCompleter` that suggests players and common durations.
- `/unmute <player>` — Unmute a player (permission: `riotchat.unmute`).

---

## Permissions

Suggested default permission nodes (adjust in your permissions system):

- `riotchat.color` — Use color codes in chat (default: op)
- `riotchat.reload` — Reload configuration (default: op)
- `riotchat.announce` — Use `/announce` (default: op)
- `riotchat.ignore` — Use ignore commands (default: op)
- `riotchat.clearchat` — Clear chat (default: op)
- `riotchat.clearchat.bypass` — Bypass clear chat effects (default: false)
- `riotchat.chat.bypass` — Bypass global chat-disabled state (default: op)
- `riotchat.chat.toggle` — Toggle chat on/off (default: op)
- `riotchat.mute` — Mute players (default: op)
- `riotchat.unmute` — Unmute players (default: op)
- `riotchat.mute.bypass` — Bypass mutes (for staff) (default: op)

---

## Troubleshooting

- DB connection errors: verify your `config.yml` database settings and reachability.

If you need further help, open an issue on the repository with server logs and configuration excerpts.

---

## Contributing & Source

This repository is intended to host plugin releases (binary JARs). It may not contain the plugin source code. If you want to contribute or request the source, please open an issue or check the project page for a link to the source repository. Pull requests against compiled-release repositories are not accepted.

---

## License

This project is provided under the MIT License by default. 

---

## Changelog (short)

- v0.1 — Core chat formatting, ignore, messaging, announce, clear-chat, tablist
- v0.2 — Persistent mute system, `/mute`/`/unmute`, `MuteTabCompleter`
