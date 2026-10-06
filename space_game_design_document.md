# Project VOID RUNNER: INFINITY
## High-Concept Game Design Document & Technical Architecture Spec
**Genre:** Mobile Open-World Space Flight & Trading Simulator (Single-File HTML5/WebGL)  
**Inspirations:** *Elite Dangerous*, *Escape Velocity*, *FTL*, *Daggerfall* (scale & procedural generation)  
**Target Platform:** Mobile Web / PWA (Standalone Single `.html` File, Offline Capable)

---

## 1. Executive Summary & Vision Statement

### 1.1 The Elevator Pitch
**VOID RUNNER: INFINITY** is a 100% client-side, zero-dependency space simulator packaged entirely inside a **single, portable `.html` file**. It translates the vast, cold realism and open-ended agency of *Elite Dangerous* into an ultra-accessible, mobile-first experience—while expanding beyond traditional space sims through modular EVA boarding actions, dynamic derelict salvage dungeons, a living simulated economy, faction territory wars, and base/station construction.

### 1.2 Core Pillars
1. **Zero Asset Burden:** Procedural generation of entire star systems, textures, planet meshes, audio waveforms, and NPC dialogues inside under 500 KB of pure HTML, CSS, and Vanilla JavaScript/WebGL.
2. **Infinite Freedom:** No forced path. Be a solitary long-haul deep-core miner, a bounty hunter tracking procedural pirate captains across light-years, a corporate insider conducting corporate sabotage, or an explorer charting derelicts outside the known jump network.
3. **Tactile Mobile Ergonomics:** Native thumb-zone ergonomics—split virtual radial thruster sticks, contextual touch-pip controls, gyroscope-assisted pitch/roll micro-adjustments, and gesture-driven planetary scanning.
4. **Persistent Offline Galaxy:** Deterministic pseudo-random seed algorithms ensure that billions of star systems exist identically across devices, saved locally via `IndexedDB` and `localStorage`.

---

## 2. Gameplay Systems (Expanded Beyond Elite)

While *Elite Dangerous* excels at ship flight and scale, *VOID RUNNER* layers deeper RPG and emergent simulation mechanics on top of flight mechanics:

### 2.1 Spaceflight & Combat Engine
* **Dual Flight Models:** Toggle between **Flight Assist On** (arcade-accessible vector steering suited for quick one-thumb commutes) and **Flight Assist Off** (pure Newtonian physics with angular inertia, drift-strafing, and reverse-thrust dogfighting).
* **Target Subsystem Targeting:** Precision fire can blow out drive thrusters, disrupt shield capacitor links, fry hyperdrive coils, or breach cargo hatches to siphon canisters into the void.
* **Electronic Warfare & Stealth:** Manage heat emissions and radar cross-section. Power down non-essential systems (Silent Running) to slip undetected through pirate patrols or authority scans around black-market orbital docks.

### 2.2 Planetary Atmospheric Flight & Surface Operations
* **Seamless Planetary Descent:** Real-time atmospheric entry heat dynamics transitioning from orbital space down to low-altitude terrain flight.
* **Surface Recon & Mineral Harvesters:** Deploy an automated Surface Rover or deploy remote drone beacons over high-yield tectonic deposits.
* **Hazardous Atmospheric Anomalies:** Lightning storms that scramble HUD avionics, corrosive atmospheres requiring thermal shielding upgrades, and gas giant skimming for tritium fuel.

### 2.3 Derelict Exploration & EVA "Dungeon Crawling"
* Unlike *Elite*, players can dock directly with dead alien vessels, destroyed battlecruisers, and abandoned research stations.
* **Seamless Mini-Engine Mode:** Transitions into a top-down, real-time tactical interior exploration mode.
* **Survival Horror Elements:** Manage your oxygen reserves, laser cutter battery, and thermal balance while navigating pitch-black corridors infested with malfunctioning security drones, biomechanical parasites, and ancient encrypted data terminals.

### 2.4 Simulated Dynamic Living Economy
* **Supply-Demand Macro-Cycles:** Stations produce and consume goods based on population, faction conflicts, famines, industrial strikes, and pirate blockades.
* **Market Manipulation:** Dump thousands of tons of rare metals to crash a local market, or blockade cargo haulers heading to an industrial refinery to spike copper and titanium prices.
* **Corporate Stock & Futures:** Purchase bonds and equity in galactic megacorporations, benefiting from territory expansions or sabotaging rival shipping lanes to maximize dividend payouts.

### 2.5 Dynamic Nemesis & Faction System
* Named pirate captains, bounty hunters, and corporate executives track your actions. Escaped targets remember you, upgrade their ships, recruit escorts, and ambush you in hyperspace transit.
* **Territory Conquest:** Align with one of four primary factions (e.g., Sol Directorate, Outer Rim Syndicate, The Ascendant Covenant, Free Miners Coalition) to sway system security levels and orbital dominance.

### 2.6 Modular Outpost & Orbital Station Construction
* Deposit raw mined ore and industrial fabrication modules at unmapped Lagrange points.
* Construct customized refueling depots, automated defense turrets, refit drydocks, and hydroponics domes that generate passive income and repair services for visiting AI traders.

---

## 3. Mobile UI/UX & Touch Controls

