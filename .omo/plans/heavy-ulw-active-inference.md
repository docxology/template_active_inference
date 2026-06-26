# Active Inference Heavy ULW Implementation Plan

## TL;DR
> Summary:      Implement the approved HEAVY ULW pass as a verifier-first, no-contract-churn change set for `template_active_inference`: refresh the AGENTS hierarchy, harden MEDIUM-TEST-PERF-1 by removing redundant heavy refreshes while preserving one end-to-end characterization, add copied-output parity validation, and capture real Chrome browser QA.
> Deliverables:
> - Root and selected child `AGENTS.md` files updated by init-deep rules, without generic duplication.
> - Faster roadmap-promotion and claim/manuscript gate tests, with RED->GREEN duration evidence and preserved negative controls.
> - Generated and copied-root output parity validation for canonical artifacts and web output.
> - Browser QA evidence for project-local and copied-root static web outputs.
> Effort:       Large
> Risk:         High - test speed work touches generated-artifact fixtures and can accidentally weaken the verifier gates or trust stale copied output.

## Scope
### Must have
- Update root `AGENTS.md` and the child `AGENTS.md` files that are too shallow to route workers safely; keep root near the existing 50-150 line init-deep target if possible, but do not delete project-specific contracts that current workers need.
- Preserve public contracts: no live track ID churn, no `_vN` sibling artifacts, no schema downgrade, no mock/skip/xfail, no weaker gate predicates, and no aggregate-boolean-only validation.
- Harden `MEDIUM-TEST-PERF-1` from `TODO.md:93`: split redundant full-refresh mutation paths in roadmap-promotion and claim/manuscript gates while retaining at least one fixed-point/end-to-end artifact-refresh characterization.
- Capture failing-first evidence: current focused hotspots are `tests/test_roadmap_promotion.py::test_promoted_roadmap_artifacts_are_written_and_valid` and `tests/test_roadmap_promotion.py::test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts`, with user-supplied interrupted baseline of 194.50s and 141.42s inside a 385.98s partial run.
- Add or extend generated/copied-output validation so the root copy at `/Users/4d/Documents/GitHub/template/output/templates/template_active_inference/` is checked against project-local `output/` after the root copy stage.
- Execute browser QA against real static web output using Chrome through Playwright; if Chrome is absent, use agent-browser from `https://github.com/vercel-labs/agent-browser`.
- Store evidence under `.omo/evidence/task-<N>-<slug>.<ext>` and keep `.codegraph` untouched.

### Must NOT have (guardrails, anti-slop, scope boundaries)
- No source or generated artifact hand-edit to make validation pass.
- No private/empirical/network/LLM/non-toy claims promoted out of `TODO.md` blocked scope.
- No edits to `.codegraph/`, no cleanup of unrelated local files, and no broad root-template refactor outside explicitly named files.
- No skipped, xfailed, deleted, renamed-away, mocked, or marker-hidden tests.
- No performance-only acceptance that ignores behavioral gates; every optimized path keeps a real negative control and a final full-suite pass.
- No browser QA substitution with HTML grep, curl-only checks, or static file existence checks.

## Verification strategy
> Zero human intervention - all verification is agent-executed.
- Test decision: TDD for test/perf and parity hardening, tests-after for AGENTS/docs-only edits; framework is pytest plus project scripts and real Chrome/Playwright browser QA.
- QA policy: every task has agent-executed scenarios, exact commands, and evidence paths.
- Evidence: `.omo/evidence/task-<N>-<slug>.<ext>`
- RED evidence: performance baseline over the named hotspots must be captured before changing tests; parity/browser negative controls must fail before the fix that makes them pass.
- GREEN evidence: focused tests, generated-output gates, copied-root parity, full project suite, and browser screenshots/action logs must all pass.

## Execution strategy
### Parallel execution waves
> Target 5-8 tasks per wave. <3 per wave (except final) = under-splitting.
> Extract shared dependencies as Wave-1 tasks to maximize parallelism.

Wave 1 (no dependencies):
- Task 1: Capture RED baseline, public-contract inventory, and perf target ledger.
- Task 2: Update AGENTS hierarchy using init-deep scoring.
- Task 3: Add generated/copied parity test skeleton and RED copied-output mismatch proof.
- Task 4: Prepare browser QA harness commands and RED missing/broken web-output proof.

Wave 2 (after Wave 1):
- Task 5: Harden shared gate-support refresh/cache helpers. Depends [1].
- Task 6: Split roadmap-promotion hotspot tests. Depends [1, 5].
- Task 7: Split claim-ledger/manuscript gate repeated refresh tests. Depends [1, 5].

Wave 3 (after Wave 2):
- Task 8: Wire copied-output validation to root copy and generated contracts. Depends [3, 5].
- Task 9: Refresh docs/TODO/test guidance with measured post-change evidence. Depends [2, 6, 7, 8].

Wave 4 (after Wave 3):
- Task 10: Run full project validation, copied-root parity, and real Chrome browser QA. Depends [4, 6, 7, 8, 9].

Critical path: Task 1 -> Task 5 -> Task 6 -> Task 9 -> Task 10

### Dependency matrix
| Task | Depends on | Blocks | Can parallelize with |
|------|------------|--------|----------------------|
| 1    | none       | 5, 6, 7, 9, 10 | 2, 3, 4 |
| 2    | none       | 9 | 1, 3, 4, 5 |
| 3    | none       | 8 | 1, 2, 4, 5 |
| 4    | none       | 10 | 1, 2, 3, 5 |
| 5    | 1          | 6, 7, 8 | 2, 3, 4 |
| 6    | 1, 5       | 9, 10 | 7, 8 |
| 7    | 1, 5       | 9, 10 | 6, 8 |
| 8    | 3, 5       | 9, 10 | 6, 7 |
| 9    | 2, 6, 7, 8 | 10 | none |
| 10   | 4, 6, 7, 8, 9 | final verification | none |

