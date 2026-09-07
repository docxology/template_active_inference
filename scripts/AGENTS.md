# Script Notes

## Thin-orchestrator contract

Scripts contain no business logic — parse arguments, resolve paths
(`Path(__file__)`-relative), call `../src/` functions, print output paths to
stdout for manifest collection. Use `MPLBACKEND=Agg` and fixed seeds. No
drift-prone facts (test counts, wall-time claims) in script output; counts
belong to test discovery and docs.

## Inventory

Every script in this directory is a thin orchestrator delegating to `../src/`;
`scripts/README.md` carries the authoritative script | purpose | delegates to |
command table and must match this directory's contents.

| Script | Delegates to |
|---|---|
| `run_analytical_sweep.py` | `src/analysis.run_analysis` |
| `compute_statistics.py` | `src/analysis.write_analysis_statistics` |
| `simulate_si_tmaze.py` | `src/simulation.si_runner`, `src/simulation.si_artifacts`, `src/simulation.pymdp_config` |
| `simulate_si_graph_world.py` | `src/simulation.graph_world` |
| `generate_figures.py` | `src/visualizations.figures` |
| `render_animation.py` | `src/visualizations.animation` |
| `generate_validation_spine.py` | `src/validation_spine` |
| `generate_toy_sweep_tracks.py` | `src/roadmap_tracks.write_toy_sweep_artifacts` |
| `generate_formal_interop_tracks.py` | `src/roadmap_tracks.write_formal_interop_artifacts` |
| `generate_integration_audit.py` | `src/roadmap_tracks.write_integration_audit_artifacts` |
| `generate_sheaf_tracks.py` | `src/roadmap_tracks.write_sheaf_track_artifacts` |
| `generate_method_inventory.py` | `src/gates.method_inventory` |
| `check_documentation_contract.py` | `src/gates.documentation_contract` |
| `compose_manuscript.py` | `src/manuscript.sheaf.cli` |
| `inject_variables.py` | `subprocess` → `scripts/z_generate_manuscript_variables.py` |
| `z_generate_manuscript_variables.py` | `src/manuscript.{variables,hydrate,sheaf}`, `src/roadmap_tracks` |
| `render_pdf.py` | `src/manuscript.{sheaf,hydrate,variables,render_helpers}` + external `pandoc`/`xelatex` |
| `validate_outputs.py` | `src/gates.validation` |
| `run_full_verification.py` | `src/orchestration.full_verification` |
| `run_ai_direct_fast.sh` | `uv run pytest tests --override-ini="python_files=test_*_direct.py"` |

## Ordering and dependencies

The `z_`-prefixed script runs last in lexicographic analysis order (it depends
on prior outputs). `generate_validation_spine.py` delegates to
`src/validation_spine/` and must run before the promoted roadmap scripts.
`generate_toy_sweep_tracks.py`, `generate_formal_interop_tracks.py`,
`generate_integration_audit.py`, and `generate_sheaf_tracks.py` must run before
`z_generate_manuscript_variables.py` so semantic certificates, dependency
graphs, canonical promoted artifacts, and hydrated variables bind the promoted
artifacts. The canonical generators also emit diffoscope, proof-extraction,
finite state-space, causal-ablation, license-audit, and release-note evidence
artifacts; do not reintroduce parallel versioned producer scripts for the same
track families. The legacy `inject_variables.py` forwarder shells out to
`z_generate_manuscript_variables.py`. `render_pdf.py` is the standalone PDF
build path (external `pandoc`/`xelatex`, no monorepo infrastructure import).
`run_ai_direct_fast.sh` cd's to the git toplevel and runs only the
`test_*_direct.py` family, so the gate-artifact prewarm never triggers.
