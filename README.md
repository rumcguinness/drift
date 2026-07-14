# DRIFT
**Polymeter designer for hardware sequencers — Perkons HD-01**

---

## What this is

DRIFT is a web app for designing polymetric drum patterns, built specifically around the Erica Synths Perkons HD-01. It is a planning and notation tool, not an instrument: the output is a programming sheet you walk to the hardware with, not audio you record from it.

The core problem it solves: the Perkons allows each of its four voices to have an independent LAST step (1–16), so V1 might cycle every 16 steps, V2 every 10, V3 every 7, V4 every 11. The resulting LCM cycle is 6,160 steps...nearly 12 minutes at 130 BPM before the pattern resets. Designing that in your head, or discovering it accidentally by ear is the only option the hardware gives you. DRIFT makes it deliberate: you set LAST values, see the full cycle length, visualise where voices align and collide across that cycle, and export a notation sheet before you touch the machine.

---

## The problem

Polymetric sequencing on hardware like the Perkons is powerful but opaque. You can stumble into good grooves but you cannot plan them without maths, and you cannot recall them without notes. This tool is to get away from that 'blank sheet of paper' analysis paralysis. The specific gaps DRIFT addresses:

- **No visibility of the full drift cycle.** The Perkons shows you 16 steps. A LAST=7 voice running against a LAST=16 kick takes 112 steps to reset. You cannot hear or see this on the machine.
- **No notation output.** Hardware sessions leave no record. The pattern in your head at 2am does not exist the next morning without a discipline of writing it down, which nobody sustains.
- **No starting point.** Sitting down to program a 4-voice polymeter from scratch is a blank-canvas problem. Genre-informed suggestions with known polyrhythmic logic are faster than random exploration. 
- **Variation slots need planning across all four voices simultaneously.** FULL / TENSION / RELEASE / BREAKDOWN need to relate to each other. The Perkons has no overview of how slots compare.

---

## How I approached it

### Design principles

- **Planning tool only.** No audio engine requirement for the core loop; previewing grooves is additive, not foundational.
- **Perkons-native parameter language.** LAST, ACC, RAT, ODDS1, ODDS2, PROB — the app uses these terms exactly, so the notation sheet map to the sequencer without translation.
- **Dogfood first.** Built for an August 2026 live set. If it generates the patterns used in that set, it works.
- **Single file, zero dependencies.** `index.html` loads from GitHub Pages, pins to home screen in Safari, runs offline after first load. `localStorage` for persistence.

### Suggestion engine

Rather than a random generator, DRIFT has six style engines encoding real polymetric approaches derived from specific producers and their known methods:

- **PAS ROLLING (133 BPM)** — locked 4/4 kick, off-beat tops, one Euclidean perc loop at a drifting LAST. Based on the rolling hypnotic approach in Planetary Assault Systems' live sets.
- **BENDERS DRIFT (130 BPM)** — fixed kick anchor at LAST 16, three sparse voices (2–3 hits each) at mismatched LASTs drawn from coprime pools. Based on Colin Benders' modular polymeter technique.
- **ORPHX INDUSTRIAL (138 BPM)** — full 16th-note noise pulse with accents every 4, all-accented stabs, ghost kick via ODDS1, ratcheted metallics. Industrial grinding forward motion.
- **T47 BROKEN (128 BPM)** — broken kick templates (1-8-11, 1-4-11-14 etc.), backbeat clap, dense metallic loop at odd LAST values. Based on Tommy Four Seven's off-kilter placement.
- **HEADLESS SPARSE (126 BPM)** — unaccented 4/4, rolling Euclidean tom loop, halftime snare, airy texture voice on ODDS2. Based on Headless Horseman's restrained groove language.
- **SURGEON TRIBAL (134 BPM)** — pounding 4/4 with ghost kick on beat 6 or 14 via ODDS1, dense rolling toms with cyclic accents, clave-derived rim line, ratcheted hats. Tribal churn.

Each engine uses Euclidean rhythm generation, per-style LAST pools (TIGHT / DRIFT / DEEP controlling how aggressively coprime values are used), deterministic seeding (Roll generates a new seed; same seed always gives the same result), and automatic derivation of TENSION, RELEASE and BREAKDOWN variation slots from the generated FULL pattern.

---

## Where we are now

### Current version: v0.3

A working single-file web app (`index.html`) with the following features:

- **Library** — create, duplicate, delete, open patterns
- **EDIT tab** — four voice lanes with per-step HIT / ACC / RAT / PROB tools, LAST stepper, voice name and sound selector, M mute
- **GEN tab** — six style engines, three drift modes (TIGHT / DRIFT / DEEP), Roll button, auto-variation derivation, Apply to pattern
- **CYCLE tab** — full LCM drift map rendered to canvas, scrubber with per-voice step readout at any point in the cycle
- **SHEET tab** — pattern-level setup notes, per-voice sound design notes, formatted notation sheet with Copy button
- **Preview** — 909-flavoured WebAudio synthesis engine; plays the actual polymetric cycle with ratchets, accents and ODDS/PROB gating
- **Persistence** — `localStorage`, no backend, no account required

### Hosting

Single `index.html` in the repo root. Open in Safari on iPhone or iPad → share sheet → Add to Home Screen to use as a pinned app.

Updates: replace `index.html`, commit. URL stays the same, no re-pinning.

### Known issues / next iteration

- Audio preview is synthesised — sufficient for groove sketching, not sound-design reference
- `localStorage` is per-device and per-browser — no sync between iPhone and iPad
- No MIDI output — planned for v1.1+ with device profiles (Elektron, Nerdseq)
- Pattern names are uppercase-forced — minor UX friction

*DRIFT project document — 13/07/26*

