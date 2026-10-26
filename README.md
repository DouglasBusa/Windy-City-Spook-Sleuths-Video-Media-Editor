# Windy City Spook Sleuths — Video Media Editor

Open-source, local-first AI-assisted video editor focused on:

- Paranormal investigation rough cuts (talking-head, long footage, EVP-friendly analysis)
- Social media content and reels (vertical formats, hooks, captions, motifs)
- Agent-driven editing with specialized bots to keep token costs low and reduce hallucination

## Strategy: Fork WeftCut (MIT)

We are forking [WeftCut](https://github.com/WeftCut/WeftCut) as the foundation.

**Confirmed fork:** [https://github.com/DouglasBusa/WeftCut](https://github.com/DouglasBusa/WeftCut)

**Why WeftCut?**
- MIT license (safe to fork and customize)
- Full desktop NLE (timeline, keyframes, effects, captions, audio mixing)
- Native MCP support so agents/bots can drive the timeline while you watch and undo
- Motifs for agent-written animated titles, lower-thirds, and reel hooks
- Local processing + professional export (H.264, HEVC, AV1, ProRes, etc.)
- Works as a normal editor even without an agent

**What we will add on top:**
- Investigation-specific tools (silence/pause detection tuned for quiet sessions, audio-event / EVP tagging, project templates)
- Social media / reels presets (9:16 framing, caption styles for Reels/TikTok, hook generation motifs, batch export)
- Specialized bot workflows (Architect, Pipeline Coder, Researcher) that issue structured tool calls instead of free-form generation
- Low-token text-only planning over transcripts

## Current Status

- Architecture document: [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
- Customization roadmap: [docs/CUSTOMIZATION-ROADMAP.md](docs/CUSTOMIZATION-ROADMAP.md)
- WeftCut fork: [https://github.com/DouglasBusa/WeftCut](https://github.com/DouglasBusa/WeftCut)
- Specialized bots are already set up for this project

## Next Steps

1. Phase 0 audit of the fork (license, MCP surface, extension points)
2. Customize branding, add investigation + reel templates
3. Connect the specialized bots via MCP
4. Build first investigation rough-cut and reel export workflows

## How to run (once ready)

1. Clone the fork: `git clone https://github.com/DouglasBusa/WeftCut.git`
2. Follow WeftCut setup (desktop install from releases or build from source)
3. Connect an MCP client / Grok bot
4. Import footage → prompt for rough cut or reel → review in the timeline → export

See `docs/ARCHITECTURE.md` and `docs/CUSTOMIZATION-ROADMAP.md` for the full design.
