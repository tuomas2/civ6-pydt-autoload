# PYDT Autoload — Civ 6 Mod

Automatically loads the most recent PYDT (Play Your Damn Turn) hotseat save when you start Civilization VI. No clicking through menus — go straight to playing your turn.

## How it works

1. When the main menu loads, the mod queries for hotseat saves
2. If a save with "PYDT" in the filename is found and it is less than 60 seconds old, it loads automatically
3. Autoload only triggers once per game session — returning to the main menu won't re-trigger it
4. If no fresh PYDT save is found, the game starts normally

## Installation

1. Copy the mod folder to your Civ 6 Mods directory:
   - **Windows (Steam):** `Documents\My Games\Sid Meier's Civilization VI\Mods\PYDTAutoload\`
   - **Windows (Epic):** `Documents\My Games\Sid Meier's Civilization VI (Epic)\Mods\PYDTAutoload\`
   - **Linux (native):** `~/.local/share/aspyr-media/Sid Meier's Civilization VI/Mods/PYDTAutoload/`
   - **Linux (Steam/Proton):** `~/.local/share/Steam/steamapps/compatdata/289070/pfx/drive_c/users/steamuser/Documents/My Games/Sid Meier's Civilization VI/Mods/PYDTAutoload/`
   - **macOS:** `~/Library/Application Support/Sid Meier's Civilization VI/Mods/PYDTAutoload/`

2. The mod folder should contain:
   ```
   PYDTAutoload/
   ├── PYDTAutoload.modinfo
   └── UI/
       └── FrontEnd/
           └── MainMenu.lua
   ```

3. The mod has `EnabledByDefault=1`, so it should activate automatically. If not, launch Civ 6, go to **Additional Content > Mods**, enable **PYDT Autoload**, and restart the game (FrontEnd mods require a restart).

## Usage with PYDT

1. Configure your [PYDT desktop client](https://www.playyourdamnturn.com/) to download turns automatically
2. When it's your turn, PYDT downloads the save to your Hotseat folder and (optionally) launches Civ 6
3. This mod detects the fresh PYDT save and loads it immediately — you're in the game

## Configuration

Edit `UI/FrontEnd/MainMenu.lua` and change the `PYDT_MAX_AGE_SECONDS` value (default: 60 seconds):

```lua
local PYDT_MAX_AGE_SECONDS = 60;  -- Change this to your preferred max age in seconds
```

The mod only loads saves with "PYDT" in the filename (case-insensitive). Regular hotseat saves are never auto-loaded.

## Debugging

Check `Logs/Lua.log` in your Civ 6 AppData directory for messages starting with `PYDT Autoload:`. Setting `EnableTuner 1` in `AppOptions.txt` enables more verbose logging.

Example log output when working:
```
MainMenu: PYDT Autoload: Found PYDT save '(PYDT) Play This One!.Civ6Save', age=31s
MainMenu: PYDT Autoload: Save is fresh (31s), loading automatically.
MainMenu: PYDT Autoload: Loading hotseat save...
```

## Technical notes

- The save age is calculated by converting Windows FILETIME (from `UI.GetSaveGameModificationTimeRaw`) to Unix epoch and comparing with `os.time()`. This works correctly under Wine/Proton.
- The correct save type enum is `SaveTypes.HOTSEAT` (not `HOTSEAT_MULTIPLAYER`).
- `EnabledByDefault=1` means the mod loads like a DLC — it appears in "Target Mods" in Modding.log, not "Enabled Mods". This is normal.

## Known limitations

- **Replaces MainMenu.lua** — incompatible with other mods that replace the main menu (e.g., Better FrontEnd, MPH). Since Civ 6 is no longer receiving updates, the base file should remain stable.
- **Gathering Storm required** — the bundled MainMenu.lua is from the Gathering Storm version of the game.

## License

MIT
