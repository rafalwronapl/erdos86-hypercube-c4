# Forum Post Draft V3 - Erdos Problems #86

Target: https://www.erdosproblems.com/86
Fallback target: https://www.erdosproblems.com/forum/
Author: R. Wrona
Date: 2026-05-11
Format: markdown / LaTeX

---

## Checklist Before Posting

1. Set up a minimal public repository first.
   - Fastest: GitHub Gist with the seven JSON certificates, verifier, README, lift parameters, hashes.
   - Better: public GitHub repository with the same files.
   - Later archival option: Zenodo DOI after the draft is stable.
2. Replace `[REPO_URL]` in the post body with the real repository or gist URL.
3. Verify Minamo Minamoto's contact email from arXiv directly:
   - Open https://arxiv.org/abs/2603.29127
   - Click the "view email" link next to the author name.
   - Use that exact email if sending the optional heads-up note.
4. Confirm whether the problem page has comments enabled.
   - If yes, post on https://www.erdosproblems.com/86
   - If not, post a new thread on https://www.erdosproblems.com/forum/

---

## Suggested Title

Explicit C4-free edge-list certificates for Q9 through Q15

---

## Post Body

Following Minamo Minamoto's recent constructions (arXiv:2603.29127), which give
`ex(Q_7,C_4) >= 304` and `ex(Q_8,C_4) >= 680`, I found explicit certified
C4-free edge-list constructions for the next seven dimensions:

| n | lower bound for `ex(Q_n,C_4)` | density `|E|/(n 2^(n-1))` | 4-cycles checked |
|---:|---:|---:|---:|
| 9 | 1505 | 0.6532 | 4608 |
| 10 | 3304 | 0.6453 | 11520 |
| 11 | 7156 | 0.6353 | 28160 |
| 12 | 15372 | 0.6255 | 67584 |
| 13 | 32856 | 0.6170 | 159744 |
| 14 | 69909 | 0.6096 | 372736 |
| 15 | 148126 | 0.6028 | 860160 |

Each entry is witnessed by an explicit JSON edge-list certificate. I verified
each certificate by exhaustive enumeration of all 4-cycles in the corresponding
hypercube; the number of violating 4-cycles is zero in all seven cases.

The construction uses a two-stage computational pipeline. First, a product lift
from `Q_n` to `Q_{n+1}` places a certificate and an automorphic copy in the two
slices and adds cross edges on an independent set of the slice-intersection
graph. Second, a local ILP repair step forces selected absent edges and optimizes
on adaptive `C_4`-incidence neighborhoods. The repair step moves the construction
outside the product-lift family and was essential for the final values above.

These are lower bounds only. I make no exactness claims for `n >= 7`.

For context, compared with the Brass-Harborth-Nienborg general estimate
`0.5 * (n + 0.9 sqrt(n)) * 2^(n-1)`, the `Q9` and `Q10` certificates improve the
numerical lower bound by about `7.4` and `15.4` edges respectively. The
`Q11-Q15` certificates are valid explicit witnesses but do not improve that
general estimate.

The densities decrease over this range and are consistent with the conjecture
`ex(Q_n,C_4) = (1/2+o(1)) n 2^(n-1)`. The `n=15` density `0.6028` is below
Baber's asymptotic upper-bound constant `0.60318`, while `n=14` is still
slightly above it; this comparison is only contextual because that upper bound
is asymptotic.

SHA-256 hashes of the seven final certificate files:

```text
Q9   q9_edges_repair_from1503_iter2.json    0994be825ec39d115b65eb1436eace7c1be324448a6ec116d69c8a7e2a75d338
Q10  q10_edges_repair_from3302_iter5.json   24317cb0f821a7e39688d1376ac58c466779fc21d3628085270ddc468ae27c09
Q11  q11_edges_repair_from7151_iter3.json   efedbcbcc759765cecd4eb50bd7cbd9cb85b10b957ff84cfbfa92b91f4dc1d8d
Q12  q12_edges_repair_from15366_iter3.json  f1e952df4c40e10418f52b9c778fd8d1313efb1ab41bc8e45266d645a368a2f5
Q13  q13_edges_repair_from32842_iter2.json  f3214c05e66d45a3a300d2a96ee3f3a77982d091990ff5457f1852e98a389dd2
Q14  q14_edges_repair_from69895_iter2.json  78d7ca75e720f920637d72134c7749f9680f7c4c60d31ac67e8948a1fa0d7e32
Q15  q15_edges_repair_from148111_iter1.json 9c98dc35a87d2a2fde9ee115f628144d2851e1dbdf57096ad6e6003d97b8f575
```

