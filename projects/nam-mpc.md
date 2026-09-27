---
layout: page
title: NAM for MPC
permalink: /projects/nam-mpc/
---

**C++ · VST2 effect for Akai MPC OS · MIT**
Source: [github.com/jacob-sabella/mpc-vst-nam](https://github.com/jacob-sabella/mpc-vst-nam)

A native VST2 effect plugin for Akai MPC standalone devices. It runs [Neural Amp Modeler](https://github.com/sdatkinson/neural-amp-modeler) amp and pedal captures as an insert effect, with a cab IR, tone stack, gate, pitch shift, delay and reverb after the model. It also has a model browser and a [TONE3000](https://www.tone3000.com) tab that downloads new captures straight onto the device. Everything runs on the MPC screen with its own skin and Q-Links.

MPC's built-in plugin host loads it directly. There's no companion app or separate GUI process: the plugin serves the TONE3000 sign-in page itself.

![AMP tab](https://raw.githubusercontent.com/jacob-sabella/mpc-vst-nam/main/docs/screenshots/amp.png)

## Signal path

Stereo in → mono sum → input gain → noise gate → NAM model → tone stack (bass/mid/treble) → cab IR → pitch shift → delay → reverb → output gain → both outputs.

## Features

- **Engine**: vendored [NeuralAmpModelerCore](https://github.com/sdatkinson/NeuralAmpModelerCore). It runs A2 "slimmable" captures at Lite size by default, which is what makes a WaveNet fit on a Cortex-A17 alongside MPC
- **BROWSE**: every `.nam` in the models folder, packs shown as folders, ALL / FAVORITES / RECENT filters, and a two-tap delete that moves files to a recycle folder. New files show up within a second without reloading the project
- **TONE3000**: browse Trending / Newest / Most downloaded / your favorites, filter by architecture, quality, gear type and amp maker (mapped to the Q-Links), and download a capture or a whole pack
- **PITCH**: a single-reader pitch shifter (±24 semitones) that picks splice points by cross-correlation so crossfades stay in phase. No comb filtering, so small shifts don't sound metallic
- **Skin**: native MPC screen skin (`TUI.json` + Q-Links) generated from a layout file, with synthwave knob strips

## How it works

- Hand-written VST2 effect. No Steinberg SDK: the AEffect ABI is declared in the source
- Parameters are append-only, because MPC stores Q-Link and automation mappings by index. The model and cab are saved by relative path, so a project still finds its model after the library is reorganised
- The TONE3000 client runs on one worker thread, with a tiny HTTP server for the OAuth PKCE sign-in. libcurl is `dlopen`'d, so the plugin still loads on a system without it. Nothing touches the audio thread
- Cross-compiled for armhf under Docker (or a bootlin toolchain). CI runs an x86 host build with ABI and audio tests, an offline pitch-shifter test against a two-tap reference, and a TruffleHog secret scan

## CPU

Measured on an MPC Key 37 (2× Cortex-A17 @ 1.6 GHz, 44.1 kHz / 128-sample blocks), as % of one core:

| Capture                         | avg     | worst block |
|---------------------------------|---------|-------------|
| A2 slimmable, **Lite** (default) | 13%     | 17%         |
| A2 slimmable, Full              | ~85%    | —           |
| A1 standard WaveNet             | 100–200% | —          |

Most of the savings come from Lite size plus an inlined GEMM: at these channel counts, Eigen's general GEMM setup costs more than the math.

## Requirements

A first-generation MPC OS standalone device (32-bit ARM) with root SSH access, which means a modded unit. Installing plugins this way is unofficial.
