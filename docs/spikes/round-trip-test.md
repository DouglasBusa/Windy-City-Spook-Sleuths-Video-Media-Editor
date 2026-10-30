# Phase 0 round-trip test: investigation clip → WeftCut → NLE

**Goal:** take a short investigation clip (1–3 min) all the way through import, rough cut, export, and open in an NLE.

**Key finding:** WeftCut exports video only (no FCPXML/OTIO/EDL). Use our pipeline's `cutlist.edl` + `captions.srt` for NLE hand-off.

## Quick start (recommended)

1. Download the latest installer from https://github.com/WeftCut/WeftCut/releases/latest
2. Install and launch WeftCut.
3. Create a new project.
4. Import a short clip (drag into the media pool).
5. Make a simple cut (split/delete or use silence tools).
6. Export an MP4.
7. Open the MP4 (or our EDL) in Resolve/Premiere to verify.

## Full checklist
See the detailed steps in the committed file for MCP connection, transcription options, markers, and success criteria.

## Success criteria
- Clip imports and a rough cut appears on the timeline.
- Exported MP4 plays and matches the expected duration.
- (Optional) EDL opens in an NLE with no offline media after relink.