Certificates, lift parameters, hashes, and a standalone verifier are available at:

`[REPO_URL]`

In addition to my verifier, the certificates were independently checked by
Minamo Minamoto using a separate sparse cherry-counting verifier. This verifier
checks the equivalent condition that no unordered vertex pair has two common
neighbours, rather than enumerating the hypercube's 4-cycles directly. All seven
certificates passed with matching SHA-256 hashes and the stated edge counts.
This is an independent certificate-level check; novelty against the full
published literature should still be checked separately.

A short note is in preparation. I would appreciate verification, error reports,
and pointers to any prior unpublished or computational certificate tables for
`ex(Q_n,C_4)` with `n >= 9`.

Contact: rafalwronapl@gmail.com

---

## Optional Heads-Up Email To Minamoto

Get the email via https://arxiv.org/abs/2603.29127 by clicking "view email" next
to the author name.

Subject: Extension of your Q7/Q8 C4-free table to Q9 through Q15

Dear Minamo Minamoto,

I followed your arXiv:2603.29127 while working on Erdos problem #86. Using a
two-stage pipeline - product lifts via automorphisms of the hypercube followed
by local ILP repair on C4-incidence neighborhoods - I obtained explicit
certified lower bounds

```text
ex(Q9,C4)  >= 1505,
ex(Q10,C4) >= 3304,
ex(Q11,C4) >= 7156,
ex(Q12,C4) >= 15372,
ex(Q13,C4) >= 32856,
ex(Q14,C4) >= 69909,
ex(Q15,C4) >= 148126.
```

All seven constructions are explicit edge-list certificates verified by
exhaustive enumeration of all 4-cycles in the corresponding hypercube. The
certificates and verifier are available at `[REPO_URL]`.

I am preparing a short note and plan to post a public announcement on the Erdos
Problems page. Since your work is the immediate predecessor for Q7 and Q8, I
wanted to send a short heads-up first. If you know of prior unpublished bounds
for any of these dimensions, or if you see an attribution issue in this framing,
I would be grateful for the correction.

Best regards,
R. Wrona
rafalwronapl@gmail.com

---

## Anticipated Responses

| likely comment | prepared response |
|---|---|
| "Do you claim exactness?" | No. These are explicit lower bounds only; no exactness claim is made for `n >= 7`. |
| "How was this verified?" | By exhaustive enumeration of all 4-cycles in each `Q_n`; cite the standalone verifier and `VERIFY_ALL_CERTIFICATES.log`. |
| "Why not Q16?" | The next case is natural, but the local ILP neighborhoods and search time grow quickly. |
| "Why not use SAT/PB to close Q7?" | Feasible in principle, but outside the scope of this note; left as an open problem. |
| "Why was an intermediate certificate sometimes used as the next lift parent?" | Empirically, the largest repaired certificate was not always the best parent for the next automorphism search; this is documented in the note and is not a theorem. |
| "Do you compare against projective-plane-based or algebraic bounds?" | This post reports explicit computational certificates. I am not aware of a published explicit certificate table for `n >= 9`; pointers to algebraic or computational bounds are welcome. |

---

## What Not To Post

- No exactness claim for any `n >= 7`.
- No claim of challenging the asymptotic conjecture.
- No conditional upper bounds presented as theorems.
- No claim that the construction is optimal.
- No long methodology; keep details for the note and repository.

---

## Minimum Contents Of REPO_URL

```text
README.md
verify_c4_free.py
q9_edges_repair_from1503_iter2.json
q10_edges_repair_from3302_iter5.json
q11_edges_repair_from7151_iter3.json
q12_edges_repair_from15366_iter3.json
q13_edges_repair_from32842_iter2.json
q14_edges_repair_from69895_iter2.json
q15_edges_repair_from148111_iter1.json
lift_params.json
SHA256SUMS
VERIFY_ALL_CERTIFICATES.log
LICENSE
```

Use raw copies of the JSON files if you want the SHA-256 hashes above to remain
valid. Re-serializing or reformatting JSON can change the byte-level hashes even
when the edge lists are mathematically identical.

---

## Timing

1. Day 0: create the public gist/repository and fill in `[REPO_URL]`.
2. Day 0: optionally send the heads-up email to Minamoto.
3. Day 0: post on the Erdos Problems page/forum.
4. Day 1-7: monitor responses and adjust the paper if needed.
5. Day 7-10: prepare arXiv submission.
