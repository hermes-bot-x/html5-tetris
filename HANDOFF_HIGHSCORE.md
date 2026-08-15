# Lead → Coder: Tetris high-score follow-up

## Goal
Add a **localStorage high-score** feature to `/opt/data/projects/html5-tetris` without breaking existing gameplay.

## Requirements
1. Persist best score in `localStorage` (key e.g. `html5-tetris-highscore`)
2. Show high score in the UI near current score
3. Update high score when game over if current score beats it
4. Optional: small "New high score!" feedback once when beaten
5. Keep single-page / no build step
6. Functional style where practical

## Host constraints
- **No Chrome/Playwright** on this Unraid host
- Do **not** install browsers or use browser automation
- Verify with file content asserts and/or local HTTP fetch only
- Do **not** git push, create remotes, or merge to main (Lead will branch/PR)

## Definition of done
- High score displayed in UI
- localStorage read on load + write on beat
- README mentions high score briefly
- No regressions to core controls
- Write `RESULT.md` with files changed + how verified
