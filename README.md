# truckersmp-cli

A simple launcher for [TruckersMP][truckersmp] to play ATS or ETS2 in multiplayer.

_truckersmp-cli_ allows to download TruckersMP and handles starting TruckersMP through [Wine][wine] while supporting the Windows versions of [American Truck Simulator][steam:ats] and [Euro Truck Simulator 2][steam:ets2]. The [Windows version of Steam][steam:windows] should already be able to run in the same Wine prefix.

On Linux it's possible to install and update the game's Windows versions automatically through [SteamCMD][steam:steamcmd] and start TruckersMP through [Proton][github:proton].
While updating all running Steam processes will be stopped to prevent Steam from asking for password and guard code at Steam's next startup. A working [native Steam][repology:steam] installation is needed for starting through Proton.

## Usage options

Short option|Long option|Description
---|---|---
`-h`|`--help`|Show help
`-a`|`--ats`|Use American Truck Simulator
`-b VERSION`|`--beta VERSION`|Set game version to VERSION, useful for downgrading (e.g. `temporary_1_35`)
`-d`|`--enable-d3d11`|Use Direct3D 11 instead of OpenGL
`-e`|`--ets2`|Use Euro Truck Simulator 2 [Default if neither ATS or ETS2 are specified]
`-g DIR`|`--gamedir DIR`|Choose a different directory for the game files [Default: `$XDG_DATA_HOME/truckersmp-cli/(Game name)/data`]
`-i APPID`|`--proton-appid APPID`|Choose a different AppID for Proton (Needs an update for changes)
`-l LOG`|`--logfile LOG`|Write log into LOG, `-vv` option is recommended [Default: Empty string (only stderr)] Note: Messages from Steam/SteamCMD won't be written, only from this script (Game logs are written into `My Documents/{ETS2,ATS}MP/logs/client_*.log`)
`-m DIR`|`--moddir DIR`|Choose a different directory for the mod files [Default: `$XDG_DATA_HOME/truckersmp-cli/TruckersMP`, Fallback: `./truckersmp`]
`-n NAME`|`--account NAME`|Steam account name to use
`-o DIR`|`--protondir DIR`|Choose a different Proton directory [Default: `$XDG_DATA_HOME/truckersmp-cli/Proton`]
`-p`|`--proton`|Start the game with Proton [Default on Linux if neither Proton or Wine are specified]
`-s`|`--start`|Start the game [Default if neither start or update are specified]
`-u`|`--update`|Update the game [Default if neither start or update are specified]
`-v`|`--verbose`|Verbose output (none:error, once:info, twice or more:debug)
`-w`|`--wine`|Start the game with Wine [Default on other systems if neither Proton or Wine are specified]
`-x DIR`|`--prefixdir DIR`|Choose a different directory for the prefix [Default: `$XDG_DATA_HOME/truckersmp-cli/(Game name)/prefix`]
(Not available)|`--activate-native-d3dcompiler-47`|Activate native 64-bit `d3dcompiler_47.dll` when starting (Needed for D3D11 renderer)
(Not available)|`--disable-proton-overlay`|Disable Steam Overlay when using Proton
(Not available)|`--self-update`|Update files to the latest release and quit
(Not available)|`--singleplayer`|Start singleplayer game, useful for save editing, using/testing DXVK in singleplayer, etc.)
(Not available)|`--use-wined3d`|Use OpenGL-based D3D11 instead of DXVK when using Proton
(Not available)|`--wine-steam-dir`|Choose a directory for Windows version of Steam [Default: `C:\Program Files (x86)\Steam` in the prefix]
(Not available)|`--version`|Print version information and quit

### Proton versions and AppIDs

