# Quartermaster

Quartermaster keeps everything the Battle.net client needs on Linux supplied and up to date, then launches it.

On every start it:

1. Updates itself from the latest GitHub release.
2. Installs or updates to the latest GE-Proton, which ships its own Wine build — nothing is required from your distribution.
3. Installs the Battle.net client if it is missing, otherwise starts it.

Everything is self-contained: GE-Proton and the Battle.net prefix live inside the Quartermaster install directory and are run from there.
Steam, system Wine and other Proton installations are never used or touched.

All binaries are downloaded from their upstream origin only — no third-party mirrors or repackaged builds:

| Component        | Source                                                                 |
|------------------|------------------------------------------------------------------------|
| Quartermaster    | [GitHub releases](https://github.com/SirBisgaard/Quartermaster/releases) |
| GE-Proton        | [GloriousEggroll/proton-ge-custom releases](https://github.com/GloriousEggroll/proton-ge-custom/releases) |
| Battle.net       | Official installer from [Blizzard](https://download.battle.net)        |

## Installation

Quartermaster is installed for the current user only, and does not need root.
It can be installed natively with the install script, or as a Flatpak.

### Native

The install script is attached to every release, so there is no need to download the source:
[install.sh](https://github.com/SirBisgaard/Quartermaster/releases/latest/download/install.sh)

```sh
curl -fsSL https://github.com/SirBisgaard/Quartermaster/releases/latest/download/install.sh | sh
```

Or download it and run it with options:

```sh
sh install.sh [options]
```

| Option                 | Description                                                      | Default                            |
|------------------------|------------------------------------------------------------------|------------------------------------|
| `--install-dir <path>` | Where Quartermaster is installed                                 | `~/.local/share/quartermaster`     |
| `--no-desktop`         | Do not create a desktop shortcut                                 | Shortcut is created                |
| `--uninstall`          | Remove Quartermaster and everything it installed                 |                                    |

The desktop shortcut is a standard `.desktop` entry, which GNOME, KDE Plasma and other freedesktop-compliant desktops pick up in their application menu.
It is placed in `~/.local/share/applications`.

### Flatpak

For those who prefer a sandboxed install. Quartermaster is not published on Flathub; the Flatpak bundle is only distributed through GitHub releases.

Download `quartermaster.flatpak` from the [latest release](https://github.com/SirBisgaard/Quartermaster/releases/latest) and install it:

```sh
flatpak install --user quartermaster.flatpak
```

## Usage

Start Quartermaster from the desktop shortcut, or run:

```sh
quartermaster
```

It checks for updates, brings everything up to date and then installs or starts Battle.net.

## Uninstall

Uninstalling removes everything Quartermaster has installed: the install directory with GE-Proton, the Battle.net prefix and all games installed through it, and the desktop shortcut.

> **Warning:** Games installed through Battle.net are deleted as well. Back up anything you want to keep before uninstalling.

### Native

Run the install script with the same options used when installing, plus `--uninstall`:

```sh
curl -fsSL https://github.com/SirBisgaard/Quartermaster/releases/latest/download/install.sh | sh -s -- --uninstall
```

If you installed to a custom directory, add `--install-dir <path>`.

### Flatpak

```sh
flatpak uninstall --user --delete-data io.github.SirBisgaard.Quartermaster
```

## Requirements

### Native

| Tool                | Used for                                         | Package                              |
|---------------------|--------------------------------------------------|--------------------------------------|
| `sh`                | Running Quartermaster                            | Any POSIX shell                      |
| `curl`              | Downloading releases                             | `curl`                               |
| `tar`, `gzip`       | Unpacking GE-Proton                              | `tar`, `gzip`                        |
| `sha256sum`, `sha512sum` | Verifying downloads                         | `coreutils`                          |
| `python3`           | Running GE-Proton                                | `python3` (`python` on Arch)         |

A GPU driver with Vulkan support, including 32-bit libraries, is also required.

### Flatpak

Only `flatpak` is required. Everything else is included in the Flatpak runtime.

## Disclaimer

Quartermaster is not associated with Blizzard Entertainment in any way, and is not endorsed or supported by them.
It exists only to make it simple to enjoy Blizzard's games on Linux.

Quartermaster will be maintained until Blizzard releases a native Linux client.

Battle.net is a trademark of Blizzard Entertainment, Inc.
