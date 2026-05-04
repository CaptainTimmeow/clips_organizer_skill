# Video Footage Labeler Reference

## Default Naming Shape

Use a practical filename pattern such as:

`EDITORIALROLE_Subject_TakeNN.ext`

Examples:

- `AROLL_TimIntro_Take01.mp4`
- `BROLL_CoffeePour_Take02.mov`
- `SCREENREC_FigmaHomepageFlow_Take01.mp4`

For vlog, family, or travel footage where editorial role is less relevant, use content-based naming:

`{THEME}_{Descriptor}_{Date}_TakeNN.ext`

Examples:

- `GYM_FoamRolling_Apr24_Take01.mp4`
- `ROBOT_HooLooBarista_Apr25_Take01.mp4`
- `SKATEBOARD_IndoorMall_May03_Take02.mp4`
- `DAILY_MarketShop_May03_Take03.mp4`

When the clip is ambiguous, prefer a conservative subject like:

- `UnknownScene`
- `UnknownSpeaker`
- `WideShot`
- `TestClip`
- `Accidental`

## Triage Defaults

Use these states unless the user asks for something else:

- `select`: clearly useful clip
- `best_take`: strongest clip in a repeated-take group
- `maybe`: uncertain but potentially useful
- `junk`: unusable, accidental, or duplicate with no advantage
- `unreviewed`: not yet assessed

## Content-Based Grouping

When organizing footage, prefer content themes over date folders:

Suggested folder naming:

- `01_JUNK/` — accidentals, tests, framing checks
- `02_STILLS/` — standalone photos
- `03_{THEME}/` — gym, workout, fitness
- `04_{THEME}/` — robot demo, tech experience
- `05_{THEME}/` — city streets, transit
- `06_{THEME}/` — civic plaza, architecture
- `07_{THEME}/` — shopping, retail
- `08_{THEME}/` — wonder world, family outing
- `09_{THEME}/` — modern plaza, landmarks
- `10_{THEME}/` — skateboard, sports
- `11_{THEME}/` — park, leisure, nature
- `12_{THEME}/` — daily life, market, community

Within each folder, use `TakeNN` numbering. If a clip doesn't fit an existing theme, extend the sequence rather than forcing it into the wrong bucket.

## Frame Extraction Guidance

Extract representative frames before proposing names:

```bash
ffmpeg -ss {midpoint} -i input.MP4 -frames:v 1 -q:v 2 "{original_name}_mid.jpg"
```

- Extract mid-frame (duration / 2) for content identification
- Extract from the start of each group to verify continuity
- Save frames to a temporary `.analysis/frames/` folder
- Name frames `{original_filename}_mid.jpg` so they map 1:1 to source clips
- View frames before deciding on themes and names

## Transcript Analysis Guidance

Extract audio and transcribe for dialogue-rich footage:

```bash
ffmpeg -i input.MP4 -vn -acodec pcm_s16le -ar 16000 -ac 1 output.wav
```

- Use 16kHz mono WAV for best STT compatibility
- Specify language explicitly (e.g., `--language Chinese` for Whisper)
- Transcribe 30-second segments from the middle of clips first
- Flag clips with recognizable dialogue for editorial priority
- For bilingual workflows, keep both original and translated snippets:
  - `transcript_cn`: original language transcript
  - `transcript_en`: English translation or transliteration
- Note: ambient noise/music may yield empty transcripts

## Metadata Inspection

Prefer `exiftool` over `ffprobe` for camera metadata:

```bash
exiftool -GPSLatitude -GPSLongitude -GPSAltitude -Make -Model -Duration -FileName input.MP4
```

Common findings:
- Device model (e.g., `DJI Osmo Nano` vs `DJI Mavic 3`)
- GPS status (`Invalid`, `Void`, or actual coordinates)
- Beauty/filter settings (face slimming, whitening, etc.)
- Original file path from device storage
- Sensor temperature, serial number

For DJI files, look for proprietary telemetry streams (`djmd`, `dbgi`) via `ffprobe` when GPS is not in standard tags.

## Cost Control Guidance

Prefer these analysis tiers in order:

1. filename and folder heuristics
2. frame-based visual inspection (local screenshot extraction)
3. metadata-aware grouping (exiftool, timestamps, device settings)
4. transcript-based reasoning
5. multimodal image+text API calls

Use multimodal calls only when:

- the user explicitly asks for deeper semantic labeling
- the provider clearly supports image input
- the folder size is small enough that cost is acceptable

Avoid multimodal API-heavy workflows when:

- the user is still validating the product direction
- the folder contains many clips
- frame and filename signals are already good enough

## Safe Apply Rules

Before executing a rename or move plan:

- preserve original extensions
- keep destination paths inside the intended root unless exporting
- reject duplicate target paths
- show a dry-run preview first
- call out clips with low confidence instead of forcing a rename
- verify every source file has a mapping (no stragglers)
- verify no target paths already exist

## NLE Handoff Formats

Generate these manifests for non-linear editors:

**CSV Marker Import** (Premiere, DaVinci Resolve):
- Columns: `clip_name`, `marker_name`, `description`, `in_time`, `out_time`, `color`
- Import via Extensions panel or third-party tools (Excalibur, Marker Import)

**CSV Manifest** (Premiere marker import, spreadsheet review):
- Columns: `old_name`, `new_name`, `folder`, `triage`, `notes`, `transcript_cn`, `transcript_en`, `marker_color`
- Use for human review before applying renames, or for NLE marker import

**JSON Manifest** (Custom scripts, Kyno, Silverstack):
- Fields: `old_name`, `new_name`, `folder`, `triage`, `notes`, `transcript_snippet`
- Use for auto-populating bins, metadata columns, or rough-cut timelines

**Batch List TXT** (Premiere native):
- Older but supported natively by Premiere Pro
- Good for simple tapeless workflows

## Session Output Structure

During an active labeling session, produce a sidecar `.analysis/` folder inside the footage root:

```
.analysis/
  frames/
    DJI_20260503123456_0063_D_mid.jpg
    DJI_20260503123503_0064_D_mid.jpg
    ...
  audio/
    DJI_20260503123456_0063_D.wav
    ...
  manifest_premiere.csv
  manifest_premiere.json
```

- `frames/`: mid-frame screenshots for visual inspection
- `audio/`: 16kHz mono WAV extracts for transcription
- `manifest_premiere.csv`: human-reviewable spreadsheet with rename plan, triage, and transcripts
- `manifest_premiere.json`: machine-readable version of the same data

This keeps all generated artifacts separate from the original footage and makes cleanup easy.

## Good Skill Triggers

This skill should trigger on prompts like:

- "Review this footage folder and suggest better filenames"
- "Help me triage these clips before editing"
- "Make a local-first rename plan for my video dump"
- "Turn this footage prep workflow into a Codex-native process"
- "Use the repo logic, but skip the web app and just help me organize the files"
- "Extract transcripts from these clips"
- "What locations are in this footage?"
- "Group my clips by content, not by date"
