# VOID RUNNER: INFINITY — Changelog

A single-file HTML5/WebGL space sim. All updates, newest first.
The same list is available in-game from the start screen (**CHANGELOG** button).

## 1.8 · FIXES (current)
- Fixed the **ENGAGE JUMP** button (duplicate element id shadowed the handler).
- Richer planetary **colour** + per-channel terrain **texture**.
- Enemies are **no longer bullet sponges** (~8 hits to kill).
- Added this in-game changelog.

## 1.7 · SURFACE+
- Impact **craters** with raised rims.
- Winding **canyon / ravine**.
- Walk-in **arched cave roofs**.
- Scattered **boulders**.
- Terrain **texturing**: grain speckle, strata bands, rocky slopes.

## 1.6 · SURFACE FIX
- Collide against the **rendered terrain mesh** (no more floor clipping).
- Smaller 2200×2200 patch with a finer 40 m grid.
- Camera raycast tests against the mesh.

## 1.5 · MOVE FIX
- Corrected surface **movement direction** (was mirrored).
- Right-side drag orbits the camera.

## 1.4 · DOGFIGHT
- Banked dogfight AI: **pursue / attack / break / regroup**.
- Lead-intercept aiming and target separation.
- **Raycast** camera collision on the surface.

## 1.3 · NO-CLIP
- Player stands on slopes via **footprint ground sampling**.
- Camera kept above the terrain.

## 1.2 · SURFACE 2.0
- Terrain **minimap** with ship + sample markers.
- **Points of interest** (ore / crash / camp / ruins) with rewards.
- Look-drag camera, sprint, surface stats HUD.

## 1.1 · LANDING
- Land on **planets** and walk the surface.
- **Procedurally generated** terrain per planet.
- Board the ship and **launch** back to orbit.

## 1.0 · UI / TARGETS
- **Target list** with type tabs (hostile / trader / asteroid / station / planet).
- **Solid filled shapes** replace wireframes.
- New **button control deck**.
- Reticle placed on the true fire axis.

## 0.9 · VFX
- **Solid-colour particle system**.
- Muzzle flashes, impact sparks, shield flares.
- Explosions, smoke and shockwave rings.
- Engine trails; opaque rendering.

## 0.8 · MINING & TRADE
- **Mine asteroids** for ore / ice / metals / rare / gems.
- **Trade ship-to-ship** with haulers.
- **Contract board**: delivery, bounty, mining.
- 12 commodities.

## 0.7 · TARGETING
- Switchable contacts, cycle + tap-radar selection.
- **Lock-on** with lead indicator.
- Target brackets (on/off screen).
- Multiple hostiles; fixed orientation math.

## 0.6 · GALAXY
- **Procedural galaxy** of star systems.
- **Hyperspace warp** with fuel.
- System types: inhabited / derelict / pirate / empty.
- Per-system economies and persistence.

## 0.5 · COMBAT & COLLISION
- Ship-vs-world **collision** with damage.
- **Swept** projectile hit detection.
- Flight-assist speed cap, heat cap, roll controls.
- Death/respawn and enemy waves.

## 0.4 · UNDOCK
- Ship launches facing away from the station.

## 0.1 · CORE
- Single-file WebGL space sim.
- 6DOF flight with flight-assist toggle.
- Seeded star system, HUD and radar.
- Combat vs AI, docking and station market.
- Procedural audio + haptics.
