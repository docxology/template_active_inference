# Final Gate Review - template_active_inference

## recommendation

REJECT

## blockers

1. `tests/gate_support.py:228` still exposes a changed-signature stamping path that does not validate the artifact tree before populating `_BOOTSTRAPPED_SIGNATURES`.

   The two previously named blocker paths are fixed in isolation:

   - `ensure_gate_artifacts()` now falls through to full rebuild when the signature changed and `_gate_artifacts_present(root)` is false; scratch probe output: `ensure_invalid_changed full refresh required old`.
   - `refresh_output_gate_contracts()` now revalidates animation, integration, and sheaf contracts and refuses to stamp when integration stays invalid; scratch probe output: `refresh_output_invalid raised stamped False`.

   However, the new `refresh_gate_artifact_session_signature()` helper records the current signature whenever `_required_gate_artifacts_signature()` returns a value. It does not call `_gate_artifacts_present()`, and the regression test at `tests/test_gate_support_contracts.py:122` explicitly asserts that it must not validate. Scratch probe with `_gate_artifacts_present = lambda _: False` produced:

   ```text
   signature_stamped changed-without-validation
   root_cached True
   ```

   That means later `ensure_gate_artifacts()` calls can hit the exact-signature fast path and skip validation against a tree that was never proven valid by the full artifact-present predicate. The helper is called directly from `tests/test_roadmap_promotion.py` at lines 122, 186, 225, 301, 420, 498, 524, 548, 571, and 707 after partial validator checks, so the cache-safety claim "changed signatures are recorded only for exact cache hits or after `_gate_artifacts_present(root)` returns true" is not true for the current diff.

2. The required `remove-ai-slops` / `programming` pass still finds unresolved maintenance slop in the touched cache helper module.

   `tests/gate_support.py` is now 310 pure Python LOC by:

   ```text
   awk '!/^[[:space:]]*$/ && !/^[[:space:]]*#/' tests/gate_support.py | wc -l
   ```

   The diff adds 46 lines to an already oversized, stateful helper that owns artifact inventories, mutation helpers, cache signatures, fixed-point generation, and bootstrap orchestration. Under the loaded `omo:programming` criterion, >250 pure LOC is a defect for a modified Python file unless explicitly justified/split; under `omo:remove-ai-slops`, unresolved oversized modules and stateful cache indirection are maintenance-burden slop. This does not replace blocker 1, but it independently prevents approval under the final-gate slop rule.

## originalIntent

The user wanted a final, read-only gate review of the current `template_active_inference` diff after a previous code reviewer blocked on two HIGH cache-validation issues. The review needed to verify whether the blockers were genuinely fixed, whether the shipped tests and evidence support completion, and whether any remaining issue should block final completion.

## desiredOutcome

Approve only if the current diff prevents invalid gate artifact cache signatures from being recorded, validates repaired animation/integration/sheaf contracts before stamping, keeps tests meaningful rather than implementation-mirroring, preserves generated/copied output validity, and passes the required slop/programming review criteria.

## userOutcomeReview

The user-visible outcome is not complete. The specific previously reported implementations were repaired, and the focused suite is green, but the diff still leaves a reachable cache-stamping helper that can mark a changed signature valid without full artifact validation. Because that helper is now used directly across roadmap tests, the user cannot rely on the final cache-safety contract that motivated the re-review.

## checked artifact paths

- `projects/templates/template_active_inference/tests/gate_support.py`
- `projects/templates/template_active_inference/tests/test_gate_support_contracts.py`
- `projects/templates/template_active_inference/tests/test_roadmap_promotion.py`
- `projects/templates/template_active_inference/tests/test_simulation_invariants.py`
- `projects/templates/template_active_inference/tests/test_support_modules.py`
- `projects/templates/template_active_inference/AGENTS.md`
- `projects/templates/template_active_inference/manuscript/AGENTS.md`
- `projects/templates/template_active_inference/src/analytical/AGENTS.md`
- `projects/templates/template_active_inference/src/gnn/AGENTS.md`
- `projects/templates/template_active_inference/src/ontology/AGENTS.md`
- `projects/templates/template_active_inference/src/simulation/AGENTS.md`
- `projects/templates/template_active_inference/.omo/evidence/template-active-inference-code-review.md`
- `projects/templates/template_active_inference/.omo/evidence/gate-support-partial-red-evidence.txt`
- `projects/templates/template_active_inference/.omo/ultrawork/notepad.md`
- `projects/templates/template_active_inference/.omo/ulw-loop/evidence/browser-qa/copied-root-browser-qa.json`
- `output/templates/template_active_inference/web/index.html`

## evidence checked

- `git status --short`: 11 modified tracked files, matching the stated scope before this report artifact.
- `git diff --name-status`: 11 intended files under `projects/templates/template_active_inference`.
- `git diff --check`: pass.
- `uv run pytest tests/test_gate_support_contracts.py -q --no-cov`: 10 passed in 0.32s.
- Project validators from exemplar root:
  - `uv run python scripts/check_documentation_contract.py --check`: `documentation_contract: ok`.
  - `uv run python scripts/generate_method_inventory.py --check`: current.
  - `uv run python scripts/compose_manuscript.py --validate-only --strict`: pass.
  - `uv run python scripts/validate_outputs.py`: all listed output/manuscript checks true.
- Copied-root validation from template root:
  - `validate_copied_outputs(Path('output/templates/template_active_inference'))`: true; pdf, web, figures, data, reports, and logs valid.
- Browser QA artifact:
  - `.omo/ulw-loop/evidence/browser-qa/copied-root-browser-qa.json`: served `/web/index.html`, status 200 at 375/768/1280, 34 images, no broken images, no page errors. Favicon 404 only.
- Previous code review report:
  - `.omo/evidence/template-active-inference-code-review.md` explicitly includes `omo:remove-ai-slops` and `omo:programming` coverage, but it is a pre-fix `REQUEST_CHANGES` report. I did not treat it as current approval.

## exact evidence gaps

- No current independent reviewer artifact approves the post-fix diff; the only code-review artifact is the prior blocker report.
- No test fails if `refresh_gate_artifact_session_signature()` records a signature while `_gate_artifacts_present(root)` would return false. The current test suite enshrines the opposite behavior.
- The final handoff claims changed signatures are recorded only for exact cache hits or after `_gate_artifacts_present(root)` succeeds, but the direct helper path and roadmap-test call sites contradict that claim.
- The touched `tests/gate_support.py` remains over the loaded programming skill's 250 pure LOC ceiling without a split or exception rationale.
