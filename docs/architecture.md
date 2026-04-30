# Architecture

Spotify Deck is a KDE Plasma applet packaged as a KPackage.

## Components

`metadata.json`

Defines the applet ID, display name, Plasma API version, entrypoint, and applet category.

`main.qml`

Owns application state. It imports KDE's MPRIS model:

```qml
import org.kde.plasma.private.mpris as Mpris
```

The active player is exposed as:

```qml
mpris2Model.currentPlayer
```

The widget reads media state through properties such as:

```qml
player.track
player.artist
player.playbackStatus
player.canGoNext
player.volume
```

Playback controls are local DBus/MPRIS method calls:

```qml
player.Play()
player.Pause()
player.Previous()
player.Next()
player.changeVolume(delta, true)
```

The explicit Spotify window button calls a static local launch command:

```qml
executable.connectSource("spotify")
```

That command is not assembled from user input or media metadata.

`SpotifyDeck.qml`

Owns presentation. It receives state through the `root` applet object and renders labels, wave bars, playback buttons, volume controls, open-app button, and the security badge.

## Data Flow

```text
Spotify desktop client
  -> MPRIS service on the session bus
  -> KDE Mpris2Model
  -> main.qml readonly properties
  -> SpotifyDeck.qml UI bindings
```

Controls flow back in the opposite direction:

```text
button click
  -> main.qml function
  -> MPRIS method call
  -> Spotify desktop client
```

## Why MPRIS Instead of Spotify Web API

MPRIS is already available on Linux desktops, works locally, and avoids OAuth. For a desktop widget, that is a better default security boundary than asking for cloud API access.
