---
layout: page
icon: fas fa-code
order: 1
---

Things I've built in public. All MIT-licensed unless noted otherwise. Sources on [GitHub](https://github.com/jacob-sabella).

## [NAM for MPC](/projects/nam-mpc/)
**C++ · VST2 effect for Akai MPC OS**
Neural Amp Modeler running natively on MPC standalone hardware. Amp/pedal captures plus cab IR, tone stack, gate, pitch, delay and reverb, a model browser, and in-plugin TONE3000 downloads. Runs Lite A2 captures at ~13% of one Cortex-A17 core.

## [XMage Web Client](/projects/xmage-web/)
**Java gateway · React + TypeScript**
Browser UI for the XMage Magic: The Gathering engine. JVM gateway bridges JBoss Remoting to REST/WebSocket JSON; the React client covers the lobby, a 3D multiplayer board with UI for every in-game decision, and a deck editor.

## [Fretscope](/projects/fretscope/)
**Rust · VST3/CLAP plugin + standalone app**
Real-time pitch and key detection with a guitar-fretboard-first UI. McLeod pitch, Krumhansl-Schmuckler key detection. Linux / macOS / Windows.

## [juballer](/projects/juballer/)
**Rust · wgpu + egui**
Utility platform for the GAMO2 FB9 USB 4×4 grid controller. 16 GPU-rendered tiles with per-cell shaders, out-of-process plugin host, live HTTP/WS editor, optional tap-rhythm mode over [memon](https://memon-spec.readthedocs.io/) charts.

## [Gridwatch](/projects/gridwatch/)
**Go · self-hosted service**
Multi-game esports TV guide. Polls Liquipedia (within their API ToU), exposes an EPG grid, JSON / iCal / XMLTV feeds, SSE live updates. Single ~14 MB static binary.

## [lazygradle](/projects/lazygradle/)
**Python · Textual TUI**
Keyboard-driven Gradle task browser and runner. Multi-project cache, saved configs, task history, vim-style output navigation. On [PyPI](https://pypi.org/project/lazygradle/).

## [Sockbowl](/projects/sockbowl/)
**Multi-repo platform · Java, Angular, Docker**
Real-time multiplayer quizbowl over WebSockets. Spring Boot game backend, Neo4j-backed question service with AI packet generation, Angular frontend, and a Docker Compose stack tying it all together.
