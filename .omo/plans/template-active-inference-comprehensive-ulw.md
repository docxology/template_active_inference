# Template Active Inference Comprehensive ULW Plan

## TODOs

- [x] Bootstrap HEAVY `.omo` state, research journal, and baseline evidence.
- [x] Complete codebase-only ultraresearch and init-deep scoring over AGENTS, tests, gates, artifacts, and copied/web output surfaces.
- [x] Update AGENTS hierarchy in init-deep update mode, preserving project-specific contracts and removing duplication.
- [x] Implement `MEDIUM-TEST-PERF-1` with failing-first perf evidence, one full-refresh characterization, cheaper equivalent negative controls, and no weakened tests.
- [x] Regenerate affected artifacts, refresh copied-root outputs, run project gates, run browser QA, and complete independent review.

## Acceptance Criteria

- `AGENTS.md` hierarchy is concise, non-duplicative, and specific to this exemplar; root trends toward the 50-150 line init-deep target without losing live contracts.
- The focused perf path demonstrates a meaningful before/after reduction in repeated full-refresh work while preserving validation behavior.
- `TODO.md` remains future-only: remove or update `MEDIUM-TEST-PERF-1` only after proof artifacts and tests exist.
- No public track IDs, artifact schemas, manuscript claim boundaries, or copied-output contracts are weakened.
- Final evidence includes project-local validation, copied-root validation, browser QA artifacts at 375/768/1280, cleanup receipts, and independent reviewer approval.

## Verification Commands

```bash
uv run pytest tests/test_roadmap_promotion.py tests/gates/test_claim_ledger.py --durations=20 -q --no-cov
uv run pytest tests/test_roadmap_promotion.py tests/gates/test_claim_ledger.py tests/gates/ -q
uv run python scripts/check_documentation_contract.py --check
uv run python scripts/generate_method_inventory.py --check
uv run python scripts/compose_manuscript.py --validate-only --strict
uv run python scripts/validate_outputs.py
COVERAGE_FILE=/tmp/template_ai_full.coverage uv run pytest tests/ --cov=src --cov-fail-under=90 --durations=20 -q
```

If artifacts are regenerated, run the established chain before validation:

```bash
uv run python scripts/generate_toy_sweep_tracks.py
uv run python scripts/generate_formal_interop_tracks.py
uv run python scripts/generate_integration_audit.py
uv run python scripts/generate_sheaf_tracks.py
uv run python scripts/z_generate_manuscript_variables.py
```

From repository root after generation:

```bash
uv run python scripts/05_copy_outputs.py --project templates/template_active_inference
uv run python -c "from pathlib import Path; from infrastructure.validation.output.validator import validate_copied_outputs; raise SystemExit(0 if validate_copied_outputs(Path('output/templates/template_active_inference')) else 1)"
```

## Manual QA Scenario

Serve copied web output with:

```bash
python3 -m http.server 8765 --directory /Users/4d/Documents/GitHub/template/output/templates/template_active_inference
```

Use Chrome against `http://127.0.0.1:8765/web/index.html`, capture screenshots at 375, 768, and 1280 px, and clean up the server process. PASS requires the page to load, copied figures to resolve, no page errors, and screenshots written under `.omo/ulw-loop/evidence/browser-qa/`.

## Final Verification Wave

- [x] Re-run all verification commands, browser QA, copied-root validation, `git diff --check`, and independent review; record cleanup receipts and final gate approval.
