---
name: taoway-ai-editing
description: TAOWAY cinematic editing guidance for AI-assisted DaVinci Resolve projects. Apply TAOWAY muted naturalist film color, select story-driven outdoor footage, and create non-destructive source-linked timelines that retain handles for Trim Edit Mode (T).
---

# TAOWAY AI Editing

## When to use
Use this skill when the user asks to analyze, select, edit, color-plan, or export TAOWAY travel, mountain, hiking, snowboarding, or everyday-life videos, especially timelines destined for DaVinci Resolve. Follow explicit project instructions over stylistic defaults.

## Non-negotiable rules
1. **Original media only in final timelines.** Every edit event references the original source clip in the Media Pool or its original file path. Do not replace source material with FFmpeg-cut excerpts, rendered preview clips, flattened composites, or re-encoded snippets.
2. **Source/time separation.** Record `source_in`, `source_out`, `timeline_start`, `timeline_duration`, frame-rate/timebase, and any retime separately. Never conflate media time with timeline time.
3. **Preserve trim handles.** The editor must be able to use Resolve **T (Trim Edit Mode)** to extend or reshape cuts within the original media's available frames. Do not bake a 5-second selection into an actual 5-second media file.
4. **Frame-accurate decisions.** Perform edits in integer frames or rational timecodes, with explicit source/timeline FPS and speed settings. Account for variable frame rate, mixed FPS, slow motion, audio sync, and effects.
5. **No unnecessary source processing.** Proxies, thumbnails, scene-detection excerpts and low-resolution previews can be used for analysis but must not replace original media references in the deliverable timeline.
6. **Never hallucinate source ranges.** Only use verified filename/media IDs, source durations, timecodes and paths. Validate all ranges against the source.
7. **Deliver editability.** Prefer a native Resolve timeline through the DaVinci Resolve Scripting API where available; otherwise create FCPXML/XML with original-media links and test import. Preserve source reel/timecode and path identity.

## TAOWAY visual identity
**Guiding style:** low-saturation naturalism × cinematic film texture × minimalist outdoor editorial. A24 references guide restrained pacing and negative space; Patagonia-like references guide authentic encounters with nature. Do not force an identical LUT onto every setting.

| Role | Suggested HEX | Application |
| --- | --- | --- |
| Rock black | `#202321` | Titles, typography, deep graphic accents |
| Paper white | `#F0EDE4` | Backgrounds, zines, restrained editorial negative space |
| Sage gray | `#828578` | Muted vegetation, supporting accents |
| Sandstone brown | `#A58C70` | Warm landscapes, natural paper/earth notes |
| Mist blue | `#566C70` | Cool skies, mountains, atmospheric accents |

The suggested **brand-graphic** balance is ~75% rock-black + paper-white and ~25% natural accent colors; this is **not** a per-shot image-color quota.

### Three scene looks
- **Cold Wilderness:** low-saturation cool grays, mist blue, muted green; detailed snow and rock; controlled highlights. Mountain, winter and snowboarding scenes.
- **Faded Memories:** creamy highlights, muted earth/sand, gentle warm sunlight and subtle film texture. Road trips, everyday life, warm travel scenes.
- **Monochrome Identity:** strong but unclipped black-and-white graphic system, clean geometric typography and generous negative space. Logos, titles, bumpers, experimental sequences.

### Grade constraints
- Moderate, natural contrast; avoid crushed blacks and blown snow/highlights.
- Soft highlight roll-off; preserve shadow detail and skin realism.
- Cool shadows or warm highlights only when motivated by the actual environment.
- Restrained saturation; avoid aggressive teal/orange, artificial greens, over-orange skin and indiscriminate faded overlays.
- Optional fine grain, not coarse visual noise. Match camera sources rather than enforcing an identical white balance on every shot.
- Grade at the clip level first for exposure/color consistency, then give the project a restrained shared look.