### Disjoint write scopes
| Task | Allowed write scope |
|------|---------------------|
| 1 | `.omo/evidence/task-1-*` only |
| 2 | `AGENTS.md`, `src/**/AGENTS.md`, `tests/**/AGENTS.md`, `scripts/AGENTS.md`, `docs/**/AGENTS.md`, `manuscript/**/AGENTS.md`, `lean/**/AGENTS.md`, `gnn/AGENTS.md`, `data/AGENTS.md` |
| 3 | `tests/test_copied_output_parity.py` or `tests/test_output_copy_parity.py`, `.omo/evidence/task-3-*` |
| 4 | `.omo/evidence/task-4-*` only |
| 5 | `tests/gate_support.py`, `tests/test_gate_support_contracts.py` |
| 6 | `tests/test_roadmap_promotion.py` only |
| 7 | `tests/gates/test_claim_ledger.py`, `tests/gates/test_manuscript_gates.py` only |
| 8 | `tests/test_copied_output_parity.py` or `tests/test_output_copy_parity.py`, `docs/reference/rendering-reproducibility.md` only |
| 9 | `TODO.md`, `tests/README.md`, `docs/reference/rendering-reproducibility.md` only |
| 10 | `.omo/evidence/task-10-*` only |

## Todos
> Implementation + Test = ONE task. Never separate.
> Every task MUST have: References + Acceptance Criteria + QA Scenarios + Commit.

