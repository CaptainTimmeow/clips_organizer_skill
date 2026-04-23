# clips_organizer_skill

Local-first skill for reviewing, labeling, triaging, and organizing video footage before editing.

This repository currently ships a single Codex/Kimi-style skill named `$video-footage-labeler`. It is designed for solo-creator footage prep, not for a hosted media app or collaborative DAM workflow.

## What It Does

The skill is meant to help an agent:

- inspect a local footage folder
- infer useful metadata from filenames, transcripts, and lightweight media inspection
- propose clearer filenames
- group likely takes and alternate angles
- assign simple triage states like `select`, `best_take`, `maybe`, and `junk`
- produce a dry-run rename or organization plan before anything changes on disk

## Why This Exists

The point of this skill is to keep the workflow local and cheap.

Instead of forcing the problem into a browser product or a hosted multimodal pipeline, this skill encourages:

- direct local file inspection
- transcript-first and heuristic-first reasoning
- multimodal API usage only when it is clearly worth the cost
- explicit dry-run outputs instead of opaque background automation

## Repo Layout

This repo is intentionally simple:

- `SKILL.md`: the main skill instructions
- `agents/openai.yaml`: UI metadata for the skill
- `references/workflow.md`: naming, triage, and cost-control defaults

The skill lives at the repo root so it is easy to point a CLI skill loader at this repository.

## Main Skill

Skill name:

`video-footage-labeler`

Typical invocation:

`Use $video-footage-labeler to review a local footage folder and propose filenames, triage, and organization.`

Good prompt examples:

- `Use $video-footage-labeler to inspect ~/Videos/inbox and suggest better filenames.`
- `Use $video-footage-labeler to triage this interview shoot into selects, best takes, maybes, and junk.`
- `Use $video-footage-labeler to make a dry-run rename and folder organization plan for this project.`

## Current Scope

What the skill is good at right now:

- defining the local-first workflow
- making the rename/triage process explicit
- steering the agent away from expensive multimodal-by-default behavior
- keeping rename and apply as separate steps

What it does not include yet:

- bundled scripts for scanning folders or generating manifests
- sample fixtures for forward testing
- automatic ffprobe/transcript helpers
- dedicated install docs for any one CLI client

## Testing Notes

If you are testing this skill in a CLI:

- make sure the client can load skills from a Git repo with a root-level `SKILL.md`
- invoke the skill by name as `$video-footage-labeler`
- test it first on a small real folder, not a huge footage dump
- start with naming-only or triage-only requests before asking it to generate an apply plan

## Design Principles

This skill assumes:

- local-first is the default
- the cheapest reliable analysis path is usually best
- filenames and transcripts often get you most of the value
- rename safety matters more than aggressive automation
- a manifest or dry-run plan is often more useful than a UI

## Recommended Next Improvements

If you want this skill to become genuinely useful, the next high-value additions are:

1. Add a small script that walks a folder and emits a clip manifest with path, extension, duration, and filename-derived metadata.
2. Add a dry-run rename planner that validates duplicate targets and invalid characters.
3. Add a few realistic prompt examples and expected outputs for interviews, B-roll dumps, and screen recordings.
4. Add a light ffprobe-based helper so device metadata and duration do not rely on manual inspection.
5. Align the repo name and skill name if you want lower confusion during installation and invocation.
