# Chunk Survival Releases

Public distribution hub for the Chunk Survival game client and launcher.
Compiled binaries are attached to GitHub Releases; this repository holds only
distribution metadata, including the `client-versions.json` catalog the launcher
uses to find and install clients. There is no game source here.

## What you get

- **Client** — the self-contained Windows x64 game, published per version as
  `ChunkSurvival-<version>-win-x64.zip` plus a `.sha256` checksum.
- **Launcher** — the stable entry point that downloads and installs the correct
  client for your network protocol line. Published as
  `ChunkSurvival-Launcher-<version>-win-x64.zip` plus a `.sha256` checksum.
- **`client-versions.json`** — the catalog mapping each network protocol line to
  the newest compatible client patch release.

## Quick start

1. Download the latest **launcher** from the
   [Releases](https://github.com/stoxello/ChunkSurvival-Releases/releases) page.
2. Run it. The launcher reads `client-versions.json`, downloads the newest
   client for your protocol line, and starts the game.

Or download a specific **client** ZIP directly and run `ChunkSurvival.exe`.

## System requirements

- **Windows x64**. Client and launcher releases are self-contained; no .NET
  runtime is required.
- A GPU capable of OpenGL 3.0+ (MonoGame/DesktopGL).

## The launcher

The launcher is the stable entry point for versioned clients:

- Files live under `%LOCALAPPDATA%\ChunkSurvival`:
  `versions\<version>\` for extracted clients, `launcher-cache\` for cached
  catalog data and downloads.
- It supports direct connection with handoff arguments, e.g.:
  `ChunkSurvival.Launcher.exe --protocol 1.8 --connect play.example.com:25599`
  (`--version` is an alias for `--protocol`; `--manifest` accepts an HTTPS or
  `file:` catalog URL).
- When a client is incompatible with a selected server, the launcher starts the
  correct version and the game hands back to it automatically.

## client-versions.json

```json
{
  "schemaVersion": 1,
  "latest": "1.8",
  "channels": {
    "1.8": {
      "version": "1.8.0",
      "url": "https://github.com/stoxello/ChunkSurvival-Releases/releases/download/client-v1.8.0/ChunkSurvival-1.8.0-win-x64.zip",
      "sha256Url": "https://github.com/stoxello/ChunkSurvival-Releases/releases/download/client-v1.8.0/ChunkSurvival-1.8.0-win-x64.zip.sha256",
      "executable": "ChunkSurvival.exe"
    }
  }
}
```

Each channel key is a network protocol line (`major.minor`). Server and client
require an exact protocol match; the catalog points at the newest compatible
patch release for that line. The catalog updates automatically after each client
release.

## Verifying downloads

Each release asset has a companion `*.sha256` file. Verify with PowerShell:

```powershell
Get-FileHash .\ChunkSurvival-1.8.0-win-x64.zip -Algorithm SHA256
```

or on Linux/macOS:

```bash
sha256sum ChunkSurvival-1.8.0-win-x64.zip
```

## Versioning

- Client releases use `client-vX.Y.Z` tags. The tag must match the compiled game
  version. Protocol-breaking changes bump the minor (channel) number; patches
  stay within a channel. Do not replace an existing client asset — publish a new
  patch tag and update the catalog entry instead.
- Launcher releases use independent `launcher-vX.Y.Z` tags.
- The game server is distributed separately in
  [Chunk Survival Server](https://github.com/stoxello/ChunkSurvival-Server).
