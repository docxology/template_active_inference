# Analysis scripts

Thin orchestrators that import from `../src/` and handle I/O only: argparse,
path bootstrap, logging, and a single delegated call. Business, data, plot, and
analysis logic lives in `../src/` and is covered by `../tests/`.

| Script | Purpose | Delegates to | Command |
|---|---|---|---|
| `run_analytical_sweep.py` | Closed-form sweep over hyperparameters. | `src/analysis.run_analysis` | `uv run python scripts/run_analytical_sweep.py` |
| `compute_statistics.py` | Write the combined analytical + simulation statistics JSON. | `src/analysis.write_analysis_statistics` | `uv run python scripts/compute_statistics.py` |
| `simulate_si_tmaze.py` | Run the pymdp sophisticated-inference T-maze: policy comparison, posterior grid, runtime diagnostics. | `src/simulation.si_runner`, `src/simulation.si_artifacts`, `src/simulation.pymdp_config` | `uv run python scripts/simulate_si_tmaze.py` |
| `simulate_si_graph_world.py` | Write deterministic graph-world summary/trace artifacts. | `src/simulation.graph_world` | `uv run python scripts/simulate_si_graph_world.py` |
| `generate_figures.py` | Render figures from generated data. | `src/visualizations.figures` | `uv run python scripts/generate_figures.py` |
| `render_animation.py` | Render the trace-derived belief trajectory GIF. | `src/visualizations.animation` | `uv run python scripts/render_animation.py` |
| `generate_validation_spine.py` | Write provenance, deterministic replay, and counterexample matrix artifacts. | `src/validation_spine` | `uv run python scripts/generate_validation_spine.py` |
| `generate_toy_sweep_tracks.py` | Write sensitivity, uncertainty, benchmark, measured policy-grid, EFE, analytical-observable, graph-world topology-trace, graph-world invariant, state-space catalog, and causal-ablation artifacts. | `src/roadmap_tracks.write_toy_sweep_artifacts` | `uv run python scripts/generate_toy_sweep_tracks.py` |
| `generate_formal_interop_tracks.py` | Write model-checking, GNN, ontology, interop, Lean theorem inventory, and proof-extraction artifacts. | `src/roadmap_tracks.write_formal_interop_artifacts` | `uv run python scripts/generate_formal_interop_tracks.py` |
| `generate_integration_audit.py` | Write producer/stale/token/figure/scope/claim/adversarial, diffoscope, license, and release-note audit artifacts. | `src/roadmap_tracks.write_integration_audit_artifacts` | `uv run python scripts/generate_integration_audit.py` |
| `generate_sheaf_tracks.py` | Write the canonical semantic certificate, dependency graph, evidence-field index, release-bundle manifest, theorem traceability matrix, gate index, artifact diffoscope, proof extraction index, state-space catalog, causal-ablation matrix, artifact license audit, release-note evidence, security-posture audit, artifact-contract index, track-improvement scope, blocked-scope manifest, and consolidated promoted track artifacts. | `src/roadmap_tracks.write_sheaf_track_artifacts` | `uv run python scripts/generate_sheaf_tracks.py` |
| `generate_method_inventory.py` | Regenerate `docs/reference/method-inventory.md` from the live `src/` and `scripts/` AST so every `def` and `class` has a documented reference entry. | `src/gates.method_inventory` | `uv run python scripts/generate_method_inventory.py` |
| `check_documentation_contract.py` | Run the fail-closed documentation contract oracle for Markdown links, generated-doc links, README/AGENTS pairs, command context, and historical evidence wording. | `src/gates.documentation_contract` | `uv run python scripts/check_documentation_contract.py` |
| `compose_manuscript.py` | Sheaf-compose the multi-track sections into flat manuscript markdown. | `src/manuscript.sheaf.cli` | `uv run python scripts/compose_manuscript.py` |
| `inject_variables.py` | Forwarder to `z_generate_manuscript_variables.py` (single hydration entry point). | `subprocess` → `scripts/z_generate_manuscript_variables.py` | `uv run python scripts/inject_variables.py` |
| `z_generate_manuscript_variables.py` | Hydrate manuscript variables from run outputs and resolve `{{token}}` placeholders. | `src/manuscript.variables`, `src/manuscript.hydrate`, `src/manuscript.sheaf`, `src/roadmap_tracks` | `uv run python scripts/z_generate_manuscript_variables.py` |
| `render_pdf.py` | Standalone compose → hydrate → pandoc/`xelatex` PDF render of the manuscript. | `src/manuscript.sheaf`, `src/manuscript.hydrate`, `src/manuscript.variables`, `src/manuscript.render_helpers` + external `pandoc`/`xelatex` | `uv run python scripts/render_pdf.py` |
| `validate_outputs.py` | Run the validation gates over generated outputs. | `src/gates.validation` | `uv run python scripts/validate_outputs.py` |
| `run_full_verification.py` | Run preflight checks, deterministic verification chunks, chunked coverage subprocesses, and postflight checks for contract-sensitive sessions. | `src/orchestration.full_verification` | `uv run python scripts/run_full_verification.py` |
| `run_ai_direct_fast.sh` | Fast local pytest loop over the `test_*_direct.py` family (no gate-artifact prewarm). | `uv run pytest tests` (`--override-ini="python_files=test_*_direct.py"`) | `uv run bash scripts/run_ai_direct_fast.sh` |

The `z_`-prefixed script runs last in lexicographic analysis order (it depends
on prior outputs). Test counts and wall-time claims live in `../tests/` and
`docs/`, not in these scripts.
