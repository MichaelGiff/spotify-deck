# Spotify Deck

Transparent KDE Plasma desktop widget for Spotify playback controls.

Spotify Deck displays the current song, artist, animated audio-wave bars, and previous/play-next controls. It integrates through local Linux desktop APIs instead of the Spotify Web API, so it does not need OAuth, API keys, browser cookies, or account credentials.

## Features

- KDE Plasma 6 applet
- Transparent Rockies-purple surface with black accents
- Current song and artist from MPRIS
- Previous, play/pause, and next controls
- Per-player volume slider
- Mouse-wheel volume adjustment
- Button to open the Spotify desktop app
- Animated wave bars while media is playing
- Local security posture indicator

## Install Locally

From the repo root:

```bash
./scripts/install.sh
```

Then add the widget:

1. Right-click the KDE desktop.
2. Select `Add Widgets`.
3. Search for `Spotify Deck`.
4. Drag it onto the desktop.

## Development Layout

```text
plasmoid/
  metadata.json
  contents/ui/main.qml
  contents/ui/SpotifyDeck.qml
docs/
  architecture.md
  threat-model.md
scripts/
  install.sh
```

`plasmoid/metadata.json` is the package manifest KDE reads.

`main.qml` owns the applet state and talks to KDE's MPRIS model.

`SpotifyDeck.qml` owns the visual surface, wave animation, labels, buttons, and security badge.

## Security Summary

Spotify Deck is designed as a local desktop control surface:

- No network requests from the widget
- No Spotify Web API token
- No stored credentials
- No parsing of untrusted remote input
- Playback commands are sent through the local MPRIS/DBus media interface

See [docs/threat-model.md](docs/threat-model.md) for the full security notes.

## Releases

Build a downloadable `.plasmoid` package locally:

```bash
./scripts/package.sh
```

GitHub Actions builds the same package on every push or merge to `main`. Version tags like `v0.0.1` create a GitHub Release with the `.plasmoid` file and SHA-256 checksum attached.

See [docs/release.md](docs/release.md) for the release process.

## Repository

Planned GitHub remote:

```text
https://github.com/MichaelGiff/spotify-deck
```

## Useful Commands

Check that KDE can discover the applet:

```bash
kpackagetool6 --type Plasma/Applet --show com.codex.spotifydeck
```

List installed applets matching this project:

```bash
kpackagetool6 --type Plasma/Applet --list | rg 'com.codex.spotifydeck|Spotify Deck'
```

Test in a window, when run from a normal desktop session:

```bash
plasmawindowed com.codex.spotifydeck
```
