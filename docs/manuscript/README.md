# Active Inference Multi-Track Exemplar manuscript

The publication source lives in this directory. Edit the
[IMRAD fragments](sections/imrad/README.md) and their
[section bindings](sheaf/manifest.yaml). The composer produces numbered
Markdown sections from those fragments. Edit the abstract, conclusion, and
references section directly. The coverage page is generated from the sheaf
registry and its coverage rules.

## Read the sections

- [Abstract](00_abstract.md) and [coverage map](00_00_sheaf_coverage.md)
- [Motivation and scope](02_intro_motivation.md) and [contributions](03_intro_contributions.md)
- Methods for the [analytical model](05_methods_analytical.md), [pymdp](06_methods_pymdp.md),
  [Lean](07_methods_lean.md), and [sheaf composition](08_methods_sheaf.md)
- Results for [mutual information](10_results_mi_sweep.md), [free energy](11_results_free_energy.md),
  [T-maze sophisticated inference](12_results_si_tmaze.md), and [invariants](13_results_invariants.md)
- [Discussion](15_discussion_outlook.md), [full sheaf appendix](16_appendix_full_sheaf.md),
  [conclusion](17_conclusion.md), and [references](99_references.md)

## Configure and render

[Paper configuration](config.yaml) holds metadata, render formats, and sheaf
settings. [The LaTeX preamble](preamble.md) controls typography and
[the bibliography](references.bib) supplies citations. [The figure registry](../../figures.yaml)
defines labels, captions, alt text, and section placements.

Use the [syntax guide](SYNTAX.md) for citations and tokens, then follow the
[quick start](../quickstart.md) to hydrate and render. The Python composer in
[`src/manuscript/`](../../src/manuscript/README.md) implements the processing.
The resolved files in `output/manuscript/` are generated renderer inputs.
