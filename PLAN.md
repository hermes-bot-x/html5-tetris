# Implementation Plan: HTML5 Tetris

## Deliverables
1. `index.html` — self-contained entry; game split into `style.css` + `tetris.js` for cleanliness (no build step, playable via `file://` or any static server).
2. `README.md` — controls, how to play, GitHub Pages URL placeholder.
3. `.gitignore` — OS/editor junk (`.DS_Store`, `Thumbs.db`, `*.swp`, `.idea/`, `.vscode/`).
4. Do NOT push to GitHub (Lead publishes).

## Architecture (vanilla JS, functional style)
- **Pure core (`tetris.js`)** — no DOM access:
  - `TETROMINOES`: 7 piece definitions (shape matrices + colors).
  - `rotate(piece, dir)`, `move(piece, dx, dy)`: return new immutable piece objects.
  - `collides(board, piece)`: boundary + overlap checks (pure).
  - `merge(board, piece)`: returns new board with piece locked in.
  - `clearLines(board)`: returns `{ board, cleared }`.
  - `scoreFor(lines, level)`, `gravityInterval(level)`: scoring/speed tables (level up every 10 lines).
- **Mutable shell (game loop only)**: single `state = { board, piece, next, score, lines, level, status }` mutated in `update()`; rendered by pure draw functions.
- **Game loop**: `requestAnimationFrame` with gravity accumulator + simple lock-on-land.
- **Renderer**: `<canvas>` board (10×20), side panel canvas for next-piece preview, DOM text for score/level/lines.

## Steps
1. **Scaffold**: `index.html` shell (canvas + HUD + controls legend), `style.css` dark responsive theme (flex layout, board centered, HUD side panel; stacks on narrow screens).
2. **Core logic**: implement and mentally trace pure functions (rotation via matrix transpose, wall-kick retry at offsets −1/+1/−2/+2 on rotation).
3. **Loop & mechanics**: spawn from `next`, gravity by level speed, soft drop (down hold accelerates), hard drop (space, instant lock + bonus points), pause (P), restart (R) — restart also on game-over screen.
4. **Rendering**: grid, ghost-friendly tinted cells, next preview, HUD numbers; pause/game-over overlays.
5. **Input**: `keydown` for arrows/WASD/space/P/R with `repeat` handling for horizontal move & soft drop; prevent default scrolling on arrows/space.
6. **Touch controls (nice-to-have)**: on-screen buttons (◀ ▶ ▼ rotate ⤓ drop ⏸) shown only on coarse pointers via CSS media query, wired to same action functions.
7. **Docs**: `README.md` (controls table, run instructions, Pages URL placeholder), `.gitignore`.

## Requirements checklist
- [ ] 10×20 well, 7 tetrominoes, rotation with basic wall kick
- [ ] Soft drop, hard drop, lock on land
- [ ] Score / level / lines; speed increases per level
- [ ] Next-piece preview
- [ ] Arrows + WASD, space hard drop, P pause, R restart
- [ ] Touch buttons
- [ ] Dark responsive UI, controls legend visible on page
- [ ] Pure functions for rotation/collision; minimal mutable state outside loop
- [ ] No frameworks/build tools, no secrets

## Verification
- Open `index.html` in browser: no console errors; full drop → line clear → score/level updates; hard drop locks instantly; pause/restart work; layout holds on mobile-width viewport.
