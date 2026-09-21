# ADR: Build Stuff 2026 soft-decline context for Pending Sessionize status

- Status: Accepted
- Date: 2026-09-21

## Context

We submitted three talks to Build Stuff Vilnius 2026:

- LLMs Under the Hood
- When You Don't Need an LLM: Deterministic Intelligence for Real World Apps
- Architecting For Ambiguity: The Rise of the Behavioral Layer

Acceptance notices appear to have gone out around 2026-08-26, but the talks still appear in Sessionize as "In Evaluation" rather than "Accepted" or "Rejected". No official confirmation or no-thank-you message has been received for any of the talks.

We do not know whether this means the conference is still actively evaluating submissions or whether Build Stuff simply does not send decline messages in the same way. The only concrete signal we have is that other speakers have reported receiving their acceptance emails, which suggests these talks are unlikely to be accepted as primary submissions but does not confirm a formal rejection.

This creates a gap between the conference's general acceptance cadence and the actual state shown in the submission system. The talks are not formally accepted, but they are also not clearly rejected.

## Decision

We will keep these talks in the TalkCircuit record as `Pending` rather than changing them to `NotAccepted` or `NoResponse`.

We will record that the status is effectively a soft-decline context, meaning:

- the conference has not sent a definitive acceptance
- the available Sessionize status still reads "In Evaluation", but we do not know whether that reflects active evaluation or a generic no-decline posture
- no formal decision has been observed yet
- other speakers have reported acceptances, which makes a primary acceptance for these talks unlikely but not impossible
- a late backup or alternate invite remains possible, but is unconfirmed

## Rationale

The repository schema already reserves `NoResponse` for the lifecycle case where a conference has passed and no outcome was received, and `NotAccepted` for a clear rejection.

This case does not meet either definition cleanly:

- it is not a confirmed acceptance
- it is not a confirmed rejection
- there is meaningful possibility of a late invite without an official notice

Keeping the record as `Pending` while documenting the surrounding context preserves the uncertainty and avoids reclassifying the outcome prematurely.

## Consequences

- These talks remain open in the TalkCircuit system until a formal outcome is received.
- We avoid incorrectly converting a soft, ambiguous status into a final rejection.
- Future reviewers will see the context that explains why the submissions were not treated as accepted, declined, or expired without response.
- If a later official decision arrives, this record can be updated to match that decision without ambiguity.

## Alternatives Considered

### 1. Reuse `NoResponse`

We considered marking the talks as `NoResponse` because no final official decision had been received. This would have matched the lifecycle logic for conferences that passed without a response, but it would have been misleading in this case because the submission system still shows "In Evaluation" rather than a clear end-of-process timeout. The distinction matters: `NoResponse` implies the conference simply never replied, whereas this record reflects an ambiguous status with a likely soft-decline signal from peer acceptance notices, without enough evidence to claim a formal decision or active evaluation.

### 2. Mark as `NotAccepted`

We considered converting the status to `NotAccepted` once it appeared that acceptance notices had gone out and the talks were not selected. This would be accurate only if the conference had sent a definitive no or the Sessionize view had clearly moved to a rejected state. Here, that clarity is missing, and converting early would create a false sense of certainty. It would also erase the fact that we still might receive a backup or alternate invite.

### 3. Introduce a new repo-wide status such as `SoftDecline` or `Waitlist`

We discussed adding a new status value to the schema to capture the ambiguous case more directly. This would make the data model more expressive, but it would also require changing the schema, lifecycle logic, and any downstream tooling or reports that assume the current enum values. The current repo pattern is intentionally small and explicit, and for this case the uncertainty is better documented in the ADR and the submission notes than by widening the status model prematurely.

We specifically rejected the idea of renaming or repurposing `NoResponse` into `SoftDecline` because the repo already uses `NoResponse` for the lifecycle meaning "conference passed and no outcome was received." A `SoftDecline` concept is useful as a human-authored explanation of a specific ambiguity, but it should remain a documented interpretation, not a replacement for the underlying lifecycle status.

## Notes

This ADR captures the decision to preserve uncertainty rather than forcing an inaccurate lifecycle transition. The underlying data file remains the source of truth for the actual submission entries, while this ADR serves as explanatory context.
