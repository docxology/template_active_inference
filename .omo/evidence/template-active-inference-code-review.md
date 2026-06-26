# Code Quality Review - template_active_inference

## Verdict

- codeQualityStatus: BLOCK
- recommendation: REQUEST_CHANGES
- reportPath: `.omo/evidence/template-active-inference-code-review.md`
- blockers:
  - `tests/gate_support.py` now records bootstrapped cache signatures for changed artifact trees after only existence/nonblank checks, skipping semantic/output validators.
  - `tests/gate_support.py` `refresh_output_gate_contracts()` can stamp the session cache after attempted rewrites without proving integration/sheaf validators are clean.

## Skill-Perspective Check

- `omo:remove-ai-slops`: loaded and applied. The diff does not add deletion-only tests, skips, xfails, or public schema/track-ID churn. It does add implementation-mirroring cache tests that verify validators are not called, which creates false confidence around the changed gate cache behavior.
- `omo:programming`: loaded, including the Python reference and code-smell reference. The blocking issue violates the validation-boundary/cache discipline. `tests/gate_support.py` also grew from 283 to 308 pure LOC, extending an already oversized shared test-support module; this compounds maintainability risk but is secondary to the cache validation blocker.

## CRITICAL

None.

## HIGH

1. `ensure_gate_artifacts()` now trusts changed artifact content signatures without validating the artifact contracts.

   Reference: `tests/gate_support.py:304`, especially `tests/gate_support.py:311` and `tests/gate_support.py:321`.

   The changed branches return when `root in _BOOTSTRAPPED_ROOTS`, a signature exists, and `_required_gate_artifacts_exist(root)` is true. `_required_gate_artifacts_exist()` only checks file presence and nonblank image dimensions, not semantic gluing, integration audit, sheaf-track validation, or claim ledger validity. This is weaker than the prior `refresh_generated_gate_artifacts(..., force=False)` path, which used `_gate_artifacts_present()` before accepting a changed signature.

   I reproduced the cache leak in memory with `PYTHONPATH=tests:src`: a bootstrapped root with a changed signature and `_required_gate_artifacts_exist=True` records `changed-invalid` while neither `_gate_artifacts_present` nor `run_analysis` is called (`calls == []`). This is exactly the hidden state/cache leakage the diff is meant to avoid.

   The new test at `tests/test_gate_support_contracts.py:52` enshrines the weak behavior by asserting that changed complete artifact trees do not trigger validation or refresh. Under the remove-ai-slops/programming perspectives, that is implementation-mirroring coverage and false confidence rather than a behavioral oracle.

2. `refresh_output_gate_contracts()` stamps the session signature without revalidating the contracts it just tried to fix.

   Reference: `tests/gate_support.py:237`, especially `tests/gate_support.py:247` and `tests/gate_support.py:252`.

   When integration or sheaf issues are present, the helper calls `write_integration_audit_artifacts(root)` and `write_sheaf_track_artifacts(root)`, but it only rechecks animation deltas before calling `refresh_gate_artifact_session_signature(root)`. It never proves `validate_integration_audit_artifacts(root) == []` or `validate_sheaf_track_artifacts(root) == []` after regeneration.

   I reproduced this in memory by forcing `validate_integration_audit_artifacts` to keep returning `["still bad"]` while the writers were no-ops; `refresh_output_gate_contracts()` still recorded `sig-after-bad` in `_BOOTSTRAPPED_SIGNATURES`. That can make later tests skip regeneration/validation against a known-bad output surface.

   The new clean-path test at `tests/test_gate_support_contracts.py:113` does not cover this dirty-path behavior.

## MEDIUM

1. `tests/gate_support.py` remains an oversized, stateful shared fixture and this diff adds more state/cache responsibilities to it.

   Reference: `tests/gate_support.py:40`, `tests/gate_support.py:228`, `tests/gate_support.py:237`, `tests/gate_support.py:304`.

   Pure LOC is now 308, up from 283 in `HEAD`. This is not the primary blocker, but the file owns artifact inventories, mutation helpers, fixed-point generation, cache signatures, and bootstrap orchestration. That size/state coupling makes the cache bugs above easier to introduce and harder to review.

## LOW

1. Browser evidence folder contains one stale/negative JSON alongside the valid copied-root artifact.

   Reference: `.omo/ulw-loop/evidence/browser-qa/browser-qa.json:2`, `.omo/ulw-loop/evidence/browser-qa/browser-qa.json:124`, `.omo/ulw-loop/evidence/browser-qa/copied-root-browser-qa.json:2`, `.omo/ulw-loop/evidence/browser-qa/copied-root-browser-qa.json:25`.

   `copied-root-browser-qa.json` is the relevant `/web/index.html` copied-root run and shows 34 images with no broken images. `browser-qa.json` is a root-URL run and records many broken relative image paths. This is not a code blocker, but the evidence directory is easy to misread unless the handoff names the copied-root JSON specifically.

## Evidence Inspected

- Full tracked diff for the 11 scoped files.
- `.omo/evidence/gate-support-partial-red-evidence.txt`: RED proof shows missing `refresh_gate_artifact_session_signature` AttributeError before implementation.
- `.omo/ulw-loop/evidence/browser-qa/copied-root-browser-qa.json`: copied-root `/web/index.html` browser evidence has status 200, 34 images, no broken images; favicon-only console noise at 375px.
- `.omo/ulw-loop/evidence/browser-qa/browser-qa.json`: non-acceptance root-URL browser run has broken relative images.

## Checks Run In Review

- `git diff --check` - pass.
- `uv run pytest tests/test_gate_support_contracts.py tests/test_support_modules.py -q --no-cov` - 16 passed.
- `uv run python scripts/check_documentation_contract.py --check` - ok.
- `uv run python scripts/generate_method_inventory.py --check` - current.
- `uv run python scripts/validate_outputs.py` - all output/manuscript checks true.
- `uv run python scripts/compose_manuscript.py --validate-only --strict` - pass.
- From template root: `validate_copied_outputs(Path("output/templates/template_active_inference"))` - true, all copied output buckets valid.

## Residual Risk

The main lane's long suite and copied-output/browser evidence are strong for the current filesystem state, but they do not discharge the new session-cache correctness risk. The fix should require validators to prove clean before any changed signature is accepted, and add dirty-path tests that fail if an invalid integration/sheaf contract can be stamped into `_BOOTSTRAPPED_SIGNATURES`.
