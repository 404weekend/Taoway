---
name: taoway-ai-editing
description: TAOWAY cinematic editing for DaVinci Resolve with original-media T-mode trim handles, full-library re-edits, editable Text+/Fusion subtitles, AI sound effects and narrative-aware single/multi-cue BGM.
---

# TAOWAY AI Editing

Version: 1.3.0

## When to use
Use for TAOWAY travel, mountain, hiking, snowboarding and everyday-life video analysis, grading, edit decisions and DaVinci Resolve timelines. Explicit project instructions override style defaults.

## Mandatory non-destructive editing rules
1. **Original media only.** Timeline clips must reference complete original Media Pool items or original files. Never substitute a pre-trimmed FFmpeg excerpt, temporary proxy, re-encoded clip, rendered preview or flattened composite.
2. **Independent time domains.** Store source in/out frames, timeline start/duration, source and timeline rational FPS, retiming, audio sync and frame-end conventions separately. Never mistake source ranges for timeline positions.
3. **T-mode handles.** Preserve full original footage so Resolve Trim Edit Mode (T) can extend or adjust boundaries into unused source media. Actual trim extent also depends on media boundaries, adjacent clips, trim mode and retiming.
4. **Verified edits.** Never invent source paths, duration, in/out or speed. Check source media and validate timecodes on export/import.
5. **Editable deliverables.** Prefer Resolve Scripting API with original Media Pool items; otherwise generate FCPXML/XML that references the original source path. Test relinking and trim behavior in the installed Resolve version.
6. **Independent typography.** Keep speech subtitles in editable Subtitle Track/SRT; use Text+ for ordinary cinematic titles and metadata; Fusion nodes or tested presets for tracking, occlusion, masks and complex animation. Never burn in text to replacement media.
7. **Full-library re-edit.** An old timeline is one edit decision, never the source inventory for the next cut. New full re-edits consider all eligible originals in the Media Pool / source registry, including unused clips. Keep prior timelines intact and reuse source-analysis cache.
8. **Editable sound and music.** Keep original ambience, Foley/SFX, BGM and voiceover on independent editable audio tracks. Choose sounds/music around story and authentic action, not merely total duration. Source-anchor action SFX, recalculate against re-edits, verify playable assets and licensing, and never fabricate inserted audio.


## TAOWAY color identity
**Low-saturation naturalism × filmic texture × minimalist outdoor editorial.** A24 is a reference for restrained pacing, atmosphere and negative space. Patagonia is a reference for authentic wilderness and environmental storytelling. Neither requires cloning specific shots or applying one LUT to all scenes.

| Brand role | HEX | Meaning |
| --- | --- | --- |
| Rock black | `#202321` | Deep graphic accents |
| Paper white | `#F0EDE4` | Backgrounds, captions |
| Sage gray | `#828578` | Muted foliage |
| Sandstone brown | `#A58C70` | Warm landscapes |
| Mist blue | `#566C70` | Cool atmosphere |

The approximate 75% black/white versus 25% natural accents is a **brand identity** guideline, not a per-shot color formula.

### Three looks
- **Cold Wilderness:** cool gray, mist blue, muted greens, believable snow and rock detail for mountains and snowboarding.
- **Faded Memories:** restrained warm sunlight, creamy highlights, sand/earth tones and subtle grain for road trips and lifestyle.
- **Monochrome Identity:** graphic black/white, ample negative space for intro/outro, editorial graphics, experiments.

Keep moderate contrast, soft highlight roll-off and credible skin tones; avoid crushed black detail, blown snow, lurid teal/orange, oversaturated vegetation and exaggerated grain. Match clips for exposure/color before applying a cohesive project look.

## Edit language and story
- Build a sequence through departure, journey, discovery, effort, pause, arrival and reflection, rather than a constant highlights montage.
- Show wide geography, medium movement, detailed textures and human moments. Protect action continuity for riding, snowboarding and climbing.
- Use real ambient sound and deliberate quiet moments; music need not dictate every cut.
- Leave sufficient verified handles and preserve alternate candidate takes.

## Full Media Pool re-edit and versioning (v1.2)
**Invariant:** `candidate_media_set = all_eligible_original_media_in_project_library` for a new re-edit, unless explicitly restricted by the user. It must **not** be the list of clips from the old timeline.

