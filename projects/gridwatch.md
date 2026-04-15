---
layout: page
title: Gridwatch
permalink: /projects/gridwatch/
---

**Go · self-hosted service · MIT**
Source: [github.com/jacob-sabella/gridwatch](https://github.com/jacob-sabella/gridwatch)

Self-hosted multi-game esports TV guide. One browser tab shows every live, upcoming, and recently-finished match across Rocket League, League of Legends, CS2, Dota 2, Valorant, and more — with direct click-through to the stream.

Ships as a single static Go binary (~14 MB) or a distroless Docker image (<15 MB). No database. No build tooling.

## Upstream data — Liquipedia, by the book

Polls [Liquipedia](https://liquipedia.net) wikis while honoring their [API Terms of Use](https://liquipedia.net/api-terms-of-use) at the code level:

- `User-Agent` in the exact `Name/version (url; contact)` format they document
- gzip mandatory and always sent
- **Global cap:** 1 request / 60 s — half their 1/30 s ceiling for the parse API
- **Per-page floor:** 600 s between fetches of any single page
- **1-hour cooldown** on 429 / 5xx (their ToU warns about temp IP bans)
- Shared `http.Client` with pooling, anonymous requests so their caches do their job
- Refuses to start if config tries to exceed the ToU ceiling, or if `contact` is missing
- Golden-file parser tests per game — template drift breaks CI loudly

In-memory store with 48 h eviction and optional JSON snapshot for cold-start warmth.

## Desktop EPG

- Horizontal scrollable grid, one row per game/tournament
- Live matches pinned in a pulsing banner above the grid
- 30 min slot granularity, configurable window (−2 h to +24 h default)
- Live "now" cursor, per-game accent colors
- Filters (game, region, tier, has-stream) mirrored to the URL for shareable links

## Mobile card view

- Auto-switches at `max-width: 720px` via CSS container queries
- Three sections: Live Now, Upcoming, Recent Results
- Thumb-reach stream button per card
- Dark / light / auto theme

## Real-time updates

- Server-sent events at `/events` emit on store writes
- Client triggers an htmx refresh in place — no full page reload
- 60 s polling fallback if EventSource is unavailable
- Proxy-friendly (sends `X-Accel-Buffering: no` for nginx)

## Feeds & APIs

- JSON: `/api/v1/matches` — same filter params as the UI, for Homepage / Home Assistant widgets
- iCal: `/api/v1/matches.ics` — Google Calendar, Fantastical, Apple Calendar
- XMLTV: `/api/v1/matches.xml` — Plex Live TV, Jellyfin, xTeVe, TVHeadend, Kodi
- Prometheus metrics at `/metrics` (opt-in)
- `/healthz` with a freshness SLA — returns 503 if no successful poll in `2 × poll.liquipedia_interval`

## Notifier (optional)

- Push live/result alerts to [ntfy](https://ntfy.sh) or a generic webhook
- Rules: filter by game, stage, region, minimum tier
- Dedupe state marked **only on 2xx delivery** — failed deliveries retry on the next cycle

## Supported games (defaults)

| slug              | game              | default Bo | default duration |
|-------------------|-------------------|:----------:|:----------------:|
| `rocketleague`    | Rocket League     | 5          | 90 min           |
| `leagueoflegends` | League of Legends | 5          | 60 min           |
| `counterstrike`   | Counter-Strike 2  | 3          | 90 min           |
| `dota2`           | Dota 2            | 3          | 75 min           |
| `valorant`        | Valorant          | 3          | 90 min           |
| `starcraft2`      | StarCraft II      | 5          | 60 min           |
| `overwatch`       | Overwatch         | 5          | 45 min           |

Any other Liquipedia slug also works — sensible defaults are generated and every field is overridable in config.
