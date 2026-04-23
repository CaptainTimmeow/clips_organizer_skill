---
name: video-footage-labeler
description: Use this skill when the user wants Codex to review, label, triage, rename, or organize local video footage folders without relying on a hosted web app. This skill fits local-first footage prep, clip naming proposals, camera-aware grouping, export planning, and practical pre-editing workflows for solo creators.
---

# Video Footage Labeler

## Overview

Use this skill for local footage inbox work: inspect a folder of clips, infer useful metadata from filenames and transcripts, propose better names, group likely takes, and produce a safe rename or organization plan before editing.

This skill is deliberately local-first. Prefer direct file inspection, lightweight manifests, and explicit dry-run style outputs over building or depending on a browser product surface.

## When To Use It

Use this skill when the user asks for any of the following:

- Review a folder of raw clips before editing
- Propose better filenames for local video files
- Triage clips into selects, maybes, junk, or best takes
- Group similar takes or alternate angles
- Organize clips into folders for Premiere, Resolve, or a general handoff
- Turn the repo’s footage logic into a direct Codex workflow instead of a web app

Do not use this skill for:

- Cloud-hosted media management
- Collaborative DAM workflows
- Full editing or timeline work

## Workflow

### 1. Start from the local folder

Gather the smallest useful set of facts first:

- root folder path
- clip count and file extensions
- whether transcripts, thumbnails, or sidecar metadata already exist
- whether the user wants naming only, triage only, or naming plus organization

Prefer shell inspection over guessing. Use `rg --files`, `find`, `ffprobe`, or repo scripts when available.

### 2. Choose the cheapest reliable analysis path

Default to the lowest-cost path that can still solve the task:

- filename and folder heuristics first
- transcripts next, if already available or cheap to produce locally
- frame/image-based multimodal calls only when the user explicitly wants deeper semantic labeling and the provider supports image input cleanly

Avoid expensive multimodal-by-default behavior for large batches. If cost could scale badly, summarize the tradeoff and prefer transcript-plus-heuristics.

### 3. Produce a reviewable clip manifest

Represent the result as a compact table or JSON/CSV-style manifest with fields such as:

- `old_path`
- `old_name`
- `proposed_name`
- `editorial_role`
- `subject`
- `person_name`
- `device`
- `triage`
- `group_id`
- `notes`

Keep the output inspectable. The user should be able to understand why a proposed filename exists.

### 4. Separate naming from applying

Never rename or move footage immediately unless the user explicitly asks for execution.

Default sequence:

1. inspect
2. propose names
3. surface conflicts or uncertainty
4. produce a dry-run plan
5. apply only on explicit confirmation

### 5. Keep safety explicit

Before any apply step, check:

- invalid filename characters
- duplicate targets
- extension preservation
- path traversal or destination escape
- overwrite risk
- no-op vs rename vs move vs rename+move

If confidence is weak, preserve the original name or mark the clip for review instead of inventing a brittle rename.

## Output Patterns

Use one of these depending on the user request:

- Quick pass: short table with original name, proposed name, confidence, and notes
- Review manifest: JSON or CSV-ready structure for a whole folder
- Apply plan: explicit old path -> new path mapping
- Handoff summary: selected clips, best takes, likely duplicate takes, and folder suggestions

For naming style, prefer practical, scan-friendly names over clever names.

## Camera And Role Guidance

Keep device detection separate from editorial role:

- device detection: what physically recorded the clip
- project role: how that device is used in this project

If two cameras are the same model, keep room for instance tags like `cam-a` and `cam-b`.

## References

Read [references/workflow.md](references/workflow.md) when you need the default naming shape, triage defaults, or guidance on when to avoid multimodal API usage.
