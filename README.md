# Stellar Death — Spacetime Relativity Simulation

A real-time, interactive WebGL simulation of the complete lifecycle of a massive star — from main-sequence hydrogen fusion through supernova, neutron star formation, black hole evolution, Hawking evaporation, and final rebirth as a planetary nebula. Built entirely in a single HTML file using Three.js and custom GLSL shaders.

![Phase: Black Hole](https://img.shields.io/badge/Phase-Black%20Hole%20(Kerr)-7c4dff)
![Three.js r128](https://img.shields.io/badge/Three.js-r128-049ef4)
![License: MIT](https://img.shields.io/badge/License-MIT-green)

---

## How It Was Generated

This simulation was built iteratively through conversation with **Claude** (Anthropic's AI assistant), developed entirely as a single self-contained HTML file with no build tools, bundlers, or external dependencies beyond the Three.js CDN.

### Development Process

1. **Initial scaffolding** — A Three.js scene was set up with a WebGL renderer, perspective camera, and orbital camera controls. The star was created as a sphere with a basic shader.

2. **Phase system architecture** — A 10-phase state machine was designed to drive the simulation through each stage of stellar evolution. Each phase has its own duration, physics parameters, visual transitions, and HUD updates.

3. **Shader development** — Every visual element uses custom GLSL vertex and fragment shaders. A shared 3D simplex noise library (Ashima/Stefan Gustavson) was injected into all shaders, providing FBM (Fractal Brownian Motion) and turbulence functions for realistic procedural surfaces.

4. **Physics integration** — Real astrophysical constants and equations were embedded into the simulation code (the `PHYS` object), driving visual parameters like accretion disk temperature profiles, Doppler beaming, gravitational redshift, and Hawking temperature.

5. **Post-processing pipeline** — A custom multi-pass bloom system was built from scratch (brightness extraction → 5-level progressive Gaussian blur → weighted composite), along with a screen-space gravitational lensing shader that projects the black hole position to UV space and applies Einstein ring deflection.

6. **Iterative refinement** — Each phase was tuned for visual accuracy and dramatic impact. Planet consumption mechanics were upgraded with FBM-driven spaghettification, tidal heating crack networks, and Keplerian debris streams. The starfield was enhanced with spectral class distribution, Milky Way band concentration, and atmospheric scintillation.

### Tech Stack

| Component | Technology |
|---|---|
| Renderer | Three.js r128 (WebGL) |
| Shaders | Custom GLSL (vertex + fragment) |
| Noise | 3D Simplex (Ashima) + FBM + Turbulence |
| Post-processing | Manual multi-pass bloom + screen-space lensing |
| UI | Pure HTML/CSS HUD overlay |
| Fonts | Bebas Neue, Chakra Petch, JetBrains Mono |
| Deployment | Single HTML file, zero build step |

---

## The 10 Phases of Stellar Death

### Phase 0 — Main Sequence Star

**Physics:** A 25 M☉ (solar mass) star fuses hydrogen into helium via the CNO cycle in its core. The core temperature is approximately 15–40 million Kelvin. Luminosity follows the mass-luminosity relation L ∝ M^3.5, meaning this star is roughly 100,000× more luminous than the Sun.

**In the simulation:** The star pulses gently with FBM-driven plasma convection cells, granulation, sunspot-like dark patches, and Claret quadratic limb darkening (u₁ ≈ 0.65). The spacetime fabric grid below shows a mild curvature well, and 8 planets orbit following Kepler's third law (ω ∝ r^−3/2).

**Key equation:** Hydrostatic equilibrium — the balance between gravitational collapse and radiation pressure that keeps the star stable.

---

### Phase 1 — Red Giant

**Physics:** When core hydrogen is exhausted, the star ignites a hydrogen-burning shell. The core contracts and heats, triggering helium fusion via the **triple-alpha process** at T ≈ 2×10⁸ K. The envelope dramatically expands. Over millions of years, the star burns through successive nuclear fuels in an onion-layer structure: H → He → C → Ne → O → Si → Fe. Each stage is shorter than the last — silicon burning to iron takes only ~1 day.

**In the simulation:** The star radius grows from 3 to 8 units, color shifts from yellow to deep red, and the corona expands. Mass drops from 25 to 15 M☉ due to stellar wind mass loss. Core temperature display cycles through each burning stage.

---

### Phase 2 — Core-Collapse Supernova

**Physics:** When the core accumulates ~1.4 M☉ of iron (the Chandrasekhar limit), fusion can no longer support it — iron fusion is endothermic. The core collapses at ~0.25c (quarter the speed of light) in under a second. At nuclear density (2.7×10¹⁴ g/cm³), the strong nuclear force halts collapse, producing a **core bounce**. A shock wave forms but stalls at ~150 km. **Neutrino-driven shock revival** (the delayed neutrino mechanism) re-energizes the shock after ~400 ms, ejecting the stellar envelope. Total neutrino energy: ~3×10⁵³ erg. Kinetic energy of ejecta: ~10⁵¹ erg (1 foe).

**In the simulation:** Three sub-phases play out: (1) rapid iron-core collapse with dimming, (2) core bounce with a blinding white flash and expanding shockwave ring, (3) envelope ejection as the star's color shifts from hot white through to blue, leaving behind a compact remnant. Screen shakes simulate the violence of the event.

**Key equation:** Iron photodisintegration — ⁵⁶Fe + γ → 13 ⁴He + 4n, absorbing 124 MeV per nucleus, which accelerates collapse.

---

### Phase 3 — Neutron Star

**Physics:** The collapsed core forms a **proto-neutron star** with mass ~1.8 M☉, radius ~12 km (constrained by NICER observations), and surface magnetic field B ~ 10¹² Gauss. It spins at millisecond periods due to angular momentum conservation (a collapsing cloud spinning slowly becomes a tiny object spinning extremely fast). The NS emits bipolar **pulsar beams** along its magnetic axis, which is tilted ~15° from the rotation axis. The interior is governed by the **Tolman-Oppenheimer-Volkoff (TOV) equation** — the general-relativistic extension of hydrostatic equilibrium. As fallback material accretes, the NS mass exceeds the **TOV limit** (~2.3 M☉), triggering further collapse.

**In the simulation:** A tiny blue-white sphere appears with rapidly rotating pulsar beam cones, dipole magnetic field lines, and a cooling temperature readout (10¹¹ K → 10⁹ K). The spin period display shows the NS spinning up. At the end, a flash signals collapse past the TOV limit.

**Key equation:** TOV equation — dP/dr = −(Gm/r²)ρ(1+P/ρc²)(1+4πr³P/mc²)/(1−2Gm/rc²)

---

### Phase 4 — Relativistic Jets

**Physics:** As the neutron star collapses to a black hole, the **Blandford-Znajek mechanism** extracts rotational energy from the spinning BH via magnetic field lines threading the event horizon. This launches bipolar relativistic jets with Lorentz factor Γ ≈ 5 (v ≈ 0.98c), half-opening angle ~11.5° (≈ 1/Γ), and synchrotron emission producing blue-shifted radiation. The jets exhibit **Kelvin-Helmholtz instabilities** at their boundaries and periodic bright **knots** from internal shocks. Frame-dragging from the Kerr metric twists the jet base.

**In the simulation:** 8,000 particles (4,000 per jet) launch from the BH poles with collimation, frame-dragging twist (Ω_FD ∝ a/r³), KH perturbation noise, and periodic knot brightening. The accretion disk simultaneously forms.

**Key equation:** Blandford-Znajek power — P_BZ = (κ/4πc) Φ²_BH Ω²_H

---

### Phase 5 — Black Hole (Kerr Metric)

**Physics:** The fully formed black hole has mass M = 10 M☉ and dimensionless spin parameter a/M = 0.7. The **Kerr metric** describes spacetime around a rotating BH, with several key radii:

- **Schwarzschild radius** r_s = 2GM/c² ≈ 29.5 km — the event horizon for a non-spinning BH
- **Photon sphere** r_ph = 3M ≈ 44.3 km — where photons orbit on unstable circular paths
- **ISCO** (innermost stable circular orbit) r_ISCO ≈ 3.6M for a=0.7 prograde — the inner edge of the accretion disk
- **Ergosphere** — the region between the event horizon and the static limit where spacetime itself is dragged along with the BH's rotation; extends to r = 2M at the equator

The **accretion disk** follows the **Novikov-Thorne** temperature profile: T(r) ∝ r^(−3/4) × [1 − √(r_ISCO/r)]^(1/4), peaking at ~5×10⁶ K. Disk material follows exact Keplerian orbits with velocity β = √(M/r) / √(1 − 3M/r). Observed radiation is modified by **relativistic Doppler beaming** (D³ intensity factor) and **gravitational redshift** (g-factor = √(1 − r_s/r)).

**In the simulation:** The accretion disk shader computes physical blackbody colors from the Novikov-Thorne temperature, applies Doppler D³ beaming (approaching side brighter, receding dimmer), gravitational redshift, and FBM turbulence for MHD instabilities and spiral density waves. The photon sphere ring flickers with Lyapunov instability. The ergosphere shell is oblate (equatorial bulge from spin) and visually rotates at the horizon angular velocity Ω_H = a/(2Mr₊). Screen-space gravitational lensing distorts the background starfield with Einstein ring deflection.

**Key equations:**
- Kerr metric: Δ = r² − 2Mr + a²
- Schwarzschild radius: r_s = 2GM/c²
- Einstein field equations: G_μν + Λg_μν = (8πG/c⁴) T_μν
- ISCO range: r_ISCO = 6M (a=0) → M (a=M)

---

### Phase 6 — Consuming Planets

**Physics:** The BH's gravitational influence causes orbiting planets to lose orbital energy and spiral inward. As a planet approaches the **Roche limit** (where tidal forces exceed the body's self-gravity), it undergoes **spaghettification** — differential gravity stretches it radially while compressing it laterally. The tidal acceleration gradient is a_tidal = 2GMd/r³, where d is the object's extent. Disrupted material forms a tidal stream that spirals into the accretion disk, heating to thousands of degrees via viscous dissipation.

**In the simulation:** Planets spiral inward one at a time following gravitational inspiral (a ∝ M/r²). As each crosses the Roche limit (r < 5 AU), its mesh is stretched via vertex shader spaghettification with FBM-driven tearing and breakup. Tidal heating produces glowing crack networks (FBM turbulence thresholded for sharp edges) that progress from deep red → orange → white-hot. Debris particle streams spiral toward the BH with Keplerian acceleration, color-ramping from planet color → heated orange → white-hot. Each planet's status updates in the HUD as "CONSUMED."

---

### Phase 7 — Hawking Evaporation

**Physics:** Stephen Hawking showed in 1974 that black holes are not perfectly black — quantum effects near the event horizon cause them to emit thermal radiation at the **Hawking temperature**: T_H = ℏc³/(8πGMk_B). For a 10 M☉ BH, T_H ≈ 6.2 nanokelvin — unimaginably cold and practically undetectable. The emitted power P ∝ 1/M² means evaporation is extraordinarily slow (t_evap ≈ 10⁷⁰ years for 10 M☉). However, as mass decreases, temperature rises and evaporation accelerates in a runaway process. Mass follows a **cubic decay law**: M(t) = M₀ × (1 − t/t_evap)^(1/3). Recent work by Wondrak et al. (2023) suggests pair production peaks at r ≈ 3M (the photon sphere), not at the horizon.

**In the simulation:** The BH visibly shrinks following the cube-root mass decay. Hawking particles emit from near the photon sphere radius, intensifying as mass drops. All GR structures (lensing ring, photon sphere, ergosphere) scale proportionally with decreasing mass. The HUD displays real-time Hawking temperature (rising as T_H ∝ 1/M) and the BH's shrinking Schwarzschild radius.

**Key equation:** T_H = ℏc³ / (8πGMk_B)

---

### Phase 8 — Black Hole Explosion

**Physics:** In the final moments of evaporation, the BH's temperature skyrockets toward ~10¹⁷ K as its mass approaches zero. At these temperatures, it emits all particle species — photons, neutrinos, electron-positron pairs, and eventually heavier particles. The final burst releases ~2×10²² joules in under a second as a gamma-ray flash. The BH ceases to exist entirely — its information content is a subject of the ongoing **black hole information paradox**.

**In the simulation:** A massive flash whites out the screen. 15,000 explosion particles burst outward in all directions. An expanding shockwave ring sweeps across the scene. Camera shake simulates the violence. The star, accretion disk, lensing effects, and all BH structures vanish.

---

### Phase 9 — Planetary Nebula

**Physics:** The combined material from the supernova ejecta, consumed planetary debris, and Hawking radiation remnants disperses into an expanding ionized shell — a **planetary nebula** (historically misnamed; not related to planets). The gas is ionized by ultraviolet radiation and emits characteristic spectral lines: O III (blue-green), H-alpha (red), N II (red), S II (red). Over thousands of years, the nebula expands and fades, seeding the interstellar medium with heavy elements that will form the next generation of stars and planets.

**In the simulation:** Six nested nebula shells expand with FBM volumetric cloud structures, each with distinct colors representing different emission lines. Dust particles drift outward. The explosion particles fade. A fog gradually tints the scene with deep cosmic hues. The simulation holds at this final state as a contemplative endpoint.

---

## Physics Constants Reference

All constants are defined in the `PHYS` object and drive the simulation's calculations:

| Constant | Value | Description |
|---|---|---|
| M_PROGENITOR | 25 M☉ | Initial stellar mass |
| T_MAIN_SEQ | 15×10⁶ K | Core temperature (main sequence) |
| T_HE_FLASH | 2×10⁸ K | Helium ignition temperature |
| T_SI_BURN | 3.5×10⁹ K | Silicon burning temperature |
| CHANDRASEKHAR | 1.44 M☉ | Maximum mass of a white dwarf |
| NS_MASS | 1.8 M☉ | Proto-neutron star mass |
| NS_RADIUS_KM | 12 km | NS radius (NICER constraint) |
| NS_B_FIELD | 10¹² G | NS magnetic field strength |
| TOV_LIMIT | 2.3 M☉ | Maximum NS mass before collapse |
| M_BH | 10 M☉ | Black hole mass |
| SPIN (a/M) | 0.7 | Kerr spin parameter |
| R_S | 29.54 km | Schwarzschild radius |
| R_PHOTON | 44.31 km | Photon sphere radius |
| R_ISCO | 53.2 km | Innermost stable circular orbit |
| EFFICIENCY (η) | 0.10 | Accretion radiative efficiency |
| T_HAWKING | 6.17×10⁻⁹ K | Hawking temperature (10 M☉) |
| T_EVAP_YR | 1.16×10⁷⁰ yr | Evaporation timescale |
| JET_GAMMA (Γ) | 5 | Jet Lorentz factor |
| JET_SPEED | 0.98c | Jet velocity |
| E_NEUTRINO | 3×10⁵³ erg | Supernova neutrino energy |

---

## Visual Techniques

### Custom GLSL Shaders

Every object in the scene uses hand-written vertex and fragment shaders. A shared noise library provides:

- **3D Simplex Noise** — Ashima/Stefan Gustavson implementation for smooth, gradient-based procedural noise
- **FBM (Fractal Brownian Motion)** — 5 octaves of layered simplex noise for multi-scale detail (clouds, plasma, terrain)
- **Turbulence** — Absolute-value FBM variant for sharper, more chaotic patterns (cracks, filaments)

### Procedural Surfaces

- **Star:** FBM plasma convection + granulation + sunspot dark patches + Claret limb darkening
- **Planets:** Per-type shaders — gas giants have banded atmospheres with storm vortices, ice giants have subtle bands, rocky planets have FBM terrain
- **Accretion disk:** Novikov-Thorne temperature → blackbody color, Doppler D³ beaming, gravitational redshift, MHD turbulence, spiral density waves
- **Nebula:** FBM volumetric clouds + turbulence filaments + color variation

### Post-Processing

- **Bloom:** 5-level progressive downsample → separable 9-tap Gaussian blur → weighted mip composite. Brightness extraction with configurable threshold.
- **Gravitational lensing:** Screen-space UV distortion based on projected BH position. Implements Einstein ring deflection (α ∝ r_s²/r), event horizon void, photon ring at 1.5× r_s, and edge darkening.

### Starfield

22,000 stars with physically-motivated properties:
- 7 spectral classes (O through M) with correct color distribution (M-type dominates at 59.7%, O-type rare at 0.3%)
- Power-law magnitude distribution (few bright, many faint)
- Milky Way band concentration (35% of stars within ±12° of galactic equator)
- Interstellar reddening for Milky Way band stars
- Multi-frequency atmospheric scintillation (twinkling)

---

## Controls

| Control | Action |
|---|---|
| **Play/Pause** | Toggle simulation time (button or Spacebar) |
| **Mouse drag** | Orbit camera |
| **Scroll / Pinch** | Zoom in/out |
| **Fabric** | Toggle spacetime curvature grid |
| **Photons** | Toggle photon wave-particle paths |
| **Lensing** | Toggle gravitational lensing ring |
| **Orbits** | Toggle orbital path lines |
| **Bloom** | Toggle HDR bloom post-processing |
| **Speed slider** | Simulation time multiplier (0.2×–5×) |
| **Wave Freq slider** | Photon wavelength visualization |
| **Bloom slider** | Bloom intensity (0–3) |
| **Phase buttons** | Skip to any phase instantly |
| **Timeline markers** | Click to jump to a phase |

---

## Running Locally

No build step required. Just open the HTML file:

```bash
# Option 1: Direct open
open stellar-death.html

# Option 2: Local server (avoids CORS issues if any)
python3 -m http.server 8000
# Then visit http://localhost:8000/stellar-death.html
```

### Requirements

- A modern browser with WebGL support (Chrome, Firefox, Edge, Safari)
- GPU recommended for smooth 60fps at full resolution
- The simulation loads Three.js r128 from the Cloudflare CDN

---

## Hosting

The simulation is a single HTML file and can be hosted anywhere that serves static files:

- **GitHub Pages** — Push to a repo, enable Pages, done
- **Netlify** — Drag-and-drop deploy at app.netlify.com
- **Cloudflare Pages** — Connect a repo or direct upload
- **Blogspot** — Host the file on any of the above, then embed via iframe

---

## References

- Chandrasekhar, S. (1931). *The Maximum Mass of Ideal White Dwarfs*
- Oppenheimer, J.R. & Volkoff, G.M. (1939). *On Massive Neutron Cores*
- Kerr, R.P. (1963). *Gravitational Field of a Spinning Mass*
- Hawking, S.W. (1974). *Black Hole Explosions?*
- Blandford, R.D. & Znajek, R.L. (1977). *Electromagnetic Extraction of Energy from Kerr Black Holes*
- Novikov, I.D. & Thorne, K.S. (1973). *Astrophysics of Black Holes*
- Claret, A. (2000). *Limb Darkening Coefficients* — Astronomy & Astrophysics
- Wondrak, M.F. et al. (2023). *Gravitational Pair Production and Black Hole Evaporation*
- NICER Collaboration — Neutron star radius constraints

---

## License

MIT — Feel free to use, modify, and share.

---

*Built with physics, shaders, and a conversation with Claude.*
