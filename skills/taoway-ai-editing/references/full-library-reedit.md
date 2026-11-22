# Full-library re-edit acceptance tests

## Scenario: 30 media sources, 10 used by V1
- Original Media Pool contains 30 eligible video assets, each with a stable `source_id` and source path.
- V1 refers to 10 of the 30, but no sources are removed from the library.
- User requests a new cut (V2), not just trim changes to V1.
- AI **considers all 30 source IDs** with metadata/cache, not only V1’s 10 IDs.
- V2 may contain any combination of those 30; it does not have to use all 30.
- V1 remains unchanged; V2 is a distinct, editable timeline.
- Every V2 event references full-length original media and retains T-mode trim access.
- Audio, subtitle tracks, Text+ and Fusion events remain independent/editable where supported.
- Media not analyzed yet is indexed and analyzed at appropriate cost-saving resolution, not silently excluded.

## Counters to log
```
source_registry_count: 30
eligible_original_sources: 30
considered_original_sources: 30
previous_timeline_unique_sources: 10
new_timeline_unique_sources: <actual>
newly_selected_from_unused: <actual>
missing_or_ineligible_sources: []
```
These counters are example/target values; report observed counts from the actual project.

## Resolve implementation notes
- Enumerate current project Media Pool folders recursively or use a separately reconciled source manifest, not just `GetItemListInTrack` results from V1.
- Create a new empty timeline with a distinct name, then populate it with Media Pool items and verified source-frame ranges.
- For cross-project FCPXML imports, a new project's pool may contain only XML-referenced items unless all sources are imported separately: explicitly import or preserve the full source library.
- If some original source files are offline, surface this blocker and do not label the re-edit as an unrestricted full-library pass.

## Request interpretation
“重新剪辑 / 新建时间轴 / 从头剪 / 换个版本 / 不满意重新来” => full-library source-scope by default.
“只用原来10条 / 不换素材，只改节奏” => explicitly restricted old-selected source-scope.
