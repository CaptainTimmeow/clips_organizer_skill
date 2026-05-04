# clips_organizer_skill

Local-first skill for reviewing, labeling, triaging, and organizing video footage before editing.

## What It Does

This skill helps an agent:

- Inspect a local footage folder with shell tools (`find`, `ffprobe`, `exiftool`)
- Extract mid-frame screenshots for visual content analysis
- Infer device type, GPS status, and camera metadata
- Extract audio transcripts for dialogue identification
- Propose clearer, content-based filenames
- Group clips by theme, location, or narrative (not just date)
- Assign triage states: `select`, `best_take`, `maybe`, `junk`
- Produce CSV/JSON manifests for Premiere, Resolve, or custom NLE pipelines
- Generate a dry-run rename or organization plan before anything changes on disk

## Why Local-First?

- No cloud upload required
- No hosted subscription
- Direct file inspection with standard tools
- Works offline after tool installation

## Repo Layout

- `SKILL.md` — main skill instructions for Codex/Kimi-style agents
- `agents/openai.yaml` — UI metadata for the skill
- `references/workflow.md` — naming patterns, triage defaults, transcript guidance, NLE handoff formats

## Skill Name

`$video-footage-labeler`

## Quick Start

```
Use $video-footage-labeler to review a local footage folder and propose filenames, triage, and organization.
```

## License

MIT