If Media Pool contains 30 original clips and V1 uses 10, V2 must be free to choose from all 30, including the 20 unused ones. V2 may reuse some, all or none of V1’s clips. Create a new timeline with a new name and leave V1 intact.

### Data stores
- `source_registry`: stable asset IDs, original path, fingerprint, Media Pool identity, duration/FPS and eligibility/availability. One entry per original asset regardless of whether V1 used it.
- `source_analysis_cache`: keyed by file fingerprint and analysis version; keyframes, scene labels, motion/action ranges, quality ratings and proposed moments. Persist across timeline versions.
- `timeline_versions`: version ID, edit decisions, narration/music/title events, style instructions and original source IDs, not short rendered media.

Example re-edit request:
```json
{
  "operation": "create_new_timeline",
  "timeline_name": "TAOWAY_V2",
  "source_scope": "all_project_media",
  "include_previously_unused": true,
  "reuse_cached_analysis": true,
  "preserve_existing_timelines": true,
  "allow_reuse_of_prior_clips": true,
  "source_selection_policy": "independent_of_previous_timeline",
  "reference_timeline": "TAOWAY_V1",
  "reference_role": "feedback_only"
}
```

### Execution
1. Traverse original Media Pool folders or reconcile a complete source manifest. Do **not** treat `GetItemListInTrack(V1)` as the inventory.
2. Count eligible originals; note missing/offline assets. Load cached analyses, analyze only newly added/changed/uncached sources.
3. Choose and order verified original-source ranges with an independent fresh storyboard. Prior timeline can supply feedback, not a whitelist.
4. Create a new independent Resolve timeline and insert original Media Pool items, keeping T-mode trim handles.
5. Validate `eligible_source_count`, `considered_source_count`, `selected_unique_source_count`, `previously_unused_selected_count`. For unrestricted re-edit, the considered count equals eligible count, subject to documented availability limits.
6. Verify V1 is unchanged, V2 relinks to originals, audio remains synchronized, text layers are independent and trims remain adjustable.

Prohibited: reshuffling only the prior timeline’s 10 clips for an unrestricted re-edit; importing only XML-referenced files and mistakenly treating them as the complete library; using trimmed/rerendered snippets as masters; claiming to have evaluated sources without checking the manifest. Detailed tests: `references/full-library-reedit.md`.

## Subtitle and typography (v1.1)
Visual direction: quiet A24-inspired minimalism with pragmatic Patagonia-inspired location information.

**Three roles:**
- **Subtitle Track** for spoken words, transcript and translations, with correctly timed editable SRT/native captions. SRT has timing and text, not Fusion title animation.
- **Text+** on an independent title track for poetic lines, locations, dates, elevation, chapters and brand statements. Edit-page Text+ already uses Fusion’s text engine.
- **Fusion nodes / Fusion Titles** when masking, subject occlusion, text tracking, handwriting or complicated procedural animation is essential. Fusion Title usually denotes a premade Fusion template, not another text renderer.

| Preset | Use | Position | Motion |
| --- | --- | --- | --- |
| `cinematic_minimal` | Poetic line | Lower center | Fade 4–8 frames |
| `outdoor_documentary` | Location/date/route/elevation | Lower left | Static/subtle fade |
| `editorial_typography` | Chapter and editorial heading | Left/center | Fade 6–12 frames; tiny rise if justified |
| `statement_title` | Brief brand intro/outro | Center | Slow fade, use sparingly |

Starting design tokens: `#F2F0E9` off-white; Helvetica Neue/Inter for English body; Source Han Sans/PingFang for Chinese; DIN Condensed/Oswald for narrow titles; occasional EB Garamond/Cormorant for poetry. For 3840×2160 rendered output, captions around 48–60 px and chapters around 100–160 px are starting **visual targets**, not fixed Inspector values. Captions initially 9–12% above bottom; adapt to portrait safe zones. Prefer natural line breaks, 1–2 lines, no constant bouncing, opaque boxes or heavy borders. Preserve clean text-free imagery.

