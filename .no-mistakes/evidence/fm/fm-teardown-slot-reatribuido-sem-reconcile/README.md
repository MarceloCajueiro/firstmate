# Evidence — fm/fm-teardown-slot-reatribuido-sem-reconcile

All live drives ran the real `bin/fm-teardown.sh` against real git worktrees and a
real `treehouse` pool leased inside a throwaway temp tree, with real `tmux` on an
isolated `TMUX_TMPDIR` (the live default tmux session was never touched). The
no-mistakes gate exemption `FM_GATE_REFUSE_BYPASS=1` was set, which is the
documented test-harness escape hatch for driving these entrypoints from a gate
worktree.

| File | What it shows |
| --- | --- |
| `live-reconcile-lifecycle.log` | End-to-end: both teardowns refuse before reconcile; `reconcile-reassigned-slot` detaches the stale record without touching the claim; stale teardown succeeds WITHOUT `--force` and spares the claimant's uncommitted work and lease; claimant teardown returns the slot to the pool. |
| `live-secondmate-preflight.log` | Round-2 fix: forced secondmate-parent teardown completes (rc=0) when a descendant was reconciled, while the pre-fix binary (e39a997) refuses the same case (rc=1). |
| `live-adversarial-refusals.log` | Fail-closed refusals with byte-identical records: nonterminal stale, live endpoint, ambiguous/unreachable claimant, claim still naming the stale record, partial detach marker, and a descendant marker whose path no longer matches its slot. |
| `targeted-suite-postfix.log` | Full `tests/fm-teardown-endpoint-safety.test.sh` on the change (all pass). |
| `targeted-suite-prefix-regression.log` | The same suite's new secondmate-preflight regression, run against the pre-fix binary: it fails with the reported refusal, proving the regression test is real. |
