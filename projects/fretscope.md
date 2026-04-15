---
layout: page
title: Fretscope
permalink: /projects/fretscope/
---

**Rust · VST3/CLAP plugin + standalone JACK/ALSA app · MIT**
Source: [github.com/jacob-sabella/fretscope](https://github.com/jacob-sabella/fretscope)

Real-time pitch and key detection with guitar fretboard visualization. Runs inside any DAW as a VST3/CLAP plugin or stand-alone. Cross-platform: Linux, macOS, Windows.

## Pitch detection

- McLeod algorithm, tuned for guitar range (50–2000 Hz)
- 2048-sample window, 50% overlap → ~23 ms latency at 44.1 kHz
- Configurable confidence threshold, spacebar listen/pause

## Key detection

- Krumhansl-Schmuckler with exponential-decay weighting
- Floating mode (auto-tracks as you play) or locked mode
- Click to pick an alternate, or set root manually (all 12 notes)

## Fretboard

- 8–30 frets, resizable, responsive layout
- Color-coded scale overlays, active-note glow/pulse
- Flip / note labels / degree labels / fret-number / open-fret / per-degree filters
- Proportional string thickness, fret markers, nut rendering

## Tuning

- 13 presets covering 6-, 7-, 8-string guitar and 4/5-string bass
- Custom tuning: 1–12 strings, per-string semitone offset

## Scales & chords

- 15 scales (major, three minors, pentatonics, blues, whole tone, diminished, all 7 modes)
- 11 chord types (major, minor, dom7, maj7, min7, dim, aug, sus2, sus4, add9, power)

## Other

- Built-in tuner with cents offset and needle bar
- Timestamped note log with in-key/out-of-key coloring and phrase grouping
- Multi-instance — each plugin slot keeps its own tuning, scale, and display state
