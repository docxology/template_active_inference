# Ultraresearch Expansion Log

## Phase 0

Core question: how to safely implement the comprehensive plan for `template_active_inference` without weakening the exemplar's generated-artifact, AGENTS, validation, or copied-output contracts.

Axes:

- AGENTS hierarchy: existing `AGENTS.md` coverage, duplication, and thin high-complexity children.
- Gate/test refresh flow: roadmap-promotion and claim-ledger slow paths, fixture refresh behavior, and negative controls.
- Generated artifact contracts: semantic gluing, figure source map, scope boundary, claim audit, and canonical sheaf-track artifacts.
- Copied/web outputs: root copy parity and browser-visible web deliverables.

Codebase relevant: yes. External: no, unless a local tool/library claim becomes contested. Browsing: yes for generated web-output QA only. Verification likely: yes. Report requested: Markdown journal/synthesis.

## Wave 1 - codebase-only findings

### AGENTS hierarchy

- Root `AGENTS.md` is 188 lines, above the init-deep 50-150 line target. It duplicates child-owned details for sheaf modules, visualization modules, layout, and command tables.
- Thin child files with meaningful local surfaces include `src/simulation/AGENTS.md` (5 lines), `src/analytical/AGENTS.md` (8 lines), `src/gnn/AGENTS.md` (6 lines), `src/ontology/AGENTS.md` (5 lines), and `manuscript/AGENTS.md` (7 lines).
- Existing richer children (`src/gates/AGENTS.md`, `src/roadmap_tracks/AGENTS.md`, `src/manuscript/sheaf/AGENTS.md`, `manuscript/sheaf/AGENTS.md`) should remain the authority for their directories rather than being repeated in the root file.

### Gate/test refresh flow

- Code graph and source inspection identify `tests/gate_support.py` as the central bootstrap surface: `ensure_gate_artifacts()`, `refresh_generated_gate_artifacts()`, and `_settle_generated_contracts()` control the expensive artifact fixed point.
- Existing contract tests already cover content-hash signatures, cached session reuse, changed valid signatures, claim-ledger validation, and fail-closed fixed-point pass parsing.
- The focused perf hotspot is not a public output schema; it is repeated test-session preparation after tests intentionally rewrite required generated artifacts. A safe fix should leave the full bootstrap path intact and only add explicit session-signature recording after a test has written and validated/asserted the artifacts it owns.

### Generated artifact contracts

- Required test bootstrap artifacts include semantic gluing, integration audit, claim ledger, sheaf tracks, promoted toy/formal artifacts, figure maps, and canonical release/security/proof reports.
- `validate_outputs.py`, `compose_manuscript.py --validate-only --strict`, `generate_method_inventory.py --check`, and `check_documentation_contract.py --check` were green at the baseline from the previous pass.
- The generation order remains toy sweep, formal interop, integration audit, sheaf tracks, manuscript variables, then root copy output refresh.

### Copied/web outputs

- The copied root at `/Users/4d/Documents/GitHub/template/output/templates/template_active_inference` already contains copied data, reports, figures, manuscript, and PDF surfaces, including promoted roadmap/sheaf artifacts.
- Browser QA still must run from the copied `web/` output surface at 375, 768, and 1280 px. No frontend code edit is justified until that real browser pass finds a concrete defect.

### Delegation notes

- Three explorer lanes returned bootstrap-state summaries instead of the requested deep findings. Their outputs are recorded as blocked research evidence, and the main lane continued with code graph plus direct source inspection.
- Two implementation lanes were launched with disjoint write ownership: gate-support/perf tests and AGENTS hierarchy update.

## Wave 2 - implementation evidence

