# Template Active Inference Comprehensive ULW Brief

Implement the approved HEAVY plan for `template_active_inference`:

- Run codebase-only ultraresearch over the AGENTS hierarchy, gate/test refresh flow, generated artifact contracts, and copied/web outputs.
- Run `init-deep` in update mode: trim the root `AGENTS.md`, enrich only high-scoring thin child `AGENTS.md` files, and remove parent/child duplication.
- Address `MEDIUM-TEST-PERF-1` by preserving one full-refresh characterization while replacing repeated full-refresh negative controls with cheaper equivalent source/row-contract controls.
- Keep public contracts stable: no track ID churn, no `_vN` siblings, no schema downgrade, no mocks/skips/xfails/weakened gates.
- Regenerate artifacts in the established order when needed, refresh copied-root outputs, validate project-local and copied output contracts, and run browser QA on copied web output.

Known baseline:

- `check_documentation_contract.py --check`, `generate_method_inventory.py --check`, `compose_manuscript.py --validate-only --strict`, `validate_outputs.py`, and copied-root validation were green before implementation.
- Focused perf baseline was interrupted after 3 passed tests in 385.98s; hotspot calls: `test_promoted_roadmap_artifacts_are_written_and_valid` 194.50s and `test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts` 141.42s.
- Leave any untracked `.codegraph` state alone unless explicitly directed.
