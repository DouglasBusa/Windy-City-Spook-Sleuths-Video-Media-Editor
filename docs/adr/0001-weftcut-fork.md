# ADR 0001 — Fork WeftCut as the editing core

- **Status:** Proposed (Phase 0 checks mostly complete; timeline export is a known gap)
- **Date:** 2026-10-26 (updated 2026-10-29)
- **Deciders:** Dougie (owner), Video Editor Architect
- **Inputs:** Spook Sleuths Video Researcher (eval), Media Pipeline Coder (extension-point audit)
- **Related:** `docs/ARCHITECTURE.md`, `docs/CUSTOMIZATION-ROADMAP.md`
- **Fork:** https://github.com/DouglasBusa/WeftCut
- **Upstream pin:** v0.5.3 / commit b159603

## Context
We need a local-first, MCP-driven editing core for rough cuts of talking-head and investigation footage, plus Reels/TikTok output. Our plan has two tracks (investigation tools, social presets) that need custom silence modes, audio-event tags, digests, 9:16 reframing, captions, and batch export. The options are:

1. **Fork WeftCut** and add our features as modules inside it.
2. **Wrap WeftCut:** use it unmodified as a dependency and drive it from our own MCP server.
3. **Build our own** thin MCP server over ffmpeg + OpenTimelineIO.

## Decision
**Recommend option 1, forking WeftCut**, with the conditions below. Our features live in separate modules, plugins, or presets so the fork stays close to upstream and merges stay cheap.

**Phase 0 blocker status (as of 2026-10-29, based on b159603 / v0.5.3):**
- License: CLEARED (MIT; keep notices; FFmpeg LGPL/GPL obligations apply to distributed builds).
- Local-only core editing: CLEARED (editing, local whisper, pause detection, export are local; optional update check and cloud TTS are not required).
- Editable timeline export (FCPXML/OTIO/EDL): OPEN GAP — video export + separate SRT/VTT only. Not a hard blocker for Phase 1; keep as roadmap item.

**Switch to option 2 or 3 only if** upstream becomes unmaintainable or we need full NLE interchange sooner than we can build it ourselves.

## Consequences
**Positive:** faster start on the timeline and editing UI, MCP tooling already there, and features we can send back upstream.
**Negative:** we take on the cost of tracking upstream, inherit any of its architectural limits, and depend on its license. No native NLE timeline export yet.
**Mitigations:** keep the fork thin, keep an `upstream` remote, rebase monthly, put our code under `wcss/` modules, and keep presets in YAML. Plan an OTIO/FCPXML exporter later.

## Phase 0 exit criteria

| # | Criterion | Owner | Status |
|---|---|---|---|
| 1 | License confirmed compatible with fork + distribution | Researcher | ✅ Cleared (MIT + FFmpeg conditions) |
| 2 | Runs fully locally (no required cloud calls) | Researcher | ✅ Cleared (core editing local) |
| 3 | Upstream maintenance activity reviewed (recent commits/releases) | Researcher | ✅ Reviewed (active, single-maintainer) |
| 4 | MCP tool inventory documented (`docs/eval/weftcut.md`) | Researcher | ✅ Done |
| 5 | Extension points mapped (`docs/eval/weftcut-extension-points.md`) | Coder | ✅ Done |
| 6 | Supports editable timeline export (FCPXML/OTIO/EDL) | Coder | ⚠️ Gap — video + SRT only |
| 7 | Fork created with `upstream` remote | Coder | ✅ Fork exists (upstream remote still recommended) |
| 8 | CI green on fork | Coder | ⏳ Not started |
| 9 | One sample investigation clip round-trips (ingest, cut, export, open in NLE) | Coder | ⏳ In progress |
| 10 | ADR status moved to Accepted | Architect + Dougie | ⏳ Pending round-trip |

**Accept when:** criteria 1–5 pass and the round-trip (9) succeeds. Timeline export gap is accepted for now.

## Next actions
1. Complete the round-trip test with a short investigation clip.
2. Commit collaboration patterns and start Phase 1 EVP-safe silence work.
3. Update this ADR to Accepted once the round-trip succeeds.
