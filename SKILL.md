---
name: video-footage-labeler
description: Use this skill when the user wants Codex to review, label, triage, rename, or organize local video footage folders without relying on a hosted web app. This skill fits local-first footage prep, clip naming proposals, camera-aware grouping, export planning, and practical pre-editing workflows for solo creators.
---

# Video Footage Labeler

## Overview

Use this skill for local footage inbox work: inspect a folder of clips, infer useful metadata from filenames, frames, transcripts, and device metadata, propose better names, group by content or theme, and produce a safe rename or organization plan before editing.

This skill is deliberately local-first. Prefer direct file inspection, lightweight manifests, frame extraction, and explicit dry-run style outputs over building or depending on a browser product surface.

## When To Use It

Use this skill when the user asks for any of the following:

- Review a folder of raw clips before editing
- Propose better filenames for local video files
- Triage clips into selects, maybes, junk, or best takes
- Group similar takes, themes, or locations
- Organize clips into folders for Premiere, Resolve, or a general handoff
- Extract transcripts or dialogue from footage
- Inspect GPS, camera metadata, or device info
- Turn the repo's footage logic into a direct Codex workflow instead of a web app

Do not use this skill for:

- Cloud-hosted media management
- Collaborative DAM workflows
- Full editing or timeline work

## Workflow

### 1. Start from the local folder

Gather the smallest useful set of facts first:

- root folder path
- clip count and file extensions
- device type (drone, handheld gimbal, phone, etc.)
- whether transcripts, thumbnails, or sidecar metadata already exist
- whether the user wants naming only, triage only, or naming plus organization

Prefer shell inspection over guessing. Use `find`, `ffprobe`, `exiftool`, or repo scripts when available.

### 2. Inspect frames and metadata

Before proposing names, actually look at the footage:

- Extract mid-frame screenshots from representative clips in each time cluster
- Use `exiftool` to read device metadata, GPS status, beauty settings, and camera model
- Use `ffprobe` for stream analysis (detect subtitle tracks, telemetry, thumbnails)
- Do not assume the device type from filenames alone

This step often reveals the true content (e.g., a "DJI_" prefix may be an Osmo Nano handheld gimbal, not a drone).

### 3. Choose the cheapest reliable analysis path

Default to the lowest-cost path that can still solve the task:

- filename and folder heuristics first
- frame-based visual inspection next
- transcripts next, if already available or cheap to produce locally
- metadata-aware grouping (GPS, timestamps, device settings)
- multimodal image+text API calls only when the user explicitly wants deeper semantic labeling and the provider supports image input cleanly

Avoid expensive multimodal-by-default behavior for large batches. If cost could scale badly, summarize the tradeoff and prefer frame-plus-heuristics.

### 4. Extract transcripts when useful

For dialogue-heavy or event footage:

- Extract audio with `ffmpeg -vn -acodec pcm_s16le -ar 16000 -ac 1`
- Use local Whisper, Google SpeechRecognition, or another STT tool
- Note language detection challenges — specify language explicitly when known
- Transcripts help identify: interview selects, accidental clips, test takes, and content themes

### 5. Produce a reviewable clip manifest

Represent the result as a compact table or JSON/CSV-style manifest with fields such as:

- `old_path`
- `old_name`
- `proposed_name`
- `content_theme` (e.g., ROBOT_COFFEE, SKATEBOARD, DAILY_LIFE)
- `editorial_role`
- `subject`
- `person_name`
- `device`
- `location_hint`
- `triage`
- `group_id`
- `transcript_snippet`
- `notes`

Keep the output inspectable. The user should be able to understand why a proposed filename exists.

### 6. Group by content, not just date

Date-based folders are a safe default, but content-based grouping is often more useful for editing:

- Group by location or venue (mall, park, home)
- Group by activity or theme (skateboarding, robot demo, market shopping)
- Group by narrative thread (character arc, event sequence)
- Within each group, rank takes and mark the best one

When in doubt, use numbered theme folders: `01_JUNK/`, `02_STILLS/`, `03_GYM/`, `04_ROBOT_COFFEE/`, etc.

### 7. Separate naming from applying

Never rename or move footage immediately unless the user explicitly asks for execution.

Default sequence:

1. inspect
2. extract frames and metadata
3. propose names and groupings
4. surface conflicts or uncertainty
5. produce a dry-run plan
6. apply only on explicit confirmation

### 8. Keep safety explicit

Before any apply step, check:

- invalid filename characters
- duplicate targets
- extension preservation
- path traversal or destination escape
- overwrite risk
- no-op vs rename vs move vs rename+move
- verify no source files were missed in the mapping

If confidence is weak, preserve the original name or mark the clip for review instead of inventing a brittle rename.

## Output Patterns

Use one of these depending on the user request:

- Quick pass: short table with original name, proposed name, confidence, and notes
- Review manifest: JSON or CSV-ready structure for a whole folder
- Apply plan: explicit old path -> new path mapping
- Handoff summary: selected clips, best takes, likely duplicate takes, and folder suggestions
- NLE manifest: CSV or JSON with fields for Premiere, Resolve, or Final Cut Pro import (markers, metadata, bins)

For naming style, prefer practical, scan-friendly names over clever names.

## Camera And Role Guidance

Keep device detection separate from editorial role:

- device detection: what physically recorded the clip (use exiftool/ffmpeg, not filenames)
- project role: how that device is used in this project

If two cameras are the same model, keep room for instance tags like `cam-a` and `cam-b`.

Do not assume "drone" from a `DJI_` filename prefix. DJI makes Osmo handheld gimbals, Action cameras, and drones. Check the actual encoder/metadata.

## References

Read [references/workflow.md](references/workflow.md) when you need the default naming shape, triage defaults, content-grouping patterns, transcript guidance, or NLE handoff formats.
