# TAOWAY Subtitle System: reference

## Content-based routing
- **Speech, dialogue, translation:** Subtitle Track; export editable SRT with precise cue timings. SRT is plain caption content/timing, not sophisticated motion design.
- **Poetic line / short voiceover accent:** Text+ with `cinematic_minimal`.
- **Geography / date / route:** Text+ with `outdoor_documentary`.
- **Chapter:** Text+ with `editorial_typography`.
- **Intro/outro statement:** Text+ or tested Fusion preset with `statement_title`.
- **Tracking, obscured text, masked reveals, animated handwriting:** Fusion nodes or templates, not flattened media.

## Typography and motion
- Warm off-white `#F2F0E9`; only restrained shadows/contrast aids when necessary.
- English body Helvetica Neue / Inter; Chinese body Source Han Sans / PingFang; narrow editorial titles DIN Condensed / Oswald; occasional poetic serif EB Garamond / Cormorant Garamond.
- 4K landscape initial visual size targets: regular captions 48–60 px, large chapters 100–160 px. Translate to Resolve controls as needed; inspect actual footage.
- Bottom-center subtitles start around 9–12% above bottom; adjust for 9:16 derivatives and platform UI safe regions.
- Default 4–8-frame fades for minor titles and 6–12-frame fades for chapters; optional upward translation ≤8–16 output pixels.
- Prefer natural line breaks and 1–2 lines per caption, with enough dwell time to read. Use no text when image and ambience carry the scene.

## Editor / AI responsibilities
1. Analyze transcript and narrative role; decide whether the event is spoken `subtitle` or designed `title`.
2. Store and export transcript+cue metadata separately from title template events, using frame-aware timeline coordinates.
3. Keep video clips referencing full original source files; never burn captions into trimmed/re-encoded media.
4. Place Text+ titles on independent tracks. Use tested Fusion macros only where tracking, masking, and compositing are needed.
5. If using FCPXML/XML or scripting, verify the installed Resolve version preserves correct text editability. If not, provide separate SRT, structured title-event data and compatible templates.
6. Verify installed fonts, Chinese glyph rendering, timing after speed changes, relinking, 16:9 and 9:16 layout, moving-background contrast, and T-mode effects on text timing.

## QA decisions
- Is the text necessary, or should this shot remain silent/clean?
- Is text still editable without a new render?
- Is this simply Text+, or is Fusion node complexity genuinely required?
- Has the target-machine Resolve import actually been tested?
