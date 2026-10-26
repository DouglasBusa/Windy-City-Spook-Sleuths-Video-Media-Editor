# WeftCut extension points — Phase 0 evaluation

**Subject:** [WeftCut/WeftCut](https://github.com/WeftCut/WeftCut) (official org repo)
**Revision read:** `main` @ `b159603065c97b44e5f29cd5c718367c5da38adb`
**License:** MIT (root `LICENSE`); bundled FFmpeg under LGPL/GPL.
**Status:** Pre-1.0, one-person project — tool names may change.

## Summary

WeftCut's primary extension surface is its **localhost MCP server**. Almost all investigation and reels features can be driven from outside with zero core changes:

- Silence modes via `detect_pauses` / `remove_pauses` + Tacks for review
- Audio events via `analyze_audio` + markers/Tacks
- 9:16 via `set_composition` + crop/keyframe transforms
- Captions via `apply_transcripts` or subtitle import
- Branded overlays via user Motifs (HTML/JS)
- Export presets via MCP

Real gaps needing core work: true in-app audio-event classification, automatic subject tracking, batch markers MCP tool (small).

## Recommended Phase 0 spikes

1. Build & connect MCP client (confirm tool names)
2. Rough-cut import from our pipeline
3. Silence mode parameter sets (EVP-safe)
4. Audio event tagging via external detector + markers
5. Reel export preset (1080×1920)
6. One branded Motif (lower third / EVP stinger)
7. Caption round-trip

See full analysis in the committed file for citations and detailed mapping.
