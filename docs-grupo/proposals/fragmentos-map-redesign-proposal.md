<!--
SPDX-FileCopyrightText: The Threadbare Authors
SPDX-License-Identifier: MPL-2.0
-->

# Proposal: los_fragmentos_del_s — Full Map Redesign (Purple-Mystic Identity)

## Intent

The quest plays correctly but is the visually plainest story quest: combat has zero atmosphere (no CanvasModulate, no music, no props), stealth is a bare gray rectangle, outro lacks music and mood. Sibling quests (eldrune, champ, shjourney) each have a distinct visual identity. This change gives `los_fragmentos_del_s` its own: a unified **purple-mystic** identity (purple/violet tints, fog, shine particles near fragments), building on the intro's existing purple CanvasModulate pulse and the mystical audio theme. Per locked user decision: full redesign — level dressing AND layout restructure (Approach B + C combined).

## Scope

### In Scope

- Redesign layout + dressing of ALL 5 scenes under `scenes/quests/story_quests/los_fragmentos_del_s/`
- Purple/violet CanvasModulate everywhere; dusk/night shaders copied into the quest folder and adapted to purple
- `fog.tscn` and `shine_particles.tscn` placement (shine near fragments)
- Configure the 4 orphan tileset atlases (bridges, decoration, fence, void_chromakey) using existing textures, and place them
- Redesign guard patrol routes (new Curve2Ds) and combat arena from scratch to fit new layouts; difficulty may change
- Background music/ambience where missing (combat, outro)

### Out of Scope

- Editing ANY file outside the quest folder — shared/other-quest assets reused by reference or copied in, never modified in place
- Gameplay-logic rewrites beyond layout-driven repositioning
- New third-party assets

## Capabilities

### New Capabilities

- `fragmentos-visual-identity`: purple-mystic standard (tint values, shaders, fog, particles, audio) applied consistently across all 5 scenes
- `fragmentos-scene-layouts`: per-scene layout/dressing requirements (intro, stealth, combat, puzzle, outro), including redesigned patrols and arena
- `fragmentos-tileset-atlases`: atlas setup for the four orphan `.tres` tilesets using existing first-party textures

### Modified Capabilities

- None.

## Approach

Phased delivery, one scene per phase in play order; each phase independently shippable and playable:

| Phase | Scene | Plan |
|---|---|---|
| 1 | 0_intro | Strengthen purple pulse, thematic prop variety, shine_particles, background dressing |
| 2 | 1_stealth | Night shader adapted to purple, fog, layout restructure using fence/decoration tilesets, new patrol Curve2Ds |
| 3 | 2_combat | Arena redesigned, purple CanvasModulate, music, elevation/decoration layers |
| 4 | 3_sequence_puzzle | Bridges tileset over water, decoration tileset, fog, shine_particles near bells |
| 5 | 4_outro | CanvasModulate mood, music, decoration layer, ambient animation |

Reuse list (from exploration): `dusk.gdshader` / `night.gdshader` (copy + adapt to purple), `scenes/game_elements/fx/fog/fog.tscn`, `shine_particles.tscn`, `clouds_shadow.tscn`, decoration props (bush, flower, rock, butterfly, tree), first-party tile PNGs for atlases, first-party music.

## Affected Areas

| Area | Impact | Description |
|---|---|---|
| `los_fragmentos_del_s/0_intro..4_outro/*.tscn` | Modified | Layout + dressing redesign |
| `los_fragmentos_del_s/tiles/*.tres` | Modified | Orphan atlases configured |
| `los_fragmentos_del_s/` (new shader copies) | New | Purple-adapted shader copies |

## Risks

| Risk | Likelihood | Mitigation |
|---|---|---|
| Patrol Curve2D coords break on new stealth layout | High | Redesign patrols with layout; playtest each phase |
| Stealth camera limits (2816×1280) vs new layers | Med | Keep bounds or update limits deliberately |
| Orphan tilesets need full atlas setup before painting | High (known) | Planned as dedicated capability |
| Third-party audio licenses | Med | Check `.license` per file; prefer `assets/first_party/music/` |
| Difficulty drift from redesigned combat/patrols | Med | Playtest; freedom is sanctioned by user decision |

