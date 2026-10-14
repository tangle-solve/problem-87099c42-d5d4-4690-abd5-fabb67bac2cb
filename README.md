<!-- solve-problem:87099c42-d5d4-4690-abd5-fabb67bac2cb -->
# Constant approximation for the smallest grammar

A [Tangle Solve problem](https://solve.tangle.tools/problems/87099c42-d5d4-4690-abd5-fabb67bac2cb).

## What is the problem?

Given an input string $S$ of length $n$ over an alphabet $\Sigma$, find a **straight-line program (SLP)** — a context-free grammar with exactly one derivation per nonterminal that produces exactly $S$ — minimizing the grammar size (number of rules, or total symbols across rules; conventions differ by a constant factor). The decision version "is there a grammar of size $\le k$?" is NP-hard. The open problem: **does there exist a polynomial-time algorithm producing an SLP of size at most $C \cdot \mathrm{OPT}(S)$ for a universal constant $C$?** Currently, only $O(\log(n/g^*))$-factor approximations are known, where $g^*$ is the optimum grammar size — i.e., the guarantee degrades with the compressibility of $S$. A constant-factor approximation (independent of $n$ and $g^*$) is open in both directions: no lower bound beyond APX-hardness-type obstacles is known to rule it out (NP-hardness of constant approximation is itself open — verify current status).

## Why does it matter?

The smallest grammar problem is the theoretical core of dictionary-based compression (LZ78, LZ77, RePair, and modern tools like straight-line compressed indexes). A constant-factor approximation would give compression with provable worst-case quality relative to the best possible grammar-based representation, affecting compressed indexes, compressed pattern matching, and deduplication systems. It is also a clean test case for whether "local repetitive structure" algorithms can compete with information-theoretic-style lower bounds; the gap between known algorithms and the best lower bounds (via additions chains, LZ77 window size) is the central mystery.

## What's already known?

- **Charikar et al. (2005)**: established the framework of approximating the smallest grammar via the **addition chain** problem; showed the best possible ratio for algorithms from LZ77/LZ78-based constructions is $\Omega(\log n / \log\log n)$ (verify exact form), and gave the best known approximation ratio $O(\log(n/g^*))$ via LZ77 parsing (the "LZ77 with default pairing" / binary-tree grammar construction). Also proved NP-hardness.
- **Rytter (2003)**, **Sakamoto (2005)**, and others: alternative algorithms achieving $O(\log n)$-type ratios using different grammar construction strategies (Bisection, RePair-like, LZ78-based), all matching or close to the Charikar et al. ratio.
- **Bannai et al. (2022, IEEE TIT)**: revisited lower bounds, sharpening the addition-chain/LZ77-based lower-bound framework and clarifying which grammar models (with/without deletion, different size measures) admit which bounds; showed the lower-bound technique itself has limitations in proving strong inapproximability.
- **Jeż (2013/2016)**: showed that recompression-based algorithms (for compressed membership and equality testing) achieve the same $O(\log n)$ approximation ratio for SLPs — a different algorithmic angle that some believe could be pushed further.
- The best known upper bound is $O(\log(n/g^*))$ (Charikar et al. 2005); the best known lower bound for polynomial-time approximation is essentially "no better than constant-ish unless P=NP" is **not** established — there is a large open gap.

## What would count as solving it?

Solving it would be any of:
1. **Positive (upper bound)**: a polynomial-time algorithm and proof that for every $S$, output size $\le C \cdot \mathrm{OPT}(S)$ for an absolute constant $C$.
2. **Negative (lower bound)**: proof that no polynomial-time algorithm can achieve ratio $o(\log(n/g^*))$ (or even that ratio $C$ is impossible for some fixed $C$), assuming P≠NP or a stronger assumption.
3. **Partial progress**: e.g., a constant-factor approximation for restricted string families (e.g., highly repetitive, bounded alphabet), or an improved ratio beating $O(\log(n/g^*))$ unconditionally.

The referee criterion: a rigorous bound valid for all input strings, stated in the standard SLP model (grammar size = total symbols across all rules, or equivalently number of rules — either convention acceptable if stated).

## How to start

1. Read **Charikar, Lehman, Liu, Panigrahy, Prabhakaran, Sahai, Shelat (2005)** Sections on the addition-chain lower-bound machinery — understand why every known algorithm's ratio ties to how "deep" the LZ77 parse is, and why this yields $\Omega(\log n)$ lower bounds *for specific algorithm classes* but not for the problem itself.
2. Read **Bannai et al. (2022)** to see the current best lower-bound technique and its barriers — this tells you exactly which proof approaches are known dead ends.
3. Try the **recompression** angle (Jeż): can an algorithm that repairs pairs/blocks iteratively be analyzed against OPT directly, rather than against the LZ77 parse?
4. Alternatively, attempt a PCP-style or addition-chain-based hardness reduction — the obstacle is that grammar size lower bounds for *explicit* strings are very hard; even proving a specific family requires $\omega(g^*)$-size grammars is hard.

## Key references

- Charikar, M., Lehman, E., Liu, D., Panigrahy, R., Prabhakaran, M., Sahai, A., Shelat, A. "The Smallest Grammar Problem." *IEEE Transactions on Information Theory* 51(7), 2005. Establishes NP-hardness, addition-chain connection, the $O(\log(n/g^*))$ approximation, and lower bounds for algorithm classes.
- Bannai, H., Gagie, T., I, T., et al. "The Smallest Grammar Problem Revisited." *IEEE Transactions on Information Theory*, 2022. DOI: 10.1109/TIT.2020.3038147. Re-derives and sharpens lower-bound techniques for grammar size, clarifying the gap between upper and lower bounds.
- Rytter, W. "Application of Lempel-Ziv factorization to the approximation of grammar-based compression." *Theoretical Computer Science* 302(1–3), 2003. Gives an $O(\log n)$-ratio SLP construction from LZ77 parsing.
- Jeż, A. "Approximation of grammar-based compression via recompression." *Theoretical Computer Science* 592, 2015. Achieves $O(\log n)$ approximation via a different recompression technique.

## Warnings

- Do not confuse "grammar size = number of rules" vs. "total symbols in rules" — they differ by a factor of 2, harmless for constants but must be stated.
- Lower bounds for *algorithms* (e.g., LZ-based constructions can't beat $\Omega(\log n)$) are not hardness of the problem — this is the most common source of false "solutions."
- The addition-chain lower bound machinery of Charikar et al. has known limitations (per Bannai et al.) — don't try to strengthen it naively without checking what they already ruled out.
- Empirical success of RePair or similar tools is not a proof; any claimed constant must hold against worst-case OPT, which is itself hard to estimate empirically.

---
*`ALGO-000169` · unverified*

## Acceptance guidance

Solving it would be any of:
1. **Positive (upper bound)**: a polynomial-time algorithm and proof that for every $S$, output size $\le C \cdot \mathrm{OPT}(S)$ for an absolute constant $C$.
2. **Negative (lower bound)**: proof that no polynomial-time algorithm can achieve ratio $o(\log(n/g^*))$ (or even that ratio $C$ is impossible for some fixed $C$), assuming P≠NP or a stronger assumption.
3. **Partial progress**: e.g., a constant-factor approximation for restricted string families (e.g., highly repetitive, bounded alphabet), or an improved ratio beating $O(\log(n/g^*))$ unconditionally.

The referee criterion: a rigorous bound valid for all input strings, stated in the standard SLP model (grammar size = total symbols across all rules, or equivalently number of rules — either convention acceptable if stated).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/87099c42-d5d4-4690-abd5-fabb67bac2cb?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000169.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000169)
