# ADR 0001 — Fork WeftCut as the editing core

- **Status:** Proposed (pending Phase 0 verification)
- **Date:** 2026-10-26
- **Deciders:** Dougie (owner), Video Editor Architect
- **Inputs:** Spook Sleuths Video Researcher (eval), Media Pipeline Coder (extension-point audit)
- **Related:** `docs/ARCHITECTURE.md`, `docs/CUSTOMIZATION-ROADMAP.md`
- **Fork:** https://github.com/DouglasBusa/WeftCut

## Context
We need a local-first, MCP-driven editing core for rough cuts of talking-head and investigation footage, plus Reels/TikTok output. Our plan has two tracks (investigation tools, social presets) that need custom silence modes, audio-event tags, digests, 9:16 reframing, captions, and batch export. The options are:

1. **Fork WeftCut** and add our features as modules inside it.
2. **Wrap WeftCut:** use it unmodified as a dependency and drive it from our own MCP server.
3. **Build our own** thin MCP server over ffmpeg + OpenTimelineIO.

## Decision
**Recommend option 1, forking WeftCut**, as long as the Phase 0 checks below pass. Our features live in separate modules, plugins, or presets so the fork stays close to upstream and merges stay cheap.

**Switch to option 2, wrapping, if any of these turn out to be true:**
- WeftCut's own MCP tools and plugin hooks already cover what we need (silence, markers, export), so a fork adds upkeep without benefit.
- Upstream is very active and changes core files often, so rebasing the fork would be expensive.
- The license allows use but makes distributing modified copies a problem (for example copyleft terms we don't want to take on).

**Fall back to option 3** if WeftCut can't run fully locally, has no usable license, is unmaintained, or can't export an editable timeline (FCPXML/OTIO/EDL).

## Consequences
**Positive:** faster start on the timeline and editing UI, MCP tooling already there, and features we can send back upstream.
**Negative:** we take on the cost of tracking upstream, inherit any of its architectural limits, and depend on its license.
**Mitigations:** keep the fork thin, keep an `upstream` remote, rebase monthly, put our code under `wcss/` modules, and keep presets in YAML.

## Phase 0 exit criteria

| # | Criterion | Owner | Status |
|---|---|---|---|
| 1 | License confirmed compatible with fork + distribution | Researcher | ⏳ Not verified |
| 2 | Runs fully locally (no required cloud calls) | Researcher | ⏳ Not verified |
| 3 | Upstream maintenance activity reviewed (recent commits/releases) | Researcher | ⏳ Not verified |
| 4 | MCP tool inventory documented (`docs/eval/weftcut.md`) | Researcher | ⏳ Not started |
| 5 | Extension points mapped (`docs/eval/weftcut-extension-points.md`) | Coder | ⏳ Not started |
| 6 | Supports editable timeline export (FCPXML/OTIO/EDL) | Coder | ⏳ Not verified |
| 7 | Fork created with `upstream` remote | Coder | ✅ Fork exists at DouglasBusa/WeftCut (upstream remote not yet confirmed) |
| 8 | CI green on fork | Coder | ⏳ Not started |
| 9 | One sample investigation clip round-trips (ingest, cut, export, open in NLE) | Coder | ⏳ Not started |
| 10 | ADR status moved to Accepted | Architect + Dougie | ⏳ Waiting on 1–9 |

**Accept when:** criteria 1–3 and 6 pass and 9 succeeds. If 1, 2, or 6 fails, revise this ADR to option 2 or 3.

## Next actions
1. Researcher: complete criteria 1–4.
2. Coder: complete criteria 5–9.
3. Architect: update this ADR with the findings and propose Accepted or a revised option.
