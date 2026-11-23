# What stays the same between versions

MealKit stays a 0.x beta. Across PATCH releases (0.9.x to 0.9.y) it keeps three things stable, so a game can update the engine without rewriting code or files. A MINOR release (0.9 to 0.10) may change them, and then the upgrade tool moves your project forward. The full list, with each format's version, is {engine}`docs/CONTRACTS.md`.

## Game code

- The engine headers a game may include are listed in {engine}`engine/public_headers.txt`. A game that builds on one version builds on later PATCH releases of the same MINOR after a rebuild.
- Other engine headers are internal and may change. `tools\check_public_headers.ps1 -Path <your project>\source` tells you if your game includes one.
- Game libraries are always rebuilt for a new engine version; only the source is kept compatible.

## Files

- Project files (`mealkit.project`, tuning, tables, scenes, prefabs, `.ui`, string tables, effects, scatter layers, scenarios) keep reading in every PATCH release. A newer engine may add keys; an older engine reading a newer file skips the keys it doesn't know and warns.
- Files on players' PCs stay readable too: saves, `settings.ini` and the keys a player changed.

## Simulation rules

- The determinism rules and SimCheck's errors stay as they are. A rule added later only warns, so a PATCH update never stops your game from building.
- The same inputs give the same results only within one simulation version. An engine update may change the results (a physics fix, for example), so replays play back only on the version that recorded them, and players can only join each other on the same version.
