# AGENTS.md

## What this is
- The whole game is **`index.html`** (inline CSS + one inline `<script>` with `"use strict"`). No `package.json`, no build, no deps, no test runner, no linter.
- `CHANGELOG.md` is user-facing history; it is **mirrored by the in-game `CHANGELOG` array** in `index.html`. Update both together.
- `space_game_design_document.md` is an aspirational design doc, much larger than what is implemented. Do not treat it as the spec.

## Run / verify
- Run by opening `index.html` directly (works over `file://`). Deployed on GitHub Pages from `master`.
- No tests exist. To verify a change:
  1. **Syntax-check the inline script** by extracting it and compiling with `new Function`:
     `node -e "const fs=require('fs');const m=fs.readFileSync('index.html','utf8').match(/<script>([\s\S]*?)<\/script>/);new Function(m[1]);console.log('SYNTAX OK')"`
  2. **Behavior**: changes have been validated with a throwaway headless harness that stubs `document`/`window`/WebGL, runs the inline script through node's `vm`, then steps frames and asserts state. It is not committed — recreate it in the temp dir if you need runtime verification.

## Windows / PowerShell gotcha
- Inline `node -e "..."` containing regex, `[`, `]`, or `||` gets mangled by PowerShell. Write a temp `.js` file (e.g. under `%TEMP%`) and run `node that.js`. Same applies to one-line node scripts with array spreads.

## Architecture map (single file, roughly in order)
- **Renderer (WebGL1).** Four programs: `prog` (lines/points), `sprog` (solid flat-shaded), `tprog` (vertex-coloured + lit, used for terrain and ships), `pprog` (particles). Draw helpers: `drawMesh` (calls `useProgram` itself), `drawSolid`, `drawTerrainMesh`, `drawParticles`. Meshes are `{buf,count[,mode]}`.
- **Math.** `V` (vec3), quaternion helpers (`qRotate`, `qLookAt`, `qMul`), `m4*` matrices. **World/ship forward is local `-Z`.**
- **State machine.** `STATE` enum + `game.state`: `BOOT/FLIGHT/DOCKING/DOCKED/DEAD/WARP/MAP/TRADE/TARGETS/SURFACE`. The main loop only simulates/renders per state.
- **Universe.** `generateSystem`/`getSystem`/`loadSystem`; `current` holds galaxy coords `{gx,gz}`. Enemies/traders/market/contracts regenerate on `loadSystem`.
- **Surface.** `buildSurface` builds height grid `H` and the terrain mesh; collision/aim use `groundHeight` and `raycastTerrain`, which call `surface.ground`.

## Critical conventions & gotchas
- **`qLookAt` must stay right-handed** (`X = fwd x up`, `Z = -fwd`). It was once left-handed, which made every ship/objective face backwards.
- **Surface physics must use `surface.ground(x,z)`** (barycentric over the *drawn* triangle grid), **never** the analytic `heightAt`. Using `heightAt` caused the player to clip through the floor.
- **Keep DOM `id`s unique.** `getElementById` returns the first match; a duplicate `id` silently bound the wrong handler (this broke the ENGAGE JUMP button — the surface JUMP and galaxy button shared `id="jumpBtn"`).
- Context buttons (`HAIL`/`MINE`) toggle **`style.visibility`**, not `display`, so the button grid does not reflow.
- HUD and overlays are DOM/CSS; the 3D scene is one canvas, and the radar + surface minimap use their own 2D canvases. Opening an overlay usually sets a non-`FLIGHT` state and hides `#hud`.
- Opening the galaxy map is `STATE.MAP`, trade is `STATE.TRADE`, the target picker is `STATE.TARGETS`; none of these run the flight update loop.

## Generation & persistence
- All procedural content is deterministic: `mulberry32(hashSeed(x,y,z))` seeded by `GALAXY_SEED = 0x5EEDC0DE`. Changing generation logic or the hash invalidates every saved world and the "identical across devices" guarantee.
- Save = `localStorage['voidrunner_save_v1']`, storing only `credits`, `cargo`, `fuel`, `gx`, `gz`. Contracts, POI progress, and trader stock are **not** persisted (they regenerate).

## Publishing
- No CI. Push to `master`; GitHub Pages rebuilds automatically (~40–100 s).
- Bump the start-screen build label (the `BUILD ...` `.hint` in the `#gate`) on each change. Pages caches HTML for ~10 min, so tell users to open with a `?v=N` query to bypass it.
