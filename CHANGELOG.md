# Changelog

All notable changes to **HazardLensAI** are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
and the project adheres to [Semantic Versioning](https://semver.org/).

## [0.3.0] — 2025

### Added
- **Video Scan studio** (`#/video`): real-time road-hazard detection on
  pre-recorded video. Upload any dashcam/phone clip (MP4 · WebM · MOV ≤ 60 MB)
  or run the built-in 9-second dashcam sample (`public/demo/road-scan-9s.mp4`).
  Keyframes are sampled client-side (the video never leaves the browser) at
  ~3 fps, batched through the detection engine chain, and hazards are drawn as
  interpolated bounding boxes **over the playing footage** — no stills are
  generated.
- **Temporal anti-hallucination layer**: cross-frame tracking merges
  re-detections of the same physical hazard; tracks verified across ≥ 2 frames
  receive a confidence boost into the **95–98% verified band**, single-frame
  hits survive only at ≥ 90%, and every dropped item is listed with its reason
  in the Guard log (strictness presets: Standard 75% · High 85% · Max 98%).
- `POST /api/video/detect-batch` (stateless batch frame inference, 240
  frames/min/IP, honest `live-unavailable` engine reporting) and
  `POST/GET /api/video/scans` (session persistence + history) — documented in
  the OpenAPI spec and `API_GUIDE.md`.
- `VideoScan` Prisma model storing detection timelines, verified tracks and
  guard reports per scan.

## [0.2.1] — 2025

### Added
- **Admin verification workflow**: completed repairs land in `VERIFICATION_PENDING`;
  the administrator reviews before/after field evidence side-by-side, then
  verifies the work — the work order closes and the hazard is marked `RESOLVED`
  (reflected in dashboard statistics). Every verification decision is audit-logged.
- **Work-order evidence**: field crews attach timestamped before/after photos,
  resolution notes, and GPS capture from the work board.
- **Verification & closure E2E flow** (`scripts/e2e-verify-flow.ts`) exercising
  report → assign → in-progress → complete → verify → resolved.
- Documentation suite: `SYSTEM_CARD`, `MODEL_CARD`, `ADMIN_GUIDE`, `USER_GUIDE`,
  `FINAL_REPORT`, `API_GUIDE`, `PRESENTATION_OUTLINE`, `DEMO_VIDEO_SCRIPT`,
  `CONTRIBUTION_EVIDENCE_TEMPLATE`, deployment guide.
- CI (`.github/workflows/ci.yml`) and tag-driven release pipeline
  (`.github/workflows/deploy.yml`) with GHCR image publishing.

### Changed
- Single unified administrator console: dashboard, work management board,
  live map, documentation — role-scoped navigation (citizen vs administrator).
- Hazard review queue with AI bounding-box inspection, approve/reject/flag,
  duplicate merge, and priority override — all audit-logged.

## [0.2.0] — 2025

### Changed
- Rebranded to **HazardLensAI** ("See the road. Prioritize the repair."),
  refreshed logo and brand tokens (Ultra Violet `#6A00F4` / Soft Apricot
  `#FFD6A5` / Porcelain `#FCFBF8`).
- Public marketing pages are open; sign-in unlocks the full product.

### Added
- Administrator role with a dedicated ops desk replacing the generic home page.

## [0.1.0] — 2025

### Added
- Next.js 16 fullstack live demo: public landing, interactive hazard map
  (Leaflet), 3-step report wizard with AI detection preview, auth
  (register/login/refresh), citizen dashboard with notifications, admin
  command center, CSV/GeoJSON/PDF exports.
- Engines in `src/lib/rg/**`: priority scoring (0–100, explainable),
  DBSCAN clustering (60 m / 3 pts / 90 days), severity heuristic,
  duplicate detection, audit logging, rate limiting.
- Production blueprint: FastAPI + PostgreSQL/PostGIS backend (`backend/`),
  YOLOv8 + MLflow pipeline (`ml/`), Dockerfiles & compose stack
  (`infrastructure/`, `docker-compose.yml`), deploy manifests (`deploy/`).
- Bengaluru demo seed (31 hazards, 3 clusters, 6 work orders).
