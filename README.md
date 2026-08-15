Live demo: https://hermes-bot-x.github.io/html5-tetris/

# Tetris — HTML5

A polished classic Tetris in a **single self-contained file** ([`index.html`](index.html)).
Vanilla JavaScript + `<canvas>`, dark responsive UI, zero dependencies, no build step.

**Play it (GitHub Pages):** `https://<your-username>.github.io/html5-tetris/` *(placeholder — Lead publishes)*

## Run locally

- Easiest: open `index.html` directly in any modern browser (`file://` works).
- Or serve the folder:

  ```sh
  python3 -m http.server 8000
  # then visit http://localhost:8000
  ```

## How to play

Pieces fall into a 10×20 well. Complete horizontal lines to clear them;
every 10 lines you level up and gravity gets faster. Game ends when the
spawn area is blocked.

### Controls

| Action      | Keyboard                    | Touch (mobile)   |
|-------------|-----------------------------|------------------|
| Move        | `←` `→` or `A` `D`          | `◀` `▶` (hold to repeat) |
| Soft drop   | `↓` or `S` (hold)           | `▼` (hold)       |
| Rotate      | `↑` or `W`                  | `↻`              |
| Hard drop   | `Space`                     | `⇓`              |
| Pause       | `P` (auto-pauses on blur)   | `❚❚`             |
| Restart     | `R`                         | `❚❚` on end screens / `Restart` button |

On the **Ready** screen, press any control key or tap the board to start.

### Scoring

| Event      | Points                     |
|------------|----------------------------|
| Soft drop  | +1 per cell                |
| Hard drop  | +2 per cell                |
| 1–4 lines  | 40 / 100 / 300 / 1200 × level (classic table) |

Level increases every 10 lines; gravity interval starts at 800 ms and shrinks
by ~15% per level (floor 60 ms).

### Best score (high score)

Your best score is saved in `localStorage` under the key
`html5-tetris-highscore` (read on load, written whenever your run beats it).
An older `tetris-best` value, if present, is migrated to the new key once on
load. When a game ends with a new record, the game-over overlay shows a
"New high score!" note.

## Gameplay details

- All 7 tetrominoes drawn from a shuffled **7-bag** randomizer (fair distribution).
- Rotation with **wall kicks** (offsets −2…+2, plus upward kicks near the floor).
- **Ghost piece** shows the landing position.
- **Next-piece preview**, score/level/lines/best HUD.
- Lock on land, pause/resume overlay, game-over overlay with restart and
  "New high score!" callout when the run sets a record.

## Implementation notes

Single file, ~all in one `<script>`:

- **Pure core** (no DOM access): `rotateMatrixCW`, `movePiece`, `rotatePiece`,
  `collides`, `merge`, `clearLines`, `ghostOf`, `spawnPiece`, `shuffledBag`,
  `scoreFor`, `gravityInterval` — pieces are plain immutable objects; board
  transforms return new boards.
- **Mutable shell**: one `state` object mutated only by game actions and the
  `requestAnimationFrame` loop (gravity accumulator + delta-time clamping).
- **Renderer**: pure draw functions (`drawBoard`, `drawPreview`) on
  devicePixelRatio-aware canvases; responsive sizing via `ResizeObserver`.
- Touch buttons appear on coarse pointers (`@media (pointer: coarse)`).

No frameworks, no secrets, nothing to install.
