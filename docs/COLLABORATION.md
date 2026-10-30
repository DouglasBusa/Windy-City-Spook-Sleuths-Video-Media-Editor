# Collaboration Patterns for a Small Team (local-first)

Status: draft. Upstream WeftCut MCP is localhost-only with no multi-user features. These patterns add team workflows **around** it without exposing it to the network or forking its core.

## Principle
One person "drives" WeftCut at a time on their own machine. The team collaborates on **small text artifacts** (digests, edit plans, notes, presets), and everyone regenerates media locally. Media is shared and stored separately, never edited in place by two people.

## 1. Shared project layout (Git for text, separate storage for media)
```
project/
  project.yaml        # session metadata, presets, media manifest (hashes)
  digests/            # transcript digests (md + json)
  plans/              # edit plans: ordered segment IDs + notes
  markers/            # event tags, reviewer flags
  notes/              # investigator notes per session
  exports/            # small: FCPXML/OTIO/EDL, manifests (no renders)
media/                # NOT in Git: raw footage + proxies
```
- **Git** (the main repo or a per-case repo) tracks everything except `media/`.
- **Media** lives on a shared drive, NAS, or synced folder (Google Drive, Syncthing). It's referenced by **file hash** in `project.yaml`, so paths can differ per machine.
- Optional: Git LFS for small proxies only, never raw footage.

## 2. Edit plans as the hand-off unit
- Edits are saved as `plans/<name>.json`: segment IDs, order, notes, and author. Not a binary project file.
- Anyone can open a plan in their own WeftCut and get the same cut, since timestamps come from the shared segment store.
- Plans diff cleanly in pull requests, so reviewers see "dropped s0142, moved s0310 before s0288" instead of an opaque file change.

## 3. Roles and hand-off flow
| Step | Who | Artifact |
|---|---|---|
| Ingest + transcribe | Media lead (machine with GPU) | `digests/`, segment store, proxies |
| Log + flag moments | Investigators | `markers/`, `notes/` |
| Rough cut | Editor | `plans/rough_v1.json` |
| Review | Team | PR comments or `markers/review_*.json` |
| Social clips | Social lead | `plans/reels_*.json`, batch-export manifest |
| Final polish | Editor | FCPXML/OTIO into Resolve/Premiere |

A simple `CASE-STATUS.md` or GitHub Project board tracks which step each session is in.

## 4. Thin wrappers (low risk, our repo only)
- **`wcss sync`:** pulls Git and verifies media hashes against the manifest. Reports missing files and doesn't download or delete anything automatically.
- **`wcss plan apply <plan.json>`:** loads a plan into the local WeftCut through its localhost MCP. This is the only path into WeftCut.
- **`wcss plan export`:** saves the current WeftCut cut back to a plan JSON for committing.
- **`wcss review <plan>`:** renders a low-res review MP4 with burned-in segment IDs and timecodes, so reviewers can give feedback like "cut s0142" without running WeftCut.
- **Lock file:** `plans/<name>.lock` holds the owner and time, committed while someone is editing that plan. Advisory only, but it prevents most overwrite conflicts.

## 5. Review without WeftCut
- Share the review MP4 plus `digest.md`. Reviewers comment by segment ID in a PR, a GitHub issue, or a `markers/review_<name>.json` file.
- Event markers from audio tagging travel as plain JSON and show up as markers in each person's NLE timeline export.

## 6. What we deliberately avoid (for now)
- Exposing WeftCut MCP beyond localhost (no tunnels, no port-forwarding). If remote access is ever needed, use a VPN like Tailscale to reach one person's machine, with that person's approval, as a documented exception.
- Two people editing the same plan at once. Use branches plus the lock file instead.
- Committing raw media or renders to Git.
- Forking WeftCut core to add users, auth, or sync.

## 7. Bot support
- **Architect:** owns this doc and the plan/marker JSON schemas.
- **Coder:** builds the `wcss` wrapper commands and schema validation in CI. A plan that references unknown segment IDs fails the check.
- **Runtime bots:** read digests and write plan JSON only, so their output goes through the same PR review as a person's.

## Rollout
1. Document the folder layout + plan schema (no code).
2. `wcss plan export/apply` + schema check in CI.
3. `wcss review` renders + lock files.
4. `wcss sync` hash checks.
