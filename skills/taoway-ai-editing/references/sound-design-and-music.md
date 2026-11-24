# TAOWAY Sound Design & AI Music Director (v1.3)

## Objective
Create a narrative-aware soundscape for AI-edited DaVinci Resolve timelines even if the user has no prebuilt SFX or music library. **Sound editorial planning is not the same as possessing a playable audio file.** Never claim to have inserted/created sound unless real audio has been acquired or generated, linked to the project, and verified in the timeline.

## Source acquisition priority
1. **Original location audio:** recover usable footsteps, snowboard edges, wind, clothes, breathing, machinery, road sound. Preserve provenance, authentic timing, camera mic constraints and ambience. Avoid duplicating an original sound atop itself.
2. **Licensed searchable recordings:** search available sound-effect/music providers or user-supplied folders using descriptive tags, duration, sound characteristics, audio previews and license restrictions. Record the asset URL/provider/creator, license, attribution requirements, allowed platforms, duration of rights, track ID and local file path. Never claim an online preview is licensed or downloaded merely because search returned it.
3. **Generated original audio:** when a permitted connected sound/music generation tool exists, send explicit prompt, desired seconds, instrumentation, intensity, location, no unwanted dialogue/melody, loop requirement, and seed if supported. Save actual output and metadata. If tools unavailable, deliver a source/search/generation brief and a **pending** event, not silent or fabricated media.
4. **Human selection:** allow user to approve/replace candidate tracks or SFX in the Resolve Media Pool.

### SFX logic
Analyze shot motion, original audio, audio transient cues, environments, visible collisions, movement, context and emotional significance:
- Snowboard: board-on-snow carving texture, light wind, clothing friction, occasional landings; avoid gratuitous whooshes.
- Mountain hike: footsteps on identifiable ground, clothes/backpack, gentle wind, distant nature, breathing; do not introduce audible wildlife not supported by location/context.
- Travel/road: wheel texture, car door where depicted, cabin ambience, subtle transitions.
- Documentary/minimal film: retain environmental continuity and dramatic silence. Effect should enhance, not contradict the footage.
- When action cannot be confidently identified, mark proposal uncertain and request review rather than invent timing.

### Action anchoring / trim-robust synchronization
Store SFX as an event anchored to `source_id + source_event_frame` and optionally a bounding interval, not merely a fixed `timeline_start`. If a clip is trimmed, moved, slipped, split, retimed or re-edited, remap each sound event using the exact source→timeline mapping, excluding source events outside selected range. Reverse/variable speed require segment-level mapping. If the audio sound is editorial (transition or atmosphere), anchor it to a timeline story beat/sequence boundary instead. Never silently keep old audio timings after picture changes.

SFX example (illustrative values, not detected events):
```json
{
  "event_type": "sfx",
  "source_id": "DJI_0001",
  "source_event_frame": 912,
  "anchor_type": "source_action",
  "effect": "snowboard_edge_carve",
  "sound_asset_id": null,
  "acquisition_status": "pending",
  "timeline_start_frame": null,
  "target_duration_frames": 54,
  "track_role": "foley_sfx",
  "gain_db": -12,
  "fade_in_frames": 2,
  "fade_out_frames": 8,
  "confidence": 0.82
}
```
Numbers in examples are planning targets until validated against original media/audio.

## AI Music Director: story before songs
- Build **story beats first**: departure → development → tension/effort → reveal → reflection/silence. Tag each beat with time range, energy curve (0–1), mood, pace, narration, environment and musical role.
- For ~60s edits prefer **one coherent licensed track or properly scoped source range** that supports the desired emotional arc. Do not assume a song has exactly a usable 60-second excerpt; inspect intro, builds, phrases and end, and respect its legal usage.
- Use multiple BGM segments when genuine emotional chapters justify them. Total program duration may be ~60s even if there are overlaps for crossfades, momentary gaps for ambience, or an intentionally quiet ending.
- Assess **BPM and half/double-time relations**, **key/harmonic compatibility**, instrumentation, phrase/downbeat boundaries, intensity, texture, melodic competition, lyrical content and narration masking.
- BPM/key compatibility is *insufficient* by itself. Audition proposed junctions; avoid harsh melody/harmony overlaps, clashing bass, incompatible reverb spaces and discontinuous loudness.
- Transition options: phrase-aligned cut, short constant-power (where appropriate) crossfade, amplitude fade, percussive pickup, atmospheric/field-sound bridge or short deliberate silence. Do not crossfade tracks blindly.
- Dialogue/voiceover gets priority; apply manual or automation-based ducking, focus on intelligibility and final loudness/true peak for delivery platform without inventing fixed mastering thresholds.
- Reject incompatible tracks and fall back to a single cue or silence. Do not force three tracks into every minute.
- Keep a human-editable BGM source on an independent audio track; avoid prematurely bouncing all music into one flattened clip.