- [ ] 1. Capture RED baseline and public-contract ledger

  What to do: Before code changes, capture the current hotspot durations, current green gate state, git cleanliness, and invariant contract list. Create `.omo/evidence/task-1-perf-baseline.json` with at least the two named tests, elapsed seconds, command, timestamp, git SHA, and pass/fail status. Record the forbidden churn surface: live track ids, schema ids, canonical artifact names, `_vN` absence, and `.codegraph` untouched status.
  Must NOT do: Do not edit tests, source, docs, generated outputs, `.codegraph`, or root copy outputs.

  Parallelization: Can parallel: YES | Wave 1 | Blocks: [5, 6, 7, 9, 10] | Blocked by: []

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `tests/test_roadmap_promotion.py:18` - module-level note identifies heavy generated-artifact tests and timeout context.
  - Pattern:  `tests/test_roadmap_promotion.py:38` - first user-named hotspot writes and validates promoted roadmap artifacts.
  - Pattern:  `tests/test_roadmap_promotion.py:188` - second user-named hotspot validates measured policy/topology trace artifacts.
  - Pattern:  `TODO.md:93` - `MEDIUM-TEST-PERF-1` scope and negative-control warning.
  - Pattern:  `src/roadmap_tracks/sheaf_track_validation.py:57` - live track registry validation rejects versioned live IDs and blocked empirical promotion.
  - Pattern:  `src/gates/output_checks.py:731` - `validate_outputs()` is the generated-output gate surface.

  Acceptance criteria (agent-executable only):
  - [ ] `test -s .omo/evidence/task-1-perf-baseline.json`
  - [ ] `python - <<'PY'\nimport json\np='.omo/evidence/task-1-perf-baseline.json'\ndata=json.load(open(p))\nrequired={'tests/test_roadmap_promotion.py::test_promoted_roadmap_artifacts_are_written_and_valid','tests/test_roadmap_promotion.py::test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts'}\ntests=data.get("tests", {})\nassert required.issubset(set(tests))\nassert all(float(tests.get(name, {}).get("seconds", 0)) > 0 for name in required)\nassert data["git_status_before"] == ''\nassert data["codegraph_untouched"] is True\nPY`
  - [ ] `uv run python scripts/validate_outputs.py > .omo/evidence/task-1-validate-outputs-baseline.json`

  QA scenarios (MANDATORY - task incomplete without these):
  > Name the exact tool AND its exact invocation - not "verify it works". Browser use: use Chrome to drive the page; if Chrome is not available, download and use agent-browser (https://github.com/vercel-labs/agent-browser). Computer use: OS-level GUI automation for a non-browser desktop app.
  ```
  Scenario: Focused hotspot RED/perf baseline
    Tool:     bash
    Steps:    mkdir -p .omo/evidence && /usr/bin/time -p uv run pytest tests/test_roadmap_promotion.py::test_promoted_roadmap_artifacts_are_written_and_valid tests/test_roadmap_promotion.py::test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts --durations=10 -q > .omo/evidence/task-1-hotspot-baseline.log 2>&1 && python - <<'PY'\nimport json,re,subprocess\nfrom pathlib import Path\nlog=Path('.omo/evidence/task-1-hotspot-baseline.log').read_text(encoding='utf-8')\nwanted=['tests/test_roadmap_promotion.py::test_promoted_roadmap_artifacts_are_written_and_valid','tests/test_roadmap_promotion.py::test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts']\nfound={}\nfor seconds,nodeid in re.findall(r'([0-9]+(?:\\.[0-9]+)?)s\\s+(?:call|setup|teardown)\\s+(tests/[^\\s]+)', log):\n    if nodeid in wanted:\n        found[nodeid]={'seconds': float(seconds)}\nreal=re.search(r'^real\\s+([0-9]+(?:\\.[0-9]+)?)', log, re.M)\nif len(found) != len(wanted) and real:\n    split=float(real.group(1))/len(wanted)\n    found={name:{'seconds': split} for name in wanted}\nstatus=subprocess.check_output(['git','status','--short'], text=True)\nPath('.omo/evidence/task-1-perf-baseline.json').write_text(json.dumps({'tests':found,'combined_seconds':sum(v['seconds'] for v in found.values()),'git_status_before':status,'codegraph_untouched':True}, indent=2), encoding='utf-8')\nassert set(wanted).issubset(found), found\nPY
    Expected: Command exits 0, log contains both named tests, and `.omo/evidence/task-1-perf-baseline.json` records positive elapsed seconds for each. This is RED for performance if either test exceeds 120s or combined elapsed exceeds 250s on this machine.
    Evidence: .omo/evidence/task-1-hotspot-baseline.log

  Scenario: Contract inventory guard
    Tool:     bash
    Steps:    git status --short > .omo/evidence/task-1-git-status.txt && find . -path '*/.codegraph/*' -prune -o -name AGENTS.md -exec wc -l {} + > .omo/evidence/task-1-agents-lines.txt && { echo 'git status:'; cat .omo/evidence/task-1-git-status.txt; echo 'AGENTS lines:'; cat .omo/evidence/task-1-agents-lines.txt; } > .omo/evidence/task-1-contract-inventory.txt
    Expected: `task-1-git-status.txt` is empty before implementation; `task-1-agents-lines.txt` records root `AGENTS.md` at 188 lines and child stubs including 5-8 line files.
    Evidence: .omo/evidence/task-1-contract-inventory.txt
  ```

  Commit: NO | Message: `test(active-inference): capture perf baseline` | Files: [.omo/evidence/task-1-*]

- [ ] 2. Update AGENTS hierarchy using init-deep scoring

  What to do: Apply init-deep update mode. Keep the root project-specific and concise; expand only child `AGENTS.md` files whose current 5-8 line stubs cannot route work safely. Add Where-to-look, local contracts, commands, and anti-patterns for high-complexity directories. Ensure children do not repeat parent content and do not introduce generic advice.
  Must NOT do: Do not edit source, tests, TODO, generated outputs, or `.codegraph`. Do not turn `AGENTS.md` into a long generated dump.

  Parallelization: Can parallel: YES | Wave 1 | Blocks: [9] | Blocked by: []

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `AGENTS.md:1` - root file title and current project-specific overview.
  - Pattern:  `AGENTS.md:11` - current registry-driven sheaf composition table.
  - Pattern:  `AGENTS.md:173` - current command list that must stay accurate.
  - Pattern:  `tests/AGENTS.md:1` - current tests child guidance surface.
  - Pattern:  `src/gates/AGENTS.md:31` - output-gate guidance that child files should link to, not duplicate.
  - Pattern:  `src/roadmap_tracks/AGENTS.md:1` - roadmap-track routing surface.
  - Pattern:  `manuscript/AGENTS.md:1` - one of the shallow child files needing init-deep review.

  Acceptance criteria (agent-executable only):
  - [ ] `find . -path '*/.codegraph/*' -prune -o -name AGENTS.md -exec wc -l {} + > .omo/evidence/task-2-agents-lines-after.txt`
  - [ ] `python - <<'PY'\nfrom pathlib import Path\nroot=Path('AGENTS.md')\nlines=root.read_text().splitlines()\nassert 50 <= len(lines) <= 220\nfor p in Path('.').rglob('AGENTS.md'):\n    if '.codegraph' in p.parts: continue\n    text=p.read_text(encoding='utf-8')\n    assert 'lorem' not in text.lower()\n    assert 'generic advice' not in text.lower()\nPY`
  - [ ] `uv run python scripts/check_documentation_contract.py --check > .omo/evidence/task-2-doc-contract.log`

  QA scenarios (MANDATORY - task incomplete without these):
  ```
  Scenario: Init-deep hierarchy has actionable child routing
    Tool:     bash
    Steps:    python - <<'PY'\nfrom pathlib import Path\nshort=[]\nfor p in Path('.').rglob('AGENTS.md'):\n    if '.codegraph' in p.parts: continue\n    n=len(p.read_text(encoding='utf-8').splitlines())\n    if n < 9: short.append(str(p))\nprint('\\n'.join(short))\nassert not short, short\nPY
    Expected: No child `AGENTS.md` remains below 9 lines unless the worker documents a project-specific reason in `.omo/evidence/task-2-short-agents-exceptions.txt`.
    Evidence: .omo/evidence/task-2-agents-lines-after.txt

  Scenario: Documentation gate rejects broken AGENTS/README pairs
    Tool:     bash
    Steps:    uv run python scripts/check_documentation_contract.py --check > .omo/evidence/task-2-doc-contract.log
    Expected: Command exits 0 and reports no README/AGENTS pair or generated-doc link failures.
    Evidence: .omo/evidence/task-2-doc-contract.log
  ```

  Commit: YES | Message: `docs(active-inference): refresh agent routing hierarchy` | Files: [AGENTS.md, **/AGENTS.md]

- [ ] 3. Add copied-output parity RED test skeleton

  What to do: Add a focused test file that proves a copied-root mismatch fails before the implementation. The RED case should compare project-local canonical artifacts with a temp copied-root mirror where one copied file has a different hash. Use real JSON files and hashes, not mocks. The test should initially fail because no shared parity helper/test exists or because the mismatch is not caught.
  Must NOT do: Do not depend on network, do not mutate root `output/templates/...` in place, and do not assert only aggregate booleans.

  Parallelization: Can parallel: YES | Wave 1 | Blocks: [8] | Blocked by: []

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `docs/reference/rendering-reproducibility.md:117` - root output parity is part of final acceptance.
  - Pattern:  `src/roadmap_tracks/sheaf_tracks_helpers.py:25` - root copied-output directory resolution.
  - Pattern:  `src/roadmap_tracks/sheaf_tracks_helpers.py:33` - `_copied_parity()` row-level hash/status contract.
  - Pattern:  `src/roadmap_tracks/sheaf_track_validation.py:407` - artifact-contract index recomputes copied parity from rows.
  - Pattern:  `src/roadmap_tracks/sheaf_track_validation.py:517` - release-bundle copied parity row validation.
  - Test:     `tests/test_track_consolidation_negative.py` - existing row-forgery negative pattern for generated artifacts.

  Acceptance criteria (agent-executable only):
  - [ ] `uv run pytest tests/test_copied_output_parity.py -q > .omo/evidence/task-3-red-copied-parity.log || true`
  - [ ] `python - <<'PY'\nlog=open('.omo/evidence/task-3-red-copied-parity.log', encoding='utf-8').read()\nassert 'FAILED' in log or 'AssertionError' in log\nassert 'copied' in log.lower() or 'parity' in log.lower()\nPY`

  QA scenarios (MANDATORY - task incomplete without these):
  ```
  Scenario: Copied mismatch fails
    Tool:     bash
    Steps:    uv run pytest tests/test_copied_output_parity.py::test_copied_output_hash_mismatch_fails -q > .omo/evidence/task-3-red-copied-parity.log 2>&1 || true
    Expected: Log contains `FAILED` or `AssertionError`; the failure is caused by a temp copied-root file hash mismatch.
    Evidence: .omo/evidence/task-3-red-copied-parity.log

  Scenario: Aggregate-forgery adversarial case fails
    Tool:     bash
    Steps:    uv run pytest tests/test_copied_output_parity.py::test_copied_output_aggregate_true_row_mismatch_fails -q > .omo/evidence/task-3-red-aggregate-forgery.log 2>&1 || true
    Expected: Log contains `FAILED` or `AssertionError`; a forged aggregate `all_copied_outputs_match_or_deferred=true` cannot hide a mismatched row.
    Evidence: .omo/evidence/task-3-red-aggregate-forgery.log
  ```

  Commit: NO | Message: `test(output): prove copied parity gap` | Files: [tests/test_copied_output_parity.py, .omo/evidence/task-3-*]

- [ ] 4. Prepare real Chrome browser QA harness and RED broken-web proof

  What to do: Capture browser QA commands before implementation and prove the QA channel fails on a deliberately broken temp copy of `output/web/index.html` or an empty temp web root. Use real Chrome through Playwright (`--channel=chrome`) because `/Applications/Google Chrome.app` exists; if the worker machine lacks Chrome, install/use agent-browser and record the substitution.
  Must NOT do: Do not treat curl, grep, or file existence as browser QA. Do not mutate real `output/web` or copied-root `web` in place.

  Parallelization: Can parallel: YES | Wave 1 | Blocks: [10] | Blocked by: []

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `output/web/index.html` - project-local static web entry point.
  - Pattern:  `/Users/4d/Documents/GitHub/template/output/templates/template_active_inference/web/index.html` - copied-root static web entry point after stage 05.
  - Pattern:  `docs/reference/rendering-reproducibility.md:36` - PDF/web outputs are render outputs, not source of truth.
  - Pattern:  `docs/reference/rendering-reproducibility.md:127` - final acceptance inspects project-local and copied-root output.
  - External: `https://github.com/vercel-labs/agent-browser` - browser fallback only if Chrome is absent.

  Acceptance criteria (agent-executable only):
  - [ ] `test -d '/Applications/Google Chrome.app' || command -v agent-browser`
  - [ ] `test -s .omo/evidence/task-4-browser-red.log`
  - [ ] `python - <<'PY'\nlog=open('.omo/evidence/task-4-browser-red.log', encoding='utf-8').read().lower()\nassert 'timeout' in log or 'not found' in log or 'failed' in log\nPY`

  QA scenarios (MANDATORY - task incomplete without these):
  ```
  Scenario: Browser QA fails on missing page
    Tool:     playwright(real Chrome)
    Steps:    mkdir -p /tmp/template-ai-empty-web && python -m http.server 48730 --bind 127.0.0.1 --directory /tmp/template-ai-empty-web > .omo/evidence/task-4-empty-web-server.log 2>&1 & echo $! > .omo/evidence/task-4-empty-web-server.pid; playwright screenshot --browser=chromium --channel=chrome --viewport-size=1440,1200 --full-page --wait-for-selector "main" http://127.0.0.1:48730/index.html .omo/evidence/task-4-browser-red.png > .omo/evidence/task-4-browser-red.log 2>&1 || true; kill $(cat .omo/evidence/task-4-empty-web-server.pid)
    Expected: Browser command fails or times out because the temp page does not contain `main`.
    Evidence: .omo/evidence/task-4-browser-red.log

  Scenario: Browser QA command can launch Chrome
    Tool:     playwright(real Chrome)
    Steps:    python -m http.server 48731 --bind 127.0.0.1 --directory output/web > .omo/evidence/task-4-web-server.log 2>&1 & echo $! > .omo/evidence/task-4-web-server.pid; playwright screenshot --browser=chromium --channel=chrome --viewport-size=1440,1200 --full-page --wait-for-selector "h1" http://127.0.0.1:48731/index.html .omo/evidence/task-4-browser-smoke.png > .omo/evidence/task-4-browser-smoke.log 2>&1; kill $(cat .omo/evidence/task-4-web-server.pid)
    Expected: Command exits 0 and `.omo/evidence/task-4-browser-smoke.png` is non-empty.
    Evidence: .omo/evidence/task-4-browser-smoke.png
  ```

  Commit: NO | Message: `test(web): capture browser qa harness` | Files: [.omo/evidence/task-4-*]

- [ ] 5. Harden shared gate-support refresh/cache helpers

  What to do: Improve `tests/gate_support.py` so repeated tests can reuse a valid generated-artifact fixed point without trusting stale state after mutation. Add tests in `tests/test_gate_support_contracts.py` proving cache signatures invalidate on required artifact hash changes and reuse without mutation does not trigger full regeneration. Keep one real full fixed-point refresh path intact.
  Must NOT do: Do not skip when `pymdp` is installed, do not mock producers, do not remove required artifacts from `_REQUIRED_GATE_ARTIFACTS`, and do not hide stale state by broad `except`.

  Parallelization: Can parallel: NO | Wave 2 | Blocks: [6, 7, 8] | Blocked by: [1]

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `tests/gate_support.py:34` - current bootstrap root/signature caches.
  - Pattern:  `tests/gate_support.py:37` - required artifact list that defines cache signature coverage.
  - Pattern:  `tests/gate_support.py:104` - signature hashes every required artifact.
  - Pattern:  `tests/gate_support.py:197` - `refresh_generated_gate_artifacts()` force/reuse behavior.
  - Pattern:  `tests/gate_support.py:239` - `_gate_artifacts_present()` validates semantic, integration, sheaf, and claim-ledger gates.
  - Pattern:  `tests/gate_support.py:271` - `ensure_gate_artifacts()` current expensive full bootstrap path.
  - Test:     `tests/test_gate_support_contracts.py` - existing contract-test file for gate-support behavior.

  Acceptance criteria (agent-executable only):
  - [ ] `uv run pytest tests/test_gate_support_contracts.py -q > .omo/evidence/task-5-gate-support-contracts.log`
  - [ ] `python - <<'PY'\nfrom pathlib import Path\ntext=Path('tests/gate_support.py').read_text()\nassert 'xfail' not in text\nassert 'skip(' not in text or 'pymdp not installed' in text\nassert '_REQUIRED_GATE_ARTIFACTS' in text\nPY`
  - [ ] `git diff -- tests/gate_support.py tests/test_gate_support_contracts.py > .omo/evidence/task-5-diff.patch`

  QA scenarios (MANDATORY - task incomplete without these):
  ```
  Scenario: Cache invalidates on artifact mutation
    Tool:     bash
    Steps:    uv run pytest tests/test_gate_support_contracts.py::test_gate_artifact_signature_changes_when_required_artifact_changes -q > .omo/evidence/task-5-cache-invalidates.log
    Expected: Test passes and proves a mutated required artifact changes the signature.
    Evidence: .omo/evidence/task-5-cache-invalidates.log

  Scenario: Cache reuses settled artifacts without full refresh
    Tool:     bash
    Steps:    uv run pytest tests/test_gate_support_contracts.py::test_ensure_gate_artifacts_reuses_valid_signature_without_forced_refresh -q > .omo/evidence/task-5-cache-reuse.log
    Expected: Test passes without mocks/skips/xfails and asserts the reuse branch from a valid signature.
    Evidence: .omo/evidence/task-5-cache-reuse.log
  ```

  Commit: YES | Message: `test(active-inference): harden gate artifact cache` | Files: [tests/gate_support.py, tests/test_gate_support_contracts.py]

- [ ] 6. Split roadmap-promotion hotspot tests without weakening gates

  What to do: Refactor `tests/test_roadmap_promotion.py` so only one test performs the full write/validate fixed-point characterization and row-only/source-only checks reuse the validated artifacts. Keep the two user-named tests behaviorally meaningful: the promoted-artifacts test still writes each producer at least once and validates all promoted artifact families; the measured-policy/topology test should avoid redundant full fixed-point refresh when artifacts are already valid. Capture before/after duration comparison.
  Must NOT do: Do not remove `pytest.mark.long_running`, do not delete negative controls, do not skip `run_figure()` coverage for `track_lane_promotion_map`, `artifact_contract_map`, or `security_posture_map`, and do not assert only file existence.

  Parallelization: Can parallel: YES | Wave 2 | Blocks: [9, 10] | Blocked by: [1, 5]

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `tests/test_roadmap_promotion.py:38` - promoted-artifacts hotspot and required path/validator assertions.
  - Pattern:  `tests/test_roadmap_promotion.py:51` - `ensure_gate_artifacts()` entry into heavy bootstrap.
  - Pattern:  `tests/test_roadmap_promotion.py:113` - toy/formal validators that must remain.
  - Pattern:  `tests/test_roadmap_promotion.py:115` - figure rendering assertions that must remain.
  - Pattern:  `tests/test_roadmap_promotion.py:123` - toy sweep negative controls, including source/row mutation expectations.
  - Pattern:  `tests/test_roadmap_promotion.py:188` - measured policy/topology hotspot.
  - Pattern:  `tests/README.md:56` - guidance to keep one end-to-end refresh characterization and split duplicate row-only cases.

  Acceptance criteria (agent-executable only):
  - [ ] `uv run pytest tests/test_roadmap_promotion.py::test_promoted_roadmap_artifacts_are_written_and_valid tests/test_roadmap_promotion.py::test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts --durations=10 -q > .omo/evidence/task-6-hotspots-green.log`
  - [ ] `uv run pytest tests/test_roadmap_promotion.py -q --durations=20 > .omo/evidence/task-6-roadmap-module-green.log`
  - [ ] `python - <<'PY'\nimport re\nlog=open('.omo/evidence/task-6-hotspots-green.log', encoding='utf-8').read()\nassert 'passed' in log\nassert 'skipped' not in log.lower()\n# Worker must update perf JSON after the run.\nimport json\ndata=json.load(open('.omo/evidence/task-6-perf-after.json'))\nassert data['combined_seconds'] <= max(250.0, data['baseline_combined_seconds'] * 0.75)\nPY`

  QA scenarios (MANDATORY - task incomplete without these):
  ```
  Scenario: Hotspots go GREEN under the perf target
    Tool:     bash
    Steps:    /usr/bin/time -p uv run pytest tests/test_roadmap_promotion.py::test_promoted_roadmap_artifacts_are_written_and_valid tests/test_roadmap_promotion.py::test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts --durations=10 -q > .omo/evidence/task-6-hotspots-green.log 2>&1 && python - <<'PY'\nimport json,re\nfrom pathlib import Path\nbaseline=json.loads(Path('.omo/evidence/task-1-perf-baseline.json').read_text(encoding='utf-8'))\nlog=Path('.omo/evidence/task-6-hotspots-green.log').read_text(encoding='utf-8')\nwanted=['tests/test_roadmap_promotion.py::test_promoted_roadmap_artifacts_are_written_and_valid','tests/test_roadmap_promotion.py::test_toy_sweep_uses_measured_policy_and_topology_trace_artifacts']\nfound={}\nfor seconds,nodeid in re.findall(r'([0-9]+(?:\\.[0-9]+)?)s\\s+(?:call|setup|teardown)\\s+(tests/[^\\s]+)', log):\n    if nodeid in wanted:\n        found[nodeid]={'seconds': float(seconds)}\nreal=re.search(r'^real\\s+([0-9]+(?:\\.[0-9]+)?)', log, re.M)\ncombined=sum(v['seconds'] for v in found.values()) or (float(real.group(1)) if real else 0.0)\nPath('.omo/evidence/task-6-perf-after.json').write_text(json.dumps({'tests':found,'combined_seconds':combined,'baseline_combined_seconds':baseline['combined_seconds']}, indent=2), encoding='utf-8')\nassert combined > 0\nPY
    Expected: Command exits 0; `.omo/evidence/task-6-perf-after.json` shows combined seconds at least 25 percent below Task 1 baseline or below 250s, whichever is less strict.
    Evidence: .omo/evidence/task-6-hotspots-green.log

  Scenario: Negative controls still catch artifact forgeries
    Tool:     bash
    Steps:    uv run pytest tests/test_roadmap_promotion.py::test_toy_sweep_negative_controls tests/test_roadmap_promotion.py::test_formal_interop_negative_controls -q > .omo/evidence/task-6-negative-controls.log
    Expected: Command exits 0; no skip/xfail; row and artifact mutations still produce validator issues.
    Evidence: .omo/evidence/task-6-negative-controls.log
  ```

  Commit: YES | Message: `test(active-inference): split roadmap promotion refreshes` | Files: [tests/test_roadmap_promotion.py]

- [ ] 7. Split claim-ledger and manuscript gate repeated refresh tests

  What to do: Apply the same performance discipline to `tests/gates/test_claim_ledger.py` and `tests/gates/test_manuscript_gates.py`: keep one real manuscript/claim-ledger gate characterization, move duplicate marker variants to cheap source/row-contract checks where valid, and prove each matching validator still fails on a concrete negative input.
  Must NOT do: Do not weaken `claim_ledger_valid`, `methods_sheaf_layers`, duplicate marker, or hydration gates. Do not use mocks or skip xelatex/pandoc-adjacent behavior by marking tests out.

  Parallelization: Can parallel: YES | Wave 2 | Blocks: [9, 10] | Blocked by: [1, 5]

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `tests/gates/test_claim_ledger.py:14` - missing claim ledger file negative.
  - Pattern:  `tests/gates/test_claim_ledger.py:26` - generated figure removal negative.
  - Pattern:  `tests/gates/test_claim_ledger.py:40` - cheap typed-claim predicate success cases.
  - Pattern:  `tests/gates/test_claim_ledger.py:173` - structured predicate failures.
  - Pattern:  `tests/gates/test_manuscript_gates.py:20` - minimal manuscript gate artifact preparation currently calls full gate support.
  - Pattern:  `tests/gates/test_manuscript_gates.py:54` - methods sheaf layers negative.
  - Pattern:  `tests/gates/test_manuscript_gates.py:66` - repeated marker variants identified as performance candidates.
  - Pattern:  `tests/gates/test_manuscript_gates.py:182` - one source-fragment duplicate marker end-to-end characterization to preserve.

  Acceptance criteria (agent-executable only):
  - [ ] `uv run pytest tests/gates/test_claim_ledger.py tests/gates/test_manuscript_gates.py --durations=20 -q > .omo/evidence/task-7-gates-green.log`
  - [ ] `python - <<'PY'\nlog=open('.omo/evidence/task-7-gates-green.log', encoding='utf-8').read().lower()\nassert 'passed' in log\nassert 'skipped' not in log\nassert 'xfailed' not in log\nPY`
  - [ ] `git diff -- tests/gates/test_claim_ledger.py tests/gates/test_manuscript_gates.py > .omo/evidence/task-7-diff.patch`

  QA scenarios (MANDATORY - task incomplete without these):
  ```
  Scenario: Claim ledger negatives still fail closed
    Tool:     bash
    Steps:    uv run pytest tests/gates/test_claim_ledger.py::test_validate_manuscript_claim_ledger_missing_file_negative tests/gates/test_claim_ledger.py::test_validate_manuscript_claim_ledger_negative -q > .omo/evidence/task-7-claim-ledger-negatives.log
    Expected: Command exits 0 and both negatives observe `claim_ledger_valid is False`.
    Evidence: .omo/evidence/task-7-claim-ledger-negatives.log

  Scenario: Manuscript marker negatives preserve one E2E path
    Tool:     bash
    Steps:    uv run pytest tests/gates/test_manuscript_gates.py::test_validate_manuscript_methods_sheaf_layers_negative tests/gates/test_manuscript_gates.py::test_validate_manuscript_duplicate_track_marker_negative -q > .omo/evidence/task-7-manuscript-negatives.log
    Expected: Command exits 0; one source-fragment duplicate marker still composes and fails the gate.
    Evidence: .omo/evidence/task-7-manuscript-negatives.log
  ```

  Commit: YES | Message: `test(active-inference): reduce redundant manuscript gate refreshes` | Files: [tests/gates/test_claim_ledger.py, tests/gates/test_manuscript_gates.py]

- [ ] 8. Wire copied-output parity validation through root copy

  What to do: Finish the copied-output parity test from Task 3 and document/validate the root copy flow. The test must create a temp copied-root fixture for row-forgery cases and also include an integration path that runs the real root copy command before comparing canonical project-local files against `/Users/4d/Documents/GitHub/template/output/templates/template_active_inference/`.
  Must NOT do: Do not edit generated output by hand. Do not let "deferred" rows count as matched after the copy stage when the copied file exists and hashes differ.

  Parallelization: Can parallel: YES | Wave 3 | Blocks: [9, 10] | Blocked by: [3, 5]

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `/Users/4d/Documents/GitHub/template/scripts/05_copy_outputs.py:67` - root copy script entry point.
  - Pattern:  `/Users/4d/Documents/GitHub/template/scripts/05_copy_outputs.py:85` - repo/project root resolution.
  - Pattern:  `/Users/4d/Documents/GitHub/template/scripts/05_copy_outputs.py:102` - `copy_final_deliverables()` invocation.
  - Pattern:  `/Users/4d/Documents/GitHub/template/scripts/05_copy_outputs.py:105` - copied-output validation after copy.
  - Pattern:  `docs/reference/rendering-reproducibility.md:122` - current documented root pipeline/core-only validation commands.
  - Pattern:  `src/roadmap_tracks/sheaf_tracks_helpers.py:33` - row-level `_copied_parity()`.
  - Pattern:  `src/gates/output_checks.py:521` - artifact-contract copied parity gate.
  - Pattern:  `src/gates/output_checks.py:543` - release-bundle copied-output parity gate.

  Acceptance criteria (agent-executable only):
  - [ ] `uv run pytest tests/test_copied_output_parity.py -q > .omo/evidence/task-8-copied-parity-tests.log`
  - [ ] `cd /Users/4d/Documents/GitHub/template && uv run python scripts/05_copy_outputs.py --project templates/template_active_inference > /Users/4d/Documents/GitHub/template/projects/templates/template_active_inference/.omo/evidence/task-8-root-copy.log`
  - [ ] `python - <<'PY' > .omo/evidence/task-8-copied-parity.json\nimport hashlib,json\nfrom pathlib import Path\nproject=Path('/Users/4d/Documents/GitHub/template/projects/templates/template_active_inference')\ncopied=Path('/Users/4d/Documents/GitHub/template/output/templates/template_active_inference')\nrels=['data/artifact_contract_index.json','reports/release_bundle_manifest.json','data/sheaf_gluing_certificate.json','data/validation_dependency_graph.json','reports/visualization_quality_audit.json','web/index.html']\nrows=[]\nfor rel in rels:\n    src=project/'output'/rel\n    dst=copied/rel\n    sh=hashlib.sha256(src.read_bytes()).hexdigest()\n    dh=hashlib.sha256(dst.read_bytes()).hexdigest()\n    rows.append({'rel':rel,'source_exists':src.is_file(),'copied_exists':dst.is_file(),'match':sh==dh,'source_sha256':sh,'copied_sha256':dh})\nassert all(r['source_exists'] and r['copied_exists'] and r['match'] for r in rows), rows\nprint(json.dumps({'rows':rows,'all_match':True}, indent=2))\nPY`

  QA scenarios (MANDATORY - task incomplete without these):
  ```
  Scenario: Real root copy parity passes
    Tool:     bash
    Steps:    cd /Users/4d/Documents/GitHub/template && uv run python scripts/05_copy_outputs.py --project templates/template_active_inference > /Users/4d/Documents/GitHub/template/projects/templates/template_active_inference/.omo/evidence/task-8-root-copy.log && cd /Users/4d/Documents/GitHub/template/projects/templates/template_active_inference && python - <<'PY' > .omo/evidence/task-8-copied-parity.json\nimport hashlib,json\nfrom pathlib import Path\nproject=Path('/Users/4d/Documents/GitHub/template/projects/templates/template_active_inference')\ncopied=Path('/Users/4d/Documents/GitHub/template/output/templates/template_active_inference')\nrels=['data/artifact_contract_index.json','reports/release_bundle_manifest.json','data/sheaf_gluing_certificate.json','data/validation_dependency_graph.json','reports/visualization_quality_audit.json','web/index.html']\nrows=[]\nfor rel in rels:\n    src=project/'output'/rel; dst=copied/rel\n    rows.append({'rel':rel,'match':hashlib.sha256(src.read_bytes()).hexdigest()==hashlib.sha256(dst.read_bytes()).hexdigest()})\nassert all(r['match'] for r in rows), rows\nprint(json.dumps({'all_match': True, 'rows': rows}, indent=2))\nPY
    Expected: Root copy exits 0 and JSON evidence reports `all_match: true` for all listed artifacts.
    Evidence: .omo/evidence/task-8-copied-parity.json

  Scenario: Copied-root row forgery fails
    Tool:     bash
    Steps:    uv run pytest tests/test_copied_output_parity.py::test_copied_output_aggregate_true_row_mismatch_fails -q > .omo/evidence/task-8-row-forgery-green.log
    Expected: Test exits 0 by proving the validator rejects a forged aggregate with mismatched copied rows.
    Evidence: .omo/evidence/task-8-row-forgery-green.log
  ```

  Commit: YES | Message: `test(output): verify copied root parity` | Files: [tests/test_copied_output_parity.py, docs/reference/rendering-reproducibility.md]

- [ ] 9. Refresh TODO, tests README, and reproducibility docs with measured evidence

  What to do: Update `TODO.md` to remove or revise `MEDIUM-TEST-PERF-1` only after the perf proof and preserved gate proof are present. Update `tests/README.md` slow-gate guidance with current post-change duration facts. Update `docs/reference/rendering-reproducibility.md` only if Task 8 changed the exact copied-output validation command. Keep wording future-only and evidence-bound.
  Must NOT do: Do not claim publication readiness from local readiness. Do not add stale historical duration claims without dates and commands. Do not keep completed TODO rows as open work.

  Parallelization: Can parallel: NO | Wave 3 | Blocks: [10] | Blocked by: [2, 6, 7, 8]

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `TODO.md:1` - backlog is future-only.
  - Pattern:  `TODO.md:9` - current verification evidence block format.
  - Pattern:  `TODO.md:26` - promotion rule forbids `_vN` sibling proliferation.
  - Pattern:  `TODO.md:78` - medium upcoming table.
  - Pattern:  `TODO.md:93` - active `MEDIUM-TEST-PERF-1` row.
  - Pattern:  `tests/README.md:32` - slow full-suite gates section.
  - Pattern:  `docs/reference/rendering-reproducibility.md:48` - producer order.
  - Pattern:  `docs/reference/rendering-reproducibility.md:117` - root output parity section.

  Acceptance criteria (agent-executable only):
  - [ ] `uv run python scripts/check_documentation_contract.py --check > .omo/evidence/task-9-doc-contract.log`
  - [ ] `uv run pytest tests/test_documentation_contracts.py tests/test_method_inventory.py -q > .omo/evidence/task-9-doc-tests.log`
  - [ ] `python - <<'PY'\nfrom pathlib import Path\ntext=Path('TODO.md').read_text()\nassert 'MEDIUM-TEST-PERF-1' not in text or 'current slowest rows' not in text\nassert '_vN' in text\nassert 'Current evidence on' in text\nPY`

  QA scenarios (MANDATORY - task incomplete without these):
  ```
  Scenario: Documentation contract remains green
    Tool:     bash
    Steps:    uv run python scripts/check_documentation_contract.py --check > .omo/evidence/task-9-doc-contract.log
    Expected: Command exits 0.
    Evidence: .omo/evidence/task-9-doc-contract.log

  Scenario: Backlog is future-only after perf closure
    Tool:     bash
    Steps:    python - <<'PY' > .omo/evidence/task-9-todo-audit.txt\nfrom pathlib import Path\ntext=Path('TODO.md').read_text()\nprint(text)\nassert 'completed' not in text.lower() or 'Completed work belongs' in text\nassert 'MEDIUM-TEST-PERF-1' not in text or 'future' in text.lower()\nPY
    Expected: Audit exits 0 and printed TODO keeps only future open rows.
    Evidence: .omo/evidence/task-9-todo-audit.txt
  ```

  Commit: YES | Message: `docs(active-inference): record perf and parity evidence` | Files: [TODO.md, tests/README.md, docs/reference/rendering-reproducibility.md]

- [ ] 10. Run full validation, copied-root parity, and real browser QA

  What to do: Execute final worker-owned validation after all implementation commits. Capture full project gates, root copy parity, and browser screenshots for project-local and copied-root web outputs. Tear down any servers and record cleanup.
  Must NOT do: Do not declare done from test logs alone. Do not leave HTTP servers, tmux sessions, temp dirs, or browser contexts running.

  Parallelization: Can parallel: NO | Wave 4 | Blocks: [final verification] | Blocked by: [4, 6, 7, 8, 9]

  References (executor has NO interview context - be exhaustive):
  - Pattern:  `src/orchestration/full_verification.py:35` - full verification workflow entry.
  - Pattern:  `src/orchestration/full_verification.py:93` - postflight full-suite sequence.
  - Pattern:  `src/orchestration/full_verification.py:117` - full-suite coverage command and `--durations=20`.
  - Pattern:  `docs/reference/rendering-reproducibility.md:117` - root output parity final acceptance.
  - Pattern:  `output/web/index.html` - project-local browser surface.
  - Pattern:  `/Users/4d/Documents/GitHub/template/output/templates/template_active_inference/web/index.html` - copied-root browser surface.

  Acceptance criteria (agent-executable only):
  - [ ] `uv run python scripts/validate_outputs.py > .omo/evidence/task-10-validate-outputs.json`
  - [ ] `COVERAGE_FILE=/tmp/template_ai_full_after.coverage uv run pytest tests/ --cov=src --cov-fail-under=90 --durations=20 -q > .omo/evidence/task-10-full-suite.log`
  - [ ] `cd /Users/4d/Documents/GitHub/template && uv run python scripts/05_copy_outputs.py --project templates/template_active_inference > /Users/4d/Documents/GitHub/template/projects/templates/template_active_inference/.omo/evidence/task-10-root-copy.log`
  - [ ] `test -s .omo/evidence/task-10-browser-local.png && test -s .omo/evidence/task-10-browser-copied.png`
  - [ ] `git status --short > .omo/evidence/task-10-git-status-final.txt`

  QA scenarios (MANDATORY - task incomplete without these):
  ```
  Scenario: Full project gate is green with durations
    Tool:     bash
    Steps:    COVERAGE_FILE=/tmp/template_ai_full_after.coverage uv run pytest tests/ --cov=src --cov-fail-under=90 --durations=20 -q > .omo/evidence/task-10-full-suite.log 2>&1
    Expected: Command exits 0, coverage is at least 90%, and log includes the durations table with no skip/xfail introduced by this work.
    Evidence: .omo/evidence/task-10-full-suite.log

  Scenario: Real Chrome browser QA for local and copied web
    Tool:     playwright(real Chrome)
    Steps:    python -m http.server 48731 --bind 127.0.0.1 --directory output/web > .omo/evidence/task-10-local-web-server.log 2>&1 & echo $! > .omo/evidence/task-10-local-web-server.pid; playwright screenshot --browser=chromium --channel=chrome --viewport-size=1440,1200 --full-page --wait-for-selector "h1" http://127.0.0.1:48731/index.html .omo/evidence/task-10-browser-local.png > .omo/evidence/task-10-browser-local.log 2>&1; kill $(cat .omo/evidence/task-10-local-web-server.pid); python -m http.server 48732 --bind 127.0.0.1 --directory /Users/4d/Documents/GitHub/template/output/templates/template_active_inference/web > .omo/evidence/task-10-copied-web-server.log 2>&1 & echo $! > .omo/evidence/task-10-copied-web-server.pid; playwright screenshot --browser=chromium --channel=chrome --viewport-size=390,844 --full-page --wait-for-selector "h1" http://127.0.0.1:48732/index.html .omo/evidence/task-10-browser-copied.png > .omo/evidence/task-10-browser-copied.log 2>&1; kill $(cat .omo/evidence/task-10-copied-web-server.pid)
    Expected: Both screenshot commands exit 0; both PNG files are non-empty; server PIDs are killed.
    Evidence: .omo/evidence/task-10-browser-local.png and .omo/evidence/task-10-browser-copied.png
  ```

  Commit: NO | Message: `test(active-inference): capture final qa evidence` | Files: [.omo/evidence/task-10-*]

## Final verification wave (MANDATORY - after all implementation tasks)
> Runs in PARALLEL. ALL must APPROVE. Surface results to the caller and wait for an explicit "okay" before declaring complete.
- [ ] F1. Plan compliance audit - every task done, every acceptance criterion met
- [ ] F2. Code quality review - diagnostics clean, idioms match, no dead code
- [ ] F3. Real manual QA - every QA scenario executed with evidence captured
- [ ] F4. Scope fidelity - nothing extra shipped beyond Must-Have, nothing Must-NOT-Have introduced

## Commit strategy
- One logical change per commit. Conventional Commits (`<type>(<scope>): <subject>` body + footer).
- Atomic: every commit builds and passes tests on its own.
- No "WIP" / "fix typo squash later" commits on the final branch - clean up before merge.
- Reference the plan file path in the final commit footer: `Plan: .omo/plans/heavy-ulw-active-inference.md`.
- Suggested commits:
  - `docs(active-inference): refresh agent routing hierarchy`
  - `test(active-inference): harden gate artifact cache`
  - `test(active-inference): split roadmap promotion refreshes`
  - `test(active-inference): reduce redundant manuscript gate refreshes`
  - `test(output): verify copied root parity`
  - `docs(active-inference): record perf and parity evidence`

## Success criteria
- All Must-Have shipped; all QA scenarios pass with captured evidence; F1-F4 approved; commit history clean.
- Focused hotspot command improves by at least 25 percent from Task 1 baseline or lands below 250 seconds combined on the same machine, while every preserved negative control still passes.
- `uv run python scripts/validate_outputs.py`, documentation contract, method inventory, focused perf tests, copied parity tests, and full suite with `--cov-fail-under=90 --durations=20` are green.
- Root-copy parity confirms project-local and `/Users/4d/Documents/GitHub/template/output/templates/template_active_inference/` canonical artifacts match after stage 05.
- Real Chrome browser QA captures non-empty desktop and mobile screenshots for local and copied web outputs.
- No `.codegraph` changes, no unrelated source edits, no track/schema/artifact churn, no mocks/skips/xfails/weakened gates.

## Risk and adversarial classes
- stale_state: generated outputs or copied-root outputs can look current while source-owned producers have changed; require producer reruns and hash parity.
- misleading_success_output: green focused tests can hide skipped/xfail paths or warm-cache no-ops; logs must show no skips/xfails and must include durations.
- aggregate_forgery: aggregate booleans can be true while rows are false; tests must mutate row-level copied parity and canonical artifact rows.
- cache_poisoning: `ensure_gate_artifacts()` can reuse a signature after mutation if required artifacts are incomplete; Task 5 must prove invalidation.
- source_only_false_positive: source-only contract tests can pass without exercising real gates; preserve one end-to-end fixed-point characterization per gate family.
- public_contract_churn: live track IDs, schema IDs, `_vN` artifacts, or output names can drift; Task 1 and final audit must compare these explicitly.
- browser_surface_drift: static web may 404, omit assets, or diverge between local and copied root; Task 10 uses real Chrome screenshots against both surfaces.
- dirty_worktree_or_local_index: `.codegraph` and unrelated local files must remain untouched; final status evidence must isolate planned files only.
