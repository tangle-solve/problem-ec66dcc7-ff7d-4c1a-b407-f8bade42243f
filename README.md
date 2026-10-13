<!-- solve-problem:ec66dcc7-ff7d-4c1a-b407-f8bade42243f -->
# Derive APSP hardness from SETH

A [Tangle Solve problem](https://solve.tangle.tools/problems/ec66dcc7-ff7d-4c1a-b407-f8bade42243f).

## What is the problem?

The All-Pairs Shortest Paths (APSP) problem: given a complete directed graph on n vertices with arbitrary (possibly negative, but no negative cycles) integer edge weights, compute the n×n distance matrix, which is the closure of the weight matrix under the (min, +) semiring. A "truly subcubic" algorithm runs in O(n^{3−ε}) time for some ε > 0 (or O((n·M)^{1−ε}) when weights are bounded by M in absolute value). The APSP conjecture asserts no such algorithm exists for general weights.

SETH (Strong Exponential Time Hypothesis): for every ε > 0 there is a k such that k-SAT on n variables cannot be solved in O(2^{(1−ε)n}) time.

**Goal:** Prove that SETH implies the APSP conjecture — i.e., a truly subcubic APSP algorithm would contradict SETH — or prove that a specified class of reductions (e.g., the "combinatorial" / low-degree polynomial reductions used in the known framework of Vassilevska Williams–Williams) cannot establish this. The census phrasing allows two success modes: a reduction, or a formal barrier result (à la NSETH).

## Why does it matter?

Fine-grained complexity currently rests on three incomparable pillars: SETH, the APSP conjecture, and 3SUM. A SETH→APSP derivation would unify dozens of conditional lower bounds (dynamic shortest paths, diameter, betweenness centrality, constraint satisfaction in OR solvers, network routing), reducing them to a single belief about SAT. Conversely, a barrier result would tell the field that new reduction technology is needed. Both outcomes reshape how hardness evidence is calibrated across algorithms research.

## What's already known?

- APSP is subcubic-equivalent to a large cluster: negative triangle detection, min-plus matrix multiplication, radius/replacement paths, and dynamic single-source shortest paths under combinatorial reductions (Vassilevska Williams & Williams 2018 surveys and extends the 2010 "Subcubic equivalences" program).
- SETH implies tight n^{2−ε} hardness for Orthogonal Vectors (OV) (Williams 2005-style argument), and OV in turn yields hardness for diameter, similarity metrics, etc. — but none of these reach the 3-exponent, min-plus structure of APSP.
- Partial bridges exist: SETH implies hardness for k-Scan / partial variants and for min-plus-type problems with additive structure (e.g., parsing, regular expression matching — Backurs–Indyk–Schmidt line, verify details), but always in settings where the min-plus structure is "one-dimensional" or the exponent differs from 3.
- Carmosino et al. introduce nondeterministic hypotheses (NSETH) showing, roughly, that co-nondeterministic algorithms for a target problem block SETH-based reductions; co-nondeterministic APSP is subcubic-easy in some formulations, hinting at a barrier (verify the exact statement).
- What's missing: any known reduction family converts a 2^n-time SAT lower bound into an n^3-time min-plus lower bound; the parameter regimes (number of variables vs. graph size) never line up.

## What would count as solving it?

1. **Positive:** A many-one or Turing reduction with the property: an O(n^{3−ε})-time algorithm for general weighted APSP implies, for some δ > 0 and fixed k, an O(2^{(1−δ)n})-time algorithm for k-SAT. The reduction must preserve the "sufficiently wide SAT" clause: the resulting formula class must be over a fixed clause width k that does not shrink as n grows.
2. **Negative:** A theorem of the form "no reduction in class R (e.g., nonuniform, black-box, combinatorial / bounded-degree algebraic reductions) can derive APSP-hardness from SETH," ideally via a co-nondeterministic or counting algorithm (NSETH-style) that SETH reductions cannot rule out.
3. A partial result — e.g., SETH hardness for APSP with weights bounded by n^{f(k)} for some tradeoff, or for a natural subcubic-equivalent problem like negative triangle — would also be significant.

## How to start

1. Read Vassilevska Williams & Williams (2018) §"The APSP problem" and the equivalences with negative triangle / min-plus product; internalize why those equivalences are all within the min-plus world and never touch SAT.
2. Read Carmosino et al. (2016) to understand the NSETH barrier machinery; check whether a co-nondeterministic truly-subcubic algorithm for negative triangle is known — if yes, that immediately rules out standard SETH reductions for the triangle formulation and tells you where to look for workarounds.
3. Try to build the reduction from the other side: take a k-SAT instance on n variables and try to encode clause assignments into min-plus matrix products so that a negative-weight triangle corresponds to a falsified assignment. The core obstacle: you get n·2^{n/k}-ish sized objects, and the natural APSP instance has N ≈ 2^{n/k} vertices, so subcubic-in-N time is already far less than 2^n — the reduction is lossy in the wrong direction; you need the complement structure.
4. Study the OV reduction from SETH (Williams) as the template, and attempt "min-plus OV"-type gadgets.

## Key references

- V. Vassilevska Williams and R. Williams, "Subcubic Equivalences Between Path, Matrix, and Triangle Problems," JACM 65(5), 2018 (extended from FOCS 2010), DOI: 10.1145/3186893. Establishes the subcubic equivalence web around APSP (negative triangle, min-plus product, etc.) via combinatorial reductions.
- M. Carmosino, J. Gao, R. Imperioli, R. Mertz, J. Reif, V. Vassilevska Williams, "Nondeterministic and Exact-Complexity Analogues of P ≠ NP" (2016/2018, ECCC/ICALP — verify venue). Introduces NSETH and demonstrates that nondeterministic algorithms for target problems block certain SETH-based lower-bound proofs.
- R. Williams, "A new algorithm for optimal 2-constraint satisfaction and related problems" (SODA 2004 / JACM 2018, verify). Source of the SETH ⇒ OV-hardness reduction that is the standard template for SETH-based fine-grained lower bounds.
- A. Backurs, P. Indyk, L. Schmidt, "On the Fine-Grained Complexity of One-Dimensional Dynamic Programming" (SODA 2017, verify). Example of SETH-hardness proved for a min-plus-flavored problem, showing what partial bridges look like.

## Warnings

- Do not conflate "SETH implies no subcubic algorithm for APSP-with-small-weights" folklore claims — no such theorem is known; polynomially bounded weights are part of the open problem, not an assumption you may use.
- Watch the weight growth: reductions from SAT naturally blow up weights; a truly subcubic bound must be in terms of both n and weight magnitude M (or weights must be bounded), or the claim is ill-posed.
- The phrase "combinatorial reductions" in the equivalence literature refers to reductions avoiding Strassen-style fast matrix multiplication; be explicit about which reduction class your barrier theorem covers, since a barrier for combinatorial reductions says nothing about algebraic ones.
- A common dead end: trying to encode assignments as single vertices rather than as the product structure of the triangle/min-plus gadget — this invariably yields an APSP instance where subcubic time is *slower* than the brute-force SAT time, giving no contradiction.

---
*`ALGO-000083` · unverified*

## Acceptance guidance

1. **Positive:** A many-one or Turing reduction with the property: an O(n^{3−ε})-time algorithm for general weighted APSP implies, for some δ > 0 and fixed k, an O(2^{(1−δ)n})-time algorithm for k-SAT. The reduction must preserve the "sufficiently wide SAT" clause: the resulting formula class must be over a fixed clause width k that does not shrink as n grows.
2. **Negative:** A theorem of the form "no reduction in class R (e.g., nonuniform, black-box, combinatorial / bounded-degree algebraic reductions) can derive APSP-hardness from SETH," ideally via a co-nondeterministic or counting algorithm (NSETH-style) that SETH reductions cannot rule out.
3. A partial result — e.g., SETH hardness for APSP with weights bounded by n^{f(k)} for some tradeoff, or for a natural subcubic-equivalent problem like negative triangle — would also be significant.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/ec66dcc7-ff7d-4c1a-b407-f8bade42243f?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000083.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000083)
