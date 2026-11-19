# Project context: Maritime Anomaly Detection

## Goal
A maritime domain awareness system: take in real AIS ship-tracking data, keep a state estimate for each vessel, flag suspicious behaviour, and show the alerts on a map dashboard.

The project has two purposes:
1. Learn serious programming and software engineering.
2. Serve as a portfolio piece for defense-tech employers, with Palantir as the main target.

The developer is a physics undergraduate (TU Wien) with a strong math background and intermediate C++. He is also studying databases and algorithms.

## How to work with me (important)
- **Don't write the code for me.** Guide me: explain concepts, point to the right approach or API, review what I write, and give hints. Give small snippets only when one is essential to explain an idea.
- Explain the *why* behind each decision. Learning is a primary goal.

## Key decisions and why
- **Chose maritime anomaly detection** over satellite tracking, sensor fusion and logistics. It is closest to Palantir's actual work: messy real-world data, integrating several sources, and a decision-support UI.
- **C++ core.** This is where most of the learning happens, and performance matters on large AIS datasets.
- **Region: Danish waters / Baltic Sea.** The data is free and rich, and recent shadow-fleet tanker and undersea-cable incidents give the demo a real story.
- **Data sources:**
  - Live: aisstream.io, a free WebSocket feed that needs an API key. It is in beta with no uptime guarantee, so it can't be the only source.
  - Historical: Danish Maritime Authority CSV files, which can also be replayed as if they were a live stream.
- **Planned stack:**
  - Core: C++20, CMake + Ninja, GoogleTest, simdjson; IXWebSocket or Boost.Beast for the live feed.
  - Storage: PostgreSQL + PostGIS; Parquet + DuckDB for historical batch work.
  - Analytics: Python via pybind11 for machine learning.
  - API: FastAPI.
  - Frontend: React + TypeScript + MapLibre/deck.gl.
  - Infrastructure: Docker Compose, GitHub Actions CI, sanitizers, clang-tidy.

## Roadmap (about 3–4 months part-time)
1. Explore the data in a Python/polars notebook.
2. Ingestion and storage: CSV replay and live stream into one shared message format, written to PostGIS.
3. Track engine: per-vessel state, Kalman filter smoothing, handling of out-of-order and duplicate messages.
4. Detectors:
   - dark ships (gaps in AIS transmissions)
   - impossible speeds and position jumps (spoofing)
   - loitering near cables or pipelines
   - ship-to-ship meetings at sea
   - identity changes (MMSI or name swaps)
5. API and map interface: live map, alert feed, per-vessel timeline.
6. Polish: benchmarks (messages per second), ML anomaly score, demo video, technical write-up.

## Done so far
- Repo setup guide written (`docs/01-repo-setup-guide.md`). It covers:
  - Fedora toolchain: git, gh, gcc-c++, cmake, ninja-build, gdb
  - public GitHub repo `maritime-anomaly-detection` with an MIT license
  - `.gitignore` excluding `build/`, `.env` and data files
  - CMake skeleton building an executable `mda`
  - starter README and first commit

## Next steps
1. Get an aisstream.io API key and download one day of Danish data.
2. Explore and plot the tracks in a notebook.
3. Add GoogleTest and GitHub Actions CI.
4. First C++ feature: parse the CSV into `Vessel`/`Position` structs, compute the speed implied by consecutive positions (haversine distance), and flag impossible jumps.

## Conventions and constraints
- **OS:** Fedora Linux.
- **Build:** C++20; strict warnings (`-Wall -Wextra -Wpedantic`); out-of-source builds in `build/`.
- **Layout:** `src/` (.cpp), `include/` (.hpp), `tests/`, `data/` (git-ignored except `.gitkeep`), `docs/`.
- **Secrets:** the API key lives in `.env` and must never be committed. The repo is public.
- **Data:** large AIS files stay out of Git; the README documents how to download them.
- **Commits:** small and frequent, one logical change each, with a message starting with a verb.
- **Data use:** the Danish AIS terms forbid combining the data in ways that identify people. Keep the project about vessels, not crews.

## Open questions
- Has the repo setup actually been completed? Check the repo state.
- Final data choice: the setup guide mentions NOAA Marine Cadastre, but the plan favours Danish data (Baltic focus).
- C++ libraries for HTTP and WebSocket: IXWebSocket or Boost.Beast?
- How much of the API layer should be C++ versus Python?
- When to add a message queue (NATS or Redpanda), if at all?
