# RareSpawns
 
[![Build Plugin](https://github.com/Valorless/RareSpawns/actions/workflows/maven.yml/badge.svg?branch=main)]()
[![GitHub Downloads (all assets, all releases)](https://img.shields.io/github/downloads/valorless/RareSpawnsAPI/total?label=Github%20Downloads)](https://github.com/Valorless/RareSpawnsAPI/releases)
[![](https://img.shields.io/github/v/release/valorless/RareSpawnsAPI?label=Latest%20Release)](https://github.com/Valorless/RareSpawnsAPI/releases)
[![Java](https://img.shields.io/badge/Java-21+-orange)](#)

[![](https://img.shields.io/badge/Versions-%201.21%20--%201.21.10%2B-brightgreen?style=flat)](#)
[![](https://img.shields.io/spiget/tested-versions/110420?label=Tested%20Version)](#)
[![](https://img.shields.io/badge/Requires-ValorlessUtils-red?style=flat)](https://www.spigotmc.org/resources/valorlessutils-1-18-1-21-10.109586/)
[![](https://img.shields.io/github/issues/valorless/RareSpawnsLite?label=Issues)](#)
[![](https://img.shields.io/bstats/servers/27928?label=Servers)](https://bstats.org/plugin/bukkit/RareSpawns/27928)

Lite: 
[![](https://img.shields.io/spiget/downloads/110420?label=Downloads)](#)
[![](https://img.shields.io/spiget/rating/110420?label=Rating)](#)
[![](https://img.shields.io/spiget/download-size/110420?label=Size)](#)
[![](https://img.shields.io/spiget/version/110420?label=Version)](#) <-- HavenBags Placeholder

Premium: 
[![](https://img.shields.io/spiget/downloads/110420?label=Downloads)](#)
[![](https://img.shields.io/spiget/rating/110420?label=Rating)](#)
[![](https://img.shields.io/spiget/download-size/110420?label=Size)](#)
[![](https://img.shields.io/spiget/version/110420?label=Version)](#) <-- HavenBags Placeholder

RareSpawns is a Spigot/Paper plugin that adds fully configurable, data‑driven “rare” entity spawns with unique loot, visual/audio feedback, abilities, and item progression systems. It also provides a standalone custom item framework so you can create collectible, command‑only, or utility items independent of entity spawns.

---

<details>
<summary><strong>Table of Contents</strong></summary>

- [Highlights](#highlights)
- [For Server Owners (Users)](#for-server-owners-users)
  - [Feature Overview](#feature-overview)
  - [Installation](#installation)
  - [Basic Configuration Flow](#basic-configuration-flow)
  - [Commands & Permissions](#commands--permissions)
  - [Rare Entity Definitions](#rare-entity-definitions)
  - [Abilities (Built‑in)](#abilities-builtin)
  - [Soul Harvester & Soul Powers](#soul-harvester--soul-powers)
  - [Power Items](#power-items)
  - [Integrations](#integrations)
  - [Protection / Safety Hooks](#protection--safety-hooks)
  - [PlaceholderAPI](#placeholderapi)
  - [Opt‑out System](#optout-system)
  - [Performance Notes](#performance-notes)
  - [Troubleshooting](#troubleshooting)
- [For Developers](#for-developers)
  - [API Surface (Quick Reference)](#api-surface-quick-reference)
  - [Events](#events)
  - [Runtime Abilities & Soul Powers](#runtime-abilities--soul-powers)
  - [Item & Entity Data Model](#item--entity-data-model)
  - [Spawn Pipeline](#spawn-pipeline)
  - [Extending & Integrating](#extending--integrating)
  - [Building from Source](#building-from-source)
  - [Contributing](#contributing)
  - [Roadmap / Ideas](#roadmap--ideas)
- [License](#license)

</details>

---

## Highlights

- Data‑driven rare entity system (YAML) with spawn groups, weights, biome/time/weather filters.
- Standalone custom items (loot, keys, utilities) with progression features ([Soul Harvester](https://github.com/Valorless/RareSpawnsLite/wiki/soulharvestercomponent)).
- Upgrade chain system: attributes, enchantments, model data, lore/name, item model swaps, powers.
- Player‑facing Soul Powers (runtime compiled Java) unlocked through item progression.
- Ability system for rare entities (packaged + extensible runtime compilation).
- Power Items (BREAK, REDSTONE, TROWEL) enabling block manipulation or placement mechanics.
- Integration: WorldGuard, GriefPrevention, mcMMO, Nexo / ItemsAdder / Oraxen (soft).
- Lightweight Java API for detection, ID resolution, programmatic spawns, listings.
- Opt‑out command for players who do not want rares spawning near them.
- Minimal dependencies; only requires Java 21+, a JDK if you use runtime compilation features, and [ValorlessUtils](https://github.com/Valorless/ValorlessUtils/releases).

---

## For Server Owners & Players

### Feature Overview

| Feature | Summary |
|--------|---------|
| Rare Entities | Weighted spawn selection; configurable loot, attributes, effects, messages, boss bars, minions/abilities. |
| Abilities | Pre‑made behaviors (summon minions, defensive phases, AoE attacks) referenced by ID in YAML. |
| Custom Items | Independent item definitions for loot, commands, or utility tools. |
| Soul Harvester | Items gather “souls” from configured actions to unlock upgrade chains and powers. |
| Soul Powers | Player triggers (onAttack, onDefence, onKill, onEquip) granting effects when unlocked. |
| Power Items | BREAKER, REDSTONE, TROWEL actions with configurable block lists, delays, sounds, permissions. |
| Integrations | Support for custom items/blocks from Nexo, ItemsAdder, Oraxen; respects protection plugins. |
| Opt‑out | Players can disable nearby rare spawns via a command. |
| API | Simple static helpers for other plugins and scripts. |

### Installation

1. Download the plugin JAR and place it in `plugins/`.
2. (Optional) Install integrations: PlaceholderAPI, WorldGuard, GriefPrevention, mcMMO, Nexo/ItemsAdder/Oraxen.
3. Start server; plugin creates default config, templates, and example entity/item files.
4. Review wiki: [RareSpawns Wiki](https://github.com/Valorless/RareSpawnsLite/wiki).

### Basic Configuration Flow

1. Define spawn groups and globals in `config.yml`.
2. Create or copy entity templates in `plugins/RareSpawns/entities/`.
3. Assign weights, spawn-group, conditions (biomes, time, weather).
4. Add drops, equipment, abilities, messages.
5. Create custom items in `plugins/RareSpawns/items/`.
6. (Optional) Add `soul-harvester` section for progression features.
7. Reload with `/rarespawns reload` or restart server.
8. Monitor console logs for successful loads or errors.

### Commands & Permissions

| Command | Description | Permission |
|---------|-------------|------------|
| `/rarespawns reload` | Reload configuration & content (abilities/items/entities). | `rarespawns.reload` |
| `/rarespawns item <id> [random]` | Give a custom item to self with fixed or random stats. | `rarespawns.item` |
| `/rarespawns spawn <id> [player]` | Spawn a rare entity at self/target player. | `rarespawns.spawn` |
| `/rarespawns kill <radius>` | Kill rares within radius around sender. | `rarespawns.kill` |
| `/rarespawns optout` | Toggle rare spawn opt‑out. | `rarespawns.optout` |

### Rare Entity Definitions

Key sections inside an entity YAML (see `entities/template.yml`):
- `weight`, `spawn-group`
- `conditions` (biomes, whitelist/blacklist, time ranges, weather)
- `type`, `health`, `effects`, `attributes`
- `items` (equipment)
- `drop-table` (supports chance, ranges, namespaced IDs for external items)
- `abilities` (list of ability IDs with optional per-use cooldown override)
- Presentation: `name`, `nameplate`, `bossbar`, `spawn-message`, `death-message`
- Misc flags: aggro-range, can-despawn, anti-stuck, visibility, variant-specific settings.

### Abilities (Built‑in)

Use `abilities:` list with IDs such as:
- `spawnbrood` (summon spiders)
- `callhive` (summon angry bees)
- `curseundead` (apply slowness/fire)
- `frostedfur` (freeze/slowness on attackers)
- `hardenediron` (projectile immunity phase)
- `nightcloak` (night invisibility)
- `thornmaw` (evoker fang lines)
- `undyingcommander` (persistent minion squad + damage transfer)

Syntax:
```yaml
abilities:
  - spawnbrood
  - thornmaw:45
```

### Soul Harvester & Soul Powers

- Soul Harvester: item section `soul-harvester:` defines gain method (KILL, MINING, DEFENCE, FISHING, BUILDING), upgrade chain with thresholds & chance.
- Upgrade types: ATTRIBUTE, ENCHANTMENT, POWER, MODELDATA, ITEMMODEL, ITEMNAME, DISPLAYNAME, LORE, LOREADD.
- Soul Powers: unlocked via `POWER` upgrade; runtime player effects with triggers & cooldowns.
- Requires JDK for runtime compilation if you add custom Soul Powers.

### Power Items

`poweritem:` section (per item) supporting:
- `type`: BREAKER | REDSTONE | TROWEL
- Block allow lists, world filters, consumable flag, sounds.
- TROWEL: hotbar block random selection, delay & durability cost.
- BREAKER: optional drops & ominous state checks.

### Integrations

| Plugin | What RareSpawns Uses |
|--------|----------------------|
| Nexo / ItemsAdder / Oraxen | Namespaced item & block IDs, paper note-block data placement (TROWEL). |
| WorldGuard | Build permission checks before PowerItem placement / soulstone spawn. |
| GriefPrevention | Claim “Build” trust checks. |
| mcMMO | Marks blocks placed by PowerItems as ineligible for skill exploitation. |
| PlaceholderAPI | Parses external placeholders (currently no native RareSpawns placeholders). |

### Protection / Safety Hooks

Before performing actions (place blocks, spawn via soulstones, use PowerItems) RareSpawns checks registered protection hooks. Actions abort silently if protections fail.

### PlaceholderAPI

- Supported if installed.
- Currently RareSpawns does not expose its own placeholders; it only parses third‑party placeholders embedded in messages or UI text.

### Opt‑out System

Players can run `/rarespawns optout` to toggle whether natural rare spawns consider them. If opted out, spawn replacement near that player is skipped.

### Performance Notes

- Spawn evaluation runs on natural creature spawn events; lightweight filters reduce overhead.
- Minion & ability tasks are periodic; prefer modest counts to avoid entity spam.
- Use staging server to test large upgrade chains or custom code (abilities / powers).
- Runtime compilation only occurs on reload/start for powers/abilities.

### Troubleshooting

| Issue | Check |
|-------|-------|
| Rares not spawning | Spawn group chance, conditions (biome/time/weather), weight values, opt‑out state. |
| Ability not firing | Correct ID, cooldown not active, rare has a target, no console errors. |
| Soul upgrades not applying | Souls count >= threshold, world whitelist/blacklist, chance <100 may need retries. |
| PowerItem inactive | Correct permission, world allowed, block list matches, delay not too high. |
| Placeholder not parsing | PlaceholderAPI installed & expansion present; RareSpawns only parses external placeholders. |
| Custom power/ability fails | Running a JDK (not JRE), correct filename/class name, no package declaration, server log diagnostics. |

---

## For Developers

### API Surface (Quick Reference)
I recommend reading the [API Wiki](https://github.com/Valorless/RareSpawnsLite/wiki/API)

Static methods (see full docs at [API Docs](https://valorless.github.io/RareSpawns)):

```java
// Detection & IDs
RareSpawnsAPI.isRare(entity);
RareSpawnsAPI.isRareItem(itemStack);
RareSpawnsAPI.getRareID(entity);
RareSpawnsAPI.getRareID(itemStack);

// Items
RareSpawnsAPI.getItem(id, randomize);
RareSpawnsAPI.getItemData(id);
RareSpawnsAPI.getItemIds();

// Rares
RareSpawnsAPI.getRare(id);
RareSpawnsAPI.getRareIds();
RareSpawnsAPI.spawnRare(id, location);

// Abilities
RareSpawnsAPI.getAbilityIds();

// Plugin handle
RareSpawnsAPI.getInstance();
```

### Events

Synchronous Bukkit events you can listen to:

| Event | Purpose |
|-------|---------|
| `RareSpawnEvent` | Fired when a rare entity is spawned (includes `SpawnCause`). |
| `RareUpdateEvent` | Fired when a rare is first tracked or refreshed. |
| `RareDeathEvent` | Fired when a rare dies; includes killer, drops, command kill flag. |

### Runtime Abilities & Soul Powers

- Implement interfaces (`Ability`, `SoulPower`) & annotate (`@AbilityInfo`, `@SoulPowerInfo`).
- Place source in data folders (`abilities/` or `soulpowers/`) default package, no package line.
- Requires JDK; server compiles them asynchronously on reload/start.
- Soul Powers: triggers (`onAttack`, `onDefence`, `onKill`) with chance, cooldown, failCooldown semantics.

### Item & Entity Data Model

- `ItemData` and `EntityData` resolve parsed YAML into runtime properties.
- Persistent tags stored in the item/entity via PDC for upgrades, minion flags, souls.
- Upgrade application writes UUID markers & soul power flags.

### Spawn Pipeline

1. Natural spawn event intercepted.
2. Filters: spawn reason NATURAL, not swimming/in water, nearest player (opt‑out check).
3. Iterate configured spawn groups, roll chance.
4. Select entity by weight; verify conditions (biome/time/weather).
5. Replace vanilla spawn with rare; apply data & metadata (nameplate, bossbar, attributes).
6. Fire `RareSpawnEvent`.

### Extending & Integrating

- Use RareSpawns API to detect rares & apply custom logic (e.g., specialized rewards).
- Add third‑party item sources (namespaced IDs) to drop tables.
- Wrap Soul Harvester progression for custom reward gating.
- Introduce new runtime powers or abilities for dynamic effects (avoid blocking operations).

### Contributing

Suggestions & issues welcome:
- Propose new upgrade types or powers.
- Submit performance profiling or integration requests.
- When contributing runtime features ensure non-blocking logic (schedule heavy tasks async).

### Roadmap / Ideas

- Native RareSpawns PlaceholderAPI placeholders.
- Additional Soul Power triggers (onEquip refinement, onBlockPlace, onFish success).
- Configurable AI profiles.
- Extended upgrade actions: particle trails, timed buffs, conditional triggers.
- Web-based config viewer/export. (More like wishful thinking atm)

---

## License

License will be added / clarified (TBD). Until formal license is published, treat source as all-rights-reserved; do not redistribute compiled builds without permission.

---

For full documentation, examples, and integration guides visit the [Docs Site](https://valorless.github.io/RareSpawns) & [Wiki Page](https://github.com/Valorless/RareSpawnsLite/wiki). Feel free to open issues for feature requests or clarification.

Enjoy crafting your world’s rare encounters!
