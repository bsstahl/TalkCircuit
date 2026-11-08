# ADR: Model conferences, instances, occurrences, and submission sets separately

- Status: Accepted
- Date: 2026-10-24

## Context

TalkCircuit tracks conference identities that may have distinct variants, recurring yearly occurrences, and submission activity. A single conference identity can have multiple instances, while each instance can recur in several years. Instance names and locations help identify a variant, but location is not always enough to determine whether two events are the same instance.

The current ledger illustrates the distinction:

- 44 conference records contain 48 durable instances.
- Those instances contain 58 recorded instance-years.
- There are 58 submission-set files, each linked to an instance-year.
- Every currently recorded instance-year has one matching submission set; none of the submission files is orphaned.

These are current inventory counts, not invariants that require the counts to remain equal. The conference schema assigns durable identity to instances and places year-dependent metadata under each instance's `Years`. The submission schema links a set to a conference, instance, and year using `ConferenceId`, `ConferenceInstanceId`, and `ConferenceInstanceYear`.

Examples show why instance identity is broader than location alone:

- Visual Studio Live! has distinct Las Vegas and Austin instances.
- MLcon has distinct New York and San Diego instances.
- Update Conference has distinct Kraków and Prague instances.
- AI Engineer CODE Summit SF and AI Engineer World's Fair are distinct instances in San Francisco, with different venues, dates, and submission endpoints.

## Decision

Represent the relationship graph as:

```text
Conference
  └── Instance (one or more)
        └── Instance-year / occurrence (one or more)
              └── Submission set (currently one per occurrence)
                    └── Talk submission (zero or more)
```

For example, Visual Studio Live! has 1 2026 instance, and 2 distinct 2027 instances:

```text
Visual Studio Live! (conference: microsoft-vslive)
  ├── Las Vegas (instance)
  │     └── 2026 occurrence (March 16-20)
  │     │     └── microsoft-vslive-las-vegas-2026.yaml
  │     │           └── 4 talk submissions
  │     └── 2027 occurrence (March 22-26)
  │           └── microsoft-vslive-las-vegas-2027.yaml
  │                 └── 5 talk submissions
  └── Austin (instance)
        └── 2027 occurrence (May 2-6)
              └── microsoft-vslive-austin-2027.yaml
                    └── DidNotSubmit (no talk submissions)
```

Both submission-set records link to the same conference identity, but each uses its own `ConferenceInstanceId` and the year `2027`. The Austin record is still a submission set because it records a deliberate decision not to submit; a set does not necessarily contain talk submissions.

- A **Conference** is the durable record for a conference brand or related event family.
- An **Instance** is a durable, organizer-defined variant. It is commonly location-specific, but may distinguish events that share a city or venue context.
- An **Instance-year** is one occurrence of an instance in a calendar year. Dates, CFP details, and other year-dependent metadata belong to this occurrence, not to the conference or durable instance.
- A **Submission set** records the submission campaign or deliberate non-submission decision for one exact occurrence. It links through the three-part conference-instance-year identity. Individual proposals are entries within the set.

The ledger's conference count measures conference records; its instance count measures durable variants; its submission-set count measures recorded occurrence-level submission decisions. These counts are related but are not expected to be 1:1. The observed one-set-per-instance-year shape describes the current data, not a requirement that the three totals match.

## Rationale

This structure retains shared conference identity without conflating distinct event variants. It allows the same instance to recur across years while keeping dates and CFP facts attached to the specific occurrence they describe. The composite submission reference prevents a submission set from being ambiguous when one conference has multiple variants or when a variant recurs.

Treating an instance as organizer-defined rather than strictly geographic also handles same-city events such as the two AI Engineer events. Conversely, treating a conference as the broader identity makes it possible to group distinct locations or related formats without duplicating the entire conference record.

## Consequences

- Conference-level counts, instance counts, occurrence counts, and submission-set counts must be labeled distinctly in reports.
- New instance-year records should be created only when the occurrence is established and its required source-backed facts are available.
- A submission set must point to the exact conference, instance, and year; its file name alone is not the relationship.
- Repeated years belong under the existing durable instance when the event variant remains the same; a distinct organizer-defined variant gets its own instance.
- The current dataset has one submission set for every recorded instance-year, including deliberate non-submission decisions where recorded. The model and count labels should not imply that an instance has only one submission set over its lifetime.

## Alternatives Considered

### 1. Require one conference record, instance, and submission set per event occurrence

This would make the counts appear 1:1 but would duplicate durable conference identity for recurring events and for related locations. It would also prevent a natural grouping of yearly occurrences and event variants under a shared conference record.

### 2. Treat each conference instance as one annual occurrence

This would collapse the durable variant and its year-specific occurrence into one record. Recurring events would need repeated instance records, making it harder to associate the same location or event variant across years and easier to accidentally carry year-specific CFP facts forward.

### 3. Define instances strictly by physical location

This works for conferences whose only variants are different cities, but it fails when an organizer runs distinct events in the same city. AI Engineer CODE Summit SF and AI Engineer World's Fair demonstrate that event identity can differ even when the city is shared.

### 4. Link submission sets only to the conference

This would leave the targeted occurrence ambiguous for conferences with multiple instances or multiple years. A set could not be reliably tied to the right dates, CFP endpoint, or event variant without reintroducing the missing identifiers elsewhere.

### 5. Flatten all occurrence metadata onto the conference

This would blur which year's dates, CFP status, formats, and submission endpoint apply, especially when separate instances or years differ. Keeping occurrence-specific values under `Instances[].Years[]` preserves their scope.
