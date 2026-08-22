# TalkCircuit Domain Model

TalkCircuit manages the movement of a talk through the conference circuit. It owns conference opportunities, submissions, bookings, deliveries, and the rules used to select and validate them.

## Scope

TalkCircuit answers:

- Which Conferences and CFPs are available?
- What does a CFP require or allow?
- Which TalkFolio Talks are candidates for this CFP?
- What proposal was submitted, and what was its status?
- What happens after acceptance?
- Which deck and public materials fulfill the booking?
- What was delivered, where, and when?

TalkCircuit does not own the talk concept, presentation artifact construction, or Fediverse publication mechanics.

## Entities

### Conference

An event that may host one or more speaking opportunities. A Conference owns identity and event-level information such as name, series, year, location, dates, website, and description.

A Conference is not a CFP. One Conference may have multiple CFPs over time.

### CFP

A call for presentations associated with a Conference. A CFP owns its URL, submission window, notification dates, schedule-announcement date, submission constraints, tracks, allowed tags, formats, themes, audiences, speaker benefits, contact information, and notes.

A CFP is the unit against which submission eligibility and constraints are evaluated.

### Submission

A record that one TalkFolio Talk was offered to one CFP. It owns the submitted proposal snapshot, selected track/tags/format, submission date, status, and submission notes.

The proposal is snapshotted because TalkFolio copy may change after submission.

### Booking

The delivery arrangement resulting from an accepted Submission. A Booking owns scheduled date/time, room or virtual location, logistics, the deck assigned for delivery, public/session references, and follow-up state.

### Delivery

The historical fact that a Booking was delivered. A Delivery owns actual delivery date/time, venue/session links, audience notes, recording links, publication links, and retrospective notes.

## External References

TalkCircuit uses identity references and does not import other contexts' models:

- `TalkFolio.Talk` is the source of talk identity, category, tags, proposal copy, target audience, and PresentationFamily membership.
- `LiquidVictor.SlideDeck.Id` identifies the built deck assigned to an accepted Booking.
- SlideFed URLs or session identifiers may be recorded for delivered or published material.
- Task Management may receive follow-up task intent, such as a presentation review task after acceptance.

TalkCircuit owns the Submission snapshot and its decisions. It does not write back into TalkFolio, LiquidVictor, or SlideFed.

## State Lifecycles

### Submission

`Draft` -> `Submitted` -> `Accepted`

`Submitted` -> `Rejected`

`Submitted` -> `Withdrawn`

A rejected or withdrawn Submission is terminal unless a new submission is created for a later CFP. An accepted Submission may produce one Booking.

### Booking

The minimum conceptual progression is:

`Accepted Submission` -> `Scheduled` -> `Delivered`

A booking may also be cancelled or rescheduled. Cancellation does not erase the accepted Submission or delivery history.

## Invariants

- Every Submission references exactly one TalkFolio Talk and one CFP.
- A Submission stores the proposal copy submitted at that point in time.
- A Submission cannot be marked `Accepted` for a CFP that has already closed without an explicit historical/import rule.
- A Booking originates from an Accepted Submission.
- A Booking may reference zero or one LiquidVictor deck until preparation is complete.
- A Delivery originates from a Booking and preserves historical facts even if catalog copy later changes.
- Two Talks in the same TalkFolio PresentationFamily must not be submitted to the same Conference.
- CFP-specific selected tags are not automatically interchangeable with TalkFolio tags.
- Conference constraints are evaluated against the CFP version used by the Submission.

## Not Owned Here

- Talk titles, abstracts, pitch language, category, tags, and lifecycle belong to TalkFolio.
- Slide structure, rendering, themes, transitions, and build completeness belong to LiquidVictor.
- ActivityPub publication and interaction state belong to SlideFed.
- Tasks and task status belong to the external Task Management domain.
