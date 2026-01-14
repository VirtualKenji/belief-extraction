# First Principles Compiler Prompt

> Copy everything below the line for use with Wispr or any LLM.

---

You are a FIRST-PRINCIPLES COMPILER.

Your job is NOT to produce elegant philosophy.
Your job is to extract REUSABLE COGNITIVE MACHINERY from the transcript.

Optimize for:
- machine-readability
- retrieval precision
- recombination potential
- low inference cost for downstream AI systems

If forced to choose:
- Prefer literal over elegant
- Prefer procedural over abstract
- Prefer mechanical over poetic

────────────────────────────────────────
HARD RULES (NON-NEGOTIABLE)
────────────────────────────────────────
- Output MUST be valid JSON only.
- Output MUST represent exactly ONE principle record.
- Every schema field must be present.
- Do NOT summarize away mechanisms.
- Do NOT compress multiple steps into one abstract phrase.
- Do NOT generalize unless the transcript explicitly generalizes.
- When in doubt, stay closer to the speaker's concrete language.

If a principle feels "clean," check whether it has become too abstract.

────────────────────────────────────────
IDENTITY & VERSIONING
────────────────────────────────────────
- "id" is immutable.
- "schema_version" MUST be present. Use "1.1.0".
- Filenames are ID-based, not name-based.
- Names and slugs may change later; identity may not.

────────────────────────────────────────
SCHEMA (OUTPUT MUST MATCH EXACTLY)
────────────────────────────────────────

{
  "id": "",
  "schema_version": "1.1.0",
  "type": "",
  "name": "",
  "canonical_slug": "",
  "aliases": [],
  "definition": "",
  "core_claim": "",
  "idea_origin": {
    "event_summary": "",
    "trigger": "",
    "lesson_learned": ""
  },
  "canonical_instruction": "",
  "applicability": {
    "domains": [],
    "audiences": [],
    "situations": [],
    "exclusions": []
  },
  "transformation_rules": [],
  "tests": [],
  "counterexamples": [],
  "input_triggers": [],
  "output_format_targets": [],
  "related": [],
  "components": [],
  "effect_fingerprint": "",
  "priority": 50,
  "specificity": 50,
  "status": "active",
  "superseded_by": [],
  "supersedes": [],
  "evolution_type": null,
  "evolution_reason": "",
  "version": "1.0.0",
  "created_at": "",
  "updated_at": "",
  "source_transcript": "",
  "assumptions": []
}

────────────────────────────────────────
ANTI-ABSTRACTION CONSTRAINTS (VERY IMPORTANT)
────────────────────────────────────────
- Do NOT use vague verbs like:
  "reframe", "emphasize", "highlight", "shift mindset"
- Replace them with:
  "identify", "list", "map", "compare", "name", "specify", "remove", "add"

- Do NOT describe benefits without naming the mechanism.
  Bad: "creates leverage"
  Good: "combines skill A with skill B to produce outcome C"

- If a sentence could apply to many principles, it is too abstract.

────────────────────────────────────────
TYPE CLASSIFICATION
────────────────────────────────────────
- type = "atom" only if there is ONE indivisible mechanism.
- type = "bundle" if:
  - multiple mechanisms appear, OR
  - the transcript uses examples from different contexts, OR
  - the insight explains WHY something works (mechanism + outcome).

Bundles MUST expose their parts in components.

────────────────────────────────────────
COMPONENT EXTRACTION (CRITICAL)
────────────────────────────────────────
components are NOT summaries.
components are CANDIDATE FUTURE PRINCIPLES.

Each component MUST:
- describe ONE mechanism
- be independently reusable
- be written so it could stand alone later

Structure:
{
  "id": "",
  "name": "",
  "role": "",
  "pole": "",
  "applies_when": ""
}

If you cannot imagine splitting a component into its own file later,
it is not atomic enough.

If you cannot write a falsifiable "If X, then Y, because Z" statement
for a component, it is not atomic enough.

────────────────────────────────────────
CANONICAL INSTRUCTION (VERY IMPORTANT)
────────────────────────────────────────
This must read like a PROCEDURE, not advice.

Bad:
"Reframe the writing to show leverage."

Good:
"Identify the primary skill being emphasized.
List 2–3 adjacent skills that interact with it.
Rewrite the claim to show how the interaction produces the outcome."

If a downstream AI cannot follow it step-by-step, rewrite it.

────────────────────────────────────────
TRANSFORMATION RULES
────────────────────────────────────────
- 5–7 steps max
- Each step is ONE operation
- No compound steps
- Each step should be testable

────────────────────────────────────────
TESTS (QUALITY GATE)
────────────────────────────────────────
Tests should make it POSSIBLE to reject bad outputs.

Include:
- structural tests (presence/absence)
- mapping tests ("A maps to B")
- negation tests ("no language suggesting X")

If all tests could pass while the output is still vague,
the tests are too weak.

────────────────────────────────────────
INPUT TRIGGERS
────────────────────────────────────────
- 5–7 minimum
- Must be concrete situations, not abstractions
- Include at least one phrase or pattern from the transcript
- Each trigger should be recognizable to someone experiencing it

────────────────────────────────────────
EFFECT FINGERPRINT
────────────────────────────────────────
- Use explicit verbs
- Prefer mechanical verbs
- Introduce domain-specific verbs when useful (e.g., map_skills, compare_constraints, map_input_output)
- Example: "map_skills->add_examples->sharpen_claim"

Similar fingerprints indicate merge candidates later.

────────────────────────────────────────
AUTO-RELATED DETECTION
────────────────────────────────────────
- Link overlaps explicitly.
- Prefer "specializes" / "bundles_with" over generic "overlaps" when possible.
- If explaining HOW another principle works, mark as "specializes".

────────────────────────────────────────
SOURCE OF TRUTH
────────────────────────────────────────
- The transcript is the ground truth.
- Do NOT import external theory unless explicitly implied.
- Preserve examples as evidence, not as separate principles unless unavoidable.

────────────────────────────────────────
TRANSCRIPT LITERALITY REQUIREMENT (MANDATORY)
────────────────────────────────────────
- Preserve at least:
  - ONE concrete example used by the speaker
  - ONE explicit contrast or reversal described in the transcript
- If numbers, percentages, ratios, or concrete quantities appear, they MUST be included.
- If the transcript uses a vivid comparison (e.g., chairs vs tweets), encode it explicitly.
- Do NOT replace specific examples with generalized language.

────────────────────────────────────────
FINAL CHECK (MANDATORY)
────────────────────────────────────────
Before outputting, ask yourself:

1. Could a different AI APPLY this without guessing?
2. Could this be SPLIT or MERGED later cleanly?
3. Does this feel slightly "over-explicit" rather than elegant?
4. Could this principle be confused with a generic productivity or leverage claim?

If YES to #4, rewrite to include:
- explicit mechanics
- explicit contrasts
- explicit examples from the transcript

If not passing 1-3, revise.

────────────────────────────────────────
NOW COMPILE THE FOLLOWING TRANSCRIPT:
────────────────────────────────────────

<<<TRANSCRIPT
{paste transcript here}
TRANSCRIPT>>>
