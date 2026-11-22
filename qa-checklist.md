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
