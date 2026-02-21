# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Marble Roulette — a web-based lucky draw app where marbles drop through a physics obstacle course. Deployed as a PWA to GitHub Pages at https://lazygyu.github.io/roulette/.

## Commands

- `yarn dev` — Start dev server on port 1235
- `yarn build` — Production build (Parcel → `dist/`, then generates service worker via Workbox)
- `yarn lint` — ESLint + Prettier on `src/**/*.ts`

No test framework is configured.

## Architecture

### Game Loop & Core

`Roulette` (`src/roulette.ts`) is the central game controller. It extends `EventTarget`, owns the game loop via `requestAnimationFrame`, and coordinates physics, rendering, camera, and UI. It exposes a public API consumed by inline JS in `index.html` through `window.roulette`.

The game loop runs a fixed-timestep physics update (10ms intervals) with variable rendering. Time scale slows down automatically near the goal line for dramatic effect.

### Physics

`IPhysics` interface (`src/IPhysics.ts`) abstracts the physics engine. The sole implementation is `Box2dPhysics` (`src/physics-box2d.ts`) using `box2d-wasm`. Physics init is async (WASM loading). Marbles are dynamic bodies; map entities are static or kinematic bodies.

### Rendering

`RouletteRenderer` (`src/rouletteRenderer.ts`) manages a single `<canvas>` element and draws everything: map entities, marbles, particles, effects, and UI objects. No framework — all raw Canvas2D API. The canvas is 1600x900 logical pixels.

### Map System

Maps are defined in `src/data/maps.ts` as `StageDef` objects containing arrays of `MapEntity` (polylines, boxes, circles with physical properties). Each entity has a shape, position, and physics props (density, restitution, angular velocity).

### Key Abstractions

- **`UIObject`** (`src/UIObject.ts`) — Interface for interactive overlay elements (RankRenderer, Minimap, FastForwarder). They receive mouse events, update each tick, and render on top of the game.
- **`GameObject`** (`src/gameObject.ts`) — Interface for transient visual effects (e.g., SkillEffect). Has `isDestroy` lifecycle flag.
- **`Marble`** (`src/marble.ts`) — Represents a marble with physics body, rendering, skill cooldown, and stuck detection. Skills (Impact) trigger based on cooldown timers and probability.
- **`Camera`** (`src/camera.ts`) — Viewport control with smooth position/zoom interpolation. Can follow marbles or be locked by minimap interaction.

### Marble Name Parsing

Names entered by users support weight (`/N`) and count (`*N`) syntax (e.g., `Alice/3*2` means Alice with weight 3, duplicated twice). Parsing happens in `parseName()` in both `src/utils/utils.ts` and inline JS in `index.html`.

### Localization

`src/localization.ts` detects browser locale and translates elements with `data-trans` attributes using translation tables in `src/data/languages.ts`. `window.translateElement()` is exposed globally for dynamic content.

### Custom Skins

`KeywordService` (`src/keywordService.ts`) fetches marble skin sprites from `marblerouletteshop.com` API, periodically refreshing. Sprites are extracted from sprite sheets and cached.

### Global State

`window.roulette` (Roulette instance) and `window.options` (Options singleton) are the primary globals. The inline `<script>` in `index.html` handles all DOM UI (settings panel, name input, buttons) and communicates with the game through these globals.

## Build & Deploy

- **Bundler**: Parcel 2 with SCSS transformer and webmanifest support
- **Public URL**: `/roulette/` (for GitHub Pages subdirectory)
- **CI**: GitHub Actions on push to `main` → build → deploy to `gh-pages` branch
- **Service Worker**: Generated post-build by `scripts/build-sw.js` using Workbox

## TypeScript

- Strict mode enabled, `experimentalDecorators: true` (used by `@bound` decorator in `src/utils/bound.decorator.ts`)
- Target: ES2017, module: System
- Entry point: `src/index.ts`
