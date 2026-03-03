# Pokemon Icons for Notion

This repo hosts processed Pokémon box sprites that can be used as page icons in Notion databases.

## The Goal

All Pokémon team member pages in Notion should have **big, consistent, centered** page icons—and they should not be cut off by cropping.

## Current State (2026-03-03)

Each game row uses sprites from its own generation's art style:
- **Games 1-9 + Staraptor** (gen7x): [pokesprite](https://github.com/msikma/pokesprite) `pokemon-gen7x/regular/` — SM/USUM-era pixel art (68×56)
- **Games 10-11 minus Staraptor** (gen8): pokesprite `pokemon-gen8/regular/` — SwSh-era pixel art (68×56)
- **Game 12** (gen9/bamq): [bamq/pokemon-sprites](https://github.com/bamq/pokemon-sprites) `pokemon/regular/` — SV-era pixel art (68×56)

**Note:** Staraptor is in Game 11 (Legends Arceus) but uses the gen7x sprite source since it's a Gen 4 Pokémon — it groups with games 1-9 for processing.

### Current Processing Pipeline

All sprites: trim transparent padding → fixed nearest-neighbor scale per group → centered on 280×280 transparent canvas → losslessly compressed with pingo (-s4).

- **Gen7x group** (57 sprites): 7x scale (max trimmed sprite: 37×30 → 259×210)
- **Gen8+ group** (17 sprites): 5x scale (max trimmed sprite: 47×42 → 235×210)

Pixel size is consistent within each group, preserving natural size differences between Pokémon.

### What Didn't Work (Tried 2026-03-03)

1. **Auto-crop + scale-to-fit 112px on 128×128** — gave each sprite a different pixel size (small Pokémon scaled more than large ones). Toxtricity looked wonky.
2. **Raw 68×56 originals, no processing** — gen8+ sprites looked fine but gen7x sprites were too small for Notion icons.
3. **Raw 68×56 centered on 128×128** — still too small.
4. **Fixed 4x scale WITHOUT trimming on 280×280** — too much transparent padding in the source sprites made everything tiny.
5. **Fixed 5x scale WITH trimming on 280×280 (all sprites same scale)** — still too small because the gen8+ sprites (max 47px trimmed) limited the scale factor for everyone.
6. **Per-group scaling (7x/5x) WITH trimming on 280×280** — current approach, still a bit small but getting closer.

### Cache Busting for Notion

Notion caches external icon URLs aggressively. To force refresh after pushing new sprites, re-set all icons via the API with `?v=<timestamp>` appended to the URL. This makes Notion treat it as a new URL and re-fetch.

## Sprite Sources

| Source           | Repo                   | Directory                | Dimensions | Games                |
| ---------------- | ---------------------- | ------------------------ | ---------- | -------------------- |
| pokesprite gen7x | `msikma/pokesprite`    | `pokemon-gen7x/regular/` | 68×56      | Games 1-9 (Gens 1-7) |
| pokesprite gen8  | `msikma/pokesprite`    | `pokemon-gen8/regular/`  | 68×56      | Games 10-11 (Gen 8)  |
| bamq             | `bamq/pokemon-sprites` | `pokemon/regular/`       | 68×56      | Game 12 (Gen 9)      |

### Pokesprite Slug Mappings (Where Name ≠ Base Slug):

```
Giratina              → giratina         (base = Altered form)
Lycanroc (Midday)     → lycanroc         (base = Midday form)
Mimikyu               → mimikyu          (base = Disguised form)
Aegislash             → aegislash        (base = Shield form)
Toxtricity (Amped)    → toxtricity       (base = Amped form)
White Kyurem          → kyurem-white
Zygarde (10%)         → zygarde-10
Necrozma (Dawn Wings) → necrozma-dawn
Hisuian Typhlosion    → typhlosion-hisui
Hisuian Zoroark       → zoroark-hisui
Hisuian Arcanine      → arcanine-hisui
```
