# Eliocraft Releases

Public distribution hub for the Eliocraft game client and launcher.
Compiled binaries are attached to GitHub Releases; this repository holds only
distribution metadata, including the `client-versions.json` catalog the launcher
uses to find and install clients. There is no game source here.

## What you get

- **Client** — the self-contained Windows x64 game, published per version as
  `Eliocraft-<version>-win-x64.zip` plus a `.sha256` checksum.
- **Launcher** — the stable entry point that downloads and installs the correct
  client for your network protocol line. Published as
  `Eliocraft-Launcher-<version>-win-x64.zip` plus a `.sha256` checksum.
- **`client-versions.json`** — the catalog mapping each network protocol line to
  the newest compatible client patch release.

## Quick start

1. Download the latest **launcher** from the
   [Releases](https://github.com/stoxello/Eliocraft-Releases/releases) page.
2. Run it. The launcher reads `client-versions.json`, downloads the newest
   client for your protocol line, and starts the game.

Or download a specific **client** ZIP directly and run `Eliocraft.exe`.

## System requirements

- **Windows x64**. Client and launcher releases are self-contained; no .NET
  runtime is required.
- A GPU capable of OpenGL 3.0+ (MonoGame/DesktopGL).

## The launcher

The launcher is the stable entry point for versioned clients:

- Files live under `%LOCALAPPDATA%\Eliocraft`:
  `versions\<version>\` for extracted clients, `launcher-cache\` for cached
  catalog data and downloads.
- It supports direct connection with handoff arguments, e.g.:
  `Eliocraft.Launcher.exe --protocol 1.8 --connect play.example.com:25599`
  (`--version` is an alias for `--protocol`; `--manifest` accepts an HTTPS or
  `file:` catalog URL).
- When a client is incompatible with a selected server, the launcher starts the
  correct version and the game hands back to it automatically.

 
 

## Verifying downloads

Each release asset has a companion `*.sha256` file. Verify with PowerShell:

```powershell
Get-FileHash .\Eliocraft-1.x.x-win-x64.zip -Algorithm SHA256
```

or on Linux/macOS:

```bash
sha256sum Eliocraft-1.x.x-win-x64.zip
```

## Versioning

- Client releases use `client-vX.Y.Z` tags. The tag must match the compiled game
  version. Protocol-breaking changes bump the minor (channel) number; patches
  stay within a channel. Do not replace an existing client asset — publish a new
  patch tag and update the catalog entry instead.
- Launcher releases use independent `launcher-vX.Y.Z` tags.
- The game server is distributed separately in
  [Eliocraft Server](https://github.com/stoxello/Eliocraft-Server).
