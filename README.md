# Fady Philip
 
**Mobile Engineer · Flutter · Clean Architecture**
 
I build Android applications with an emphasis on maintainability and architectural correctness — feature-first clean architecture, strict layer separation, and domain-driven design using functional error handling. My work prioritizes structural decisions that scale, not just code that runs.
 
---
 
## Technical Stack
 
| Domain | Tools |
|---|---|
| Language | Dart |
| Framework | Flutter |
| Architecture | Feature-First Clean Architecture, SOLID |
| State Management | BLoC / Cubit |
| Error Handling | `fpdart` (`Either` throughout the domain layer) |
| Persistence | SQLite (`sqflite`), ObjectBox, Hive |
| DI / Routing | `get_it`, `auto_route` |
| Code Generation | `freezed`, `injectable`, `build_runner` |
 
---
 
## Projects
 
### [Osserva](https://github.com/fadyphil/Music-App-Player) — Offline Music Player with Local Analytics
 
The most architecturally complete project in my portfolio — 9 features, 221 files, 139 directories. The core idea: give users the listening analytics that streaming services provide, without routing any data off-device.
 
**What's technically interesting:**
 
- **Analytics data layer split into 4 distinct responsibilities** — `analytics_recorder` (write path), `analytics_reader` (query path), `analytics_aggregator` (rollup computation), and `audio_analytics_tracker` (session tracking). These are separate datasource classes, not methods on a single class. 7 use cases sit above them in the domain layer, including `watch_playback_history` which exposes a reactive stream rather than a one-shot fetch.
 
- **Star schema SQLite analytics engine** — play events recorded against a fact table with dimension tables for songs, artists, albums, and genres. Daily rollups use a hot/cold record pattern to minimize write amplification on repeat plays. All timestamps anchored to local midnight for accurate daily aggregation.
 
- **Per-feature DI modules** — 8 dedicated `get_it` modules (`analytics_module`, `artists_module`, `favorites_module`, etc.), each registering only its own dependencies. No monolithic init file; features are self-contained at the DI level.
 
- **Onboarding route guard** — `auto_route` guard (`OnboardingGuard`) intercepts navigation at the router level rather than in widget build logic, keeping routing concerns out of the presentation layer.
 
- **Android Home Screen Widget** with full cold-start support — `OsWidgetManager` + `WidgetSyncService` in `core` handle state sync; playback can be initiated from the widget even when the app process is not running.
 
- **Background audio service** (`background_notification` feature) with lock-screen and notification controls via `just_audio` + `audio_service`.
 
- **`fpdart` Either types** propagated through every repository and use-case boundary — no raw exceptions crossing layer boundaries. Each feature defines its own typed failure classes (`AnalyticsFailure`, `ArtistFailure`, `PlaylistFailure`, etc.).
 
- **Forked `on_audio_query`** to patch media retrieval behavior not addressed upstream.
 
**Stack:** Flutter · BLoC · `sqflite` · `fpdart` · `freezed` · `get_it` · `auto_route` · `home_widget` · `just_audio` · `audio_service`
 
---
 
### [Trip Planner](https://github.com/fadyphil/trip-planner) — Offline-First Travel Manager
 
Feature-first clean architecture applied to a multi-feature CRUD application. Offline-first by design using ObjectBox for persistence.
 
**Notable:** Budget estimation with live currency conversion using a cached-fallback strategy — the app degrades gracefully when the conversion API is unavailable, falling back to the last known rate.
 
**Stack:** Flutter · BLoC/Cubit · ObjectBox · Dio · `freezed` · `auto_route`
 
---
 
### Culinary Companion — Architectural Progression (v1 → v2)
 
Two sequential iterations of the same application, each targeting a specific architectural concern.
 
- **[v1](https://github.com/fadyphil/Edges-project-1):** BLoC pattern and computed state properties. `ExploreState` exposes derived getters (`todaysChallenge`, `filteredRecipes`) to keep the UI purely declarative.
- **[v2](https://github.com/fadyphil/Edges-project-1-api-integrated-v2-):** REST API integration with explicit repository-level error mapping — `DioException` is caught at the data layer and converted to typed `Failure` objects before crossing into the domain.
 
**Stack:** Flutter · BLoC · Dio · `freezed` · `auto_route` · `shared_preferences`
 
---
 
## GitHub Activity
 
[![Activity Graph](https://github-readme-activity-graph.vercel.app/graph?username=fadyphil&theme=github-compact)](https://github.com/ashutosh00710/github-readme-activity-graph)
 
<a href="https://github.com/fadyphil">
  <img src="https://github-readme-streak-stats-eight.vercel.app/?user=fadyphil&theme=monokai-metallian&hide_border=true&short_numbers=true" />
</a>
 
---
 
**Contact:** fadyph2003@gmail.com
