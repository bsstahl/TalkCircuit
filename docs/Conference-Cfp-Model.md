# Conference and CFP Model

TalkCircuit treats the Conference and the CFP as separate concepts. The Conference is the event; the CFP is the time-bounded opportunity to submit.

## Conference Data

A Conference may contain:

- name
- series
- year
- location, including venue, city, state, and country
- event start and optional end dates
- website
- description

A recurring event should retain its Conference identity while each submission cycle is represented by its own CFP.

## CFP Data

A CFP contains:

- submission URL
- timeline: opening, closing, notification, and schedule-announcement dates
- local deadlines with timezone and date-time
- submission constraints
- available tracks
- session formats and durations
- session types
- themes and priority topics
- topics the CFP does not want
- intended audience
- speaker benefits
- contact information
- source notes

The CFP close date is required. Other dates may be unknown when the source does not publish them.

## Submission Constraints

Constraints may include:

- maximum submissions per speaker
- per-format caps
- co-speaker limits
- allowed tracks
- required number or range of selected tracks
- allowed CFP tags
- tag-selection rules
- session language

A missing constraint means “not known,” not automatically “unrestricted.” The implementation must preserve that distinction where it affects recommendation or validation.

## CFP Tags

CFP tags are the vocabulary imposed by a particular conference or submission system. They are distinct from TalkFolio topical tags.

TalkCircuit may map TalkFolio tags to CFP tags when preparing a submission, but the selected CFP values belong to the Submission snapshot. A later change to TalkFolio tags must not rewrite historical submissions.

## Versioning and Provenance

Imported CFP data should retain:

- source URL
- retrieval or observation date
- extraction notes
- confidence or unresolved-field notes when applicable
- the CFP data used when a Submission was prepared

If a CFP changes after a submission, the historical Submission remains tied to the earlier known constraints. A new planning operation may use the latest CFP information.

## Prior Art

`C:\s\r\CfpPrep\Data\TestConf_Schema.json` is the starting schema reference. Its top-level areas are suitable for the first TalkCircuit schema:

- `event`
- `cfp`
- `sessions`
- `themes_topics`
- `speaker_benefits`
- `contact`
- `notes`

The schema should be re-created or ported deliberately; CfpPrep is a prototype, not a dependency of the TalkCircuit domain model.
