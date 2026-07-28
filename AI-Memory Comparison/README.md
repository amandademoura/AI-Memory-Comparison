# Dataset

## 1. Deterministic set — `deterministic/`

A fictional company, **Solvane Robotics**, built spec-first so the ground truth is authoritative and never drifts from the ingested documents.

- `spec.json` — the source of truth: 7 facts (team leadership, product specs, pricing, policies), each
  with a dated history of values and a `change_type` (`initial` / `update` / `correction` /
  `clarifying_addition`). One fact (Nav Kit battery life) is a deliberate trap: a later document *adds*
  a new figure without superseding the earlier one, testing whether a provider wrongly treats an
  addition as a contradiction.
- `documents/` — 15 natural-language documents (handbook, announcements, product spec sheets, an
  incident report, pricing pages, meeting notes, and a deliberately stale org-chart snapshot as a
  distractor) — this is what you ingest into each provider. Every one of these is fully written out
  already (not a stub or a citation) — see "About the documents" below.
- `ground_truth_qa.json` — 29 QA pairs derived from `spec.json` and the documents above, so answers
  can't drift from what's actually ingested: 14 "as of [date]" questions, 7 undated "what's current"
  questions, 6 multi-hop questions requiring connecting two or more documents, and 2 distractor-check
  questions testing whether stale documents get treated as current.

## 2. Non-deterministic set — `non_deterministic/`

### `custom_contradiction_set/`
Eleven short, hand-written 3-session personas — original content, no licensing restrictions. Nine are
genuinely unresolved contradictions, unflagged by the speaker (no ground-truth string to match
against); two (Morgan P. and Ravi S.) are contrast cases where a correction *does* eventually land,
one explicitly and one only through hedged, tentative language, so you can also test whether providers
correctly recognize a real update instead of over-hedging on everything. Each file has the session
transcripts to ingest, `probe_queries` to run at retrieval time, a `needle_probes` array (see below),
and an `annotation` block — the tension, why it's unresolved (or how it resolves), what a good
response looks like, and specific failure modes to watch for. See `custom_contradiction_set/README.md`
for the full persona-by-persona breakdown.

**`needle_probes`** — a small array per persona of sparse, single-mention, incidental facts embedded
in the sessions (a phone number, a pet's age, a badge number), unrelated to that persona's core
contradiction. Tests recall precision under a full session transcript rather than reasoning. Every
persona has at least one.

## Known limitations of this dataset

- Small scale by design (proportionate to a comparison notebook, not a published benchmark) — enough
  to reveal qualitative differences between providers, not enough for statistically rigorous claims.
  Conflict Resolution (n=7) is especially sensitive to single-question swings.
- Fictional content

## File tree

```
memory_provider_eval/
├── README.md                              
├── memory_provider_comparison.ipynb
├── deterministic/
│   ├── spec.json
│   ├── documents/                         (15 .md files)
│   └── ground_truth_qa.json               (29 QA pairs)
└── non_deterministic/
    ├── index.json
    ├── persona-01_priya.json ... persona-11_ravi.json
    └── README.md
```