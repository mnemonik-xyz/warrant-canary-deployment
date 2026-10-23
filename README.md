# warrant-canary-deployment

Operations for the public-chain canary of Warrant. This repository holds the driver scripts,
the read-only chain probes, the canary workload and the evidence of each run.
It holds no Warrant core code.

## Status

The files moved here from `policy-execution` without change. No history is kept.
**The scripts do not run from this repository yet.** They use paths of the
`policy-execution` layout: `ROOT/target/...`, `ROOT/contracts` and `ROOT/scripts/solver-agent.py`.
The first task is to give them one explicit setting for a `policy-execution` checkout or release.
`ArcSolverProbe.t.sol` stays in `policy-execution/contracts/test`. It imports the core contracts, and `check-arc-solver.py before` simulates it with `eth_call`.
The last `policy-execution` commit that held the original files is `e6863fe`.

## Contents

| Path | Purpose |
|---|---|
| `scripts/solver-canary.py` | Drives one solver bounty on a public chain, one step per command |
| `scripts/test_solver_canary.py` | Guard tests for the driver; no chain needed |
| `scripts/check-arc-solver.py` | Read-only probe of Arc mainnet, phases `before` and `after` |
| `scripts/check-arc.py` | Read-only probe of Arc testnet for task and invoice escrows |
| `scripts/test_check_arc_solver.py` | Tests for the solver probe; every external command is mocked |
| `scripts/solver-agent.py` | Heuristic seller agent that quotes and delivers |
| `scripts/test_instance_ionet.py` | Ties `instance-ionet.json` to its written record |
| `solver-bounty/instance-ionet.*` | Workload of the mainnet canary deal |
| `solver-bounty/ci-result.json` | Result of the CI proof run on a private Anvil chain |
| `solver-bounty/canary/` | Evidence of each run, its manifest schema and the rules for what must never be committed |
| `docs/` | Canary plan and kickoff notes |

## Rules that the driver enforces

- A step that spends is a dry run. It broadcasts only with `--execute`, `--confirm-chain` and `--confirm-amount`.
- The bounty cannot exceed 10 USDC.
- Keys stay in a Foundry keystore or a Ledger. The script never reads a private key.
- Mainnet needs four different signers.
- The script saves each transaction intent before it broadcasts. It never retries.

## Evidence

One mainnet run is recorded under `solver-bounty/canary/mainnet/20261016T202706Z`.
Read `solver-bounty/canary/README.md` before you add a run.
