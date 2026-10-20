# Minimap Cleared

Removes the black grid lines from the minimap, for a clean map view.

A mod for Baldur's Gate 3 (Patch 8), made with game version 4.1.1.7398727. This README is for developers. What
the mod does, how to install it and its compatibility are in [openmods.md](openmods.md), the mod page on
[openmods](https://openmods.nexus).

## How it works

- `GUI/Library/Lib_Keyboard.xaml` (keyboard and mouse) and `GUI/Library/Lib_Controller.xaml` (controller) hold the
  same style for the map control `ls:LSWorldMap`. The style has no key, so it replaces the game's style for that
  control.
- The grid comes from the fog shader of the map: `ls:ShroudBrushShader` with `GridOpacity="0.0"` draws no grid lines.
  All other parts of the style stay as they are.
- The files keep Windows line endings (`.gitattributes`), so a build from this repository is byte-identical.

## Build

Requirements: Windows PowerShell and [LSLib](https://github.com/Norbyte/lslib/releases) (`Divine.exe`).
`build.ps1` looks for `_tools\lslib\Packed\Tools\Divine.exe` in the parent folders of the project; set `BG3_TOOLS` to
a folder that holds `lslib\` to use another place.

```powershell
powershell -ExecutionPolicy Bypass -File .\build.ps1             # dist\bg3_minimapcleared.pak
powershell -ExecutionPolicy Bypass -File .\build.ps1 -Deploy     # build, then copy the .pak to the Mods folder
powershell -ExecutionPolicy Bypass -File .\deploy.ps1 -Wait      # copy the .pak when the game is closed
```

`deploy.ps1` copies the .pak to the `Mods` folder below `BG3_DATA_DIR` (the BG3 user-data folder), else to
`%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\Mods`; `-ModsDir` overrides both. The running game locks the .pak, so
the script stops (exit code 2) or, with `-Wait`, waits until the game is closed.

## Project layout

```
src/                             Mod files (packed into the .pak)
  Mods/bg3_minimapcleared/meta.lsx   Mod metadata (UUID 17105aa7-9aca-4b2d-967e-208a6729f87d)
  Mods/bg3_minimapcleared/GUI/Library/Lib_Keyboard.xaml     Map style, keyboard and mouse
  Mods/bg3_minimapcleared/GUI/Library/Lib_Controller.xaml   Map style, controller
docs/                            Cover and screenshots for openmods.md
scripts/bump_version.py          Raises the last version digit in meta.lsx
build.ps1                        Builds dist/bg3_minimapcleared.pak from src/
deploy.ps1                       Copies the .pak to the Mods folder
openmods.json, openmods.md       Data and mod page for openmods.nexus
```

Versioning: every change raises the last version digit (1.0.0.0 -> 1.0.0.1):

```powershell
python scripts\bump_version.py           # both Version64 values in meta.lsx
python scripts\bump_version.py --show
```

## License

MIT, see [LICENSE](LICENSE).
