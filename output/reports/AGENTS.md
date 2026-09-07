# AGENTS.md — `template_active_inference/output/reports`

> Generated 2026-08-29 during the ongoing-docs fleet pass. Description derived
> from the on-disk listing; semantics marked (unverified) where not confirmed
> by running code.

## Role

Generated validation/test/report JSON and text outputs.

## Layout

- Files: `ablation_sensitivity_report.json`, `adversarial_audit.json`, `artifact_diffoscope.json`, `artifact_license_audit.json`, `artifact_manifest.json`, `blocked_scope_manifest.json`, `claim_evidence_audit.json`, `counterexample_matrix.json`, `evidence_registry.json`, `figure_hash_manifest.json`, `gnn_lint_report.json`, `graph_world_invariants.json`, `invariants.json`, `lean_graph_world_inventory.json`, `lean_theorem_inventory.json`, `manuscript_composition.json`, `manuscript_staleness_report.json`, `model_checking_witnesses.json`, `output_statistics.json`, `output_statistics.txt`, `producer_completeness.json`, `pymdp_runtime_diagnostics.json`, `release_attestation.json`, `release_bundle_manifest.json`, `release_notes_evidence.json`, `rendered_provenance.json`, `replay_matrix.json`, `reproducibility_replay.json`, `scope_boundary_audit.json`, `security_posture_audit.json`, `sheaf_render_log.json`, `si_invariants.json`, `si_tmaze_run_report.json`, `stale_artifact_report.json`, `test_results.json`, `test_results.md`, `validation_report.json`, `validation_report.md`, `visualization_quality_audit.json`

## Invariants

- Local-only path under `projects/ongoing/` (root `.gitignore` rule
  `projects/*`) — never commit. Repo-wide policy: see
  `/Volumes/external_drive/Git/template/projects/ongoing/AGENTS.md`.
- Generated outputs in these trees are regenerable via the repo's canonical
  pipeline; do not hand-edit artifacts to "fix" results.
- This repo lives in the public docxology mirror — anything written here
  becomes public once pushed to the repo's GitHub remote.

## Gotchas

- No silent omissions: if a child folder above is undocumented, that is a
  known gap of this auto-generated doc — replace it with a verified
  description when you work here.
