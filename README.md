# THE BACKROOMS
### First-Person Horror Explorer — Single HTML File

> *You weren't supposed to find this place. Now you can't leave.*

A fully self-contained first-person horror game built in Three.js. No server, no npm, no folder structure

## Play
https://backroomms.netlify.app

## Controls

### Desktop
| Key | Action |
|-----|--------|
| `Click` | Capture mouse (pointer lock) |
| `W A S D` or Arrow keys | Move |
| `Mouse` | Look around |
| `Esc` | Release mouse |

### Mobile
- **Left joystick** — move
- **Right joystick** — look
- **⛶ button** (bottom right) — fullscreen
- Requires **landscape orientation** (portrait shows rotate prompt)

---

## What's In The File

| Asset | Original Size | Embedded |
|-------|-------------|----------|
| `original_backrooms.glb` | 4.5 MB | Base64 in HTML |
| `backrooms_another_level.glb` | 4.3 MB | Base64 in HTML |
| `accurate_backrooms_bacteria_v2_with_animations.glb` | 11.2 MB | Base64 in HTML |
| **Total HTML** | — | **~27 MB** |

---

## Features

### Phase 1 — Core Scene
- Two backrooms levels joined end-to-end
- First-person camera at eye height (1.65 units)
- Head bob on movement
- Wet carpet footstep sounds (Web Audio — no audio files)
- 6 fluorescent ceiling point lights with warm yellow tint
- Exponential fog (`FogExp2`) for depth
- Bacteria entity at end of corridor, floating with `stalkingpose` animation

### Phase 2 — AI Horror Loop
- **Ghost AI state machine**: `patrol → stare → chase/vanish → jumpscare`
- **Patrol**: entity walks waypoints using `stalkingpose` clip
- **Stare**: player enters 9-unit radius → blackout → entity freezes in `bendingoverscene` for 3.5s
- **Chase**: 55% chance after stare → sprints at player with `chasesceneend` clip
- **Vanish**: 45% chance → fade out, teleport, fade back in
- **Jumpscare**: caught within 1.8 units (XZ) → `jumpscare` clip, static noise burst, screen freeze
- **Sanity bar**: top-left, drains near entity, vignette darkens edges
- **Heartbeat**: starts when sanity drops below 30%
- **Game Over**: "FOUND" screen, static burst, TRY AGAIN reloads
- All distance checks use XZ-only distance (corrects for player eye height vs entity floor position)

### Phase 3 — Polish & Atmosphere
- **Random ambient events** (6–20s intervals, weighted): creak, distant thud, hum surge, whisper, light flicker, subtle flash
- **Camera idle sway** — subtle Z/X rotation when standing still
- **Wall collision** — player clamped inside room AABB bounds, can't walk through walls
- **Sanity = 0 auto game-over** — stare at entity long enough and you lose
- **iOS AudioContext unlock** — audio works on iPhone/iPad (resumes on first touch)
- **Fullscreen button** — mobile only, bottom right
- **Portrait blocker** — "ROTATE YOUR DEVICE" overlay on mobile portrait
- **Landscape joystick scaling** — joysticks shrink on short screens (max-height: 500px)
- **Pixel ratio cap** — `min(devicePixelRatio, 1.5)` on mobile, shadows disabled on mobile

---

## Audio Architecture

Everything synthesised with Web Audio API. Zero audio files.

| Layer | Sound | Technique |
|-------|-------|-----------|
| Background hum | Constant backrooms drone | 5 detuned oscillators (60Hz base), lowpass 420Hz |
| Flicker buzz | Fluorescent crackle | White noise burst, bandpass 3kHz |
| Footsteps | Wet carpet thud | Low noise burst, lowpass 200Hz |
| Proximity whine | Louder near entity | Sine 3.4kHz, gain = f(XZ distance) |
| Heartbeat | Sanity < 30% | Noise buffer, lowpass 120Hz, 70bpm rhythm |
| Creak | Ambient event | Sawtooth 180→60Hz sweep, lowpass 300Hz |
| Distant thud | Ambient event | Low noise burst, lowpass 90Hz |
| Hum surge | Ambient event | humGain boost then decay |
| Whisper | Ambient event | Noise × sin envelope, bandpass 2.8kHz |
| Static burst | Game over sting | Full-spectrum noise, gain spike |

---

## Entity Animations

The bacteria entity (`accurate_backrooms_bacteria_v2_with_animations.glb`) has 5 animation clips, all used:

| Clip (stored lowercase) | State used in |
|------------------------|---------------|
| `stalkingpose` | Patrol |
| `bendingoverscene` | Stare |
| `chasesceneend` | Chase |
| `jumpscare` | Caught |
| `tposelifeform` | (reserved) |

---

## Browser Compatibility

| Browser | Status |
|---------|--------|
| Chrome 90+ | ✅ Full support |
| Firefox 88+ | ✅ Full support |
| Safari 15+ (macOS) | ✅ Works |
| Safari iOS 15+ | ✅ Works (audio unlocked on first touch) |
| Edge 90+ | ✅ Full support |

> **Note:** File is 27MB. On a slow connection the CDN scripts (Three.js, ~600KB) load first; the base64 assets decode locally with no network required.

---

## Project Structure

```
backrooms_phase3.html     ← The entire game. One file.
README.md                 ← This file.
```

Source GLB files (not needed to play, only needed to rebuild):
```
original_backrooms.glb                        (4.5 MB)
backrooms_another_level.glb                   (4.3 MB)
accurate_backrooms_bacteria_v2_with_animations.glb  (11.2 MB)
```

---



---

## Known Limitations

- **27MB file size** — large for a web page, normal for a game. Loads instantly from local disk.
- **No Draco compression** on these particular GLBs (they were not pre-compressed). Adding Draco could bring the HTML to ~12MB.
- **Single room loop** — no procedural generation yet. The two rooms are placed end-to-end.
- **No save state** — refreshing resets everything.

---

## Built With

- [Three.js r128](https://threejs.org/) — 3D rendering
- Web Audio API — all sound synthesis
- GLB models from Sketchfab (Huuxloc, original artist credits in model metadata)
- Zero build tools. Zero npm. Zero bundlers.

---

*Made for a hackathon. The judge will not look away.*
