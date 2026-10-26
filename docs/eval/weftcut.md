# WeftCut evaluation (Phase 0)

- **Upstream:** https://github.com/WeftCut/WeftCut
- **Commit examined:** `b159603065c97b44e5f29cd5c718367c5da38adb`
- **Evaluated:** 2026-10-26

## Key findings

### License
MIT for WeftCut code. FFmpeg is LGPL (in-process decode) + GPL (CLI sidecar). Compatible with forking if GPL stays process-isolated.

### Maintenance
Single-maintainer project (UncleChair, ~3750 commits). Active recent releases (v0.5.3 on 2026-10-23). Pre-1.0.

### Local-only
Editing, MCP, pause detection, and analysis are local. Optional cloud transcription/vision. Update check hits the network. No required cloud for core editing.

### MCP tools relevant to us
- `detect_pauses` / `remove_pauses` — silence detection (use carefully on EVP sessions)
- `analyze_audio` — onsets and energy changes
- `transcribe_clip` — local whisper.cpp option
- `apply_transcripts` / `restyle_captions` / `export_captions`
- `update_composition` — can set 1080×1920
- `create_tacks` — review notes (good for human review of possible events)
- Motifs and export presets available

### Caveats for investigation + reels
- `remove_pauses` will cut quiet EVP listening periods by default — lock tracks or mark-only first.
- No built-in protected regions; we must enforce that ourselves.
- No NLE timeline export (FCPXML/OTIO/EDL) — video export only.
- Captions are burn-in only in export.
- Single-maintainer risk; consider pinning a release tag.

Full detailed inventory and sources are in the committed file.
