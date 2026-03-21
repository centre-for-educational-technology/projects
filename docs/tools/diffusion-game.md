<div class="lab-detail-header" markdown>

<div class="lab-detail-badges" markdown>
<span class="lab-badge lab-badge--stack">Laravel</span>
<span class="lab-badge lab-badge--stack">React / TypeScript</span>
<span class="lab-badge lab-badge--stack">Zustand</span>
<span class="lab-badge lab-badge--stack">shadcn/ui</span>
</div>

# Diffusion Simulation Game

<div class="lab-detail-desc">
An educational simulation game for teaching innovation adoption strategies based on E. Rogers' Diffusion of Innovations theory. Originally a board game created at Indiana University in the 1980s (Hannafin & Frick), rebuilt as a modern web platform with multiplayer sessions, a data-driven game engine, and a game version editor.
</div>

<a href="https://diffusiongame.eu" class="lab-detail-link">diffusiongame.eu</a>

</div>

<span class="lab-section-num">// 01</span>

## Overview

The Diffusion Simulation Game lets players experiment with change management strategies in a simulated organisation. Players manage staff, select activities, and respond to feedback as they attempt to spread an innovation, learning firsthand how adopter categories, communication channels, and social systems affect diffusion outcomes.

The platform supports multiple game variants, Kahoot-style multiplayer sessions with facilitator controls, and a visual game version editor for researchers and instructional designers to create custom scenarios.

### Key Capabilities

- **Playable game board** — staff management, activity selection, feedback cards, timeline tracking, and game-over screen with responsive desktop and mobile layouts
- **Data-driven game engine** — modular TypeScript engine with 186 feedback rules, 16 activities, and full adoption simulation driven by JSON configuration
- **Kahoot-style multiplayer** — facilitators create sessions with 6-character join codes, players join without accounts, real-time lobby, live progress monitoring, and post-game leaderboard
- **Game version editor** — 6-tab editor (General, Staff, Activities, Feedback, UI Text, Settings) with auto-save, publish workflow, and fork/template system
- **Analytics dashboard** — session analytics with player leaderboard, Recharts visualizations (activity usage, adoption timeline), research-grade CSV/JSON export with optional anonymization
- **Internationalization** — English, Estonian, and Romanian UI with per-version game content stored in the database

<span class="lab-section-num">// 02</span>

## Technical Architecture

<div class="lab-specs" markdown>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Backend</div>
<div class="lab-spec-val">Laravel, PHP, Eloquent ORM</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Database</div>
<div class="lab-spec-val">MariaDB</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Frontend</div>
<div class="lab-spec-val">React, TypeScript, Tailwind CSS, shadcn/ui (Radix primitives)</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">State Management</div>
<div class="lab-spec-val">Zustand (client) + TanStack Query (server state, 2–3s polling)</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Game Engine</div>
<div class="lab-spec-val">Custom TypeScript engine — modular, data-driven, 81 unit tests</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">i18n</div>
<div class="lab-spec-val">react-i18next (EN, ET, RO)</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Auth</div>
<div class="lab-spec-val">Laravel Sanctum (Bearer tokens)</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Testing</div>
<div class="lab-spec-val">Vitest (engine), PHPUnit (backend)</div>
</div>
<div class="lab-spec-row" markdown>
<div class="lab-spec-key">Deployment</div>
<div class="lab-spec-val">Docker Compose (multi-stage build)</div>
</div>
</div>

<span class="lab-section-num">// 03</span>

## Game Engine Design

The game engine is a standalone TypeScript module (`GameEngine.ts`) that orchestrates the entire simulation. It is fully data-driven — all game content (staff profiles, activities, feedback rules, settings) is defined in JSON configuration, making it possible to create entirely different game scenarios without code changes.

### Core Mechanics

- **Staff & Adopter Categories** — simulated organization members following Rogers' adopter categories (innovators, early adopters, early majority, late majority, laggards)
- **Activities** — 16 change management activities players can deploy each round, each with different effects on different adopter types
- **Feedback Rules** — 186 conditional rules that generate contextual feedback based on game state, player choices, and adoption progress
- **Adoption Simulation** — models how innovation spreads through the organization based on accumulated player actions, social influence, and adopter characteristics

### Extensibility

Game versions are self-contained packages of configuration. The editor allows designers to customize all aspects through a visual interface, and versions can be exported/imported as JSON for offline editing, translation, or backup. The template system lets designers share versions as forkable starting points.

<span class="lab-section-num">// 04</span>

## Session & Multiplayer System

Sessions follow a managed lifecycle: **draft → lobby → active → paused → completed → archived**.

Facilitators create sessions linked to a published game version and receive a 6-character join code. Players join without needing accounts: they enter the code and a display name. The facilitator sees a real-time lobby and controls session start, pause, resume, and end.

During active play, facilitators monitor live player progress via polling - current week, score, adoption rate - with session-level aggregate statistics. After completion, a leaderboard displays final results.

<span class="lab-section-num">// 05</span>

## Theoretical Foundation

The game is grounded in Everett Rogers' **Diffusion of Innovations** theory (1962), which describes how new ideas and technologies spread through social systems. Key concepts modeled in the simulation:

- **Adopter categories** — innovators, early adopters, early majority, late majority, laggards — each with distinct receptiveness to change
- **Communication channels** — different activities represent mass media vs. interpersonal channels, with varying effectiveness per adopter type
- **Rate of adoption** — the S-curve of innovation spread, influenced by perceived attributes of the innovation and the change agent's strategy
- **Social system** — the organizational context that enables or constrains diffusion

The original board game was designed at Indiana University by Hannafin & Frick in the 1980s as a teaching tool for educational technology and change management courses. This digital version preserves the pedagogical design while adding multiplayer capability, analytics, and configurability.

<span class="lab-section-num">// 06</span>

## Analytics & Research Export

The analytics dashboard provides multiple levels of insight:

- **System-wide statistics** — admin overview of all sessions, players, and versions
- **Session analytics** — per-session player leaderboard with Recharts visualizations showing activity usage patterns and adoption timelines
- **Version analytics** — cross-session statistics for a given game version
- **Research export** — CSV and JSON export with optional anonymization for use in research studies

<span class="lab-section-num">// 07</span>

## Role-Based Access

| Role | Capabilities |
|------|-------------|
| Admin | User management, system analytics, full access |
| Designer | Create/edit/fork game versions, manage templates |
| Facilitator | Create and manage sessions using published versions |
| Player | Join sessions via code, play games |

Designers see only their own versions plus shared templates. Facilitators see only published versions. Fine-grained authorization policies enforce visibility boundaries.


