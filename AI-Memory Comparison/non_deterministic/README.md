# Custom contradiction supplement

Eleven short, hand-written synthetic personas (3 sessions each, dated weeks/months apart),
purpose-built for the part of a memory-provider comparison that a factual-recall benchmark can't
cover: unresolved contradictions.

## Files

- `persona-0X_name.json` — one persona each. Each file has `sessions` (dated turns to ingest),
  `probe_queries` (what to ask the provider at retrieval time), `needle_probes` (see below), and
  `annotation` (the tension, why it's unresolved, what a good response looks like, and specific
  failure modes to watch for).
- `index.json` — quick lookup of all eleven personas and their tension type.

## How this differs from the deterministic set

The deterministic set (Solvane Robotics) has contradictions too, but every one resolves to a
single correct answer via an explicit rule (most recent, explicit correction memo, etc.) — you can
score it with exact-match QA. Most of these personas are the opposite on purpose: the
contradictions are never explicitly flagged or resolved by the speaker, so there is no
ground-truth string to match against. Evaluation here is qualitative — use the `annotation` block
per persona as your rubric, not an answer key. Two personas (07, 11) are deliberate exceptions —
see below.

## Grading rubric — what each verdict means

When these personas are evaluated, every probe response gets classified into exactly one of four
verdicts:

- **GOOD** — the response surfaces the tension instead of silently picking a side, or, for the two
  contrast-case personas where a real resolution *was* given (07, 11), correctly uses that
  resolution without hedging on something that's no longer actually open.
- **SILENT_PICK** — confidently answers with only one side of a genuine contradiction, no
  acknowledgment that conflicting information exists.
- **HALLUCINATED_RESOLUTION** — invents an explanation or reconciliation the user never actually
  gave.
- **OVER_HEDGED** — only applies to personas 07 and 11 (the two where a resolution genuinely was
  given). Fires if a provider treats an already-resolved point as if it were still open.

"Good" here doesn't mean "matches a fixed answer" the way the deterministic set does — it's a
grade on *behavior* (appropriately uncertain vs. silently confident vs. fabricated), not a factual
match.

## Persona summary

1. **Priya M.** — states vegetarian, later casually mentions eating steak, no acknowledgment.
2. **Sam K.** — states aggressive risk tolerance, later describes real distress over volatility.
3. **Jordan T.** — control case: a *genuine, acknowledged* opinion change (not a contradiction),
   included to check providers don't over-flag ordinary opinion drift as unresolved conflict.
4. **Noor A.** — relationship status implied, never stated; implication of upheaval, never
   confirmed.
5. **Devon R.** — introversion stated explicitly, then extroverted behavior described with equal
   confidence.
6. **Casey L.** — pet referred to as a dog, then described with cat-like behavior; deliberately the
   murkiest case (could be a joke, a slip, or a real conflict) to see if providers fabricate
   confidence either way.
7. **Morgan P.** — contrast case: a budget figure *is* explicitly corrected, but only in the final
   session — tests whether providers correctly weight a late, clear correction rather than treating
   all three numbers mentioned across the conversation as equally valid.
8. **Elena V.** — two entirely different professions (nursing, freelance graphic design) described
   in separate sessions with equal confidence, no career-change narrative given.
9. **Theo B.** — two different home cities described with equal confidence and specific local
   detail, no relocation ever mentioned.
10. **Aisha R.** — two named coworkers give conflicting secondhand reports about the same event;
    the user explicitly says neither was confirmed with the actual source. Tests source
    attribution, not just recency.
11. **Ravi S.** — a deadline slips, but only via hedged, tentative language ("we'll see how it
    goes") rather than a firm correction — tests whether providers overstate the user's own
    uncertainty into false confidence.

## What "good" looks like across all of them

For the nine genuinely unresolved personas, the target behavior is the same: surface the tension or
hedge, don't silently pick a side, and don't invent a resolution the user never gave. A provider
that answers every probe query with a single, unqualified fact is failing this test even if the
fact it picked happens to be the more recent one — confidently guessing right is still guessing.
For the two contrast cases (07, 11), the test flips: failing to recognize an actual (if softly
worded) update is its own mistake, distinct from over-hedging on things that were never actually in
question.

## Needle-in-haystack probes (`needle_probes`)

Separate from the contradiction test above: a small array of sparse, single-mention facts embedded
in a persona's sessions, unrelated to that persona's core tension. Tests whether a provider retains
an incidental detail buried in a full conversation, not reasoning or hedging.

| Persona | Needle fact |
|---|---|
| 01 Priya M. | Dentist appointment: Thursday at 2pm |
| 02 Sam K. | Nephew's birthday: March 15 |
| 03 Jordan T. | Cat's name: Mochi |
| 04 Noor A. | Address: 42 Ridge Lane |
| 05 Devon R. | Car from sister: blue Honda Civic |
| 06 Casey L. | Biscuit's age (3) and the plant knocked over (pothos) |
| 07 Morgan P. | Out-of-office day: next Wednesday |
| 08 Elena V. | Phone number: 555-0142 |
| 09 Theo B. | Commute highway: I-35 |
| 10 Aisha R. | Count/sources of meeting-date reports (three: Ben, Priya, Marcus) |
| 11 Ravi S. | Badge number: B-4471 |

If you add your own personas later, keep the same convention: one throwaway detail per persona,
framed as an aside, with zero relevance to the core contradiction being tested.