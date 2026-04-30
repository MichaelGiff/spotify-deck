# Security Policy

## Supported Scope

This project is a local KDE Plasma widget. Security review focuses on local desktop behavior, DBus usage, command execution, and handling of media metadata supplied by local MPRIS players.

## Reporting Issues

For a public GitHub repository, open a private vulnerability report if enabled. Otherwise, open an issue with:

- affected version or commit
- KDE Plasma version
- Linux distribution
- reproduction steps
- expected versus actual behavior
- any relevant logs from `journalctl --user`

Do not include Spotify credentials, cookies, OAuth tokens, or private listening history in reports.

## Current Security Posture

- The widget does not call the Spotify Web API.
- The widget does not store secrets.
- The widget does not persist song metadata.
- The widget uses KDE's local MPRIS model for playback status and controls.
- The only executable action is a static `spotify` launch command when no player is available.
- The explicit open-app button uses the same static `spotify` launch command.
- The volume slider controls only the active MPRIS player volume, not arbitrary shell or network state.

## Known Tradeoff

The `spotify` launch command is intentionally static and does not include user-supplied arguments. This keeps command injection risk low while preserving the convenience of opening Spotify from the play button or open-app button.