## Rollback Plan

Each phase ships as one scene-scoped change; reverting its commit(s) restores the previous scene fully. No files outside the quest folder are touched, so rollback never affects other quests.

## Dependencies

- None external — all assets already in-repo.

## Success Criteria

- [ ] Each scene visually coherent with the purple-mystic identity
- [ ] Quest playable end to end after every phase
- [ ] gdlint/gdformat pre-commit passes on all touched files
- [ ] REUSE/SPDX headers intact on touched and copied files
- [ ] All four orphan tilesets configured and placed in scenes

---

## Appendix A — Tileset Inventory & Reuse Analysis

### Sources

| Source | Path | License | Purple-mystic fit | Verdict |
|---|---|---|---|---|
| Ninja Adventure | `assets/third_party/ninja_adventure/Stone_And_Sand_Tiles.png` | CC0 1.0 (public domain) | HIGH — dark stone floor | READY, already in `exterior_floors.tres` |
| First-party tiles | `assets/first_party/tiles/` (Cliff_Mines, Bridge_All, fence, void chromakey + animated void, void stars) | Project-owned (MPL-2.0) | HIGH (mines/void), MED (bridges) | Free reuse anywhere |
| Tiny-Swords | `assets/third_party/tiny-swords/Terrain/` (water, foam, shadows) | UNCLEAR — no license file; sibling folder is non-CC0 | MEDIUM | Keep existing uses; avoid expanding |
| Champ quest | `champ/tiles/assets/shore_tiles.png` | MPL-2.0 | LOW — coastal theme | Technically reusable, thematically irrelevant |

Cross-quest referencing confirmed: Godot `.tres` tilesets use absolute `res://` paths, so any quest's tileset can be referenced from this quest without modification.

### Orphan tilesets (already configured, never placed)

| Tileset | Atlas | Content | Fit |
|---|---|---|---|
| `los_fragmentos_del_s_void_chromakey.tres` | Void chromakey + 8 animated void edge sheets (14 frames @ 10fps) | Animated void borders | VERY HIGH — core narrative motif |
| `los_fragmentos_del_s_decoration.tres` | `Cliff_Mines_Decoration_Tiles.png` | Mine/dungeon props, pillars, debris | HIGH |
| `los_fragmentos_del_s_bridges.tres` | `Bridge_All.png` | Wooden walkways with collision | MEDIUM — paths over void |
| `los_fragmentos_del_s_fence.tres` | `fence.png` | Fence barriers | LOW — generic |

Note: these tilesets have full physics/terrain configuration — activation means wiring TileMapLayer nodes into scenes and painting tiles, not atlas work from scratch (earlier exploration understated their readiness).

### Recommended tilesets per scene

| Scene | Keep | Add |
|---|---|---|
| 0_intro | exterior_floors (Stone terrain, suppress Grass/Dirt) | void_chromakey for mystic boundary edges |
| 1_stealth | exterior_floors + shadows | decoration (mine props); prefer Cliff_Mines (darker) for elevation |
| 2_combat | exterior_floors + shadows | decoration + bridges — ruined stone arena with walkways over void |
| 3_sequence_puzzle | water + exterior_floors | void_chromakey animated edges around the pool; bridges |
| 4_outro | exterior_floors | elevation + void_chromakey — ruins-collapsing-into-void climax |

### Open design decision — how "purple" is achieved

No purple-tinted tile textures exist in the project. Two options:

1. **Color modulation (recommended)**: purple `modulate` on TileMapLayer nodes + CanvasModulate + adapted shaders. Zero new assets, consistent with the "no new third-party assets" non-goal, reversible per scene.
2. **New purple texture variants**: more authentic hue control, but requires authoring new art assets — out of scope for this change.

This proposal adopts option 1; the design phase will define the exact tint values per scene.
