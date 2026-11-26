# Quick start

## Install

Clone this repository and run the commands from its root. Install Git and
[uv](https://docs.astral.sh/uv/). The package requires Python 3.11 or later.
This guide selects Python 3.14 so the project and the pinned template use the
same Python series.

```bash
uv sync --locked --extra dev --python 3.14
```

The tracked `output/` tree contains a publication snapshot and gate fixtures.
Use a disposable clone for commands that regenerate artifacts.

## Run the analytical and simulation tracks

```bash
uv run python scripts/run_analytical_sweep.py
uv run python scripts/simulate_si_tmaze.py --seed 0 --mode state_inference
uv run python scripts/simulate_si_graph_world.py
uv run python scripts/compute_statistics.py
```

The sweep writes `output/data/parameter_sweep.csv`. The graph-world runner
writes a deterministic summary and trace. The statistics script combines the
available analytical and simulation evidence in
`output/data/analysis_statistics.json`.

The T-maze command writes the summary, trace, mode comparison, policy
posterior grid, and runtime diagnostics. The locked PyMDP release replays
the checked-in T-maze evidence with the default config and seed.
See [T-maze dependency compatibility](troubleshooting.md#t-maze-dependency-compatibility)
before changing the lockfile.

Use [`pymdp.yaml`](../pymdp.yaml) to configure the T-maze, [`tracks.yaml`](../tracks.yaml)
for producer and validation tracks, and [`figures.yaml`](../figures.yaml) for
figure generation and section bindings. [The reproducibility guide](reference/rendering-reproducibility.md#producer-order)
lists the full refresh order when inputs or manuscript claims change.

## Verify

```bash
uv run python scripts/compose_manuscript.py --validate-only --strict
uv run python scripts/check_documentation_contract.py --check
uv run python scripts/generate_method_inventory.py --check
uv run python scripts/validate_outputs.py
uv run python scripts/run_full_verification.py
```

The test runner uses serial, bounded batches and combines branch coverage
against the 90% floor. To run the Lean gate in `validate_outputs.py`, install
[Elan](https://github.com/leanprover/elan) and use the toolchain pinned in
[`lean/lean-toolchain`](../lean/lean-toolchain). See the [test guide](../tests/README.md)
for focused commands and how to refresh a stale gate fixture intentionally.

## Render a standalone PDF

Install Pandoc, the matching [pandoc-crossref](https://github.com/lierdakil/pandoc-crossref/releases)
filter, and XeLaTeX with the packages listed in
[the local preamble](manuscript/preamble.md). For an offline review based on
the checked-in evidence snapshot, run the local renderer.

```bash
uv run python scripts/render_pdf.py
```

The result is `output/pdf/template_active_inference_standalone.pdf`. For the
full publication PDF with template bookends and the HTML bundle, use the
pinned pipeline below.

## Render and validate with the pinned template

Install the [template's Pandoc and TeX prerequisites](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/README.md#L565-L574).
Start from the Active Inference project root with the source revision committed.

```bash
project_root="$(pwd)"
template_root="$(mktemp -d)/template"
git clone https://github.com/docxology/template "$template_root"
git -C "$template_root" checkout --detach 87704feae0a4490ef7dad9c931d268e8c30273c2
mkdir -p "$template_root/projects/working"
git clone "$project_root" "$template_root/projects/working/template_active_inference"

(
  cd "$template_root" &&
  uv sync --locked --no-dev --python 3.14 &&
  uv run --no-sync python scripts/pipeline/stage_03_render.py --project working/template_active_inference &&
  uv run --no-sync python scripts/pipeline/stage_04_validate.py --project working/template_active_inference
)
```

The physical checkout under `projects/working/` lets the pinned runner perform
supervised hydration. Stage 03 reads the [canonical manuscript](manuscript/README.md)
and the local evidence in `output/`. Stage 04 validates the enabled formats,
figure and evidence registries, and artifact structure. In the working project,
inspect these outputs.

- `output/pdf/template_active_inference_combined.pdf` for the publication PDF.
- `output/web/index.html` for the combined HTML.
- `output/reports/validation_report.md` for the validation receipt.
