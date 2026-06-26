# template_active_inference Final Approval Code Review

codeQualityStatus: BLOCK
recommendation: REQUEST_CHANGES
reportPath: .omo/evidence/template_active_inference_final_approval-code-review.md

## Skill Perspective Check

- `omo:remove-ai-slops`: consulted. The diff still has a cache-stamping false-confidence failure mode: tests prove some requested removals but miss the invalid post-rebuild stamp path, which is exactly the kind of overfit/slop review this perspective rejects.
- `omo:programming` + Python reference: consulted. The cache helper still records derived state without parsing/proving the full boundary contract after generation, and the tests do not lock the observed unsafe branch.
- `engineering:code-review`: consulted for correctness, maintainability, and regression risk.

## CRITICAL

None.

## HIGH

1. `tests/gate_support.py:221-225` and `tests/gate_support.py:352-355` still stamp invalid rebuilt gate state.

   `refresh_generated_gate_artifacts()` and `ensure_gate_artifacts()` both call `_settle_generated_contracts(...)`, recompute `_required_gate_artifacts_signature(root)`, and write `_BOOTSTRAPPED_SIGNATURES[root] = signature` without proving `_gate_artifacts_present(root)` or delegating to `refresh_gate_artifact_session_signature(root)`. That leaves the original cache-safety class unresolved for the post-rebuild path: a generated tree can be incomplete or semantically invalid, get stamped, and the next `ensure_gate_artifacts()` call will return on the exact cached signature at `tests/gate_support.py:321-324`.

   I reproduced this against the current module with an in-memory probe:

   ```text
   stamped= invalid-after-rebuild
   bootstrapped= True
   gate_valid_after= False
   ```

   I also reproduced the same unsafe behavior through `refresh_generated_gate_artifacts(force=True)`:

   ```text
   stamped= invalid-after-refresh
   bootstrapped= True
   gate_valid_after= False
   ```

   This contradicts the stated success criterion that changed signatures are recorded only on exact cache hit or after `_gate_artifacts_present(root)` passes.

2. `tests/gate_support.py:286-299` omits the animation validator, so direct session-signature refresh can still stamp invalid output-gate state.

   `refresh_gate_artifact_session_signature()` now requires `_gate_artifacts_present(root)`, but `_gate_artifacts_present()` checks required files, semantic gluing, integration audit, sheaf tracks, and claim ledger only. It does not call `validate_animation_frame_deltas()`, even though animation deltas are a required artifact at `tests/gate_support.py:96` and are validated by the output gate. Direct callers remain in `tests/test_roadmap_promotion.py:123`, `tests/test_roadmap_promotion.py:421`, `tests/test_roadmap_promotion.py:499`, `tests/test_roadmap_promotion.py:525`, `tests/test_roadmap_promotion.py:549`, `tests/test_roadmap_promotion.py:572`, and `tests/test_roadmap_promotion.py:708`.

   I reproduced this with the current module by making semantic/integration/sheaf/claim checks clean while animation validation reports an issue:

   ```text
   stamped= sig-with-bad-animation
   animation_issues= ['bad animation']
   ```

   This means the "unsafe partial call sites cannot stamp invalid gate state" criterion is not actually met.

## MEDIUM

1. `tests/test_gate_support_contracts.py:80-99` covers only the pre-rebuild invalid-signature branch, not the invalid post-rebuild stamp that remains in production code.

   The test named `test_ensure_gate_artifacts_rejects_invalid_bootstrapped_changed_signature` stops at a monkeypatched `run_analysis()` exception before the rebuild tail can execute. It therefore gives green coverage for the earlier reviewer concern while leaving the current vulnerable lines (`tests/gate_support.py:352-355`) untested. The `refresh_output_gate_contracts()` tests at `tests/test_gate_support_contracts.py:155-198` cover revalidation for that helper, but not the two other stamp paths above.

## LOW

1. `.omo/ulw-loop/evidence/browser-qa/final-copied-root-browser-qa.json:9-13` and `.omo/ulw-loop/evidence/browser-qa/final-copied-root-browser-qa.json:21-27` do not fully match the clean browser-QA summary.

   The artifact records a 404 console resource at the 375px viewport and `horizontalOverflow: true` with `scrollWidth` 485 vs `clientWidth` 375. This is not a blocker for the cache review, and the 404 may be the favicon probe, but the evidence should not be summarized as entirely clean without noting the mobile overflow.

## Verification Performed

- `git diff --check` passed.
- `PYTHONDONTWRITEBYTECODE=1 uv run pytest tests/test_gate_support_contracts.py -q --no-cov -p no:cacheprovider` passed: 11 passed in 0.23s.
- `PYTHONDONTWRITEBYTECODE=1 uv run python scripts/check_documentation_contract.py --check` passed: `documentation_contract: ok`.
- `PYTHONDONTWRITEBYTECODE=1 uv run python scripts/validate_outputs.py` passed on current generated outputs.
- Confirmed browser QA JSON and viewport PNG evidence paths exist.
- Ran three read-only in-memory probes demonstrating unsafe cache stamping for invalid post-rebuild, invalid force-refresh, and invalid animation states.

## Blockers

- Make all signature-stamping paths fail closed: after `_settle_generated_contracts(...)`, stamp only through a validation function that proves the full gate state, or raise without changing `_BOOTSTRAPPED_SIGNATURES` / `_BOOTSTRAPPED_ROOTS`.
- Include animation-frame validation in the state required by `refresh_gate_artifact_session_signature()`, or remove/direct-call replace the remaining partial session-signature call sites so they cannot stamp while animation/output gates are invalid.
- Add regression tests that fail against the current code for the invalid post-rebuild and invalid animation-stamp paths.