### Example 60-second narrative music plan (illustrative)
| Time | Story beat | Audio recommendation |
| --- | --- | --- |
| 0–18 s | Preparation and departure | Sparse ambient/piano with real steps/wind |
| 18–43 s | Hike / effort / discovery | Organic crescendo or rhythmic instrumental |
| 43–54 s | Mountain reveal | Warm atmospheric release |
| 54–60 s | Ending | Natural ambience or silence, soft brand title |
If using three BGM cues, validate the transitions, source music rights, actual audio and program duration. The timeline schedule is an intent, not proof of available audio.

## Audio tracks and editorial layers
Suggested independent tracks (adapt to project):
- A1: Original location ambience and native sync audio
- A2: BGM (multiple segments permitted)
- A3: Foley / SFX
- A4: Narration / voiceover
Actual track numbers can vary. Separate tracks support fades, slip, trim, gain, EQ, limiting, ambience and dialogue ducking. Store musical and sound events in structured JSON; keep original audio files untrimmed where possible and set edit ranges non-destructively.

### Music event example
```json
{
  "event_type": "bgm",
  "story_beat": "ascent",
  "asset_id": "licensed_track_B",
  "licensing_status": "verified",
  "source_in_seconds": 45.0,
  "source_out_seconds": 70.0,
  "timeline_in_frame": 540,
  "timeline_out_frame_exclusive": 1290,
  "timeline_fps": "30/1",
  "audio_track": "A2",
  "gain_db": -17.0,
  "transition_in": "reviewed_crossfade",
  "transition_out": "ambience_bridge",
  "editable": true
}
```
This is sample schema, **not** an approved mix or proof that track B exists.

## Re-edit behavior
When a 60s film becomes 75s or reselects from 30 original clips, rebuild the **music story map** and remap source-anchored SFX. Reuse actual licensed/generated audio library and audio-analysis cache as appropriate. Keep previous timeline, soundtrack edit, audio source files and attribution manifest unchanged; produce new timeline version, new event schedule and updated transition checks. Do not stretch the last song or repeat sound arbitrarily just to fill time.

## Practical implementation and verification
1. Analyze real visual/action and original audio; create audio event proposals with source anchors and confidence.
2. Enumerate authorized available music/SFX files. If none, record pending candidates and concrete retrieval/generation prompts; do not insert placeholders as if completed.
3. For found audio, confirm playable media, file path, rights and duration. Analyze musical structure, tempo, potential key and loudness locally where possible; use listening checks for junctions.
4. Place audio assets in Resolve Media Pool and separate audio tracks at frame-accurate timeline positions via supported API or project interchange. Confirm current Resolve version's scripting functionality instead of assuming Fairlight gain envelopes/crossfades can be controlled by every API.
5. If mixing automation is unsupported, deliver editable audio clips plus a track-level mix plan, fade/gain metadata and explicit manual finishing steps.
6. Perform sound check: synchronization, dialogue clarity, cumulative loudness, clipping, SFX plausibility, abrupt joins, music rights and re-edit behavior.

## Output artifacts
- `sound-events.json` (source-anchored Foley/SFX, available/pending source states and confidence)
- `music-storyboard.json` (story beats, options single/multi-track, energy envelope and planned boundaries)
- `audio-assets.json` (local file, provenance, rights, attribution, generated prompts/settings)
- Resolve native or verified XML timeline, maintaining separate editable tracks
- QA status showing what is **planned**, **generated/downloaded**, **placed**, **mixed** and **verified**, without conflating stages.
