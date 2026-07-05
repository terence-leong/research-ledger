# Failed Calls / Broken Theses

> Losses, defects, and misses — logged with the same rigor as wins. A profile that only shows elegant systems and successful examples is curated, not credible. Every entry here is real; entries are added as they occur and as hash-committed ledger calls resolve. The ledger's no-survivorship rule means future failures cannot be quietly omitted: every committed hash gets revealed, wins and losses both.

## Failure taxonomy

Every entry carries exactly one primary class:

| Class | Meaning |
|---|---|
| `CALL_WRONG` | Thesis invalidated; the research conclusion was incorrect |
| `EXECUTION_FAILURE` | Research was right; the position was not taken or not managed per plan |
| `DATA_PACK_FAILURE` | Required inputs missing/unparsed; run proceeded or degraded |
| `ENGINE_DEFECT` | Scoring/sizing/gating logic produced a wrong or unsafe intermediate |
| `SIZING_DEFECT` | Position size inconsistent with data completeness or risk rules |

## Post-mortem schema (every entry)

```
ID / DATE / REF (ledger call or run, if public)
THESIS (one line)
WHAT HAPPENED (receipts or cross-reference)
CLASS (from taxonomy)
ROOT CAUSE (engine vs execution vs data)
IMPACT
FIX SHIPPED (public artifact reference where applicable)
STATUS (open / fixed / monitoring)
```

---

## FC-001 — The founding failure: right call, zero position

- **DATE:** call 2025-12-19; window resolved Q1 2026
- **REF:** [CS-001](CS-001_index_inclusion.md) — resolution receipts there
- **THESIS:** Two index-inclusion candidates ranked #1 and #2 for the Q1-2026 S&P 500 window.
- **WHAT HAPPENED:** Both resolved as included within the predicted window (announcement receipts in CS-001). **No position was entered in either.** The research engine produced the answer; no execution protocol existed to act on it.
- **CLASS:** `EXECUTION_FAILURE`
- **ROOT CAUSE:** Execution, not engine. No confirmation workflow, no designated executor, no confidence threshold tied to action.
- **IMPACT:** Fully-documented thesis, correctly resolved, outcome not captured.
- **FIX SHIPPED:** A written execution protocol — two-analyst cross-confirmation on every call, a designated order executor, entry/exit prices recorded at decision time, weekly review cadence — and this ledger's commit-reveal mechanism, which forces every call to carry an auditable timestamp and an eventual outcome.
- **STATUS:** Fixed; monitored via ledger cadence.

---

## Engine defect log (found in live production runs; tickers withheld)

Defects below were surfaced by the engine's own self-audit layer during live runs in 2026 and are reproduced generically in the public demo artifacts ([narrative report §10–13](https://github.com/terence-leong/equity-pipeline-demo/blob/main/demo/DEMO-A_report.md)).

### ED-001 — Provisional sizing computed before the data-completeness gate

- **WHAT HAPPENED:** A live run drafted a high-conviction provisional position size while core technical arrays were missing. A downstream valuation cap compressed the size to a fraction of the draft, so no unsafe size shipped — but an uninformed size existed at all.
- **CLASS:** `SIZING_DEFECT`
- **ROOT CAUSE:** Order-of-operations: sizing ran ahead of the missing-driver check; the cap caught it late.
- **FIX SHIPPED:** Missing-driver abort — any MISSING core driver caps provisional size at the starter tier *before* any cap audit runs (tier values private). Demonstrated in the runnable toy: the tape gate cannot clear on PARTIAL data ([test suite](https://github.com/terence-leong/equity-pipeline-demo/blob/main/demo/test_demo_pipeline.py)).
- **STATUS:** Fixed.

### ED-002 — Composite total printed clean while a module ran dark

- **WHAT HAPPENED:** A live run printed a confident total score while one scoring module was FLAGGED for missing inputs; nothing at the total level disclosed that a module was dark.
- **CLASS:** `ENGINE_DEFECT`
- **ROOT CAUSE:** No quality companion on the composite; module-level honesty didn't propagate upward.
- **FIX SHIPPED:** `TOTAL_QUALITY` companion tag — every total prints with FULL or PARTIAL(n modules dark); non-total layers (momentum, reward/risk) print authority labels based on input completeness (criteria private).
- **STATUS:** Fixed in spec; rollout monitored.

### ED-003 — Dilution materiality anchored to the wrong denominator

- **WHAT HAPPENED:** A live run labeled stock-based compensation a mild factor because materiality was anchored to market capitalization; anchored to trailing net income, the same SBC represented a majority of actual profits — a structurally different picture the score understated.
- **CLASS:** `ENGINE_DEFECT`
- **ROOT CAUSE:** Denominator choice hid income-relative equity leakage on richly-valued names.
- **FIX SHIPPED:** Materiality re-anchored to trailing net income, with a mandatory financing-score penalty above a burn threshold (threshold and penalty private).
- **STATUS:** Fixed in spec.

---

## Guards that fired (the counterpart record)

- **GF-001:** In the ED-001 run, the valuation cap independently compressed an oversized provisional layout before anything shipped — the layered design catching a defect upstream of it. Logged because a guard working is only credible next to the defect it caught.

---

## Intake rule

New entries are added (a) whenever a hash-committed ledger call resolves against the thesis, (b) whenever a run's self-audit surfaces a defect, or (c) whenever execution deviates from plan. Entries may redact tickers and private parameters; they may not redact the failure.

## Disclaimer

Educational research-process documentation. Nothing here is investment advice, a performance record, or a solicitation.
