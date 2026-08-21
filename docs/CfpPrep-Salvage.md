# CfpPrep Salvage Notes

`C:\s\r\CfpPrep` is existing prior work for the TalkCircuit bounded context. It should be treated as a prototype and source of reusable concepts, not copied blindly.

## Bring Forward

### Conference/CFP schema

Source: `C:\s\r\CfpPrep\Data\TestConf_Schema.json`

This is the strongest artifact in the prototype. It already models:

- `event`: name, series, year, location, dates, website, description
- `cfp`: URL, opens/closes/notifications/schedule announcement
- `submission_constraints`: max submissions, format caps, co-speaker limits, allowed tracks, required track selection counts, available tags, tag-selection rules, language
- `sessions`: formats and session types
- `themes_topics`: priorities, not-wanted topics, audience
- `speaker_benefits`: travel, accommodation, fee
- contact and notes fields

This should become the starting point for TalkCircuit's Conference/CFP schema.

### C# model

Source: `C:\s\r\CfpPrep\src\CfpPrep.Core\Messages\ConferenceDetails.cs`

This record model already maps the conference JSON structure and serializes to YAML via YamlDotNet. It should be ported or re-created in TalkCircuit once the implementation project exists.

## Do Not Bring Forward

Source: `C:\s\r\CfpPrep\Data\TestKeyData_Schema.json`

This appears to be an earlier, thinner version of the Conference model. Prefer `TestConf_Schema.json`.

## Rework Before Bringing Forward

Source: `C:\s\r\CfpPrep\src\CfpPrep.Core\Services\Workflow.cs`

Useful workflow shape:

1. Fetch CFP content from a URL or file.
2. Extract normalized `ConferenceDetails`.
3. Build a per-conference notes folder.
4. Generate submission recommendations.

Parts to change:

- Remove hard-coded local paths.
- Replace markdown concatenation as the main recommendation input with structured TalkFolio/TalkCircuit queries.
- Keep LLM/probabilistic steps for extraction and rationale drafting where they add value, not as the primary data model.

## Existing Prototype Inputs

The current `Workflow` reads:

- `C:\s\r\bss-notes\Community\Presentations\README.md`
- `C:\s\r\CognitiveInheritance\Pages\Talk-Catalog.md`
- `C:\s\r\CognitiveInheritance\Pages\Speaking-Engagements.md`
- `C:\s\r\bss-notes\Community\Conference-Details\Conference-Submission-Preferences.md`

These map to future structured sources:

| Prototype input | Future owner |
|---|---|
| presentation README / mindmap | generated reporting output from TalkFolio plus LiquidVictor |
| Talk-Catalog.md | TalkFolio |
| Speaking-Engagements.md | TalkCircuit |
| Conference-Submission-Preferences.md | TalkCircuit rules/preferences |
