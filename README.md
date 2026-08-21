# TalkCircuit

TalkCircuit is the SpeakerOps bounded context for managing conference CFPs, submissions, bookings, and delivery history.

It owns the movement of talks through the conference circuit: finding CFPs, understanding conference constraints, deciding what to submit, recording submissions, tracking acceptance/rejection, and connecting accepted bookings to built decks and published materials.

## Responsibilities

TalkCircuit owns:

- conferences and CFPs
- CFP timelines, constraints, tracks, allowed tags, and speaker benefits
- submission records and statuses
- submitted proposal snapshots
- acceptance, rejection, withdrawal, and booking outcomes
- delivery history
- rules for deciding whether a talk can or should be submitted to a conference
- enforcement of the rule that two talks from the same PresentationFamily are never submitted to the same conference

TalkCircuit does not own talk catalog copy, deck construction, or Fediverse publication mechanics.

## SpeakerOps Integrations

| Context | Relationship |
|---|---|
| TalkFolio | TalkCircuit references TalkFolio talks, tags, categories, proposal copy, and PresentationFamily data when preparing or validating submissions. |
| LiquidVictor | TalkCircuit references LiquidVictor `SlideDeck.Id` values once an accepted booking is tied to a specific built deck. |
| SlideFed | TalkCircuit may reference SlideFed public URLs or PresentationSession URIs for delivered or follow-up materials. |

## Prior Work

`C:\s\r\CfpPrep` is a prototype for much of this domain. It includes a strong Conference/CFP schema and a workflow that fetches CFP pages, extracts structured conference data, and generates recommended submissions from existing notes.

See [docs/CfpPrep-Salvage.md](docs/CfpPrep-Salvage.md).

## Status

Documentation-first repository. Domain model and schemas are not yet implemented.
