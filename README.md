# Quartermaster

Quartermaster keeps everything the Battle.net client needs on Linux supplied and up to date, then launches it.

On every start it:

1. Updates itself from the latest GitHub release.
2. Installs or updates to the latest GE-Proton, which ships its own Wine build — nothing is required from your distribution.
3. Installs or updates the Steam Linux Runtime.
4. Installs the Battle.net client if it is missing, otherwise starts it.

Everything is self-contained: GE-Proton, the Steam Linux Runtime and the Battle.net prefix live inside the Quartermaster install directory and are run from there.
Steam, system Wine and other Proton installations are never used or touched.

All binaries are downloaded from their upstream origin only — no third-party mirrors or repackaged builds:

| Component        | Source                                                                 |
|------------------|------------------------------------------------------------------------|
| Quartermaster    | [GitHub releases](https://github.com/SirBisgaard/Quartermaster/releases) |
| GE-Proton        | [GloriousEggroll/proton-ge-custom releases](https://github.com/GloriousEggroll/proton-ge-custom/releases) |
| Steam Linux Runtime | [Valve's runtime images](https://repo.steampowered.com/steamrt4/images/latest-public-stable/) |
| Battle.net       | Official installer from [Blizzard](https://download.battle.net)        |

## Installation

Quartermaster is installed for the current user only, and does not need root.

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

## Usage

Start Quartermaster from the desktop shortcut, or run:

```sh
quartermaster
```

It checks for updates, brings everything up to date and then installs or starts Battle.net.

> **GNOME users:** GNOME has no system tray by default. Install the
> [AppIndicator and KStatusNotifierItem Support](https://extensions.gnome.org/extension/615/appindicator-support/)
> extension to show the Battle.net tray icon in the top bar.

## Uninstall

Uninstalling removes everything Quartermaster has installed: the install directory with GE-Proton, the Battle.net prefix and all games installed through it, and the desktop shortcut.

> **Warning:** Games installed through Battle.net are deleted as well. Back up anything you want to keep before uninstalling.

Battle.net or games installed to a location outside the install directory, such as another disk, are not removed.

Run the install script with the same options used when installing, plus `--uninstall`:

```sh
curl -fsSL https://github.com/SirBisgaard/Quartermaster/releases/latest/download/install.sh | sh -s -- --uninstall
```

If you installed to a custom directory, add `--install-dir <path>`.

## Requirements

| Tool                | Used for                                         | Package                              |
|---------------------|--------------------------------------------------|--------------------------------------|
| `sh`                | Running Quartermaster                            | Any POSIX shell                      |
| `curl`              | Downloading releases                             | `curl`                               |
| `tar`, `gzip`, `xz` | Unpacking GE-Proton and the Steam Linux Runtime  | `tar`, `gzip`, `xz-utils` (`xz` on Fedora and Arch) |
| `sha256sum`, `sha512sum` | Verifying downloads                         | `coreutils`                          |

A GPU driver with Vulkan support, including 32-bit libraries, is also required.

The Steam Linux Runtime needs unprivileged user namespaces, which are enabled by default on most distributions.

## Disclaimer

Quartermaster is not associated with Blizzard Entertainment in any way, and is not endorsed or supported by them.
It exists only to make it simple to enjoy Blizzard's games on Linux.

Quartermaster will be maintained until Blizzard releases a native Linux client.

Battle.net is a trademark of Blizzard Entertainment, Inc.
