# Gatekeeper — Design Spec

**Date:** 2026-06-16
**Status:** Approved, ready for implementation planning

## What this is

A small mobile web game for kids aged 9–12. One samurai guards a village gate at night. The player watches where raiders will sneak in, sets defenses on those spots from memory, then catches a disguised oni hiding in a line of returning villagers. The whole thing is one HTML file, played by tapping.

The game trains three things without ever stopping to quiz the player: spatial memory (the raid path), planning (placing defenses), and observation (spotting the disguise). The learning is the gameplay itself rather than a quiz layer sitting on top of it.

## Who it's for

Kids 9–12. They can read a short instruction, handle a bit of strategy, and they'll quit fast if it feels like a worksheet. The art leans into an anime samurai look because that's what lands with this age right now.

## Platform and constraints

- A single self-contained HTML file: markup, styles, and script together, no build step and no server.
- Runs by opening the file or hosting it anywhere static.
- Portrait orientation, sized for a phone, tap-only. No keyboard, no drag.
- High score saved locally in the browser (localStorage). Nothing leaves the device.

## The core loop — one "night"

Each round is one night and lasts roughly 20–30 seconds. It runs in three beats on a 4×3 grid:

1. **Watch.** Torches light up the tiles tonight's raiders will cross, then go dark after about two seconds. The player has to hold the path in memory.
2. **Defend.** The player taps the tiles they remember to place a set number of guards (start with three). Tiles placed on the real path block raiders; misses let them through.
3. **Spot & Slash.** A row of villagers walks up to the gate. One is an oni in disguise, given away by a single wrong detail (a horn, a glowing eye, an odd mask). Tapping it triggers a katana slash, a smoke poof, and a combo bump. Tapping a real villager costs honor.

Spot & Slash is the payoff beat. It's the most satisfying tap in the game, and where the juice budget goes first.

## Difficulty

The game opens almost trivially easy and tightens night after night: longer raid paths, more villagers in the line, and disguises that get harder to catch. The feeling to aim for is the near-miss, losing by a hair so the player taps "one more try." A combo meter rewards catching raiders back to back.

## Scoring and lives

- **Score** comes from path tiles covered correctly, catching the right oni, and a combo bonus for consecutive catches.
- **Village Honor** is the life bar. It drains when raiders slip through or when the player slashes an innocent villager. At zero, the night ends.
- Game over shows the score, the saved high score, and a "one more try" button.

## Feel (the juice)

This is most of why a simple game feels good, so it counts as a real requirement rather than a nice-to-have: a slash flash and screen shake on a catch, the oni bursting into smoke, score numbers popping up, and a combo counter that builds visibly. Without these the game is flat. With them even the simple loop feels alive.

## Art and assets

The look is a night scene — dusk sky, moon, Mt. Fuji silhouette, a red torii gate, drifting fog — with anime samurai characters.

**Division of labor on art:** the user produces the character art (in Canva or an AI image generator) and exports PNGs with transparent backgrounds. These drop into fixed slots in the layout. The code handles positioning, animation, and effects; the art quality is whatever the PNGs bring, with no code rework when they change.

Placeholder SVG figures ship in the meantime so the layout is testable before final art arrives.

### Asset list

| Asset | Notes |
| --- | --- |
| Hero samurai | Standing/idle at the gate; faces right toward the villagers. |
| Oni (disguised) | Looks almost like a villager, one tell-tale detail off. A few variants keep it fresh. |
| Villagers | 2–3 honest variants so the line-up isn't identical clones. |
| Guard / defense token | What gets placed on grid tiles (archer or ronin). |
| Background | Night sky, Fuji, torii, fog — can be one image or layered. |
| Effects | Slash, smoke poof — can be CSS/SVG rather than supplied art. |

PNG, transparent background, sized for phone display (roughly 2× the on-screen size for sharpness).

### Art-generation prompt starters

For whoever makes the sprites (Canva AI, or another image model):

- **Hero:** "anime-style samurai gatekeeper, full body, idle stance, red lacquer armor, topknot and headband, night palette, transparent background, game sprite, facing right"
- **Oni:** "anime-style oni demon disguised as a peasant villager, subtle horns and glowing eyes, full body, transparent background, game sprite"
- **Villager:** "anime-style Japanese peasant villager, simple kimono, friendly, full body, transparent background, game sprite"
- **Background:** "anime night sky over a village, full moon, Mount Fuji silhouette, red torii gate, drifting fog, no characters"

## Out of scope (deliberately not building)

- No accounts, login, or backend.
- No level editor or multiple worlds.
- No story mode or cutscenes beyond a one-line prompt per beat.
- No tutorial flow beyond a short on-screen hint.
- No multiplayer.

## Open items for implementation planning

- Exact timings: how long torches stay lit per night, how long villagers take to cross.
- Exact difficulty curve numbers (path length and villager count per night).
- Whether sound is in the first version or added later.