## Narrative/editing decisions
1. Identify the **experience**: departure, movement, discovery, effort, pause, arrival, reflection. Avoid making every video just a highlights montage.
2. Favor believable motion, scale, weather, ambient sound and small actions. Protect action continuity in snowboarding and via ferrata footage.
3. Use wide environmental establishing frames, human-scale shots, details and quiet holds. Pacing should feel intentional, not artificially slow.
4. Keep enough pre/post-action handles for manual trims. For important actions, keep alternate takes or ranked ranges rather than permanently discarding them.
5. Use minimal graphics, tasteful small typography, and ambient sound. Music should support the sequence, not dictate every cut.

## Token-efficient vision workflow
1. **Local ingest:** enumerate files, FPS, duration, resolution, codecs, audio, rotation, timecode; hash/fingerprint material for caching.
2. **Local scene and activity scan:** detect cuts, motion, blur, exposure, silence/peaks, speech and duplicates using non-LLM tools when possible.
3. **Sparse previews:** create low-resolution keyframes/contact sheets with source IDs and timecodes; avoid per-frame cloud vision.
4. **Adaptive revisit:** use denser analysis (or short proxy clips) only for motion-heavy or promising ranges such as snowboard turns or climbing movements.
5. **Persist metadata:** cache source analyses and edit candidates so style revisions do not require re-analyzing the original video.
6. **Edit decision:** produce structured selections, story order, transitions, beat markers and color-look tags, not final recoded clips.
7. **Timeline assembly:** import/link source media, construct edits using source in/out frames, then validate via Resolve.

## Deliverable contract
For each selected clip include:
- Stable original-media identifier plus original path/relative path.
- Source FPS/timebase, source in/out frame numbers, verified total frame count.
- Timeline FPS/timebase and timeline start frame.
- Intended playback speed, reverse/retime details if any.
- Scene/action description, edit purpose, optional grade look and confidence.
- Before/after **available handles** calculated from original source boundaries (not invented padded media).

Example of a **decision record**, not footage to render:

```json
{
  "source_file": "Media/Snowboard/DJI_0001.MP4",
  "source_fps": "60000/1001",
  "source_total_frames": 3600,
  "source_in_frame": 1200,
  "source_out_frame_exclusive": 1500,
  "timeline_fps": "30000/1001",
  "timeline_start_frame": 240,
  "speed": 1.0,
  "look": "cold_wilderness",
  "purpose": "continuous_carving_action",
  "media_type": "original",
  "reencode": false
}
```

The timeline duration must be calculated from source duration, FPS conversion, and retiming; it is not automatically `source_out - source_in` timeline frames. The example is illustrative; do not use it as a real edit without verifying the media.

## Resolve implementation checks
- Use imported **original** Media Pool items for all timeline clips. `AppendToTimeline` or an equivalent API mechanism can populate native clips with supported source frame ranges; verify behavior and available API methods in the installed Resolve version.
- For interchange, use FCPXML (or an appropriate Resolve-readable XML) carrying original media references; check relinking on the target machine.
- After import, open an edit on the timeline and test both left- and right-side extension with **Trim Edit Mode (T)**, subject to original-media boundaries and neighboring clips. Ripple/Roll/Slip/Slide have different effects; do not guarantee arbitrary extension in all contexts.
- Check that a representative clip can extend beyond its AI-chosen boundaries; Media Pool source duration should be the **full original** duration.
- Check no unintended transcodes, proxies linked as masters, flattened replacement footage, missing media, broken timecodes or audio sync errors.
- For retimed footage, inspect speed and start/end frames carefully before claiming frame-accurate handles.

## Final response format
Report: (1) story/edit intent, (2) chosen look and grading intent, (3) sequence of original-linked clips with source timecodes, (4) deliverable project/XML/JSON paths as actually created, and (5) editability/trim-handle validation results. Clearly flag any unverified import or unavailable API rather than declaring success.

See `references/qa-checklist.md` for handoff validation.
