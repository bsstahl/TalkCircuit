# Submission, Booking, and Delivery Workflow

## 1. Discover

Identify a Conference and its current CFP. Capture the source URL and the known timeline, constraints, tracks, formats, themes, audience, and benefits.

## 2. Prepare

Query active TalkFolio Talks using CFP themes, audience, allowed formats, tracks, tags, speaker preferences, prior outcomes, and PresentationFamily conflicts.

Preparation may draft rationale or proposal copy, but recommendations must ultimately be explainable from structured facts and rules.

## 3. Validate

Before submission, check:

- the Talk is eligible for submission;
- the CFP is open or the record is explicitly historical;
- the proposal uses the selected format and track correctly;
- CFP tag selection rules are satisfied;
- the maximum submission and co-speaker limits are satisfied;
- no Talk from the same PresentationFamily is already submitted to the Conference;
- required proposal fields are present; and
- the proposal snapshot is ready to preserve.

A validation failure blocks submission planning until it is resolved or explicitly overridden with a recorded reason.

## 4. Submit

Create or update a Submission with the CFP reference, TalkFolio Talk reference, proposal copy snapshot, selected CFP tags, selected track and format, submission date, and notes.

The Submission status becomes `Submitted` only after the proposal has actually been sent to the conference system.

After the Submission is created, TalkCircuit may send an update to Calendaring for the Speaker's conference activity. The calendar update should reference the Conference, CFP, and Submission and use an idempotency key when available.

## 5. Record the Outcome

Record `Accepted`, `Rejected`, or `Withdrawn` with the relevant date and notes. Do not mutate the submitted proposal snapshot when TalkFolio's current copy changes.

When the outcome is `Accepted` or `Rejected`, TalkCircuit may update the corresponding Calendaring entry with the outcome and any known next action. A `Withdrawn` outcome does not use the acceptance/rejection update path and requires an explicitly defined local calendar policy.

An acceptance may create a follow-up task in Task Management asking the Speaker to review the presentations assigned to the conference.

## 6. Book

For an accepted Submission, create a Booking with schedule, room or virtual location, logistics, and the selected LiquidVictor `SlideDeck.Id` when known. A deck may be assigned later as preparation progresses.

## 7. Deliver

After the event, record a Delivery with actual timing, venue/session links, audience notes, recording links, publication links, and retrospective notes. Delivery history is not replaced by later edits to the Talk or deck.

## Rules and Overrides

The standard workflow should fail closed for missing required information and violated hard constraints. Historical imports and exceptional conference arrangements may use an explicit override that records:

- which rule was overridden;
- why it was overridden;
- who or what authorized it; and
- the date of the decision.

Overrides are facts about a particular Submission or Booking; they do not change the general rule.
