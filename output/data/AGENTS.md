# AGENTS.md — `template_active_inference/output/data`

> Generated 2026-08-29 during the ongoing-docs fleet pass. Description derived
> from the on-disk listing; semantics marked (unverified) where not confirmed
> by running code.

## Role

Generated analysis data (CSV/NPZ/JSON) emitted by project analysis stages.

## Layout

- Files: `analysis_statistics.json`, `analytical_assumption_index.json`, `analytical_observable_sweep.json`, `animation_frame_deltas.json`, `artifact_contract_index.json`, `artifact_provenance.json`, `causal_ablation_matrix.json`, `cross_track_symbol_table.json`, `evidence_field_index.json`, `figure_source_map.json`, `gnn_roundtrip_report.json`, `interop_roundtrip_report.json`, `manuscript_evidence_tables.json`, `manuscript_token_provenance.json`, `manuscript_variables.json`, `ontology_alias_index.json`, `ontology_profile_matrix.json`, `parameter_sweep.csv`, `proof_dependency_graph.json`, `proof_extraction_index.json`, `publication_ledger.json`, `pymdp_policy_posterior_grid.json`, `scholarship_source_matrix.json`, `sensitivity_sweep.json`, `sheaf_coverage_matrix.json`, `sheaf_evidence_crosswalk.json`, `sheaf_gluing_certificate.json`, `sheaf_section_status_matrix.json`, `si_efe_terms.json`, `si_graph_world_summary.json`, `si_graph_world_topology_sweep.json`, `si_graph_world_topology_traces.json`, `si_graph_world_trace.json`, `si_policy_comparison.json`, `si_policy_grid.json`, `si_tmaze_summary.json`, `si_tmaze_trace.json`, `state_space_catalog.json`, `state_transition_table.json`, `statistical_visualization_bridge.json` … (+8 more)

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
