# Video Footage Labeler Reference

## Default Naming Shape

Use a practical filename pattern such as:

`EDITORIALROLE_Subject_TakeNN.ext`

Examples:

- `AROLL_TimIntro_Take01.mp4`
- `BROLL_CoffeePour_Take02.mov`
- `SCREENREC_FigmaHomepageFlow_Take01.mp4`

When the clip is ambiguous, prefer a conservative subject like:

- `UnknownScene`
- `UnknownSpeaker`
- `WideShot`

## Triage Defaults

Use these states unless the user asks for something else:

- `select`: clearly useful clip
- `best_take`: strongest clip in a repeated-take group
- `maybe`: uncertain but potentially useful
- `junk`: unusable, accidental, or duplicate with no advantage
- `unreviewed`: not yet assessed

## Cost Control Guidance

Prefer these analysis tiers in order:

1. filename and folder heuristics
2. transcript-based reasoning
3. metadata-aware grouping
4. multimodal image+text API calls

Use multimodal calls only when:

- the user explicitly asks for deeper semantic labeling
- the provider clearly supports image input
- the folder size is small enough that cost is acceptable

Avoid multimodal API-heavy workflows when:

- the user is still validating the product direction
- the folder contains many clips
- transcript and filename signals are already good enough

## Safe Apply Rules

Before executing a rename or move plan:

- preserve original extensions
- keep destination paths inside the intended root unless exporting
- reject duplicate target paths
- show a dry-run preview first
- call out clips with low confidence instead of forcing a rename

## Good Skill Triggers

This skill should trigger on prompts like:

- "Review this footage folder and suggest better filenames"
- "Help me triage these clips before editing"
- "Make a local-first rename plan for my video dump"
- "Turn this footage prep workflow into a Codex-native process"
- "Use the repo logic, but skip the web app and just help me organize the files"
