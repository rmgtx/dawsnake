# dawsnake

A browser snake game. One file, no dependencies, no build step.

**Play:** https://rmgtx.github.io/dawsnake/

## Controls

| Input | Action |
|---|---|
| Arrow keys / `WASD` | Steer |
| `Space` | Start · pause · restart |
| `P` | Pause |
| `R` | Restart |
| `M` | Sound on/off |
| Swipe / on-screen d-pad | Steer (touch devices) |

## Rules

Classic snake. Eat the fruit, grow by one, score one. Hitting a wall or your own
tail ends the run. The board speeds up as you grow, from ~7.6 moves/sec at the
start down to a floor of ~15.6 moves/sec so it stays playable.

Your best score persists in `localStorage`. Sound is synthesized with WebAudio
(no audio files) and is muted by default.

## Structure

Everything lives in [`index.html`](index.html) — markup, styles, and game code.
Tuning constants sit in the `CFG` object at the top of the script:

```js
var CFG = {
  grid:       21,   // cells per side
  startLen:   4,    // initial snake length
  maxBoardPx: 560,  // board never renders larger than this
  tickStart:  132,  // ms per move at score 0
  tickMin:    64,   // fastest the game will ever get
  tickStep:   2.8,  // ms shaved off per apple eaten
  bufferMax:  2,    // queued turns retained
  swipeMin:   22,   // px before a swipe counts
  deathMs:    620   // shake + flash duration
};
```

## Notes

The snake is drawn as a single stroked path with round caps and joins, so the
rounded body shape falls out of the geometry rather than being composed from
per-cell sprites. Movement is interpolated between grid positions each frame,
which is why it glides instead of snapping. Turns are buffered so fast double
taps aren't dropped, and 180° reversals are rejected rather than being treated
as instant deaths.

Design decisions for the build are recorded in
[`docs/artifacts/2026-07/decisions/snake-game-design.html`](docs/artifacts/2026-07/decisions/snake-game-design.html).
