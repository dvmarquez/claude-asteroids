# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Asteroids clone in plain HTML5 Canvas + vanilla JS. No dependencies, no bundler, no package.json, no tests, no linter. README and UI text are in Spanish.

## Running

Open `index.html` directly in a browser, or serve statically (`npx serve .` → http://localhost:3000). There is no build step; edit and reload.

## Architecture

Everything lives in `game.js` (loaded by `index.html` via a plain `<script>`, canvas fixed at 800×600 → `W`/`H` constants).

- **Entities** are classes (`Bullet`, `Asteroid`, `Ship`, `Particle`), each with its own `update(dt)` / `draw()`. Game state is module-level globals (`ship, bullets, asteroids, particles, score, lives, level, state, deadTimer`).
- **State machine**: `state` is `'playing' | 'dead' | 'gameover'`; `initGame()` resets, `nextLevel()` spawns the next wave, `killShip()` handles life loss and respawn timer.
- **Loop**: `loop(ts)` computes `dt` from `requestAnimationFrame`, calls `update(dt)` then `draw()`. Movement is dt-based (seconds).
- **Input**: `keys` (held) and `justPressed` (edge-triggered, consumed via `pressed(code)`) — use `pressed()` for one-shot actions like restart.
- **World is toroidal**: positions wrap with the `wrap(v, max)` helper; keep this in mind for collisions and spawning.
- **Asteroid sizes** are indexed 1–3 through the parallel arrays `RADII`, `SPEEDS`, `POINTS` (index 0 unused). Destroying a size-N asteroid splits it into size N−1.
