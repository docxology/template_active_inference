# Ultrawork Notepad

## Skill Survey

- `omo:ulw-loop`: required for durable goal execution, evidence ledger, and real-surface QA.
- `omo:start-work`: required for Prometheus plan execution, Boulder state, delegated implementation, and completion gates.
- `omo:init-deep`: required for AGENTS hierarchy discovery, scoring, update-mode generation, and deduplication.
- `omo:ultraresearch`: required because the approved plan asks for codebase-only saturation research and an expansion journal.
- `omo:frontend`: required for generated web-output browser QA; design/perfection references loaded for QA routing, with no UI implementation unless a concrete defect is found.

## Tier

HEAVY. The approved work spans documentation architecture, test infrastructure, generated artifacts, copied outputs, browser-visible web output, and independent review.

## Success Criteria

- SC1: Codebase ultraresearch journal covers AGENTS, gates/tests, artifact contracts, copied/web outputs, and closes expansion leads.
- SC2: AGENTS hierarchy is updated in init-deep update mode with root trimmed and high-scoring thin child files enriched.
- SC3: `MEDIUM-TEST-PERF-1` is implemented with before/after duration evidence and preserved gate behavior.
- SC4: Generated and copied outputs validate green after any regeneration.
- SC5: Browser QA captures 375/768/1280 screenshots from copied web output and cleans up the server.
- SC6: Independent reviewer approves final diff and evidence.

## Manual QA Invocations

- CLI/data surface: exact verification commands listed in `.omo/plans/template-active-inference-comprehensive-ulw.md`.
- Browser surface: `python3 -m http.server 8765 --directory /Users/4d/Documents/GitHub/template/output/templates/template_active_inference`, then Chrome to `http://127.0.0.1:8765/web/index.html` at 375/768/1280. Serving the copied root is required because the generated web pages reference sibling copied-output folders such as `../figures/`.

## Findings

- Initial tracked exemplar/output status was clean.
- Existing memory says copied-root parity is part of done for this exemplar family.
- Subagent exploration was partially blocked, so main-lane code graph and source inspection supplied the missing AGENTS, gate-support, artifact, and copied-root findings.
- Init-deep update result: root `AGENTS.md` trimmed from 188 to 94 lines; high-surface thin children enriched at `manuscript/AGENTS.md`, `src/analytical/AGENTS.md`, `src/gnn/AGENTS.md`, `src/ontology/AGENTS.md`, and `src/simulation/AGENTS.md`.
- Failing-first perf proof: `.omo/evidence/gate-support-partial-red-evidence.txt` records the RED `AttributeError` for the new session-signature contract before implementation; after final cache hardening, `uv run pytest tests/test_gate_support_contracts.py -q --no-cov` passed `14 passed in 0.33s`.
- Independent review blocked earlier implementations for valid cache-stamping risks. Final fix removed existence-only changed-signature cache stamps from `ensure_gate_artifacts()`, made `refresh_output_gate_contracts()` revalidate animation, integration, and sheaf contracts before recording a signature, made `refresh_gate_artifact_session_signature()` refuse invalid full gate state, made post-rebuild stamp paths reuse that strict helper, and included animation deltas in `_gate_artifacts_present()`.
- Focused perf proof: final `uv run pytest tests/test_roadmap_promotion.py tests/gates/test_claim_ledger.py --durations=20 -q --no-cov` passed `18 passed in 273.73s`, compared with the prior interrupted baseline `3 passed in 385.98s`.
- Hotspot deltas: `test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts` 141.42s -> 55.53s; `test_integration_audit_negative_controls` 280.05s -> 14.44s; `test_promoted_claims_have_falsifiable_negative_controls` 159.14s -> 5.45s. `test_toy_sweep_negative_controls` now spends 53.48s repairing dependent contracts before caching instead of stamping partial state.
- Focused correctness proof: `uv run pytest tests/test_roadmap_promotion.py tests/gates/test_claim_ledger.py tests/gates/ -q` passed `46 passed in 374.41s`.
- Full project proof: `COVERAGE_FILE=/tmp/template_ai_full.coverage uv run pytest tests/ --cov=src --cov-fail-under=90 --durations=20 -q` passed `422 passed in 1324.95s` with `90.32%` source coverage.
- Project gates were green after final regeneration: documentation contract, method inventory, strict compose validation, and `scripts/validate_outputs.py`.
- Copied-root refresh was run from the template root after final regeneration with `uv run python scripts/05_copy_outputs.py --project templates/template_active_inference`, followed by copied-output validation on `output/templates/template_active_inference`.
- Browser QA evidence: Chrome screenshots and diagnostics are in `.omo/ulw-loop/evidence/browser-qa/`, with final evidence in `final-copied-root-browser-qa.json` and `final-viewport-{375,768,1280}.png`. At 375/768/1280 the served copied-root page returned 200, all 34 images resolved, no page errors occurred, and viewport screenshots were inspected. The only 404 was the browser favicon probe; wide generated manuscript tables remain horizontally scrollable on small screens, producing expected horizontal overflow at 375px.
- Final independent review proof: `lazycodex-code-reviewer` approved the current diff with no blockers in `.omo/evidence/template-active-inference-final-review-code-review.md`; watch items were limited to the pre-existing oversized gate helper and the documented browser QA caveats.
