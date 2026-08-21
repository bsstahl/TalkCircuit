# Integration Strategy

TalkCircuit coordinates the conference-facing workflow for SpeakerOps. It depends on TalkFolio for talk concepts and on LiquidVictor/SlideFed for delivery artifacts, but it owns submission workflow state.

## TalkFolio

TalkCircuit references TalkFolio Talks when planning and recording submissions.

TalkCircuit consumes:

- Talk identity
- Category and Tags
- PresentationFamily membership
- LifecycleStatus
- proposal copy
- target audience and takeaways
- known LiquidVictor deck references, when available

Rules:

- TalkCircuit does not own canonical talk copy.
- A Submission stores a snapshot of the proposal copy actually submitted.
- TalkCircuit enforces PresentationFamily conflict rules when preparing submissions.

## LiquidVictor

TalkCircuit references LiquidVictor `SlideDeck.Id` values when a submission or booking needs a concrete built deck.

Rules:

- LiquidVictor remains the source of truth for deck structure and build mechanics.
- TalkCircuit does not write fields into LiquidVictor YAML.
- A booking can exist before the final deck is ready, but delivery readiness should be resolved before the event.

## SlideFed

TalkCircuit may reference SlideFed public deck URLs or PresentationSession URIs for accepted/delivered talks.

Rules:

- SlideFed owns federated publication and session mechanics.
- TalkCircuit owns delivery history and may attach SlideFed references as evidence or follow-up material.

## Conferences / CFPs

TalkCircuit directly interacts with external conference/CFP systems.

Expected data captured from CFPs:

- event name, series, year, location, dates, website
- CFP URL and timeline
- submission constraints
- allowed tracks and formats
- allowed tags and tag-selection rules
- themes/topics/audience priorities
- speaker benefits
- contact details and notes

## Recommendation Flow

Initial recommendation flow:

1. Capture or fetch CFP details.
2. Normalize CFP details into a structured Conference/CFP model.
3. Query TalkFolio for Active talks matching CFP themes, allowed tracks, allowed tags, and audience.
4. Remove candidates that violate PresentationFamily conflict rules.
5. Consider prior submission/delivery history.
6. Produce recommended submissions and rationale.

The old `CfpPrep` prototype performed this by concatenating markdown and asking an LLM for recommendations. The target TalkCircuit design should use structured data first and reserve probabilistic assistance for interpretation, extraction, and rationale drafting.
