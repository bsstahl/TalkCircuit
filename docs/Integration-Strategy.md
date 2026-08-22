# TalkCircuit Integration Strategy

TalkCircuit coordinates the conference-facing workflow for SpeakerOps. It owns submission workflow state and conference-circuit decisions while referring to other contexts by stable identity and snapshots.

## Reference Rules

No context imports another context's internal model.

| Direction | Reference | Purpose |
|---|---|---|
| TalkCircuit -> TalkFolio | Talk ID | Identify the talk being considered or submitted |
| TalkCircuit -> LiquidVictor | `SlideDeck.Id` | Identify the deck assigned to an accepted Booking |
| TalkCircuit -> SlideFed | URL or session identifier | Link a delivered or published session |
| TalkCircuit -> Task Management | Task intent and source reference | Request follow-up work for the Speaker |

TalkFolio remains the owner of Talk identity, proposal copy, category, tags, target audience, PresentationFamily membership, and concept lifecycle. TalkCircuit consumes those values when planning or validating submissions.

## Snapshots

A Submission snapshots the proposal copy and selected CFP values used at submission time. This protects historical accuracy when TalkFolio changes its catalog copy or when a CFP changes its rules.

A Booking may reference the current LiquidVictor deck selected for delivery. Delivery records preserve historical event facts and links.

## TalkFolio

TalkCircuit consumes Talk identity, Category and Tags, PresentationFamily membership, LifecycleStatus, proposal copy, target audience, takeaways, and known LiquidVictor deck references. TalkCircuit does not own canonical talk copy or write back into TalkFolio.

## LiquidVictor and SlideFed

TalkCircuit references LiquidVictor `SlideDeck.Id` values when a Submission or Booking needs a concrete built deck. A Booking may exist before the final deck is ready, but delivery readiness should be resolved before the event.

TalkCircuit may reference SlideFed public deck URLs or PresentationSession identifiers for accepted or delivered talks. SlideFed owns federation and publication mechanics; TalkCircuit owns delivery history and may attach publication references as evidence or follow-up material.

## Conferences and CFPs

Conference and CFP websites or submission systems are external sources and destinations. TalkCircuit may fetch or record CFP information and may submit proposals through a supported workflow, but the conference system remains authoritative for acceptance and rejection notifications.

Expected CFP data includes event identity, timeline, local deadlines, submission constraints, tracks, formats, allowed tags, tag-selection rules, themes, audience priorities, speaker benefits, contact details, and notes.

## Outbound Task Integration

TalkCircuit may publish task intent to an external Task Management domain. A conference acceptance is one trigger: TalkCircuit can create a task for the Speaker to review and update the presentations assigned to that conference.

The task payload should carry enough context to act without importing Task Management's model, such as:

- task title and actionable description;
- source type and source identity, such as `TalkCircuit.AcceptedSubmission`;
- Conference, CFP, Submission, or Booking identity;
- suggested due date, when known; and
- deduplication key or idempotency key.

Task Management owns task identity, assignment, status, scheduling, and completion. TalkCircuit should not duplicate those fields as authoritative state.

## Recommendation Flow

1. Capture or fetch CFP details.
2. Normalize CFP details into a structured Conference/CFP model.
3. Query TalkFolio for Active Talks matching CFP themes, allowed tracks, allowed tags, and audience.
4. Remove candidates that violate PresentationFamily conflict rules.
5. Consider prior Submission and Delivery history.
6. Produce recommended Submissions and an explainable rationale.

Structured facts and rules should drive recommendations. LLM or other probabilistic assistance may help with CFP extraction, interpretation, and rationale drafting, but should not replace the domain data model.

## Reporting

A reporting or composition tool may read TalkCircuit together with TalkFolio, LiquidVictor, and SlideFed to produce portfolio views or conference planning reports. That tool is not a reason to merge the bounded contexts.
