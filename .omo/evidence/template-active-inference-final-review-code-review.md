# Code Quality Review: template_active_inference Final Review

codeQualityStatus: WATCH
recommendation: APPROVE
reportPath: .omo/evidence/template-active-inference-final-review-code-review.md

## Scope

Goal: read-only final review of the current `template_active_inference` working tree diff and final evidence for correctness risks, especially cache/session signature correctness, validator weakening, AGENTS doc drift, and tests that might mask failures.

Changed tracked files inspected:

- `AGENTS.md`
- `manuscript/AGENTS.md`
- `src/analytical/AGENTS.md`
- `src/gnn/AGENTS.md`
- `src/ontology/AGENTS.md`
- `src/simulation/AGENTS.md`
- `tests/gate_support.py`
- `tests/test_gate_support_contracts.py`
- `tests/test_roadmap_promotion.py`
- `tests/test_simulation_invariants.py`
- `tests/test_support_modules.py`

Evidence/notepad inspected:

- `.omo/ultrawork/notepad.md`
- `.omo/ulw-loop/evidence/browser-qa/final-copied-root-browser-qa.json`
- `.omo/evidence/template-active-inference-final-gate-review.md` (stale against current diff; not treated as approval)
- `.omo/evidence/template_active_inference_final_approval-code-review.md` (stale against current diff; not treated as approval)

## Skill Perspective Check

- `omo:remove-ai-slops`: consulted. I specifically checked for deletion-only tests, requested-removal-only tests, tautological/implementation-mirroring tests, weakened gates, unnecessary extraction/parsing, and hidden performance/complexity cost.
- `omo:programming` plus its Python reference: consulted. I checked strict Python maintainability concerns, boundary validation, cache state proof, monkeypatch-heavy tests, untyped escape hatches, and the 250 pure LOC smell.
- `omo:review-work`: consulted for review dimensions. Its five-subagent orchestration could not run because this harness does not expose `multi_agent_v1`; I performed the required single-reviewer pass directly.

The diff does not violate the remove-ai-slops perspective in a blocking way: the new tests are not deletion-only, do not merely assert removal, and still exercise negative controls or cache fail-closed behavior. The programming perspective flags one non-blocking maintainability watch item: `tests/gate_support.py` remains oversized at 307 pure LOC (pre-existing baseline was 283), so future edits should split this helper rather than continue growing it.

## CRITICAL

None.

## HIGH

None.

## MEDIUM

None.

## LOW

1. `tests/gate_support.py` remains an oversized test-support module.

   The file is 307 pure Python LOC after this diff, up from 283 in `HEAD`. This is a maintainability warning under the loaded programming/remove-ai-slops perspectives, but I am not treating it as a blocker here because the size smell was pre-existing, the current diff is focused on fixing cache correctness, and the added lines are directly tied to stricter artifact-state validation and regression coverage rather than speculative abstraction.

2. `.omo/ulw-loop/evidence/browser-qa/final-copied-root-browser-qa.json` records expected-but-not-clean browser signals.

   The copied-root browser QA artifact records status 200 at 375/768/1280, 34 images, no broken images, no page errors, and no resource failures. It also records a 375px console 404 consistent with a favicon request and `horizontalOverflow: true` from wide manuscript tables. This matches the user's stated caveat and is not a blocker.

## Verification Performed

- `git status --short`: tracked diff is limited to the 11 intended files.
- `git diff --name-status`: no tracked `.omo/` or `.codegraph/` changes.
- `git diff --check`: passed.
- `PYTHONDONTWRITEBYTECODE=1 uv run pytest tests/test_gate_support_contracts.py -q --no-cov -p no:cacheprovider`: 14 passed in 0.30s.
- `PYTHONDONTWRITEBYTECODE=1 uv run python scripts/check_documentation_contract.py --check`: `documentation_contract: ok`.
- `PYTHONDONTWRITEBYTECODE=1 uv run python scripts/generate_method_inventory.py --check`: current.
- `PYTHONDONTWRITEBYTECODE=1 uv run python scripts/compose_manuscript.py --validate-only --strict`: exit 0.
- `PYTHONDONTWRITEBYTECODE=1 uv run python scripts/validate_outputs.py`: exit 0, all reported output and manuscript checks true.
- From `/Users/4d/Documents/GitHub/template`: `PYTHONDONTWRITEBYTECODE=1 uv run python -c "from pathlib import Path; from infrastructure.validation import validate_copied_outputs; ok = validate_copied_outputs(Path('output/templates/template_active_inference')); print(f'copied_outputs_valid={ok}'); raise SystemExit(0 if ok else 1)"`: `copied_outputs_valid=True`.

## Review Notes

- Cache/session signature correctness: current `refresh_gate_artifact_session_signature()` refuses missing signatures and invalid full gate state before writing `_BOOTSTRAPPED_SIGNATURES`. Current post-rebuild paths in `refresh_generated_gate_artifacts()` and `ensure_gate_artifacts()` route through that strict helper. `_gate_artifacts_present()` includes required artifact presence, nonblank images, semantic gluing, animation deltas, integration audit, sheaf tracks, and claim ledger.
- Validator contract strength: I did not find gate weakening. `refresh_output_gate_contracts()` validates animation, integration, and sheaf contracts before and after repair, raises on remaining issues, and then records the session signature only through the strict helper.
- Test relevance: the cache tests use monkeypatching, but they target the changed test-support cache seam directly and include invalid current signature, invalid changed signature, invalid post-rebuild signature, animation requirement, and output-contract revalidation cases. I did not find deletion-only or tautological tests that create false confidence.
- AGENTS drift: the root AGENTS file is now a routing layer and child AGENTS files preserve local edit boundaries, contracts, and commands. Referenced child AGENTS/test paths checked in the new docs exist.

## Blockers

None.
