# Explicit C4-Free Subgraphs of Hypercubes Q9 Through Q15

This repository contains explicit edge-list certificates for C4-free subgraphs
of hypercubes `Q9` through `Q15`.

For the Erdos Problems #86 notation `ex(Q_n, C4)`, the certificates establish
the following lower bounds:

| n | certified lower bound | certificate |
|---:|---:|---|
| 9 | 1505 | `q9_edges_repair_from1503_iter2.json` |
| 10 | 3304 | `q10_edges_repair_from3302_iter5.json` |
| 11 | 7156 | `q11_edges_repair_from7151_iter3.json` |
| 12 | 15372 | `q12_edges_repair_from15366_iter3.json` |
| 13 | 32856 | `q13_edges_repair_from32842_iter2.json` |
| 14 | 69909 | `q14_edges_repair_from69895_iter2.json` |
| 15 | 148126 | `q15_edges_repair_from148111_iter1.json` |

The claim is only that these are explicit certified lower bounds. No exactness
claim is made for `n >= 7`, and these finite-dimensional constructions do not
contradict the asymptotic conjecture.

## Verification

Run:

```bash
python verify_c4_free.py
```

The verifier checks that every listed edge is a valid hypercube edge and then
enumerates every 4-cycle in `Q_n`. A certificate passes only if no 4-cycle has
all four edges selected.

Expected verification summary:

```text
q9_edges_repair_from1503_iter2.json: n=9, edges=1505, cycles=4608, violations=0
q10_edges_repair_from3302_iter5.json: n=10, edges=3304, cycles=11520, violations=0
q11_edges_repair_from7151_iter3.json: n=11, edges=7156, cycles=28160, violations=0
q12_edges_repair_from15366_iter3.json: n=12, edges=15372, cycles=67584, violations=0
q13_edges_repair_from32842_iter2.json: n=13, edges=32856, cycles=159744, violations=0
q14_edges_repair_from69895_iter2.json: n=14, edges=69909, cycles=372736, violations=0
q15_edges_repair_from148111_iter1.json: n=15, edges=148126, cycles=860160, violations=0
```

An earlier local verification run is recorded in
`VERIFY_ALL_CERTIFICATES.log`.

## Contents

- `verify_c4_free.py` - standalone verifier.
- `q*_edges_*.json` - edge-list certificates.
- `PAPER_V2.md` - draft note explaining the construction and verification.
- `lift_params.json` - product-lift parameters used in the search stage.
- `SHA256SUMS` - raw file hashes for the certificates.
- `FORUM_POST_DRAFT_V3.md` - draft forum post for Erdos Problems.

## Disclosure

This was an AI-assisted computational discovery. The result should be judged by
the explicit edge-list certificates and the independent verifier, not by
authorship.