- `AGENTS.md` hierarchy update: root file is 94 lines after trimming project-specific duplication into child files. Enriched child files are `manuscript/AGENTS.md`, `src/analytical/AGENTS.md`, `src/gnn/AGENTS.md`, `src/ontology/AGENTS.md`, and `src/simulation/AGENTS.md`.
- Gate-support contract: added `refresh_gate_artifact_session_signature()` to record the current required-artifact content signature after caller-owned validation/restoration, avoiding expensive full-refresh validation on later tests in the same session.
- RED evidence: `.omo/evidence/gate-support-partial-red-evidence.txt` captures `tests/test_gate_support_contracts.py::test_refresh_gate_artifact_session_signature_records_current_signature` failing with missing helper before implementation.
- GREEN evidence: initial `uv run pytest tests/test_gate_support_contracts.py -q --no-cov` passed `8 passed in 0.67s`; after independent-review hardening it passed `10 passed in 0.41s`; after strict signature validation it passed `11 passed in 0.22s`; after final post-rebuild and animation regressions it passed `14 passed in 0.33s`.
- Focused duration evidence: final `uv run pytest tests/test_roadmap_promotion.py tests/gates/test_claim_ledger.py --durations=20 -q --no-cov` passed `18 passed in 273.73s`.
- Performance deltas from baseline/follow-up duration tables: `test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts` 141.42s -> 55.53s; `test_integration_audit_negative_controls` 280.05s -> 14.44s after removing per-case writer churn; `test_promoted_claims_have_falsifiable_negative_controls` 159.14s -> 5.45s after converting to prepared-artifact row contracts. The toy negative-control test now spends 53.48s repairing dependent contracts before caching rather than recording a partial gate signature.

## Wave 2.5 - independent review hardening

- First independent code review blocked on two real cache-validation risks: changed bootstrapped signatures could be stamped after existence-only checks, and repaired output contracts could be stamped without revalidating integration/sheaf validators.
- Fix: `ensure_gate_artifacts()` now records changed signatures only on exact cache hits or after `_gate_artifacts_present(root)` passes; existence-only changed-signature cache stamps were removed.
- Fix: `refresh_output_gate_contracts()` now revalidates animation, integration, and sheaf contracts after repairs and raises instead of stamping a signature if any issues remain.
- Fix: `refresh_gate_artifact_session_signature()` now checks `_gate_artifacts_present(root)` before recording the signature, so caller mistakes cannot stamp invalid full gate state.
- Fix: post-rebuild stamp paths in `refresh_generated_gate_artifacts()` and `ensure_gate_artifacts()` now reuse the strict signature helper instead of assigning cache signatures directly.
- Fix: `_gate_artifacts_present()` now includes `validate_animation_frame_deltas()`, so direct signature refresh cannot ignore animation contract failures.
- Regression tests added: `test_ensure_gate_artifacts_rejects_invalid_bootstrapped_changed_signature`, `test_refresh_output_gate_contracts_revalidates_repaired_artifacts`, `test_refresh_gate_artifact_session_signature_rejects_invalid_current_signature`, `test_refresh_generated_gate_artifacts_rejects_invalid_post_rebuild_signature`, `test_ensure_gate_artifacts_rejects_invalid_post_rebuild_signature`, and `test_gate_artifacts_present_requires_animation_deltas`.

## Wave 3 - final validation and browser QA

- Focused correctness: `uv run pytest tests/test_roadmap_promotion.py tests/gates/test_claim_ledger.py tests/gates/ -q` passed `46 passed in 374.41s`.
- Full coverage: `COVERAGE_FILE=/tmp/template_ai_full.coverage uv run pytest tests/ --cov=src --cov-fail-under=90 --durations=20 -q` passed `422 passed in 1324.95s` with `90.32%` source coverage.
- Regeneration chain was rerun in the established order: toy sweep, formal interop, integration audit, sheaf tracks, manuscript variables. Post-regeneration `scripts/validate_outputs.py` returned all checks true.
- Copied-root refresh: from `/Users/4d/Documents/GitHub/template`, `uv run python scripts/05_copy_outputs.py --project templates/template_active_inference` copied 155 files and passed the copy-stage validator. Explicit `validate_copied_outputs(Path("output/templates/template_active_inference"))` also passed after final regeneration.
- Browser QA: served `/Users/4d/Documents/GitHub/template/output/templates/template_active_inference` and opened `/web/index.html` in system Chrome at 375, 768, and 1280 px. Final evidence files are `.omo/ulw-loop/evidence/browser-qa/final-copied-root-browser-qa.json` and `final-viewport-{375,768,1280}.png`. All 34 images resolved, no page errors occurred, and resource failures were empty except for the browser favicon probe recorded in the server log. The 375 px screenshot shows wide generated manuscript tables using horizontal scroll rather than missing content; `horizontalOverflow: true` at 375px is therefore recorded as a known manuscript-table behavior.
- Expansion closed: no additional project-owned UI source edits were justified after copied-root serving and visual inspection; the remaining mobile table overflow is inherited pandoc manuscript-table behavior, not a broken exemplar artifact.
