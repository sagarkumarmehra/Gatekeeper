# Gatekeeper — Implementation Plan

Build target: one file, `Games/Gatekeeper/index.html`, with all HTML, CSS, and JS inline. No build step, no dependencies. Opens by double-click or static hosting. Portrait, tap-only.

Spec: `DESIGN.md`. This plan fills in the timing and difficulty numbers that the spec left open.

## Proposed numbers (adjust during playtest)

- Grid: 4 columns x 3 rows (12 tiles).
- Guards per night: 3.
- Path length by night: starts at 3 tiles on night 1, +1 every 2 nights, capped at 8.
- Villagers in the line: starts at 3 on night 1, +1 every 2 nights, capped at 6.
- Torch reveal time: 2.2s on night 1, shrinking 0.1s per night, floor 1.0s.
- Disguise subtlety: night 1 uses an obvious tell (horns), later nights use smaller tells (eye color, mask edge).
- Village Honor: 5 points. Lose 1 per raider that crosses an uncovered path tile, lose 1 per innocent villager slashed. Honor reaching 0 ends the run.
- Combo: each consecutive correct oni catch adds +1 multiplier to the catch score; a miss resets it.

## Build phases

Each phase ends in something you can open and check.

### Phase 1 — shell and state machine
- Single HTML file, phone-framed layout, night-scene background (CSS gradient + the SVG torii/Fuji/moon/fog from the mockup).
- A state machine with phases: `title -> watch -> defend -> spot -> resolve -> (next night | gameover)`.
- HUD: Village Honor bar, night counter, score. Wired to state, no gameplay yet.
- Checkpoint: tapping through advances the phases with placeholder content.

### Phase 2 — the grid and the Watch beat
- Render the 4x3 grid with the perspective tilt from the mockup.
- Generate a random connected path of the night's length.
- Light the path tiles, hold for the reveal time, then hide.
- Checkpoint: each night shows a different path that flashes and disappears.

### Phase 3 — the Defend beat
- After the path hides, let the player tap tiles to place up to 3 guards.
- Track which placed tiles are actually on the path.
- A "ready" tap locks placement.
- Checkpoint: placements register and the count is enforced.

### Phase 4 — the Spot beat
- Spawn the villager line (placeholder SVG sprites), one randomly chosen as the oni with a disguise tell scaled to the night.
- Tap to slash. Correct oni = catch; innocent = honor loss.
- Checkpoint: the right target resolves correctly.

### Phase 5 — scoring, honor, and loop
- Resolve a night: raiders cross the path, blocked on covered tiles, honor drops for uncovered ones.
- Apply catch score and combo multiplier.
- Advance the night and ramp difficulty, or end the run at 0 honor.
- Game-over screen with score, high score (localStorage), and a "one more try" button.
- Checkpoint: a full run plays start to finish and is replayable.

### Phase 6 — juice
- Slash flash, screen shake, oni smoke poof, score pop, animated combo counter.
- Honor bar drain animation, torch flicker.
- Checkpoint: the catches feel satisfying, not flat.

### Phase 7 — art swap
- Replace placeholder SVG figures with `<img>` tags pointing at `assets/*.png`, in fixed slots.
- Graceful fallback to the SVG placeholder if a PNG is missing, so the game runs before art arrives.
- Checkpoint: dropping a PNG into `assets/` shows up in-game with no code change.

## Testing

- Manual playtest in a mobile browser (or desktop devtools phone emulation) at the end of each phase.
- Confirm the golden path: watch, place, slash, score, next night, lose, retry.
- Confirm edge cases: placing fewer than 3 guards, tapping the same tile twice, slashing nothing, honor hitting 0 mid-night.

## Open for playtest tuning

The numbers at the top are starting guesses. The near-miss difficulty curve is the thing to tune by feel once it runs.
