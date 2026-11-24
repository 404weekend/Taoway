# TAOWAY AI editing handoff checklist

## Media and timebase
- [ ] Every timeline clip maps to an original source file / Media Pool item.
- [ ] Source paths, clip IDs, original durations, FPS and timecodes are verified.
- [ ] Source in/out and timeline position/duration are separate fields.
- [ ] Edit frame bounds are valid; the end frame convention is explicit.
- [ ] Retime, mixed FPS, audio and variable-frame-rate sources have been addressed.

## Resolve editability
- [ ] No rendered, trimmed or re-encoded clips were substituted as timeline masters.
- [ ] Reconnect/relink original media works.
- [ ] Manually test T-mode left and right edge trims on representative clips.
- [ ] Recoverable handles correspond to real original footage.
- [ ] Ripple/Roll behavior and adjacent clip constraints are acknowledged.
- [ ] XML project opens without missing media and audio stays synced.

## TAOWAY aesthetic
- [ ] Look is natural, moderate contrast, restrained saturation.
- [ ] Cold Wilderness / Faded Memories / Monochrome Identity applied contextually.
- [ ] Snow detail, skin tones, blacks and highlights remain plausible.
- [ ] Narrative conveys movement, effort and atmosphere with deliberate pauses.
- [ ] Typography and graphics remain simple and consistent.

## Efficiency
- [ ] Local metadata / scene analysis done before remote image inference.
- [ ] Sparse/adaptive keyframes and low-resolution previews were used.
- [ ] Analysis results cached by source identity / analysis version.
- [ ] Final XML/project references originals rather than preview assets.

## Subtitles, Text+, and Fusion
- [ ] Speech uses editable Subtitle Track or validated SRT import.
- [ ] Cinematic one-liners, location slates and chapters use independent editable Text+ overlays.
- [ ] Advanced masking/tracking/occlusion uses properly connected Fusion nodes or validated presets.
- [ ] No text or compositing is burned into replacement master media.
- [ ] Font availability, Chinese glyphs, readability, line breaks and caption timing checked.
- [ ] Four TAOWAY preset roles used consistently, with intentional text-free shots.
- [ ] Independent text layers and trim handles survive Resolve import/relink where supported.
- [ ] Any unsupported XML/API title properties documented with fallback deliverables.
- [ ] 16:9 and 9:16 outputs validated separately when needed.

## New timeline and full-library re-edit
- [ ] Source registry enumerates all eligible original Media Pool clips, not only prior timeline references.
- [ ] Previous and new timelines have distinct names/IDs and previous timeline remains unmodified.
- [ ] Re-edit candidate set includes previously unused source media unless user explicitly narrows scope.
- [ ] Cached source analysis is reused; only newly added/changed/uncached media is freshly analyzed.
- [ ] Logged eligible/considered/selected-source counts and offline exclusions.
- [ ] All new timeline selections reference original files with available trim handles (T mode).
- [ ] Timeline import/relink does not inadvertently reduce the whole project Media Pool to clips referenced by prior XML.

## Sound design and narrative music (v1.3)
- [ ] SFX is motivated by identified real source action, ambience or specific editorial transition.
- [ ] Source-anchored SFX uses source ID/event frame and re-maps after trimming, moving, slipping or speed change.
- [ ] Location ambience is preserved where valuable; no gratuitous whooshes or duplicated impact sounds.
- [ ] Audio source exists as a playable file, and license/attribution/commercial rights are recorded.
- [ ] Pending AI generation/search is labeled pending, not reported as imported or mixed.
- [ ] BGM plan starts from story beats and energy curve; defaults to one cohesive track where suitable.
- [ ] Multi-cue music has reviewed phrase, tempo, harmony, timbre, volume and emotional transitions.
- [ ] Music gaps, ambient bridges and purposeful silence are allowed; songs are not stretched mechanically to fill time.
- [ ] Separate editable tracks for ambience, BGM, Foley/SFX and voiceover; fades and dialogue ducking tested.
- [ ] Re-edited timeline rebuilds music plan and remaps sound events without modifying prior timeline.
- [ ] Scripting/XML support and actual Resolve audio placement, mix state, clipping and sync verified.
