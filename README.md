# T7 LuaForge

T7 LuaForge is a one-click Lua debug builder for Call of Duty: Black Ops III.
It compiles and links BO3 mods while preserving the source paths, line numbers,
and function names needed for readable T7Patch Lua stack traces.

## Features

- Finds buildable mods in the BO3 `mods` directory.
- Supports `core_mod`, `mp_mod`, `cp_mod`, and `zm_mod` zones.
- Compiles referenced Lua source into BO3-compatible bytecode.
- Produces `.luadebug` and `.luacallstackdb` symbol sidecars.
- Updates the BO3 asset database and runs the official Mod Tools linker.
- Links English by default, with an option to link every supported language.
- Restores the original zone source after every successful or failed build.
- Embeds the debug Lua compiler, so releases require only one executable.

## Requirements

- Call of Duty: Black Ops III
- Black Ops III Mod Tools
- [L3akMod](https://dtzxporter.com/tools/l3akmod)
- [T7Patch](https://github.com/Scroptss/T7Patch/releases) v3.07 or later

T7 LuaForge does not include or install the game, BO3 Mod Tools, L3akMod, or
T7Patch.

## Installation

1. Download `T7LuaForge.exe` from the latest release.
2. Place it in the Call of Duty: Black Ops III directory beside
   `BlackOps3.exe`.
3. Make sure the BO3 Mod Tools and L3akMod are installed.

A typical installation path is:

```text
C:\Program Files (x86)\Steam\steamapps\common\Call of Duty Black Ops III\
```

## Usage

1. Open `T7LuaForge.exe`.
2. Select a mod from the list.
3. Enable **Link all languages** if the mod requires localized builds.
4. Click **Compile Lua + Link Mod**.
5. Read the build log for the result.

LuaForge examines the selected mod's zone files for Lua `rawfile` entries. For
each referenced source file, it creates:

```text
example.lua              Original source
example.luac             BO3-compatible compiled bytecode
example.luadebug         Full compiler debug information
example.luacallstackdb   T7Patch stack-trace symbol map
```

Generated files remain in the mod directory so T7Patch can resolve them at
runtime. Existing generated files with the same names are replaced by the new
build.

## Zone handling and recovery

LuaForge temporarily changes Lua entries in each selected `.zone` file to the
compiled bytecode and its symbol sidecars. It restores the exact original zone
file after linking, even if compilation or linking fails.

During a build, the original bytes are stored in a temporary
`.t7debugbuilder.bak` file. If LuaForge or Windows closes unexpectedly, the
backup is recovered automatically the next time that mod is built.

## Troubleshooting

### No buildable mods were found

The mod must be under `Black Ops III\mods` and contain at least one of these
files in its `zone_source` directory:

```text
core_mod.zone
mp_mod.zone
cp_mod.zone
zm_mod.zone
```

### Referenced Lua source was not found

A zone may reference `example.luac`, but LuaForge still needs the corresponding
`example.lua` source file in the same location to create a debug build.

### L3akMod is reported as missing

Install L3akMod into the BO3 `bin` directory according to its documentation.
LuaForge checks for its `libtiff64r.dll` linker injection before building.

### The linker failed

Review the linker output in the LuaForge log. Ordinary missing assets, invalid
zone entries, and Mod Tools configuration errors must be fixed in the mod just
as they would when using the official launcher.

## Credits

Built with and inspired by:

- Jake-NotTheMuss — HKSC HavokScript compiler
- Lua.org / PUC-Rio — Lua 5.1
- The D3V Team (DTZxPorter, SE2Dev, Nukem) — L3akMod
- Treyarch Games and the ModLauncher contributors — BO3 Mod Tools build workflow
- shiversoftdev / Alyssa — original T7Patch
- JariKCoding — CoDLuaDecompiler
- Katalash and jam1garner — DSLuaDecompiler
- DTZxPorter and Scobalula — Lua tooling and research
- CommandGenius, SensitiveWebUser, SashaPrawn, Luisete2105,
  InvoxiPlayGames, and momo5502 — T7Patch contributions
