---
layout: page
title: Sockbowl
permalink: /projects/sockbowl/
---

**Multi-repo platform · Java / Angular / Docker · MIT**

Real-time multiplayer quizbowl over WebSockets. Four repos divide cleanly along service boundaries: a Spring Boot game backend, a Neo4j-backed question service with AI-assisted packet generation, an Angular frontend, and a Docker Compose stack that ties them together.

## Architecture at a glance

| Repo | Stack | Role |
|---|---|---|
| [sockbowl-game](https://github.com/jacob-sabella/sockbowl-game) | Java, Spring Boot, Redis, Kafka | Game session state + realtime messaging |
| [sockbowl-questions](https://github.com/jacob-sabella/sockbowl-questions) | Java, Spring Boot, Neo4j, Spring GraphQL, Spring AI | Packet repository + AI question generation |
| [sockbowl-ng](https://github.com/jacob-sabella/sockbowl-ng) | Angular, SCSS, Material | Player-facing web client |
| [sockbowl-docker](https://github.com/jacob-sabella/sockbowl-docker) | docker-compose, shell | Infra bring-up for the whole stack |

---

## sockbowl-game

The core game service.

- Create / join / manage sessions via unique join codes
- STOMP over WebSockets for interactive play
- Kafka for event-driven game-state messaging
- Redis document repository caching roster, match state, rounds, progression
- Gradle build, Spring Boot 2.7.x

## sockbowl-questions

Packet + tossup backend.

- Graph storage in Neo4j (packets, topics, difficulty relations)
- GraphQL API (Spring GraphQL) for packet fetch and search
- AI-assisted NAQT-style tossup generation with pyramidality and factual-accuracy prompts (Spring AI, custom prompt templates)
- Custom difficulty and topic modeling per packet

## sockbowl-ng

The Angular frontend.

- WebSocket client for live game state from `sockbowl-game`
- Team and player roster management
- Round / score / answer-outcome visualization
- Pulls packets from the `sockbowl-questions` GraphQL API
- SCSS + Angular Material, responsive
- Runtime env config (`src/environments/environment.ts`) so the same build deploys anywhere
- Nginx-based production Dockerfile included

## sockbowl-docker

Infra + orchestration for the platform.

- Centralized `.env` drives all services — root host, protocols, CORS, WS URLs, auth endpoints, Keycloak realm config
- Runtime config for the Angular image (no rebuild to re-point at a new host)
- Shell scripts copy Redis modules (`redisearch.so`, `rejson.so`) and download Neo4j plugins (APOC, Graph Data Science)
- Neo4j auto-import of base data on first boot if not already present
- Optional demo accounts (`player1..3`, `testuser` — `demo123`) for local testing
- Container-ready for CI/CD

### Compose stack

Kafka · PostgreSQL · Keycloak · Neo4j · Redis · `sockbowl-game` · `sockbowl-questions` · `sockbowl-ng` · Watchtower (auto-update).

```sh
cp .env.example .env
docker-compose up -d
```
