---
layout: page
title: XMage Web Client
permalink: /projects/xmage-web/
---

**Java + React / TypeScript · MIT**
Source: [jacob-sabella/mage · `Mage.Client.Web`](https://github.com/jacob-sabella/mage/tree/vibe/xmage-claude-react-ui/Mage.Client.Web)

A browser-based UI for [XMage](https://github.com/magefree/mage), the open-source Magic: The Gathering engine, written from scratch as a replacement for the legacy Swing client.

## Architecture

Browsers can't speak the XMage server's transport (JBoss Remoting over Java serialization), so the module is a thin JVM gateway in the middle that reuses the shared `Mage.Common` session unchanged:

```
browser  ⇄  HTTP/WS + JSON  ⇄  gateway  ⇄  JBoss Remoting  ⇄  XMage server
```

- The gateway opens a real upstream session per browser, so no protocol or serialization code is duplicated
- It exposes a small REST + WebSocket/JSON API and serves the static UI
- The UI is a React + Vite + TypeScript app built into the gateway's resources, so the gateway runs without Node

Because the engine, networking and protocol stay shared, all existing functionality is still reachable. Only the presentation layer is new.

## Features

- **Connect**: official-server presets, real server errors surfaced (e.g. version mismatch), session resume across refresh
- **Lobby**: live tables with Join / Watch, new game vs AI with 1–5 opponents (Two Player Duel or multiplayer Free For All; constructed or Freeform Commander, depending on the deck), room chat, match history
- **Game**: 3D table with a seat per player and snap-view cameras, life / mana pool / zones / hand / stack, combat display, real card art with a generated fallback
- **Every server decision callback has UI**: priority with a playable-cards bar, targets (cards and players), mana payment, mulligan, choices, amounts, ability picker, pile choice, multi-amount distribution, discard, concede, plus skip/stop hotkeys
- **Deck editor**: filtered card search, quantities, mana curve / color / type stats, sideboard, load and save `.dck`
- **Look & feel**: dark theme, three.js particle backdrop, framer-motion shared-element zone transitions
- Keep-alive ping and WebSocket auto-reconnect

## Testing

- Playwright suite drives the built UI against a faux backend (stubbed REST + mock WebSocket), with helpers that jump straight to a populated game, lobby, mulligan, target, combat, pile or multi-amount screen
- A live smoke test creates a real game vs AI and auto-plays it through the gateway, checking that the protocol loop survives every prompt kind and every combat phase
