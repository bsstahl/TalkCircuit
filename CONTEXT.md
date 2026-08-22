# TalkCircuit Context

Glossary-only domain language for TalkCircuit. This file records canonical terms and invariants, not implementation details.

## Term: Conference
Definition: An event that may host one or more speaking opportunities.
Owns: name, series, year, location, dates, website, description.
Invariants:
- A Conference may have one or more CFPs over time.
- A Conference is not the same thing as a CFP; the CFP is the call/submission window.

## Term: CFP
Definition: A call for presentations for a Conference.
Owns: URL, open/close dates, notification dates, schedule-announcement dates, submission constraints, tracks, tags, formats, themes/topics, and speaker benefits.
Invariants:
- A CFP has a close date.
- A CFP may define an allowed tag vocabulary.
- A CFP may constrain number of submissions, formats, tracks, co-speakers, or languages.

## Term: Submission
Definition: A record that a TalkFolio Talk was submitted to a specific CFP.
Owns: submitted proposal copy snapshot, selected track/tags/format, submission date, status, and notes.
Canonical statuses: Draft, Submitted, Accepted, Rejected, Withdrawn.
Invariants:
- A Submission references a TalkFolio Talk.
- A Submission may later reference a LiquidVictor `SlideDeck.Id` when accepted and prepared for delivery.
- Submitted proposal copy is snapshotted because catalog copy may change later.

## Term: Booking
Definition: An accepted Submission that is scheduled for delivery.
Owns: scheduled date/time, room/location/virtual link, assigned deck, speaker logistics, and post-event follow-up state.
Invariants:
- A Booking originates from an Accepted Submission.
- A Booking may reference a LiquidVictor `SlideDeck.Id` and SlideFed public/session URLs.

## Term: Delivery
Definition: The historical fact that a booked talk was delivered.
Owns: actual delivery date/time, venue/session links, audience notes, recording links, publication links, and retrospective notes.
Invariants:
- Delivery history belongs to TalkCircuit, not TalkFolio.
- A delivery may reference SlideFed resources when the session was federated or published.

## Term: SubmissionConstraint
Definition: A rule imposed by a CFP.
Examples: max submissions per speaker, allowed tracks, required number of tracks, allowed tags, language, co-speaker limits.
Invariants:
- Constraints are interpreted in TalkCircuit.
- Constraints may require mapping TalkFolio Tags to conference-specific allowed tags.

## Term: PresentationFamily Conflict Rule
Definition: Talks from the same TalkFolio PresentationFamily must not be submitted to the same Conference/CFP.
Invariants:
- TalkCircuit enforces this rule at submission planning time.
- TalkFolio owns family membership; TalkCircuit consumes it.

## Term: Recommendation
Definition: A candidate set of Talks to submit to a CFP, based on TalkFolio metadata, conference constraints, prior submission/delivery history, and speaker preferences.
Invariants:
- Recommendation logic belongs to TalkCircuit.
- Recommendations should eventually be structured queries/rules over TalkFolio and TalkCircuit data, not prompt-only summaries over markdown.

## Term: Task Management Integration
Definition: An outbound integration through which TalkCircuit requests follow-up work from an external Task Management domain.
Invariants:
- TalkCircuit may create a task when a conference event changes the work needed from the Speaker, such as an acceptance requiring presentation review.
- The task request may include source identity, related Conference/CFP/Submission/Booking identity, actionable description, suggested due date, and an idempotency key.
- Task Management owns task identity, assignment, status, scheduling, and completion state.
- TalkCircuit does not import or become the owner of the Task Management model.

## Term: Calendaring Integration
Definition: An outbound integration through which TalkCircuit updates an external calendar for the Speaker's conference activity.
Invariants:
- TalkCircuit may create or update a calendar entry when a Submission is created.
- TalkCircuit may update that entry when the Submission outcome is Accepted or Rejected.
- Calendar entries may include Conference, CFP, and Submission identity, title, relevant dates, outcome, and an idempotency key.
- Calendaring owns calendar-entry identity, scheduling details, recurrence, reminders, and calendar-entry status.
- A calendar entry is a projection of TalkCircuit state, not the authoritative Submission or Booking record.
