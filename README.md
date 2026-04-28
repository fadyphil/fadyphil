# Fady Philip

**Mobile Engineer · Flutter · Clean Architecture**

I build Android applications with an emphasis on maintainability and architectural correctness — feature-first clean architecture, strict layer separation, and domain-driven design using functional error handling. My work prioritizes structural decisions that scale, not just code that runs.

---

## Technical Stack

| Domain | Tools |
| --- | --- |
| Language | Dart |
| Framework | Flutter |
| Architecture | Feature-First Clean Architecture, SOLID |
| State Management | BLoC / Cubit |
| Error Handling | `fpdart` (`Either` throughout the domain layer) |
| Persistence | SQLite (`sqflite`), ObjectBox, Hive |
| DI | `get_it` |
| Routing | `auto_route`, `go_router` |
| Networking | `dio` |
| Backend | Firebase (Auth, Firestore) |
| AI / ML | Sherpa-ONNX (VAD/STT/TTS), Groq API, Gemini API |
| Code Generation | `freezed`, `injectable`, `build_runner` |
| CI/CD | GitHub Actions |

---

## Projects

### [Valoqui](https://github.com/fadyphil/Valoqui) — Real-Time AI Spanish Conversation Tutor

Real-time voice conversation partner for language learning. Eliminates wait times by running VAD and TTS fully on-device — one network roundtrip per exchange (LLM) rather than three. Built with AI agents under my architectural direction — see [JOURNEY.md](https://github.com/fadyphil/Valoqui/blob/main/JOURNEY.md).

**What's technically interesting:**

- **Segment-based VAD contract** — `SherpaVadDatasource` emits complete `Float32List` utterances after silence detection rather than raw bytes plus a separate boolean stream. Eliminates the timing correlation problem between two independent streams; STT receives exactly what VAD identified as speech.

- **Perceived latency optimization** — partial STT decoding begins while the user is still speaking. By the time silence is detected the final transcript arrives almost immediately rather than requiring a full decode pass post-speech. The actual decode time is identical; the perceived wait is not.

- **Sentence-boundary TTS trigger** — the BLoC detects `.`, `?`, `!` in the LLM token stream and fires TTS on the first complete sentence before the full response has arrived. Warm-up runs fire-and-forget during LLM generation so the engine is hot by the time the first boundary is hit.

- **Application-level echo cancellation** — VAD monitoring stops during TTS playback and restarts after, flushing internal buffers. Without this, Lucia's voice feeds back into the mic as a spurious VAD segment and drops the user's next utterance.

- **Lifecycle correctness under session cycling** — repositories registered as `registerFactory`, datasources as `registerLazySingleton`. Prevents the dead-singleton crash on second session when a route-scoped BLoC disposes a singleton on close. A registration type decision, not a code change.

**Stack:** Flutter · BLoC · Freezed · GetIt · fpdart · Firebase · Sherpa-ONNX (VAD/STT/TTS) · Groq LLaMA 3.3 70B · Gemini 2.0 Flash · Dio · GoRouter

---

### [Osserva](https://github.com/fadyphil/Music-App-Player) — Offline Music Player with Local Analytics

9 features, 221 files, 139 directories. The core idea: give users the listening analytics that streaming services provide, without routing any data off-device.

**What's technically interesting:**

- **Analytics data layer split into 4 distinct responsibilities** — `analytics_recorder` (write path), `analytics_reader` (query path), `analytics_aggregator` (rollup computation), and `audio_analytics_tracker` (session tracking). Separate datasource classes, not methods on a single class. 7 use cases sit above them including `watch_playback_history`, which exposes a reactive stream rather than a one-shot fetch.

- **Star schema SQLite analytics engine** — play events recorded against a fact table with dimension tables for songs, artists, albums, and genres. Daily rollups use a hot/cold record pattern to minimize write amplification on repeat plays.

- **Per-feature DI modules** — 8 dedicated `get_it` modules, each registering only its own dependencies. No monolithic init file; features are self-contained at the DI level.

- **Android Home Screen Widget** with full cold-start support — `OsWidgetManager` + `WidgetSyncService` handle state sync; playback can be initiated from the widget even when the app process is not running.

- **`fpdart` Either types** propagated through every repository and use-case boundary. Each feature defines its own typed failure classes (`AnalyticsFailure`, `ArtistFailure`, `PlaylistFailure`, etc.) rather than sharing a global failure type.

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

<div align="center">
<a href="https://github.com/fadyphil">
  <img src="https://github-readme-streak-stats-eight.vercel.app/?user=fadyphil&theme=monokai-metallian&hide_border=true&short_numbers=true" />
</a>
  
---

**Contact:** fadyph2003@gmail.com

</div>


