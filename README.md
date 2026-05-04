# clips_organizer_skill

Local-first skill for reviewing, labeling, triaging, and organizing video footage before editing.

This repository ships a single Codex/Kimi-style skill named `$video-footage-labeler`. It is designed for solo-creator footage prep, not for a hosted media app or collaborative DAM workflow.

## What It Does

The skill helps an agent:

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
- Multimodal API usage only when clearly worth the cost

## Repo Layout

This repo is intentionally simple:

- `SKILL.md`: the main skill instructions for Codex/Kimi-style agents
- `agents/openai.yaml`: UI metadata for the skill
- `references/workflow.md`: naming patterns, triage defaults, transcript guidance, NLE handoff formats, and cost-control guidance

The skill lives at the repo root so it is easy to point a CLI skill loader at this repository.

## Main Skill

Skill name:

`video-footage-labeler`

Typical invocation:

```
Use $video-footage-labeler to review a local footage folder and propose filenames, triage, and organization.
```

Good prompt examples:

- `Use $video-footage-labeler to inspect ~/Videos/inbox and suggest better filenames.`
- `Use $video-footage-labeler to triage this interview shoot into selects, best takes, maybes, and junk.`
- `Use $video-footage-labeler to make a dry-run rename and folder organization plan for this project.`
- `Use $video-footage-labeler to extract transcripts and group clips by content theme.`

## Current Scope

What the skill is good at right now:

- Defining the local-first workflow
- Making the rename/triage process explicit
- Steering the agent away from expensive multimodal-by-default behavior
- Keeping rename and apply as separate steps
- Guiding frame extraction, metadata inspection, and transcript extraction

What it does not include yet:

- Bundled scripts for scanning folders or generating manifests
- Sample fixtures for forward testing
- Dedicated install docs for any one CLI client

## Testing Notes

If you are testing this skill in a CLI:

- Make sure the client can load skills from a Git repo with a root-level `SKILL.md`
- Invoke the skill by name as `$video-footage-labeler`
- Test it first on a small real folder, not a huge footage dump
- Start with naming-only or triage-only requests before asking it to generate an apply plan

## Design Principles

This skill assumes:

- Local-first is the default
- The cheapest reliable analysis path is usually best
- Filenames, frames, and transcripts often get you most of the value
- Rename safety matters more than aggressive automation
- A manifest or dry-run plan is often more useful than a UI

## License

MIT
