# Troubleshooting — Active Inference

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| Sheaf composition fails | Track registry and manifest are out of sync | Re-run `generate_sheaf_tracks.py` |
| Figure registry validation fails | Figures added but `figures.yaml` not updated | Update figure specs and re-run |
| Lean gate missing | `lake`/`lean` binaries not on PATH | Install the Lean 4 toolchain (see `lean/lean-toolchain` / `lean/AGENTS.md`) so `lake build` succeeds; the gate only auto-skips when `lean/lakefile.lean` itself is absent (`src/gates/lean.py::lean_project_present`) — there is no CLI flag to skip it while the lakefile is present |
| Analytical/simulation mismatch | Tracks disagree on shared claims | Run cross-track validation tests |


## T-maze dependency compatibility

Run the project with its own locked environment. A PyMDP 1.x environment
expects JAX lists of modality and factor arrays. The T-maze runner uses the
NumPy model API from PyMDP 0.0.7.1, which produced the checked-in evidence.
The lockfile also retains JAX for the runtime backend diagnostics.

From a disposable checkout, install the project environment and replay the
default seed. For example, compare the regenerated summary and policy grid
against the tracked baseline.

```bash
uv sync --locked --extra dev --python 3.14
uv run --no-sync python scripts/simulate_si_tmaze.py --seed 0 --mode state_inference
git diff --exit-code -- output/data/si_tmaze_summary.json output/data/pymdp_policy_posterior_grid.json
```

The policy comparison records the expected-utility fallback when the runner
cannot use the installed policy API. Check the trace, posterior grid, and
runtime diagnostics before adopting a different PyMDP release or adapter.
