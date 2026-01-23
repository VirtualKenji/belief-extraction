# First Principles Quality Gate v1.2.0

This document contains the full quality gate criteria for validating extracted principles.

---

## GATE 1 — Mechanism Explicitness

**Question:** Can you clearly point to a concrete mechanism in the transcript and see it encoded literally?

**FAIL if:**
- The principle relies on abstract benefits (e.g., "leverage", "clarity", "better thinking")
- The "why" is not mechanically specified

**Score 9-10:** Mechanism explicit and falsifiable, "If X, then Y, because Z" is clear
**Score 6-8:** Mechanism present but partially abstract
**Score 3-5:** Mechanism vague or buried in abstraction
**Score 0-2:** No discernible mechanism, reads like advice

**Weight:** HIGH for all roles

---

## GATE 2 — Procedural Executability

**Question:** Could another AI apply this principle step-by-step without guessing?

**FAIL if:**
- `canonical_instruction` reads like advice, not a procedure
- `transformation_rules` contain vague or compound verbs
- Steps are not independently actionable

**Role-specific:**
- **executable**: Imperative verbs required ("Write X", "List Y"). Cognitive predicates penalized.
- **archival**: Cognitive predicates acceptable if well-defined.
- **hybrid**: Imperative preferred, cognitive acceptable with definitions.

**Score 9-10:** Each step single concrete operation, naive executor could follow
**Score 6-8:** Mostly clear, minor ambiguity in 1-2 steps
**Score 3-5:** Abstract or compound steps, significant guessing required
**Score 0-2:** Instructions read like advice, no clear sequence

**Weight:** VERY HIGH for executable, MEDIUM for archival

---

## GATE 3 — Component Atomicity

**Question:** Could each component be split into its own standalone principle later?

**FAIL if:**
- Any component encodes multiple mechanisms
- You cannot write "If X, then Y, because Z" for a component
- Components are descriptive instead of causal

**Value statement rule:** Components must be causal mechanisms, not value statements. Value statements ("prefer X over Y") belong in assumptions, not components. Flag but do not FAIL if values appear in components — mark `value_contamination: true`.

**Score 9-10:** Each component ONE mechanism, independently reusable
**Score 6-8:** Most atomic, 1-2 bundle multiple ideas
**Score 3-5:** Components are summaries, hard to split
**Score 0-2:** Components missing or placeholder

**Weight:** HIGH for all roles

---

## GATE 4 — Transcript Literal Preservation

**Question:** Does the principle preserve concrete evidence from the transcript?

**Evidence must be present via EITHER path:**

**(A) Embedded path:** Evidence in `source_transcript` field (full text or 500+ char excerpt)

**(B) Constrained path:** Evidence encoded as concrete constraints in tests/instructions that would fail if evidence absent

**FAIL if:**
- NEITHER path satisfied
- All examples abstracted away
- Numbers, ratios, vivid contrasts missing when present in transcript
- "Before vs after" or "blocked vs unblocked" states not encoded

**Role-specific:**
- **executable**: Path (B) sufficient
- **archival**: Path (A) strongly preferred, evidence in primary fields
- **hybrid**: Either acceptable, evidence in at least one primary field

**Score 9-10:** Concrete examples preserved, contrasts encoded
**Score 6-8:** Some evidence preserved, minor abstraction
**Score 3-5:** Significant abstraction, key examples missing
**Score 0-2:** All evidence abstracted away

**Weight:** LOW for executable, HIGH for archival

---

## GATE 5 — Benchmark Comparison

**Question:** Would this principle plausibly score Tier-A or higher alongside the benchmark (FP-0016)?

**FAIL if:**
- Clearly less mechanical than benchmark
- Requires more inference to apply
- Feels safer, cleaner, or more generic than benchmark

**When in doubt, FAIL.**

Compare to benchmark-FP-0016.json:
- Similar specificity in `effect_fingerprint`
- Similar detail in `transformation_rules`
- Similar concreteness in `input_triggers`

**Weight:** HIGH for all roles

---

## GATE 6 — Effect Fingerprint Specificity (NEW in v1.2.0)

**Question:** Is the `effect_fingerprint` specific enough to identify this principle uniquely?

**FAIL if:**
- Fingerprint could apply to multiple unrelated principles
- Uses only generic verbs
- Has fewer than 4 verbs

**Generic fingerprints that trigger FAIL:**
- "concretize->add_examples->surface_tradeoffs"
- "identify->analyze->recommend"
- "list->compare->choose"
- Any fingerprint with fewer than 4 verbs

**Valid fingerprints must:**
- Include domain-specific verbs
- Be specific enough that similar fingerprints indicate merge candidates
- Example: "identify_tracking_method->list_consistent_actions->map_action_to_outcome->flag_zero_effect_actions->propose_iteration_test"

**Weight:** HIGH for all roles (critical for merging at scale)

---

## ROLE-BASED WEIGHT SUMMARY

| Gate                        | executable | archival | hybrid |
|-----------------------------|------------|----------|--------|
| 1. Mechanism Explicitness   | HIGH       | HIGH     | HIGH   |
| 2. Procedural Executability | VERY HIGH  | MEDIUM   | HIGH   |
| 3. Component Atomicity      | HIGH       | HIGH     | HIGH   |
| 4. Transcript Literalness   | LOW        | HIGH     | MEDIUM |
| 5. Benchmark Comparison     | HIGH       | HIGH     | HIGH   |
| 6. Fingerprint Specificity  | HIGH       | HIGH     | HIGH   |

---

## PASS THRESHOLDS BY ROLE

Score each gate 0-10:

- **executable**: Must score 7+ on Gates 1, 2, 3, 5, 6. Gate 4 can be 5+.
- **archival**: Must score 7+ on Gates 1, 3, 4, 5, 6. Gate 2 can be 6+.
- **hybrid**: Must score 6+ on all gates, 7+ on at least 4.

---

## VALIDATION OUTPUT FORMAT

When validating, output:

```json
{
  "verdict": "PASS | FAIL",
  "artifact_role": "executable | archival | hybrid",
  "scores": {
    "mechanism_explicitness": 0,
    "procedural_executability": 0,
    "component_atomicity": 0,
    "transcript_literalness": 0,
    "benchmark_comparison": 0,
    "fingerprint_specificity": 0
  },
  "execution_readiness": {
    "mechanical_executability": 0,
    "inference_cost": "low | medium | high",
    "value_contamination": false,
    "merge_candidate_score": 0
  },
  "failure_reasons": ["reason 1", "reason 2"],
  "notes": ""
}
```

---

## IMPORTANT ATTITUDE

- You are not here to be fair
- You are here to protect the dataset
- False negatives are acceptable
- False positives are unacceptable
- Role-awareness is precision, not leniency