```
+--------------------------------------------------------------+
| [NAV/SYS]  [Heat: 23%]    [Shields: 100%]    [Cargo: 42/80]   |
| Target: Corsair Mk.II     Dist: 1.4km         Subsys: Drives  |
|                                                              |
|                  +-----------------------+                   |
|                  |       HUD RETICLE     |                   |
|                  |     (Target Vector)   |                   |
|                  +-----------------------+                   |
|                                                              |
|   (LEFT THUMB ZONE)                     (RIGHT THUMB ZONE)   |
|   +---------------+                     +----------------+   |
|   | Virtual Stick |                     | Primary Fire   |   |
|   | Pitch / Yaw   |                     | Secondary Fire |   |
|   | (Floating)    |                     | Boost / Drift  |   |
|   +---------------+                     +----------------+   |
|                                                              |
| [THROTTLE SLIDER]   [POWER PIPS: SYS/ENG/WEP]   [HYPERDRIVE] |
+--------------------------------------------------------------+
```

### 3.1 Touch Control Layout
* **Adaptive Dual Virtual Controls:**
  * **Left Side:** Dynamic floating joystick (appears wherever the thumb touches) for Pitch and Yaw.
  * **Right Side:** Contextual action diamond (Primary Laser, Secondary Railgun/Missile, Afterburner, Flight Assist toggle).
* **Vertical Throttle Bar (Left Edge):** Slide smoothly to set forward/reverse velocity with dedicated detents at 0%, 50% (optimal turn rate), and 100% thrust.
* **Power Distribution Pips (Bottom Center):** Tap to assign engine, system, and weapon capacitor power points (`SYS`, `ENG`, `WEP`), identical to the tactical triage of *Elite*.
* **Gyroscope Option:** Tilt-to-roll and fine-pitch calibration for combat dogfighting.

---

## 4. Single-File Architecture Specification

To fit an expansive universe inside a self-contained `.html` file that runs in any mobile browser (Safari, Chrome, Firefox) with zero network requests:

```
single_page_game.html
├── <head>
│   ├── Inline CSS (Custom Glassmorphism HUD, CRT scanline overlay, Mobile Viewport meta)
│   └── Audio Synthesis Engine (Web Audio API - FM / Subtractive synth for engine roars, SFX, laser chirps)
├── <body>
│   ├── <canvas id="glCanvas"> (WebGL 3D Context / Fast 2.5D Canvas Hybrid)
│   ├── <div id="hud-overlay"> (Responsive Touch UI, Pips, Target Radar, Comm Feed)
│   └── <script>
│       ├── Engine Core (Loop, Delta time, State Manager)
│       ├── PRNG & Seed Generator (Mulberry32 / SplitMix64)
│       ├── Procedural Universe Generator (Galaxy Map, Solar Systems, Stations)
│       ├── Physics & Collision (Verlet/Euler integration, AABB, Raycasting)
│       ├── Audio Synthesizer (Zero MP3/WAV assets; pure oscillator nodes)
│       ├── Procedural Texture & Mesh Synthesis (Ship hulls, wireframe vectors, planet shaders)
│       ├── AI State Machines (Flocking, dogfight tactics, trader routes, patrol logic)
│       ├── EVA & Station Interior Generator (Cellular automata dungeon maps)
│       └── Persistence Engine (IndexedDB auto-save & exportable base64 string)
```

### 4.1 Asset-Free Audio Architecture (Web Audio API)
* **Thrusters & Engine Rumble:** Brown noise generator connected to a low-pass filter modulated by the current throttle level.
* **Laser Blaster SFX:** Fast frequency-ramped sine/sawtooth oscillator wave ($880\text{ Hz} \to 110\text{ Hz}$) over $80\text{ ms}$.
* **Hyperspace Jump:** Frequency modulation synthesis with increasing resonance filter, culminating in a white noise sweep with spatial stereo panning.
* **Ambient Space Drone:** Dual detuned triangle waves with slow LFO modulation ($0.05\text{ Hz}$) producing deep, hypnotic sci-fi atmosphere.

### 4.2 Seed-Based Galaxy Coordinate System
Every star system is calculated deterministically from a 64-bit coordinate seed:
$$\text{SystemSeed} = \text{Hash}(x \cdot 73856093 \oplus y \cdot 19349663 \oplus z \cdot 83492791)$$
* Yields consistent:
  * Spectral star classification (O, B, A, F, G, K, M, Pulsar, Black Hole).
  * Planetary orbits, sizes, atmospheric densities, and mineral compositions.
  * Station allegiances, market commodity baseline prices, and black-market availability.

---

## 5. Technical Implementation Checklist

| Phase | Module | Scope |
| :--- | :--- | :--- |
| **01** | **Core Engine & Renderer** | WebGL context, pseudo-3D/orthographic camera projection, starfield parallax, touch input handler. |
| **02** | **Flight Dynamics & Ship Systems** | Newtonian physics, inertial dampeners, heat buildup, power pip allocation (`SYS`/`ENG`/`WEP`). |
| **03** | **Procedural Universe & Persistence** | Coordinate hashing, galaxy map viewer, orbital bodies, `IndexedDB` save state. |
| **04** | **Combat, Radar & AI** | Subsystem targeting, laser/projectile physics, enemy pursuit/evasion state trees, 3D radar disc. |
| **05** | **Economy, Trading & Outfitting** | 24 dynamic commodities, station docking sequence, ship upgrade hangar, equipment hardpoints. |
| **06** | **EVA Derelict Salvage** | Interior grid generator, tactical touch controls, loot tables, drone security hazards. |
| **07** | **Audio & Polish** | Full Web Audio procedural sound suite, haptic vibration API integration, responsive screen scaling. |