Store text separately from source media:
```json
{
  "event_type": "text",
  "track_role": "title",
  "text": "SECEDA / DOLOMITES / ITALY",
  "template": "outdoor_documentary",
  "timeline_start_frame": 420,
  "timeline_duration_frames": 90,
  "timeline_fps": "30000/1001",
  "implementation": "Text+",
  "editable": true
}
```
Keep transcript/caption cues separately timed; validate retiming, fonts, glyphs, 16:9/9:16 and import behavior. FCPXML/XML does not guarantee that all Text+ and Fusion node parameters survive. Deliver SRT + title event JSON + compatible templates as needed. Detailed rules: `references/subtitle-system.md`.

## AI sound effects and Music Director (v1.3)
**Music and effects serve the story, not vice versa.** AI can plan audio without a sound library but must acquire/generate real licensed playable assets before claiming they were inserted. Preference: source-recorded ambience/foley → licensed searchable audio → permitted AI-generated sound/music → human review. Save each asset's location, source, rights/attribution and acquisition status.

**SFX:** Identify genuine action on original footage (e.g., snow carve, footsteps, fabric, wind, impact). Avoid unsupported, artificial effects and overused Whooshes. Anchor action sounds to original `source_id + source_event_frame` (and duration/segment) rather than only a fixed timeline second; re-map after Trim Edit Mode adjustments, retiming, slipping, moving and full-library re-edits. Anchor non-diegetic effects to story beats or edit boundaries.

**BGM:** For a 60-second film, choose one suitably structured track by default; use two or three cues only when story chapters and emotional changes warrant it. Assess phrase boundaries, rhythmic fit/BPM, key/harmony, instrumentation, energy trajectory, dynamics and vocal clashes. Actual musical junctions require audition/verification; BPM/key alone cannot guarantee compatibility. Use transitions through short crossfade, phrase cut, natural ambience bridge or silence. Music may intentionally end before the picture. Prioritize intelligible speech and real sound.

**Track organization:** Original ambience, BGM, Foley/SFX and voiceover occupy separate editable audio layers (e.g. A1–A4). Include source in/out, timeline ranges in the timeline FPS, gain/fades, beat anchors, licensing and sync status in JSON. Insert actual audio files through supported Resolve functionality; Fairlight automation and mixing support vary by Resolve version, so verify placement and report any manual finishing needs.

**Re-edit:** When scene selection, source in/out, tempo or video duration changes, recompute the music storyboard and SFX source-to-timeline mappings. Preserve old timeline and music edit, reuse cached audio metadata, and avoid blindly looping/extending songs or leaving sounds attached to obsolete timecodes. See `references/sound-design-and-music.md` for templates, source policy and QA.

## Token-efficient AI footage analysis
1. Locally extract original metadata, timecodes, rotation, FPS, codecs, duration, audio and stable fingerprints.
2. Analyze scene cuts, blur, exposure, repeated footage, motion, silence and speech locally where feasible.
3. Generate sparse adaptive low-resolution keyframes with source IDs and timecodes instead of uploading every frame.
4. Use denser inspection or short proxies for promising movement-intensive clips, especially snowboard turns and climbing action.
5. Cache scene labels, selected actions and edit candidates by original identity and analysis version. Reuse for all future timeline versions.
6. AI outputs story/edit decision JSON, not re-encoded footage.

## Edit decision record
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
This is illustrative, not a verified media range. Derive timeline duration with rational FPS and retiming; it does not automatically equal the number of source frames. Report source ID/path, verified in/out, available original handles, timeline timebase, speed, role and grade look for each selected clip.

## Resolve import / editability QA
- Source item in Media Pool retains full original duration, even after AI chooses a short interval.
- Re-link original media, check Edit-page T mode left and right trims with available source handles, acknowledge neighboring clips and Ripple/Roll/Slip/Slide differences.
- Never link the final timeline to a low-resolution analysis proxy as master.
- Test mixed FPS, retime, audio sync, subtitles, Text+, Fusion presets and missing assets.
- XML/Scripting API support varies by installed Resolve version; do not claim success unless inspected.
- New full-library re-edit preserves earlier timelines, logs inventory counts and has access to all eligible source originals.

## Final handoff
Report narrative intent, chosen look, original-linked clip and frame-range decisions, real project/XML/JSON paths created, and verified trim/editability results. Flag untested operations rather than inventing success.

See `references/full-library-reedit.md`, `references/subtitle-system.md`, `references/sound-design-and-music.md` and `references/qa-checklist.md`.
