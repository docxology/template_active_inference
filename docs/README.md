# Active Inference exemplar documentation

This project studies deterministic toy Active Inference models. It combines a
closed-form Bernoulli–Ising analysis with pymdp simulations, a Lean proof
boundary, and shared GNN and ontology notation. The sheaf composer merges
track contributions into a manuscript and checks their shared evidence.

| Task | Guide |
| --- | --- |
| Install, run, render, or verify | [Quick start](quickstart.md) |
| Understand the model and interpretation limits | [Conceptual foundations](conceptual-foundations.md) |
| Understand module and track boundaries | [Architecture](architecture.md) and [pipeline track declarations](../tracks.yaml) |
| Edit the manuscript | [Manuscript index](manuscript/README.md) |
| Inspect data, figures, and publication products | [Rendering and reproducibility](reference/rendering-reproducibility.md) |
| Use the exemplar in another project | [Standalone and fork guide](standalone.md) |
| Investigate installation or test failures | [Troubleshooting](troubleshooting.md) and [testing philosophy](testing_philosophy.md) |
| Look up callable definitions | [Generated method inventory](reference/method-inventory.md) |
| Follow the design and maintenance record | [Ideal State Artifact](ISA.md), [backlog](TODO.md), and [code quality review](thermo-nuclear-code-quality-review.md) |

Use the [script directory reference](../scripts/README.md) to find artifact
producers and the [agent guide](agent_instructions.md) for contribution rules.

From the project root, check documentation, generated references, composition,
and artifact gates with the following commands.

```bash
uv run python scripts/check_documentation_contract.py --check
uv run python scripts/generate_method_inventory.py --check
uv run python scripts/compose_manuscript.py --validate-only --strict
uv run python scripts/validate_outputs.py
```
