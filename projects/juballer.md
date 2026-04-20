---
layout: page
title: juballer
permalink: /projects/juballer/
---

**Rust · wgpu + egui · MIT**
Source: [github.com/jacob-sabella/juballer](https://github.com/jacob-sabella/juballer)

Utility platform for the [GAMO2 FB9](https://www.gamo2.com/) — a USB 4×4 grid controller. Treats the device as a programmable display + input surface: each of the 16 cells is its own GPU-rendered tile, the row above the grid is a free-form HUD, and physical key presses route to whichever widget owns that cell.

Built on [wgpu](https://wgpu.rs) and [egui](https://github.com/emilk/egui) so every cell can host a shader, an image, an animated widget, or a custom UI panel — without giving up a sub-frame input path.

Linux is the daily-driver target. Windows builds on every CI run (fmt + clippy + `cargo build`) but has not been exercised against real hardware.

## Deck runtime

- 16 GPU tiles + 1 free-form top region. Each cell gets its own viewport, scissor, and pipeline state
- Per-cell WGSL fragment shaders with a shared uniform block — animated backgrounds, audio-reactive scopes, status indicators
- Geometry calibration overlay nudges edge padding and per-axis gaps, writes back to the active profile
- Per-profile `[keymap]` table maps `(row, col)` → `KEY_*` so the same composition works across keyboards or controllers
- Live config watcher: edit `deck.toml`, save, deck rebuilds widgets without restarting (debounced [`notify`](https://crates.io/crates/notify))
- Built-in widgets: clock, now-playing, image tile, shader tile

## Plugin host

- Out-of-process plugins over a length-prefixed JSON protocol on a Unix socket — a plugin crash stops painting one tile, not the deck
- Typed wire schema in `juballer-deck-protocol` (cell rects, paint commands, input events, lifecycle messages) — any language that can speak length-prefixed JSON can write a plugin
- Action registry exposes `play`, `calibrate`, `settings`, `mods`, `rhythm-launch`, etc. so a tile press can switch modes

## Live editor

- `juballer-deck editor` brings up an axum HTTP + WebSocket server; REST surface for pages / widgets, WS bus for live updates
- Profile reloads, page writes, preview activations all flow through one event stream
- Atomic page writes: temp file + rename, no half-written `deck.toml`

## Rhythm-game mode

The grid is the right shape for a 4×4 tap rhythm game, so the deck ships one as an optional surface. Nothing else in the workspace depends on it.

- Loads charts in [memon v1.0.0](https://memon-spec.readthedocs.io/) — open-source JSON chart format. Tap notes, long (held) notes, mid-song BPM changes, fractional `t` times, per-chart timing overrides
- Real-time judging with configurable hit windows, per-grade SFX, life bar, post-session offset suggestion
- Chart picker: paginated grid over a `*.memon` directory with audio preview, jacket art, banner art, favorites, per-chart audio-offset overrides
- `calibrate-audio` utility for dialing in audio offset
- First-run tutorial mode
- In-app rhythm settings + mods editors write back to `deck.toml`

## Workspace layout

| crate                      | role                                                                 |
|----------------------------|----------------------------------------------------------------------|
| `juballer-core`            | wgpu render loop, geometry calibration, raw input, profile loader    |
| `juballer-egui`            | shared egui overlay scaffolding for HUDs, picker, editor preview     |
| `juballer-deck`            | deck runtime, widget registry, plugin host, editor server, rhythm    |
| `juballer-deck-protocol`   | typed message schema for the plugin socket                           |
| `juballer-gestures`        | corner-hold / multi-press gesture recognizer for shortcuts + exits   |
