# Open Questions

## Identity

- Should `Conference`, `CFP`, `Submission`, `Booking`, and `Delivery` use GUIDs, slugs, or both?
- Should CFP identity be nested under Conference identity or independent?

## Statuses

- Are `Draft`, `Submitted`, `Accepted`, `Rejected`, and `Withdrawn` sufficient for Submission status?
- Should Booking and Delivery have separate status vocabularies?

## Recommendation Rules

- How should prior rejection history affect future recommendations?
- How should recently delivered talks be weighted or suppressed for nearby conferences?
- How should speaker travel preferences, cost, and opportunity value be modeled?

## Integrations

- How should TalkCircuit authenticate against external CFP platforms, if at all?
- Should TalkCircuit store submitted proposal text as immutable snapshots?
- Should accepted bookings require a LiquidVictor `SlideDeck.Id`, or allow a placeholder until a deck is built?
