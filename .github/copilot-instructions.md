# Copilot instructions for carambolas

## Project overview

This repository is a small browser game built as a single static HTML page. The app does not use a framework, bundler, or package manager. The entire gameplay experience is defined in `index.html`, including:

- the canvas element and page layout
- CSS for the game screen
- all JavaScript game state and rendering logic
- keyboard and mouse event handlers
- the animation loop that advances the game

The game is a classic breakout clone: a ball bounces around the canvas, a paddle is moved by keyboard/mouse input, and a grid of bricks is destroyed as collisions are detected.

## Commands

There is no build pipeline, test runner, or lint configuration in this repository.

For local development, serve the page from the repository root with:

```bash
cd /workspaces/carambolas
python3 -m http.server 8000
```

Then open `http://localhost:8000` in a browser to play or test the game.

If the repo later gains project tooling, prefer the minimal existing command set over introducing new automation. For now, the practical verification step is opening the page in a browser and exercising the gameplay loop directly.

## Architecture and code flow

The app is intentionally simple and centered on a single file:

- `index.html` contains the full game.
- The canvas context is obtained once with `document.getElementById("gameCanvas")` and `canvas.getContext("2d")`.
- Game state is stored in top-level variables such as `x`, `y`, `dx`, `dy`, `paddleX`, `score`, and `lives`.
- Brick layout is modeled as a 2D array (`bricks[c][r]`) where each brick has a `status` flag.
- Input handlers (`keydown`, `keyup`, `mousemove`) update controller state that is read in the main animation loop.
- `draw()` is the central render/update function: it clears the canvas, draws the bricks/ball/paddle/HUD, checks collisions, updates positions, and schedules the next frame with `requestAnimationFrame(draw)`.

This repo follows a classic imperative canvas-game pattern rather than component-driven UI or data-flow architecture.

## Conventions specific to this codebase

- Keep the project lightweight: do not add a framework, module bundler, or package.json unless the project requirements explicitly change.
- Preserve the current single-file structure unless there is a clear need to split sections for a larger feature.
- Match the existing imperative style: global state variables, direct canvas drawing calls, and a frame loop are the baseline pattern.
- The codebase already uses Portuguese identifiers and comments in places (for example `ballRadius`, `drawBricks`, `paddleX`, and UI text like `Pontos` / `Vidas`). Follow that naming and commentary style when editing existing logic.
- Treat the canvas coordinate system as the source of truth: positions and collision checks are based on pixel arithmetic and the current frame state, not on DOM layout.
- When making gameplay changes, validate by running the page locally and checking the affected interaction (ball motion, collisions, win/lose state, and input responsiveness).

## Scope for changes

Most changes should remain within `index.html` unless a new gameplay feature clearly requires additional files. Favor surgical edits that preserve the current game loop and state model instead of introducing abstractions that do not fit the repo's existing architecture.
