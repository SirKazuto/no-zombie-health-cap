# No Zombie Health Cap

**BO7 Zombies mod for Kronos — Made by Kazuto**

Removes the zombie health cap from Black Ops 7 Zombies. Zombie health keeps scaling with rounds using the game's own formula, instead of stopping at round 55.


## Versions

| Version | Rule |
|---|---|
| **v2** (latest) | No cap for regular enemies. Elites, specials and bosses stop at their round-55 health |
| **v1** | No cap for every enemy |

### v2 — no cap
Zombie · Armored zombies · Fire zombies (Agni) · Ravager · Toxic zombie · Frost zombie · Rad-Hound · Hellhound · Deathspinner (Rex Infernus spiders)

### v2 — capped at round 55
Every other enemy: elites (Abomination, Amalgam, Zursa, UBER Klaus, O.S.C.A.R., Loom), specials (Mimic, Necropincer, Ghoul, Orion, Vermin, Parasite) and all HVT versions. Their health stops at the round-55 value, using the Mister Peeks Mayhem health table and cap, in every mode.

Bosses have fixed health and are unchanged in both versions.

## Health comparison (solo)

| Enemy | Round 100: Stock | v1 | v2 | Round 999: Stock | v1 | v2 |
|---|---|---|---|---|---|---|
| Normal Zombie | 50,000 | 125,169 | 125,169 | 50,000 | 1,622,903 | 1,622,903 |
| Armored (Heavy) | 75,000 | 205,465 | 205,465 | 75,000 | 2,794,585 | 2,794,585 |
| Ravager | 17,500 | 43,587 | 43,587 | 17,500 | 560,512 | 560,512 |
| Rad-Hound | 20,000 | 50,975 | 50,975 | 20,000 | 662,295 | 662,295 |
| Deathspinner | 37,500 | 97,260 | 97,260 | 37,500 | 1,283,940 | 1,283,940 |
| Mimic | 300,000 | 735,471 | 306,936 | 300,000 | 9,296,648 | 306,936 |
| Necropincer | 325,000 | 772,586 | 326,051 | 325,000 | 9,693,363 | 326,051 |
| Abomination | 1,000,000 | 2,020,164 | 1,000,000 | 1,000,000 | 22,247,664 | 1,000,000 |
| Warden (boss) | 2,400,000 | 2,400,000 | 2,400,000 | 2,400,000 | 2,400,000 | 2,400,000 |

Every release includes a spreadsheet with the health of 49 enemy types from round 1 to 999.

## What is not changed

- Rounds 1–54 are identical to stock
- Weapon damage, multipliers, critical/headshot damage, penetration, ammo and reload
- Zombie count, spawn rate, movement speed, attack speed and damage, AI and special mechanics

## Supported modes

| Mode | Status |
|---|---|
| Standard | Tested |
| Cursed | Tested |
| Mister Peeks Mayhem | Tested |
| Survival | Tested |
| Exfil | Tested |
| Other Zombies modes | Supported, not tested |
| Dead Ops Arcade | Not supported (separate health system) |

## Install


### Manual
Download `mod.json` and `zm_no_health_cap.gsc` from the [latest release](https://github.com/SirKazuto/no-zombie-health-cap/releases/latest) and place them here:

```
cod25\kronos\mods\no_health_cap\mod.json
cod25\kronos\mods\no_health_cap\scripts\zm_no_health_cap.gsc
```

Then enable the mod in the Mods menu. Use only one version at a time.

## How it works

Zombie health in BO7 is calculated in `scripts\mp\health_scaling_round.gsc` (`get_starting_health`). Each enemy type has a health table with a base value, per-round increases in round brackets, and a maximum. After the calculation, the game clamps the result to that maximum.

The game reaches this function through a shared function slot (`#"zombie" / #"health_scaling_get_starting_health"`). The mod registers its own copy of the function in that slot. The copy is line for line the stock code (same tables, same Cursed table replacements, same per-player scaling, same health scale dvar) with the clamp removed (v1), or replaced by the round-55 rule for capped enemies (v2).

The game's second health lookup (`health_scaling_get_subclass_health`) is left untouched. It is used to size damage from traps, equipment and some abilities, so all damage values stay stock.

A safety limit of 2,147,483,647 (32-bit maximum) prevents overflow. A normal zombie would need about round 1,289,000 to reach it.

## Settings (v2)

Edit the `SETTINGS` section at the top of `zm_no_health_cap.gsc`:

- `zhc_uncapped_list()` — enemies with no cap
- `zhc_capped_round()` — round where capped enemies stop (default 55)
- `zhc_supported_mode()` — modes the mod runs in

Editing the file changes its hash; a Workshop install will then refuse it. Use a manual install for edited files.

## Known limitations

- The **laser trap** and **Klaus' melee / ground smash** deal damage based on capped normal-zombie health, so they need more hits after round 55.
- Uncapped enemies keep stock weapon damage, so they take more hits to kill at high rounds.
- Other mods that replace the same shared function will conflict.

## Credits

- **Kazuto** — mod author
- **Nova v2** — idea (requested the mod)
- **MeowRaK** — concept and design details

This code was AI-assisted.
