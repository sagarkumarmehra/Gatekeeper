# Gatekeeper

A small tap-only memory game for kids aged 9 to 12. You play a lone samurai guarding a village gate at night: watch where raiders will sneak in, place your defenses from memory, then catch the disguised oni hiding in a line of returning villagers.

## What's in this folder

- `DESIGN.md` — the full design spec. Start here.
- `mockups/gate-screen.html` — a visual mockup of the game screen. Open it in a browser to see the layout and art direction. The characters in it are rough SVG placeholders, not final art.
- `assets/` — drop your character art here (see below).

## The art plan

You make the characters in Canva or an AI image generator and export them as PNGs with transparent backgrounds. They go in `assets/`. The game code positions and animates them, so swapping in better art never means rewriting code.

See the asset table and the prompt starters in `DESIGN.md` for the full list and suggested generation prompts. The short version of what's needed:

- hero samurai
- oni (disguised, a few variants)
- 2 to 3 honest villagers
- a guard/defense token
- a night background (moon, Fuji, torii, fog)

## Status

Design is approved. Next step is the implementation plan, then the actual game build.