Version|AppID
---|---
5.0 (Default)|[1245040](https://steamdb.info/app/1245040/)
4.11|[1113280](https://steamdb.info/app/1113280/)

## Install

### From repository (recommended)

#### System repository (recommended)

If available install from your [repository][repology:truckersmp-cli]. Updates will ship through your normal system update process.

#### Python Package Index

Operation|System-wide|Per-user (recommended)
---|---|---
Installation|`pip install truckersmp-cli`|`pip install --user truckersmp-cli`
Optional dependencies|`pip install truckersmp-cli[optional]`|`pip install --user truckersmp-cli[optional]`
Update|`pip install truckersmp-cli --upgrade`|`pip install --user truckersmp-cli --upgrade`
Additional information|This usually requires root permission and can interfere with other python packages installed through your normal repository so be careful.|Make sure the binary path (e.g. `$HOME/.local/bin`) is in your `$PATH`.

### Manual download

You can get the latest pre-built release from [the release page][github:release-page] and decompress it into the desired folder. This version is capable to update itself by running `--self-update` so make sure it's placed in a folder where your user has write permissions.

## Build

1. Clone or download this repository
1. Run `make` in the main folder to build the injector executable. Bash/zsh completion files will also be generated if [genzshcomp][python:genzshcomp] is available.
1. Optional run [`setup.py`][setuptools:command-reference] to manually start the installation process.

### bash/zsh completion

If [`genzshcomp`][python:genzshcomp] is installed, `make` generates shell completion files for bash (bash-completion) and zsh. They enable tab-completion of available command-line options.

#### System-wide installation

Shell|System-wide search paths
---|---
bash|`$(pkg-config --variable=completionsdir bash-completion)` (e.g. `/usr/share/bash-completion/completions/`), `/usr/local/share/bash-completion/completions/`
zsh|`/usr/share/zsh/site-functions/`, `/usr/local/share/zsh/site-functions/`

* The bash-completion file `truckersmp-cli.bash` needs to be renamed to `truckersmp-cli`
* Debian-based systems are using the `/usr/share/zsh/vendor-completions/` directory for zsh completions

#### Per-user installation

##### bash

Copy `truckersmp-cli.bash` to `$XDG_DATA_HOME/bash-completion/completions/truckersmp-cli`.

```
$ mkdir -p "${XDG_DATA_HOME:-~/.local/share}/bash-completion/completions"
$ cp truckersmp-cli.bash "${XDG_DATA_HOME:-~/.local/share}/bash-completion/completions/truckersmp-cli"
```

##### zsh

Copy `_truckersmp-cli` to a directory that is part of `$fpath` and run `compinit`.

## Runtime dependencies

### Required

* [`python`][repology:python] in version 3.3 (released in September 2012) or later
* [`sdl2`][repology:sdl2] in x86_64
* `steam` either the [native Linux version][repology:steam] in use with Proton or the [Windows Steam][steam:windows] in use with Wine

### Optional

* `pkg_resources` (part of [`setuptools`][repology:python-setuptools]) to get the version information from the Python package
* [`vdf`][python:vdf] to automatically detect the steam account with saved credentials
* [`wine`][repology:wine] as a possible replacement to Proton

## Buildtime dependencies

### Required

* [`gcc-mingw-w64`][repology:gcc-mingw-w64] to build the injector executable
* [`make`][repology:make]

### Optional

* [`genzshcomp`][python:genzshcomp] to generate bash/zsh completions
* [`git`][repology:git] to clone this repo and help developing
* [`setuptools`][repology:python-setuptools] to run `setup.py`

## Usage examples

### Basic

#### Everything default

To just try out TruckersMP (ETS2) on Linux with nothing already downloaded you can simply run `truckersmp-cli` which will download everything needed to get you going with Proton and a native Steam installation.

#### Install and update Euro Truck Simulator 2

```
$ truckersmp-cli --ets2 --update
```

#### Start American Truck Simulator with TruckersMP

```
$ truckersmp-cli --ats --start
```

### Advanced

#### Install, update and start TruckersMP (ATS) with Proton from a custom location

```
$ truckermsp-cli --ats --update --start --gamedir "/path/to/gamedir"
```

#### Start TruckersMP (ETS2) using Wine

```
$ truckersmp-cli --ets2 --start --wine
```
Make sure that
* The Windows version of Steam is already running in the same Wine prefix **or**
* The Windows version of Steam is installed in `C:\Program Files (x86)\Steam` in the same Wine prefix **or**
* You're specifying the path to the Window version of Steam with `--wine-steam-dir`

#### Using a different prefix location

```
$ truckersmp-cli --ets2 --start --proton --prefixdir "/path/to/prefix"
$ truckersmp-cli --ets2 --start --wine --prefixdir "/path/to/prefix/pfx"
```
* While the prefix for Wine will point directly to the prefix location, Proton uses a subfolder `pfx` for the actual prefix and points to the parent folder.
* Your prefix must be 64bits, the mod is not 32bits-compatible.

## Rendering backends

### OpenGL (default)

* Stable and faster than wined3d. But slower than DXVK.
* Useful if you're not using Vulkan-capable GPU.

### Direct3D 11 (DXVK or wined3d)

* Faster than OpenGL when DXVK is used.
    * DXVK requires Vulkan support.
    * DXVK 1.4.6 or newer is needed because older versions have rendering issue. If you're using Proton, use 4.11-10 or newer.
* Windows native 64-bit `d3dcompiler_47.dll` is needed for multiplayer.
    * Without this native DLL, TruckersMP login screen will not be shown.
    * When `--activate-native-d3dcompiler-47` is specified with `-s` (`--start`), `truckersmp-cli` downloads/activates the DLL.
    * Once the DLL is activated, there's no need to specify the option again.
* Proton uses DXVK by default.
    * When using Proton, wined3d can be used by specifying `--use-wined3d`, but it's not recommended because this is slower than OpenGL.
* Used only when `-d` or `--enable-d3d11` is specified.

## Default directories

### Game data

Game|Path
---|---
ATS|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/`
ETS2|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/`

### Wineprefix

Game|Path
---|---
ATS|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/prefix/pfx/`
ETS2|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/prefix/pfx/`

### Game logs

Game|Proton|Wine
---|---|---
ATS|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/prefix/pfx/drive_c/users/steamuser/My Documents/American Truck Simulator/game.log.txt`|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/prefix/pfx/drive_c/users/(os_login_name)/My Documents/American Truck Simulator/game.log.txt`
ATSMP|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/prefix/pfx/drive_c/users/steamuser/My Documents/ATSMP/logs/`|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/prefix/pfx/drive_c/users/(os_login_name)/My Documents/ATSMP/logs/`
ETS2|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/prefix/pfx/drive_c/users/steamuser/My Documents/Euro Truck Simulator 2/game.log.txt`|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/prefix/pfx/drive_c/users/(os_login_name)/My Documents/Euro Truck Simulator 2/game.log.txt`
ETS2MP|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/prefix/pfx/drive_c/users/steamuser/My Documents/ETS2MP/logs/`|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/prefix/pfx/drive_c/users/(os_login_name)/My Documents/ETS2MP/logs/`

### Singleplayer mods and ProMods

Game|Proton|Wine
---|---|---
ATS|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/prefix/pfx/drive_c/users/steamuser/My Documents/American Truck Simulator/mod/`|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/prefix/pfx/drive_c/users/(os_login_name)/My Documents/American Truck Simulator/mod/`
ETS2|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/prefix/pfx/drive_c/users/steamuser/My Documents/Euro Truck Simulator 2/mod/`|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/prefix/pfx/drive_c/users/(os_login_name)/My Documents/Euro Truck Simulator 2/mod/`

### Season (weather) mods for ETS2MP/ATSMP

Game|Proton|Wine
---|---|---
ATS|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/prefix/pfx/drive_c/users/steamuser/My Documents/ATSMP/mod/`|`$XDG_DATA_HOME/truckersmp-cli/American Truck Simulator/prefix/pfx/drive_c/users/(os_login_name)/My Documents/ATSMP/mod/`
ETS2|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/prefix/pfx/drive_c/users/steamuser/My Documents/ETS2MP/mod/`|`$XDG_DATA_HOME/truckersmp-cli/Euro Truck Simulator 2/prefix/pfx/drive_c/users/(os_login_name)/My Documents/ETS2MP/mod/`

See [TruckersMP Knowledge Base][truckersmp:knowledge-base].

## Additional information

* If Steam is running while SteamCMD is using the same session credentials the Steam client looses all connections and asks for the password and the guard code at the next startup. This script closes all Steam processes before acting with SteamCMD so **starting an update with a shortcut out of the Steam client won't work** because Steam waits for the script to finish and the script waits for Steam to quit.

## Credits

* I was greatly inspired by mewrev's [Inject][github:inject] tool
and TheUnknownNO's unofficial [TruckersMP-Launcher][github:truckersmp-launcher].
* Amit Malik's [article][article:dll-injection] on dll injection was also a great help.
* [kakurasan][github:kakurasan] and [Lucki][github:Lucki] for the helper script.

[article:dll-injection]: http://securityxploded.com/dll-injection-and-hooking.php
[github:inject]: https://github.com/mewrev/inject
[github:kakurasan]: https://github.com/kakurasan
[github:Lucki]: https://github.com/Lucki
[github:proton]: https://github.com/ValveSoftware/Proton
[github:release-page]: https://github.com/lhark/truckersmp-cli/releases
[github:truckersmp-launcher]: https://github.com/TheUnknownNO/TruckersMP-Launcher
[python:genzshcomp]: https://github.com/hhatto/genzshcomp
[python:vdf]: https://github.com/ValvePython/vdf
[repology:gcc-mingw-w64]: https://repology.org/project/gcc-mingw-w64/versions
[repology:git]: https://repology.org/project/git/versions
[repology:make]: https://repology.org/project/make/versions
[repology:python]: https://repology.org/project/python/versions
[repology:python-setuptools]: https://repology.org/project/python:setuptools/versions
[repology:sdl2]: https://repology.org/project/sdl2/versions
[repology:steam]: https://repology.org/project/steam/versions
[repology:truckersmp-cli]: https://repology.org/project/truckersmp-cli/versions
[repology:wine]: https://repology.org/project/wine/versions
[setuptools:command-reference]: https://setuptools.readthedocs.io/en/latest/setuptools.html#command-reference
[steam:ats]: https://store.steampowered.com/app/270880/American_Truck_Simulator/
[steam:ets2]: https://store.steampowered.com/app/227300/Euro_Truck_Simulator_2/
[steam:steamcmd]: https://developer.valvesoftware.com/wiki/SteamCMD
[steam:windows]: https://steamcdn-a.akamaihd.net/client/installer/SteamSetup.exe
[truckersmp]: https://truckersmp.com/
[truckersmp:knowledge-base]: https://truckersmp.com/knowledge-base
[wine]: https://www.winehq.org/
