# Ballot Front Finder Lab — comparison across runs

Older inputs:

- 35 file(s) lack columns added later: campaign read as 0.0, risk read as 0, risk_tolerance read as 0.1, coordination read as 0.25, individual read as 0, cands_vote read as 0, honesty read as none, response read as arithmetic: aggressive-kind/01-aggressive=0.0, aggressive-kind/02-aggressive=1.0, base, base-rest …

**Runs of different code, compared anyway (--force-versions):** 66 method(s) give different results in different runs (321, ARA, ARA-Top, Approval, Approval cond. runoff, Approval runoff, BTR-Borda, BTR-IRV …).

**Mixed simulator versions, compared anyway (--force-versions): 1.0.0, 2.1.0.** Results of different versions may not be comparable; see the list of inputs below.

Folder: ../v12combined. 44 run or sweep folder(s), 78 results file(s); simulator version 1.0.0, 2.1.0; analysis version 2.1.0. 81 distinct condition(s); 71 methods. Bootstrap: 1000 paired replicates per condition.

How to read this:

- **Every condition counts once**, whatever the number of runs or elections behind it. Averages and shares are over the conditions listed below, and only over those a method was run in (column *conditions*); they describe this collection of runs, not all possible electorates.
- **Clearly better** and **dominates** are as in analyze.py's reports: the paired 95% interval of the difference within the condition excludes 0 (sincerity: and the difference is at least 1 point); dominance on VSE\*, WME\* and sincerity. **Near the front (1 / 2)**: no method in the condition is better by 1 (2) points on VSE\* and WME\* at once (point estimates); the guide lines of the arrow plots, drawn with analyze.py --overlay guides.
- Pair counts are over the conditions both methods were run in.

## Conditions

| Condition | Differs from the defaults | Elections | Methods | Inputs |
|---|---|---|---|---|
| c1 | defaults | 4000 | 71 | base, base-rest, defaults |
| c2 | response to a move arithmetic | 2000 | 38 | campaigns |
| c3 | campaigns 1.0, response to a move arithmetic | 2000 | 38 | campaigns |
| c4 | strategic share 1.0, adaptive share 0.0, response to a move arithmetic | 2000 | 38 | campaigns |
| c5 | strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic | 2000 | 38 | campaigns |
| c6 | candidate dispersion 0.5 | 2000 | 38 | cand-dispersion |
| c7 | candidate dispersion 2.0 | 2000 | 38 | cand-dispersion |
| c8 | candidate shift 0.5 | 2000 | 38 | cand-shift |
| c9 | candidate shift 1.0 | 2000 | 38 | cand-shift |
| c10 | candidates 4 | 2000 | 38 | candidates |
| c11 | candidates 8 | 2000 | 38 | candidates |
| c12 | candidates 10 | 2000 | 38 | candidates |
| c13 | coordination share 0.0 | 2000 | 38 | coordination |
| c14 | coordination share 0.1 | 2000 | 38 | coordination |
| c15 | coordination share 0.5 | 2000 | 38 | coordination |
| c16 | search 1, individual voters 0 | 2000 | 38 | groups |
| c17 | strategic share 1.0, adaptive share 0.0, search 1, individual voters 0 | 2000 | 38 | groups |
| c18 | candidate dispersion 0.5, laziness 0.25, poll error 4.0 | 2000 | 38 | hard |
| c19 | honesty cost 0.1 | 2000 | 38 | honesty |
| c20 | honesty cost 1.0 | 2000 | 38 | honesty |
| c21 | laziness 0.25 | 2000 | 38 | laziness |
| c22 | laziness 0.5 | 2000 | 38 | laziness |
| c23 | first poll previous | 2000 | 38 | lockin |
| c24 | max score 99 | 2000 | 29 | max-score |
| c25 | electorate model clusters | 2000 | 38 | models |
| c26 | electorate model field | 2000 | 38 | models |
| c27 | electorate model ks | 2000 | 38 | models |
| c28 | exits 0, clones 0 | 2000 | 38 | no-nomination |
| c29 | poll error 1.0 | 2000 | 38 | poll-error |
| c30 | poll error 4.0 | 2000 | 38 | poll-error |
| c31 | poll rounds 1 | 2000 | 38 | rounds |
| c32 | poll rounds 2 | 2000 | 38 | rounds |
| c33 | poll rounds 4 | 2000 | 38 | rounds |
| c34 | poll rounds 5 | 2000 | 38 | rounds |
| c35 | samples 10 | 2000 | 38 | samples |
| c36 | samples 20 | 2000 | 38 | samples |
| c37 | strategic share 0.5, adaptive share 0.0 | 2000 | 38 | strategic-share |
| c38 | strategic share 1.0, adaptive share 0.0 | 2000 | 38 | strategic-share |
| c39 | utility linear | 2000 | 38 | utility |
| c40 | utility quadratic | 2000 | 38 | utility |
| c41 | voters 199 | 2000 | 38 | voters |
| c42 | voters 40, electorate model ks-quinn, utility linear, poll rounds 1, strategic share 0.5, adaptive share 0.0, max score 10, exits 0, clones 0, candidates vote 0 | 15000 | 11 | vsesim |
| c43 | voters 40, electorate model ks-quinn, utility linear, poll rounds 1, strategic share 1.0, adaptive share 0.0, max score 10, exits 0, clones 0, candidates vote 0 | 15000 | 11 | vsesim |
| c44 | width 1.4 | 2000 | 38 | width |
| c45 | samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | aggressive-kind |
| c46 | samples 10, aggressive share 1.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | aggressive-kind |
| c47 | samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 2000 | 64 | base, base-rest, defaults |
| c48 | candidate dispersion 0.5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | cand-dispersion |
| c49 | candidate dispersion 2.0, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | cand-dispersion |
| c50 | candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 700 | 34 | candidates |
| c51 | candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 700 | 34 | candidates |
| c52 | candidates 7, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 700 | 34 | candidates |
| c53 | candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 700 | 34 | candidates |
| c54 | candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 700 | 34 | candidates |
| c55 | candidates 10, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 700 | 34 | candidates |
| c56 | candidate dispersion 0.5, samples 10, laziness 0.1, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | hard |
| c57 | samples 10, laziness 0.1, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | laziness |
| c58 | samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | laziness |
| c59 | samples 10, exits 0, clones 0, search 10, first poll previous, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | lockin |
| c60 | samples 10, max score 9, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 2000 | 27 | max-score |
| c61 | samples 10, max score 99, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 2000 | 27 | max-score |
| c62 | electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | models |
| c63 | electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | models |
| c64 | electorate model ks, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | models |
| c65 | samples 10, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | nomination |
| c66 | exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | poll-size |
| c67 | poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | rounds |
| c68 | poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | rounds |
| c69 | poll rounds 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | rounds |
| c70 | poll rounds 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | rounds |
| c71 | poll rounds 6, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | rounds |
| c72 | exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | samples |
| c73 | samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | strategic-share |
| c74 | samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | strategic-share |
| c75 | utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | utility |
| c76 | utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | utility |
| c77 | voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | voters |
| c78 | voters 40, electorate model ks-quinn, utility linear, poll rounds 1, samples 10, strategic share 0.5, adaptive share 0.0, max score 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 15000 | 11 | vsesim |
| c79 | voters 40, electorate model ks-quinn, utility linear, poll rounds 1, samples 10, strategic share 1.0, adaptive share 0.0, max score 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 15000 | 11 | vsesim |
| c80 | width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | width |
| c81 | width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic | 1000 | 34 | width |

## Methods

Methods run in at least 41 of the 81 conditions, sorted by how often undominated, then near the front. Means are weighted by condition; worst: the lowest value over the conditions, with the condition.

| Method | Group | Conditions | Undominated | Pareto front | Near front (1 / 2) | Mean VSE\* | Mean WME\* | Mean sincerity | Worst VSE\* | Worst WME\* |
|---|---|---|---|---|---|---|---|---|---|---|
| Schulze | condorcet | 78 | 71% | 45% | 97% / 97% | 85.2 | 98.6 | 98.7 | 54.4 (c7) | 91.1 (c4) |
| Ranked pairs | condorcet | 78 | 69% | 54% | 95% / 97% | 85.1 | 98.6 | 98.7 | 54.0 (c7) | 90.8 (c4) |
| Kemeny-Young | condorcet | 74 | 69% | 39% | 95% / 97% | 84.4 | 98.7 | 98.8 | 54.0 (c7) | 89.8 (c5) |
| Soft Minimax-D | hybrid | 77 | 62% | 77% | 99% / 100% | 86.9 | 96.6 | 100.0 | 55.5 (c7) | 91.6 (c48) |
| Minimax | minimax-family | 74 | 62% | 32% | 96% / 97% | 84.5 | 98.7 | 98.8 | 54.3 (c7) | 90.8 (c4) |
| MMWV | minimax-family | 74 | 62% | 28% | 95% / 97% | 84.4 | 98.6 | 99.0 | 54.3 (c7) | 89.2 (c4) |
| Score runoff | two-round | 77 | 51% | 74% | 100% / 100% | 86.7 | 96.5 | 99.9 | 59.8 (c7) | 91.7 (c56) |
| BTR-score | hybrid | 77 | 48% | 45% | 96% / 97% | 86.4 | 96.7 | 99.9 | 55.0 (c7) | 91.1 (c48) |
| MMPO | minimax-family | 74 | 47% | 27% | 93% / 97% | 84.3 | 98.5 | 99.0 | 54.2 (c7) | 88.5 (c4) |
| Soft Minimax | hybrid | 77 | 40% | 77% | 99% / 100% | 86.4 | 97.0 | 100.0 | 54.9 (c7) | 91.7 (c48) |
| Smith//Score | condorcet-based | 77 | 34% | 29% | 99% / 100% | 86.6 | 96.6 | 100.0 | 55.0 (c7) | 91.1 (c48) |
| Copeland | condorcet | 74 | 30% | 24% | 85% / 97% | 84.4 | 98.3 | 98.7 | 53.4 (c7) | 92.9 (c56) |
| Soft Minimax-40 | hybrid | 77 | 22% | 68% | 97% / 100% | 87.6 | 95.4 | 100.0 | 55.6 (c7) | 89.4 (c48) |
| ARA | hybrid | 77 | 21% | 48% | 97% / 99% | 87.7 | 95.0 | 100.0 | 56.8 (c7) | 88.3 (c48) |
| MARS | hybrid | 77 | 18% | 44% | 94% / 99% | 87.1 | 95.8 | 100.0 | 57.4 (c7) | 89.1 (c48) |
| MARS-T | hybrid | 77 | 16% | 40% | 92% / 97% | 87.2 | 95.6 | 100.0 | 57.4 (c7) | 89.1 (c48) |
| STAR | hybrid | 81 | 10% | 32% | 93% / 96% | 87.6 | 94.9 | 99.9 | 59.6 (c7) | 81.0 (c43) |
| Approval runoff | two-round | 74 | 4% | 7% | 58% / 93% | 86.3 | 93.6 | 99.7 | 53.1 (c18) | 79.8 (c6) |
| DMS | rated | 77 | 4% | 29% | 71% / 90% | 87.6 | 91.0 | 100.0 | 57.6 (c18) | 77.7 (c48) |
| ESR-MARS | hybrid | 42 | 2% | 26% | 86% / 93% | 85.9 | 94.6 | 100.0 | 57.9 (c7) | 88.9 (c38) |
| BTR-IRV | condorcet-based | 74 | 1% | 12% | 80% / 95% | 84.3 | 97.9 | 98.6 | 54.0 (c7) | 91.0 (c18) |
| ESR | rated | 77 | 1% | 19% | 82% / 96% | 87.4 | 93.9 | 100.0 | 56.6 (c7) | 84.4 (c48) |
| MJ | median-based | 81 | 1% | 2% | 20% / 79% | 87.1 | 91.5 | 100.0 | 55.2 (c7) | 77.4 (c48) |
| IMDS | rated | 42 | 0% | 7% | 90% / 100% | 84.9 | 96.9 | 100.0 | 54.6 (c7) | 92.6 (c18) |
| Delegated 321 | delegated | 41 | 0% | 0% | 76% / 98% | 85.9 | 92.8 | nan | 56.8 (c7) | 86.3 (c18) |
| Score | rated | 81 | 0% | 16% | 64% / 93% | 87.7 | 91.6 | 100.0 | 59.1 (c7) | 76.4 (c43) |
| 321 | hybrid | 80 | 0% | 2% | 59% / 91% | 87.5 | 91.1 | 99.9 | 56.8 (c7) | 74.0 (c43) |
| MAV | median-based | 81 | 0% | 4% | 47% / 80% | 87.3 | 90.7 | 100.0 | 55.2 (c7) | 73.7 (c48) |
| Approval cond. runoff | two-round | 74 | 0% | 0% | 28% / 77% | 86.0 | 92.4 | 99.8 | 52.7 (c18) | 79.3 (c18) |
| Borda | positional | 78 | 0% | 3% | 9% / 29% | 83.8 | 91.3 | 98.0 | 54.7 (c7) | 64.9 (c43) |
| Approval | approval-based | 78 | 0% | 1% | 6% / 38% | 85.7 | 87.4 | 99.9 | 43.3 (c18) | 65.6 (c48) |
| ER-IRV | plurality-based | 74 | 0% | 1% | 5% / 36% | 84.9 | 92.3 | 98.7 | 54.6 (c7) | 74.0 (c48) |
| TTR | two-round | 74 | 0% | 0% | 4% / 14% | 83.6 | 90.9 | 97.3 | 52.9 (c18) | 70.6 (c48) |
| Plurality | plurality-based | 78 | 0% | 1% | 3% / 3% | 75.1 | 73.5 | 98.4 | 18.2 (c6) | 40.5 (c18) |
| Asset | delegated | 41 | 0% | 2% | 2% / 7% | 80.5 | 84.0 | 98.0 | 52.1 (c7) | 59.2 (c18) |
| IRV-3 | plurality-based | 74 | 0% | 0% | 1% / 7% | 83.0 | 88.7 | 98.6 | 50.7 (c18) | 69.4 (c6) |
| IRV | plurality-based | 78 | 0% | 0% | 0% / 8% | 83.3 | 89.3 | 97.9 | 52.2 (c6) | 64.9 (c43) |
| Supplementary vote | plurality-based | 74 | 0% | 0% | 0% / 1% | 82.3 | 84.3 | 98.4 | 40.3 (c18) | 58.7 (c18) |

**Run in fewer conditions** (shares over few conditions say little; not in the pair matrix):

| Method | Group | Conditions | Undominated | Pareto front | Near front (1 / 2) | Mean VSE\* | Mean WME\* | Mean sincerity | Worst VSE\* | Worst WME\* |
|---|---|---|---|---|---|---|---|---|---|---|
| Schulze-ER | condorcet | 2 | 100% | 0% | 100% / 100% | 83.7 | 99.2 | 99.1 | 83.1 (c47) | 99.1 (c47) |
| Minimax-ER | minimax-family | 2 | 100% | 0% | 100% / 100% | 83.7 | 99.2 | 99.1 | 83.0 (c47) | 99.2 (c47) |
| MMWVxScore | hybrid | 5 | 60% | 60% | 100% / 100% | 85.5 | 98.0 | 100.0 | 83.8 (c61) | 96.0 (c47) |
| Smith//Approval | condorcet-based | 2 | 50% | 0% | 100% / 100% | 85.6 | 98.5 | 100.0 | 84.9 (c24) | 97.6 (c1) |
| Copeland//Margins | condorcet | 2 | 50% | 0% | 100% / 100% | 83.8 | 99.1 | 98.8 | 83.3 (c47) | 99.1 (c1) |
| Nanson | condorcet-based | 2 | 50% | 0% | 100% / 100% | 83.6 | 99.2 | 98.9 | 83.1 (c47) | 99.2 (c1) |
| MMWV+Score | hybrid | 5 | 40% | 80% | 100% / 100% | 85.4 | 98.1 | 100.0 | 83.7 (c61) | 96.3 (c47) |
| IC-MARS | hybrid | 5 | 20% | 20% | 100% / 100% | 87.0 | 96.3 | 100.0 | 86.2 (c61) | 94.4 (c47) |
| Soft Minimax-L | hybrid | 5 | 0% | 60% | 100% / 100% | 88.4 | 94.8 | 100.0 | 87.7 (c1) | 93.9 (c60) |
| ARA-C | hybrid | 2 | 0% | 0% | 100% / 100% | 86.3 | 96.6 | 100.0 | 86.0 (c24) | 95.6 (c1) |
| INS | rated | 5 | 0% | 0% | 100% / 100% | 85.3 | 97.3 | 99.9 | 83.8 (c61) | 95.7 (c47) |
| Black | condorcet-based | 2 | 0% | 50% | 100% / 100% | 83.9 | 98.8 | 98.8 | 83.2 (c47) | 98.7 (c47) |
| SICT | condorcet | 2 | 0% | 0% | 100% / 100% | 83.7 | 97.9 | 99.0 | 83.6 (c47) | 97.4 (c47) |
| MMPOWV | minimax-family | 2 | 0% | 50% | 100% / 100% | 83.7 | 99.1 | 99.1 | 82.9 (c47) | 99.0 (c47) |
| Benham | condorcet-based | 2 | 0% | 0% | 100% / 100% | 83.7 | 98.2 | 98.7 | 83.3 (c47) | 98.0 (c47) |
| Baldwin-ER | condorcet-based | 2 | 0% | 0% | 100% / 100% | 83.5 | 98.9 | 99.1 | 83.2 (c47) | 98.9 (c1) |
| Copeland//Margins-ER | condorcet | 2 | 0% | 0% | 100% / 100% | 83.5 | 98.8 | 99.0 | 82.7 (c47) | 98.5 (c47) |
| BTR-Borda | condorcet-based | 2 | 0% | 0% | 100% / 100% | 83.5 | 98.9 | 98.8 | 82.9 (c47) | 98.8 (c47) |
| Baldwin | condorcet-based | 2 | 0% | 0% | 100% / 100% | 83.5 | 98.9 | 98.9 | 83.1 (c47) | 98.8 (c1) |
| Score+ | hybrid | 5 | 0% | 0% | 80% / 100% | 86.1 | 96.2 | 99.9 | 85.2 (c61) | 95.1 (c47) |
| Coombs | plurality-based | 2 | 0% | 0% | 50% / 100% | 82.9 | 97.2 | 98.6 | 82.8 (c47) | 96.5 (c1) |
| IDS | rated | 5 | 0% | 20% | 40% / 100% | 87.8 | 92.1 | 100.0 | 86.4 (c1) | 90.9 (c47) |
| ARA-Top | hybrid | 5 | 0% | 0% | 20% / 80% | 86.5 | 94.0 | 100.0 | 85.0 (c24) | 92.4 (c47) |
| STLR | hybrid | 5 | 0% | 0% | 20% / 80% | 85.4 | 95.0 | 99.9 | 83.8 (c61) | 94.0 (c24) |
| Reciprocal score | rated | 5 | 0% | 0% | 0% / 60% | 87.1 | 90.6 | 99.7 | 85.3 (c24) | 88.7 (c61) |
| MDDA | approval-based | 2 | 0% | 0% | 0% / 50% | 86.8 | 88.6 | 99.9 | 86.2 (c1) | 86.6 (c47) |
| MAMPO | approval-based | 2 | 0% | 0% | 0% / 50% | 86.5 | 88.7 | 99.9 | 86.0 (c1) | 86.8 (c47) |
| Dowdall | positional | 2 | 0% | 0% | 0% / 50% | 86.1 | 89.6 | 98.7 | 84.3 (c1) | 89.3 (c47) |
| DSC | plurality-based | 2 | 0% | 0% | 0% / 0% | 86.0 | 88.4 | 98.8 | 85.6 (c1) | 85.7 (c47) |
| DAC | plurality-based | 2 | 0% | 0% | 0% / 0% | 85.9 | 89.5 | 99.0 | 85.7 (c1) | 88.2 (c47) |
| MCA | median-based | 4 | 0% | 0% | 0% / 0% | 84.2 | 91.3 | 100.0 | 84.0 (c47) | 91.0 (c1) |
| Borda-ER | positional | 2 | 0% | 0% | 0% / 50% | 83.2 | 92.9 | 98.6 | 81.9 (c1) | 91.0 (c1) |
| Asset-foresight | delegated | 1 | 0% | 0% | 0% / 0% | 80.6 | 87.3 | 99.3 | 80.6 (c1) | 87.3 (c1) |

## Who dominates whom (the 12 leading methods)

Row dominates column in this many of the conditions both were run in (shared conditions after the slash).

| | Schulze | Ranked pairs | Kemeny-Young | Soft Minimax-D | Minimax | MMWV | Score runoff | BTR-score | MMPO | Soft Minimax | Smith//Score | Copeland |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Schulze** | — | 7/78 | 5/74 | 3/74 | 6/74 | 10/74 | 3/74 | 6/74 | 12/74 | 6/74 | 6/74 | 41/74 |
| **Ranked pairs** | 5/78 | — | 10/74 | 3/74 | 8/74 | 6/74 | 4/74 | 6/74 | 14/74 | 8/74 | 5/74 | 41/74 |
| **Kemeny-Young** | 3/74 | 1/74 | — | 2/74 | 6/74 | 9/74 | 2/74 | 6/74 | 11/74 | 6/74 | 6/74 | 38/74 |
| **Soft Minimax-D** | 6/74 | 7/74 | 6/74 | — | 5/74 | 6/74 | 27/77 | 19/77 | 8/74 | 25/77 | 15/77 | 10/74 |
| **Minimax** | 1/74 | 6/74 | 5/74 | 3/74 | — | 10/74 | 4/74 | 6/74 | 15/74 | 7/74 | 7/74 | 40/74 |
| **MMWV** | 4/74 | 6/74 | 3/74 | 2/74 | 4/74 | — | 1/74 | 5/74 | 8/74 | 6/74 | 4/74 | 38/74 |
| **Score runoff** | 10/74 | 10/74 | 9/74 | 10/77 | 9/74 | 11/74 | — | 16/77 | 12/74 | 18/77 | 17/77 | 29/74 |
| **BTR-score** | 4/74 | 4/74 | 4/74 | 3/77 | 5/74 | 5/74 | 12/77 | — | 6/74 | 7/77 | 11/77 | 11/74 |
| **MMPO** | 6/74 | 5/74 | 5/74 | 1/74 | 4/74 | 2/74 | 0/74 | 4/74 | — | 6/74 | 4/74 | 32/74 |
| **Soft Minimax** | 7/74 | 7/74 | 8/74 | 14/77 | 8/74 | 9/74 | 12/77 | 16/77 | 11/74 | — | 23/77 | 15/74 |
| **Smith//Score** | 3/74 | 5/74 | 5/74 | 4/77 | 4/74 | 6/74 | 12/77 | 7/77 | 8/74 | 9/77 | — | 9/74 |
| **Copeland** | 2/74 | 3/74 | 2/74 | 3/74 | 2/74 | 3/74 | 2/74 | 5/74 | 7/74 | 5/74 | 4/74 | — |

## Clear dominance (beatpath)

An arrow A → B: A dominates B in at least 50% of the conditions both were run in (--beat-share), and more often than B dominates A. Arrows implied by a chain of others are left out, so a method is dominated at this level by everything above it along the arrows. Methods of the main table only.

![clear dominance](beatpath.png)

Dominated by none at this level: Schulze, Ranked pairs, Kemeny-Young, Soft Minimax-D, Minimax, MMWV, Score runoff, BTR-score, MMPO, Soft Minimax, Smith//Score.

- **Schulze** → Copeland (55%), BTR-IRV (84%)
- **Ranked pairs** → Copeland (55%), BTR-IRV (86%)
- **Kemeny-Young** → Copeland (51%), BTR-IRV (80%)
- **Soft Minimax-D** → Soft Minimax-40 (52%), ARA (58%), MARS (56%), MARS-T (61%), STAR (53%), IMDS (71%)
- **Minimax** → Copeland (54%), BTR-IRV (81%)
- **MMWV** → Copeland (51%), BTR-IRV (81%)
- **Score runoff** → ESR-MARS (69%)
- **BTR-score** → MARS-T (53%), IMDS (52%)
- **MMPO** → BTR-IRV (80%)
- **Soft Minimax** → IMDS (69%)
- **Smith//Score** → ESR-MARS (50%), ESR (58%)
- **Copeland** → Borda (88%), ER-IRV (66%)
- **Soft Minimax-40** → ESR-MARS (81%), ESR (73%)
- **ARA** → ESR-MARS (62%), ESR (68%)
- **MARS** → ESR-MARS (79%), ESR (69%)
- **MARS-T** → ESR-MARS (74%), ESR (68%)
- **STAR** → ESR (55%)
- **Approval runoff** → Approval cond. runoff (68%)
- **DMS** → Approval (80%), TTR (77%), IRV-3 (93%), IRV (85%)
- **ESR-MARS** → Approval runoff (68%), MJ (74%), Delegated 321 (63%), Score (86%), 321 (68%), MAV (79%)
- **BTR-IRV** → Borda (91%), ER-IRV (64%)
- **ESR** → Approval runoff (57%), MJ (75%), Delegated 321 (61%), Score (71%), 321 (74%), MAV (87%)
- **MJ** → Borda (55%), Approval (94%), ER-IRV (55%)
- **IMDS** → ESR (52%)
- **Delegated 321** → DMS (51%), Borda (90%)
- **Score** → Borda (63%), Approval (82%), ER-IRV (61%)
- **321** → Borda (55%), Approval (90%), ER-IRV (58%)
- **MAV** → Borda (51%), Approval (87%), TTR (86%), IRV-3 (92%)
- **Approval cond. runoff** → Borda (72%), Approval (89%), ER-IRV (51%)
- **Borda** → IRV (53%)
- **Approval** → Supplementary vote (99%)
- **ER-IRV** → TTR (78%), IRV-3 (70%), IRV (57%)
- **TTR** → Supplementary vote (68%)
- **Asset** → Plurality (85%)
- **IRV-3** → Supplementary vote (81%)
- **IRV** → Supplementary vote (84%)
- **Supplementary vote** → Asset (56%)

## Map of the methods

How alike the methods are in what they do: the distance between two methods is the share of elections in which they elect different winners, averaged over the conditions (each counting once). The map lays these distances out in two dimensions (classical multidimensional scaling) with honest (open) and learned ballots (filled) in one layout, an arrow from each method's honest to its strategic behaviour. The two dimensions show 41% of the spread; methods close together elect the same winner in most elections, whatever their family. Coordinates in method-map.csv.

![map of the methods](method-map.png)

## Requested pairs

The pairs asked for with --pairs, in that order: how often each method is ahead, and where the other one is.

**Approval — Plurality**

- Approval dominates Plurality in 77 of 78 conditions.
- VSE\*: Approval clearly better than Plurality in 75 of 78 conditions; Plurality better in 1: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic).
- WME\*: Approval clearly better than Plurality in 78 of 78 conditions.

**Supplementary vote — Plurality**

- Supplementary vote dominates Plurality in 68 of 74 conditions.
- VSE\*: Supplementary vote clearly better than Plurality in 71 of 74 conditions; Plurality better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- WME\*: Supplementary vote clearly better than Plurality in 72 of 74 conditions; Plurality better in 1: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic).

**TTR — Supplementary vote**

- TTR dominates Supplementary vote in 50 of 74 conditions.
- VSE\*: TTR clearly better than Supplementary vote in 22 of 74 conditions; Supplementary vote better in 1: c38 (strategic share 1.0, adaptive share 0.0).
- WME\*: TTR clearly better than Supplementary vote in 73 of 74 conditions.

**IRV — TTR**

- IRV dominates TTR in 30 of 74 conditions; TTR dominates IRV in 26: c1 (defaults), c2 (response to a move arithmetic), c3 (campaigns 1.0, response to a move arithmetic), c6 (candidate dispersion 0.5), c7 (candidate dispersion 2.0), c8 (candidate shift 0.5), c9 (candidate shift 1.0), c10 (candidates 4), c14 (coordination share 0.1), c16 (search 1, individual voters 0), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c19 (honesty cost 0.1), c21 (laziness 0.25), c22 (laziness 0.5), c23 (first poll previous), c25 (electorate model clusters), c27 (electorate model ks), c28 (exits 0, clones 0), c29 (poll error 1.0), c30 (poll error 4.0), c31 (poll rounds 1), c32 (poll rounds 2), c35 (samples 10), c39 (utility linear), c44 (width 1.4), c49 (candidate dispersion 2.0, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: IRV clearly better than TTR in 16 of 74 conditions; TTR better in 16: c1 (defaults), c3 (campaigns 1.0, response to a move arithmetic), c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c7 (candidate dispersion 2.0), c9 (candidate shift 1.0), c14 (coordination share 0.1), c16 (search 1, individual voters 0), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c19 (honesty cost 0.1), c23 (first poll previous), c25 (electorate model clusters), c31 (poll rounds 1), c35 (samples 10), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear).
- WME\*: TTR clearly better than IRV in 26 of 74 conditions; IRV better in 15: c13 (coordination share 0.0), c20 (honesty cost 1.0), c36 (samples 20), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c54 (candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c56 (candidate dispersion 0.5, samples 10, laziness 0.1, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c68 (poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c76 (utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c80 (width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**IRV-3 — TTR**

- TTR dominates IRV-3 in 32 of 74 conditions; IRV-3 dominates TTR in 30: c13 (coordination share 0.0), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c46 (samples 10, aggressive share 1.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c47 (samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c48 (candidate dispersion 0.5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c51 (candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c52 (candidates 7, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c54 (candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c56 (candidate dispersion 0.5, samples 10, laziness 0.1, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c57 (samples 10, laziness 0.1, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c59 (samples 10, exits 0, clones 0, search 10, first poll previous, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c65 (samples 10, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c68 (poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c69 (poll rounds 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c70 (poll rounds 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c71 (poll rounds 6, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c72 (exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c76 (utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c80 (width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: TTR clearly better than IRV-3 in 18 of 74 conditions; IRV-3 better in 9: c13 (coordination share 0.0), c47 (samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c54 (candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c59 (samples 10, exits 0, clones 0, search 10, first poll previous, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: TTR clearly better than IRV-3 in 39 of 74 conditions; IRV-3 better in 1: c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Approval cond. runoff — TTR**

- Approval cond. runoff dominates TTR in 57 of 74 conditions; TTR dominates Approval cond. runoff in 1: c9 (candidate shift 1.0).
- VSE\*: Approval cond. runoff clearly better than TTR in 55 of 74 conditions.
- WME\*: Approval cond. runoff clearly better than TTR in 32 of 74 conditions; TTR better in 13: c1 (defaults), c2 (response to a move arithmetic), c3 (campaigns 1.0, response to a move arithmetic), c8 (candidate shift 0.5), c9 (candidate shift 1.0), c13 (coordination share 0.0), c15 (coordination share 0.5), c19 (honesty cost 0.1), c23 (first poll previous), c29 (poll error 1.0), c31 (poll rounds 1), c32 (poll rounds 2), c36 (samples 20).

**Approval cond. runoff — Approval**

- Approval cond. runoff dominates Approval in 66 of 74 conditions; Approval dominates Approval cond. runoff in 2: c19 (honesty cost 0.1), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Approval cond. runoff clearly better than Approval in 12 of 74 conditions; Approval better in 3: c19 (honesty cost 0.1), c38 (strategic share 1.0, adaptive share 0.0), c46 (samples 10, aggressive share 1.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: Approval cond. runoff clearly better than Approval in 67 of 74 conditions.

**Approval runoff — Approval cond. runoff**

- Approval runoff dominates Approval cond. runoff in 50 of 74 conditions; Approval cond. runoff dominates Approval runoff in 1: c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Approval runoff clearly better than Approval cond. runoff in 10 of 74 conditions; Approval cond. runoff better in 1: c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: Approval runoff clearly better than Approval cond. runoff in 48 of 74 conditions.

**BTR-score — IRV**

- BTR-score dominates IRV in 74 of 74 conditions.
- VSE\*: BTR-score clearly better than IRV in 56 of 74 conditions.
- WME\*: BTR-score clearly better than IRV in 72 of 74 conditions.

**IMDS — INS**

- No clear difference on any of the front axes in any of the 2 conditions both were run in.

**BTR-score — Smith//Score**

- BTR-score dominates Smith//Score in 11 of 77 conditions; Smith//Score dominates BTR-score in 7: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c8 (candidate shift 0.5), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Smith//Score clearly better than BTR-score in 6 of 77 conditions; BTR-score better in 5: c2 (response to a move arithmetic), c30 (poll error 4.0), c32 (poll rounds 2), c40 (utility quadratic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: BTR-score clearly better than Smith//Score in 7 of 77 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with response to a move arithmetic.

## Exceptions

Pairs of leading methods where each is ahead somewhere: how often each is ahead, and the conditions where the less usual outcome happens, with the settings they share. Pairs asked for with --pairs are above, not repeated here.

**Schulze — Ranked pairs**

- Schulze dominates Ranked pairs in 7 of 78 conditions; Ranked pairs dominates Schulze in 5: c3 (campaigns 1.0, response to a move arithmetic), c31 (poll rounds 1), c34 (poll rounds 5), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c79 (voters 40, electorate model ks-quinn, utility linear, poll rounds 1, samples 10, strategic share 1.0, adaptive share 0.0, max score 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Schulze clearly better than Ranked pairs in 3 of 78 conditions; Ranked pairs better in 3: c3 (campaigns 1.0, response to a move arithmetic), c31 (poll rounds 1), c34 (poll rounds 5).
- WME\*: Schulze clearly better than Ranked pairs in 7 of 78 conditions; Ranked pairs better in 3: c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c79 (voters 40, electorate model ks-quinn, utility linear, poll rounds 1, samples 10, strategic share 1.0, adaptive share 0.0, max score 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic.

**Schulze — Kemeny-Young**

- Schulze dominates Kemeny-Young in 5 of 74 conditions; Kemeny-Young dominates Schulze in 3: c27 (electorate model ks), c34 (poll rounds 5), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Schulze clearly better than Kemeny-Young in 3 of 74 conditions; Kemeny-Young better in 3: c27 (electorate model ks), c34 (poll rounds 5), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: Schulze clearly better than Kemeny-Young in 3 of 74 conditions.

**Schulze — Soft Minimax-D**

- Soft Minimax-D dominates Schulze in 6 of 74 conditions; Schulze dominates Soft Minimax-D in 3: c21 (laziness 0.25), c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than Schulze in 68 of 74 conditions.
- WME\*: Schulze clearly better than Soft Minimax-D in 68 of 74 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Schulze — Minimax**

- Schulze dominates Minimax in 6 of 74 conditions; Minimax dominates Schulze in 1: c22 (laziness 0.5).
- VSE\*: Schulze clearly better than Minimax in 2 of 74 conditions.
- WME\*: Schulze clearly better than Minimax in 5 of 74 conditions; Minimax better in 1: c22 (laziness 0.5).

**Schulze — MMWV**

- Schulze dominates MMWV in 10 of 74 conditions; MMWV dominates Schulze in 4: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c56 (candidate dispersion 0.5, samples 10, laziness 0.1, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with individual voters 0.
- VSE\*: Schulze clearly better than MMWV in 5 of 74 conditions.
- WME\*: Schulze clearly better than MMWV in 5 of 74 conditions; MMWV better in 1: c56 (candidate dispersion 0.5, samples 10, laziness 0.1, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Schulze — Score runoff**

- Score runoff dominates Schulze in 10 of 74 conditions; Schulze dominates Score runoff in 3: c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c38 (strategic share 1.0, adaptive share 0.0), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Score runoff clearly better than Schulze in 56 of 74 conditions.
- WME\*: Schulze clearly better than Score runoff in 64 of 74 conditions; Score runoff better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with response to a move arithmetic.

**Schulze — BTR-score**

- Schulze dominates BTR-score in 6 of 74 conditions; BTR-score dominates Schulze in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: BTR-score clearly better than Schulze in 60 of 74 conditions.
- WME\*: Schulze clearly better than BTR-score in 70 of 74 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Schulze — MMPO**

- Schulze dominates MMPO in 12 of 74 conditions; MMPO dominates Schulze in 6: c9 (candidate shift 1.0), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Schulze clearly better than MMPO in 9 of 74 conditions; MMPO better in 2: c9 (candidate shift 1.0), c10 (candidates 4).
- WME\*: Schulze clearly better than MMPO in 6 of 74 conditions; MMPO better in 1: c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Schulze — Soft Minimax**

- Soft Minimax dominates Schulze in 7 of 74 conditions; Schulze dominates Soft Minimax in 6: c7 (candidate dispersion 2.0), c21 (laziness 0.25), c37 (strategic share 0.5, adaptive share 0.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c44 (width 1.4).
- VSE\*: Soft Minimax clearly better than Schulze in 56 of 74 conditions.
- WME\*: Schulze clearly better than Soft Minimax in 64 of 74 conditions; Soft Minimax better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Schulze — Smith//Score**

- Schulze dominates Smith//Score in 6 of 74 conditions; Smith//Score dominates Schulze in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: Smith//Score clearly better than Schulze in 61 of 74 conditions.
- WME\*: Schulze clearly better than Smith//Score in 71 of 74 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Schulze — Copeland**

- Schulze dominates Copeland in 41 of 74 conditions; Copeland dominates Schulze in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- VSE\*: Schulze clearly better than Copeland in 9 of 74 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- WME\*: Schulze clearly better than Copeland in 41 of 74 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Ranked pairs — Kemeny-Young**

- Ranked pairs dominates Kemeny-Young in 10 of 74 conditions; Kemeny-Young dominates Ranked pairs in 1: c36 (samples 20).
- VSE\*: Ranked pairs clearly better than Kemeny-Young in 6 of 74 conditions.
- WME\*: Ranked pairs clearly better than Kemeny-Young in 4 of 74 conditions; Kemeny-Young better in 1: c36 (samples 20).

**Ranked pairs — Soft Minimax-D**

- Soft Minimax-D dominates Ranked pairs in 7 of 74 conditions; Ranked pairs dominates Soft Minimax-D in 3: c21 (laziness 0.25), c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than Ranked pairs in 68 of 74 conditions.
- WME\*: Ranked pairs clearly better than Soft Minimax-D in 67 of 74 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Ranked pairs — Minimax**

- Ranked pairs dominates Minimax in 8 of 74 conditions; Minimax dominates Ranked pairs in 6: c13 (coordination share 0.0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c35 (samples 10), c36 (samples 20), c41 (voters 199), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Ranked pairs clearly better than Minimax in 5 of 74 conditions; Minimax better in 3: c41 (voters 199), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: Ranked pairs clearly better than Minimax in 4 of 74 conditions; Minimax better in 4: c13 (coordination share 0.0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c35 (samples 10), c36 (samples 20).

**Ranked pairs — MMWV**

- Ranked pairs dominates MMWV in 6 of 74 conditions; MMWV dominates Ranked pairs in 6: c13 (coordination share 0.0), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c20 (honesty cost 1.0), c35 (samples 10), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Ranked pairs clearly better than MMWV in 3 of 74 conditions.
- WME\*: Ranked pairs clearly better than MMWV in 3 of 74 conditions; MMWV better in 3: c13 (coordination share 0.0), c20 (honesty cost 1.0), c35 (samples 10).

**Ranked pairs — Score runoff**

- Score runoff dominates Ranked pairs in 10 of 74 conditions; Ranked pairs dominates Score runoff in 4: c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Score runoff clearly better than Ranked pairs in 54 of 74 conditions.
- WME\*: Ranked pairs clearly better than Score runoff in 64 of 74 conditions; Score runoff better in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Ranked pairs — BTR-score**

- Ranked pairs dominates BTR-score in 6 of 74 conditions; BTR-score dominates Ranked pairs in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: BTR-score clearly better than Ranked pairs in 62 of 74 conditions.
- WME\*: Ranked pairs clearly better than BTR-score in 70 of 74 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Ranked pairs — MMPO**

- Ranked pairs dominates MMPO in 14 of 74 conditions; MMPO dominates Ranked pairs in 5: c13 (coordination share 0.0), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c20 (honesty cost 1.0), c35 (samples 10), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Ranked pairs clearly better than MMPO in 9 of 74 conditions.
- WME\*: Ranked pairs clearly better than MMPO in 7 of 74 conditions; MMPO better in 3: c13 (coordination share 0.0), c20 (honesty cost 1.0), c35 (samples 10).

**Ranked pairs — Soft Minimax**

- Ranked pairs dominates Soft Minimax in 8 of 74 conditions; Soft Minimax dominates Ranked pairs in 7: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c6 (candidate dispersion 0.5), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c26 (electorate model field), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Soft Minimax clearly better than Ranked pairs in 56 of 74 conditions.
- WME\*: Ranked pairs clearly better than Soft Minimax in 65 of 74 conditions; Soft Minimax better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Ranked pairs — Smith//Score**

- Ranked pairs dominates Smith//Score in 5 of 74 conditions; Smith//Score dominates Ranked pairs in 5: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Smith//Score clearly better than Ranked pairs in 62 of 74 conditions.
- WME\*: Ranked pairs clearly better than Smith//Score in 68 of 74 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Ranked pairs — Copeland**

- Ranked pairs dominates Copeland in 41 of 74 conditions; Copeland dominates Ranked pairs in 3: c2 (response to a move arithmetic), c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with response to a move arithmetic.
- VSE\*: Ranked pairs clearly better than Copeland in 10 of 74 conditions; Copeland better in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c55 (candidates 10, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with response to a move arithmetic.
- WME\*: Ranked pairs clearly better than Copeland in 42 of 74 conditions; Copeland better in 3: c2 (response to a move arithmetic), c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with response to a move arithmetic.

**Kemeny-Young — Soft Minimax-D**

- Soft Minimax-D dominates Kemeny-Young in 6 of 74 conditions; Kemeny-Young dominates Soft Minimax-D in 2: c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than Kemeny-Young in 69 of 74 conditions.
- WME\*: Kemeny-Young clearly better than Soft Minimax-D in 68 of 74 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Kemeny-Young — Minimax**

- Kemeny-Young dominates Minimax in 6 of 74 conditions; Minimax dominates Kemeny-Young in 5: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c28 (exits 0, clones 0), c41 (voters 199), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Kemeny-Young clearly better than Minimax in 3 of 74 conditions; Minimax better in 3: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c28 (exits 0, clones 0), c41 (voters 199).
- WME\*: Kemeny-Young clearly better than Minimax in 3 of 74 conditions; Minimax better in 3: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with response to a move arithmetic.

**Kemeny-Young — MMWV**

- Kemeny-Young dominates MMWV in 9 of 74 conditions; MMWV dominates Kemeny-Young in 3: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with adaptive share 0.0, individual voters 0.
- VSE\*: Kemeny-Young clearly better than MMWV in 4 of 74 conditions.
- WME\*: Kemeny-Young clearly better than MMWV in 6 of 74 conditions.

**Kemeny-Young — Score runoff**

- Score runoff dominates Kemeny-Young in 9 of 74 conditions; Kemeny-Young dominates Score runoff in 2: c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Score runoff clearly better than Kemeny-Young in 59 of 74 conditions.
- WME\*: Kemeny-Young clearly better than Score runoff in 65 of 74 conditions; Score runoff better in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with response to a move arithmetic.

**Kemeny-Young — BTR-score**

- Kemeny-Young dominates BTR-score in 6 of 74 conditions; BTR-score dominates Kemeny-Young in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: BTR-score clearly better than Kemeny-Young in 62 of 74 conditions.
- WME\*: Kemeny-Young clearly better than BTR-score in 70 of 74 conditions; BTR-score better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Kemeny-Young — MMPO**

- Kemeny-Young dominates MMPO in 11 of 74 conditions; MMPO dominates Kemeny-Young in 5: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with individual voters 0.
- VSE\*: Kemeny-Young clearly better than MMPO in 5 of 74 conditions; MMPO better in 1: c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: Kemeny-Young clearly better than MMPO in 7 of 74 conditions; MMPO better in 1: c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Kemeny-Young — Soft Minimax**

- Soft Minimax dominates Kemeny-Young in 8 of 74 conditions; Kemeny-Young dominates Soft Minimax in 6: c7 (candidate dispersion 2.0), c21 (laziness 0.25), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c44 (width 1.4), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Soft Minimax clearly better than Kemeny-Young in 58 of 74 conditions.
- WME\*: Kemeny-Young clearly better than Soft Minimax in 64 of 74 conditions; Soft Minimax better in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0.

**Kemeny-Young — Smith//Score**

- Kemeny-Young dominates Smith//Score in 6 of 74 conditions; Smith//Score dominates Kemeny-Young in 5: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Smith//Score clearly better than Kemeny-Young in 60 of 74 conditions.
- WME\*: Kemeny-Young clearly better than Smith//Score in 69 of 74 conditions; Smith//Score better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Kemeny-Young — Copeland**

- Kemeny-Young dominates Copeland in 38 of 74 conditions; Copeland dominates Kemeny-Young in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- VSE\*: Kemeny-Young clearly better than Copeland in 3 of 74 conditions; Copeland better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c55 (candidates 10, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with response to a move arithmetic.
- WME\*: Kemeny-Young clearly better than Copeland in 39 of 74 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Soft Minimax-D — Minimax**

- Soft Minimax-D dominates Minimax in 5 of 74 conditions; Minimax dominates Soft Minimax-D in 3: c21 (laziness 0.25), c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than Minimax in 69 of 74 conditions.
- WME\*: Minimax clearly better than Soft Minimax-D in 68 of 74 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Soft Minimax-D — MMWV**

- Soft Minimax-D dominates MMWV in 6 of 74 conditions; MMWV dominates Soft Minimax-D in 2: c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than MMWV in 71 of 74 conditions.
- WME\*: MMWV clearly better than Soft Minimax-D in 68 of 74 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Soft Minimax-D — Score runoff**

- Soft Minimax-D dominates Score runoff in 27 of 77 conditions; Score runoff dominates Soft Minimax-D in 10: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c51 (candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c52 (candidates 7, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c57 (samples 10, laziness 0.1, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c59 (samples 10, exits 0, clones 0, search 10, first poll previous, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with response to a move arithmetic.
- VSE\*: Soft Minimax-D clearly better than Score runoff in 24 of 77 conditions; Score runoff better in 15: c1 (defaults), c2 (response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c7 (candidate dispersion 2.0), c13 (coordination share 0.0), c14 (coordination share 0.1), c16 (search 1, individual voters 0), c20 (honesty cost 1.0), c21 (laziness 0.25), c22 (laziness 0.5), c24 (max score 99), c25 (electorate model clusters), c30 (poll error 4.0), c36 (samples 20), c41 (voters 199).
- WME\*: Soft Minimax-D clearly better than Score runoff in 40 of 77 conditions; Score runoff better in 33: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c28 (exits 0, clones 0), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c47 (samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c48 (candidate dispersion 0.5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c49 (candidate dispersion 2.0, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c51 (candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c52 (candidates 7, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c54 (candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c55 (candidates 10, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c57 (samples 10, laziness 0.1, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c59 (samples 10, exits 0, clones 0, search 10, first poll previous, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c64 (electorate model ks, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c68 (poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c69 (poll rounds 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c70 (poll rounds 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c71 (poll rounds 6, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c72 (exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c76 (utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c80 (width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Soft Minimax-D — BTR-score**

- Soft Minimax-D dominates BTR-score in 19 of 77 conditions; BTR-score dominates Soft Minimax-D in 3: c39 (utility linear), c41 (voters 199), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Soft Minimax-D clearly better than BTR-score in 15 of 77 conditions.
- WME\*: Soft Minimax-D clearly better than BTR-score in 6 of 77 conditions; BTR-score better in 4: c39 (utility linear), c40 (utility quadratic), c41 (voters 199), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Soft Minimax-D — MMPO**

- Soft Minimax-D dominates MMPO in 8 of 74 conditions; MMPO dominates Soft Minimax-D in 1: c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than MMPO in 72 of 74 conditions.
- WME\*: MMPO clearly better than Soft Minimax-D in 66 of 74 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Soft Minimax-D — Soft Minimax**

- Soft Minimax-D dominates Soft Minimax in 25 of 77 conditions; Soft Minimax dominates Soft Minimax-D in 14: c7 (candidate dispersion 2.0), c8 (candidate shift 0.5), c19 (honesty cost 0.1), c27 (electorate model ks), c37 (strategic share 0.5, adaptive share 0.0), c38 (strategic share 1.0, adaptive share 0.0), c41 (voters 199), c52 (candidates 7, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c70 (poll rounds 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c72 (exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Soft Minimax-D clearly better than Soft Minimax in 37 of 77 conditions; Soft Minimax better in 2: c24 (max score 99), c61 (samples 10, max score 99, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with max score 99.
- WME\*: Soft Minimax clearly better than Soft Minimax-D in 27 of 77 conditions; Soft Minimax-D better in 3: c24 (max score 99), c60 (samples 10, max score 9, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c61 (samples 10, max score 99, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Soft Minimax-D — Smith//Score**

- Soft Minimax-D dominates Smith//Score in 15 of 77 conditions; Smith//Score dominates Soft Minimax-D in 4: c9 (candidate shift 1.0), c39 (utility linear), c41 (voters 199), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Soft Minimax-D clearly better than Smith//Score in 13 of 77 conditions; Smith//Score better in 1: c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: Smith//Score clearly better than Soft Minimax-D in 4 of 77 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c25 (electorate model clusters).

**Soft Minimax-D — Copeland**

- Soft Minimax-D dominates Copeland in 10 of 74 conditions; Copeland dominates Soft Minimax-D in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c21 (laziness 0.25), c22 (laziness 0.5).
- VSE\*: Soft Minimax-D clearly better than Copeland in 67 of 74 conditions; Copeland better in 1: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic).
- WME\*: Copeland clearly better than Soft Minimax-D in 63 of 74 conditions; Soft Minimax-D better in 2: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, individual voters 0.

**Minimax — MMWV**

- Minimax dominates MMWV in 10 of 74 conditions; MMWV dominates Minimax in 4: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c56 (candidate dispersion 0.5, samples 10, laziness 0.1, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with individual voters 0.
- VSE\*: Minimax clearly better than MMWV in 6 of 74 conditions.
- WME\*: Minimax clearly better than MMWV in 5 of 74 conditions; MMWV better in 1: c56 (candidate dispersion 0.5, samples 10, laziness 0.1, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Minimax — Score runoff**

- Score runoff dominates Minimax in 9 of 74 conditions; Minimax dominates Score runoff in 4: c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Score runoff clearly better than Minimax in 56 of 74 conditions.
- WME\*: Minimax clearly better than Score runoff in 65 of 74 conditions; Score runoff better in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Minimax — BTR-score**

- Minimax dominates BTR-score in 6 of 74 conditions; BTR-score dominates Minimax in 5: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: BTR-score clearly better than Minimax in 61 of 74 conditions.
- WME\*: Minimax clearly better than BTR-score in 69 of 74 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Minimax — MMPO**

- Minimax dominates MMPO in 15 of 74 conditions; MMPO dominates Minimax in 4: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with individual voters 0.
- VSE\*: Minimax clearly better than MMPO in 12 of 74 conditions.
- WME\*: Minimax clearly better than MMPO in 6 of 74 conditions; MMPO better in 1: c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Minimax — Soft Minimax**

- Soft Minimax dominates Minimax in 8 of 74 conditions; Minimax dominates Soft Minimax in 7: c6 (candidate dispersion 0.5), c7 (candidate dispersion 2.0), c21 (laziness 0.25), c37 (strategic share 0.5, adaptive share 0.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c44 (width 1.4).
- VSE\*: Soft Minimax clearly better than Minimax in 58 of 74 conditions.
- WME\*: Minimax clearly better than Soft Minimax in 64 of 74 conditions; Soft Minimax better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Minimax — Smith//Score**

- Minimax dominates Smith//Score in 7 of 74 conditions; Smith//Score dominates Minimax in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Smith//Score clearly better than Minimax in 60 of 74 conditions.
- WME\*: Minimax clearly better than Smith//Score in 70 of 74 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Minimax — Copeland**

- Minimax dominates Copeland in 40 of 74 conditions; Copeland dominates Minimax in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- VSE\*: Minimax clearly better than Copeland in 9 of 74 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- WME\*: Minimax clearly better than Copeland in 38 of 74 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**MMWV — Score runoff**

- Score runoff dominates MMWV in 11 of 74 conditions; MMWV dominates Score runoff in 1: c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Score runoff clearly better than MMWV in 62 of 74 conditions.
- WME\*: MMWV clearly better than Score runoff in 63 of 74 conditions; Score runoff better in 5: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**MMWV — BTR-score**

- MMWV dominates BTR-score in 5 of 74 conditions; BTR-score dominates MMWV in 5: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: BTR-score clearly better than MMWV in 66 of 74 conditions.
- WME\*: MMWV clearly better than BTR-score in 69 of 74 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**MMWV — MMPO**

- MMWV dominates MMPO in 8 of 74 conditions; MMPO dominates MMWV in 2: c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic.
- VSE\*: MMWV clearly better than MMPO in 7 of 74 conditions; MMPO better in 2: c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic.
- WME\*: MMWV clearly better than MMPO in 4 of 74 conditions.

**MMWV — Soft Minimax**

- Soft Minimax dominates MMWV in 9 of 74 conditions; MMWV dominates Soft Minimax in 6: c6 (candidate dispersion 0.5), c7 (candidate dispersion 2.0), c37 (strategic share 0.5, adaptive share 0.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c44 (width 1.4).
- VSE\*: Soft Minimax clearly better than MMWV in 60 of 74 conditions.
- WME\*: MMWV clearly better than Soft Minimax in 62 of 74 conditions; Soft Minimax better in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0.

**MMWV — Smith//Score**

- Smith//Score dominates MMWV in 6 of 74 conditions; MMWV dominates Smith//Score in 4: c6 (candidate dispersion 0.5), c7 (candidate dispersion 2.0), c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Smith//Score clearly better than MMWV in 68 of 74 conditions.
- WME\*: MMWV clearly better than Smith//Score in 67 of 74 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**MMWV — Copeland**

- MMWV dominates Copeland in 38 of 74 conditions; Copeland dominates MMWV in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0).
- VSE\*: MMWV clearly better than Copeland in 4 of 74 conditions; Copeland better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0).
- WME\*: MMWV clearly better than Copeland in 36 of 74 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Score runoff — BTR-score**

- Score runoff dominates BTR-score in 16 of 77 conditions; BTR-score dominates Score runoff in 12: c8 (candidate shift 0.5), c9 (candidate shift 1.0), c10 (candidates 4), c15 (coordination share 0.5), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c19 (honesty cost 0.1), c26 (electorate model field), c37 (strategic share 0.5, adaptive share 0.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c44 (width 1.4), c65 (samples 10, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Score runoff clearly better than BTR-score in 31 of 77 conditions; BTR-score better in 17: c28 (exits 0, clones 0), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c47 (samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c48 (candidate dispersion 0.5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c49 (candidate dispersion 2.0, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c51 (candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c55 (candidates 10, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c68 (poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c70 (poll rounds 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c71 (poll rounds 6, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c72 (exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c76 (utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c80 (width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with exits 0, clones 0.
- WME\*: BTR-score clearly better than Score runoff in 40 of 77 conditions; Score runoff better in 32: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c28 (exits 0, clones 0), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c47 (samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c48 (candidate dispersion 0.5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c49 (candidate dispersion 2.0, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c51 (candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c52 (candidates 7, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c54 (candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c55 (candidates 10, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c57 (samples 10, laziness 0.1, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c59 (samples 10, exits 0, clones 0, search 10, first poll previous, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c64 (electorate model ks, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c68 (poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c69 (poll rounds 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c70 (poll rounds 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c71 (poll rounds 6, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c72 (exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c76 (utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c80 (width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Score runoff — MMPO**

- Score runoff dominates MMPO in 12 of 74 conditions.
- VSE\*: Score runoff clearly better than MMPO in 64 of 74 conditions.
- WME\*: MMPO clearly better than Score runoff in 62 of 74 conditions; Score runoff better in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Score runoff — Soft Minimax**

- Score runoff dominates Soft Minimax in 18 of 77 conditions; Soft Minimax dominates Score runoff in 12: c6 (candidate dispersion 0.5), c8 (candidate shift 0.5), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c19 (honesty cost 0.1), c26 (electorate model field), c37 (strategic share 0.5, adaptive share 0.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c65 (samples 10, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c80 (width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Score runoff clearly better than Soft Minimax in 32 of 77 conditions; Soft Minimax better in 15: c47 (samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c48 (candidate dispersion 0.5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c51 (candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c60 (samples 10, max score 9, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c61 (samples 10, max score 99, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c68 (poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c71 (poll rounds 6, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c72 (exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c76 (utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c80 (width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic.
- WME\*: Soft Minimax clearly better than Score runoff in 39 of 77 conditions; Score runoff better in 29: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c28 (exits 0, clones 0), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c47 (samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c48 (candidate dispersion 0.5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c49 (candidate dispersion 2.0, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c51 (candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c54 (candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c55 (candidates 10, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c57 (samples 10, laziness 0.1, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c60 (samples 10, max score 9, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c61 (samples 10, max score 99, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c64 (electorate model ks, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c68 (poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c69 (poll rounds 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c71 (poll rounds 6, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c72 (exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c76 (utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Score runoff — Smith//Score**

- Score runoff dominates Smith//Score in 17 of 77 conditions; Smith//Score dominates Score runoff in 12: c8 (candidate shift 0.5), c9 (candidate shift 1.0), c10 (candidates 4), c12 (candidates 10), c15 (coordination share 0.5), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c19 (honesty cost 0.1), c26 (electorate model field), c37 (strategic share 0.5, adaptive share 0.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c44 (width 1.4).
- VSE\*: Score runoff clearly better than Smith//Score in 27 of 77 conditions; Smith//Score better in 17: c28 (exits 0, clones 0), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c47 (samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c48 (candidate dispersion 0.5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c51 (candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c54 (candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c68 (poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c71 (poll rounds 6, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c76 (utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c80 (width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with exits 0, clones 0.
- WME\*: Smith//Score clearly better than Score runoff in 39 of 77 conditions; Score runoff better in 34: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c28 (exits 0, clones 0), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c47 (samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c48 (candidate dispersion 0.5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c49 (candidate dispersion 2.0, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c51 (candidates 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c52 (candidates 7, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c54 (candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c55 (candidates 10, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c57 (samples 10, laziness 0.1, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c59 (samples 10, exits 0, clones 0, search 10, first poll previous, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c60 (samples 10, max score 9, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c64 (electorate model ks, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c67 (poll rounds 1, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c68 (poll rounds 2, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c69 (poll rounds 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c70 (poll rounds 5, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c71 (poll rounds 6, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c72 (exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c73 (samples 10, strategic share 0.5, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c75 (utility linear, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c76 (utility quadratic, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c80 (width 1.4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**Score runoff — Copeland**

- Score runoff dominates Copeland in 29 of 74 conditions; Copeland dominates Score runoff in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0).
- VSE\*: Score runoff clearly better than Copeland in 63 of 74 conditions.
- WME\*: Copeland clearly better than Score runoff in 45 of 74 conditions; Score runoff better in 6: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c50 (candidates 4, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with individual voters 0.

**BTR-score — MMPO**

- BTR-score dominates MMPO in 6 of 74 conditions; MMPO dominates BTR-score in 4: c6 (candidate dispersion 0.5), c7 (candidate dispersion 2.0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: BTR-score clearly better than MMPO in 65 of 74 conditions.
- WME\*: MMPO clearly better than BTR-score in 68 of 74 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**BTR-score — Soft Minimax**

- Soft Minimax dominates BTR-score in 16 of 77 conditions; BTR-score dominates Soft Minimax in 7: c9 (candidate shift 1.0), c13 (coordination share 0.0), c20 (honesty cost 1.0), c32 (poll rounds 2), c39 (utility linear), c55 (candidates 10, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c81 (width 2.8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: BTR-score clearly better than Soft Minimax in 8 of 77 conditions; Soft Minimax better in 4: c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c24 (max score 99), c60 (samples 10, max score 9, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c61 (samples 10, max score 99, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: Soft Minimax clearly better than BTR-score in 17 of 77 conditions; BTR-score better in 3: c24 (max score 99), c60 (samples 10, max score 9, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c61 (samples 10, max score 99, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).

**BTR-score — Copeland**

- BTR-score dominates Copeland in 11 of 74 conditions; Copeland dominates BTR-score in 5: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c6 (candidate dispersion 0.5), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c21 (laziness 0.25), c22 (laziness 0.5).
- VSE\*: BTR-score clearly better than Copeland in 64 of 74 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- WME\*: Copeland clearly better than BTR-score in 63 of 74 conditions; BTR-score better in 2: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, individual voters 0.

**MMPO — Soft Minimax**

- Soft Minimax dominates MMPO in 11 of 74 conditions; MMPO dominates Soft Minimax in 6: c6 (candidate dispersion 0.5), c7 (candidate dispersion 2.0), c9 (candidate shift 1.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c44 (width 1.4).
- VSE\*: Soft Minimax clearly better than MMPO in 60 of 74 conditions.
- WME\*: MMPO clearly better than Soft Minimax in 61 of 74 conditions; Soft Minimax better in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0.

**MMPO — Smith//Score**

- Smith//Score dominates MMPO in 8 of 74 conditions; MMPO dominates Smith//Score in 4: c6 (candidate dispersion 0.5), c7 (candidate dispersion 2.0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Smith//Score clearly better than MMPO in 65 of 74 conditions.
- WME\*: MMPO clearly better than Smith//Score in 66 of 74 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**MMPO — Copeland**

- MMPO dominates Copeland in 32 of 74 conditions; Copeland dominates MMPO in 7: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c11 (candidates 8), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c21 (laziness 0.25), c22 (laziness 0.5), c58 (samples 10, laziness 0.5, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Copeland clearly better than MMPO in 7 of 74 conditions; MMPO better in 3: c38 (strategic share 1.0, adaptive share 0.0), c62 (electorate model clusters, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c66 (exits 0, clones 0, poll size 9, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- WME\*: MMPO clearly better than Copeland in 31 of 74 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Soft Minimax — Smith//Score**

- Soft Minimax dominates Smith//Score in 23 of 77 conditions; Smith//Score dominates Soft Minimax in 9: c9 (candidate shift 1.0), c15 (coordination share 0.5), c44 (width 1.4), c45 (samples 10, aggressive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c46 (samples 10, aggressive share 1.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c53 (candidates 8, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c54 (candidates 9, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c63 (electorate model field, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), c77 (voters 49, samples 10, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic).
- VSE\*: Smith//Score clearly better than Soft Minimax in 11 of 77 conditions; Soft Minimax better in 2: c24 (max score 99), c61 (samples 10, max score 99, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with max score 99.
- WME\*: Soft Minimax clearly better than Smith//Score in 25 of 77 conditions; Smith//Score better in 2: c24 (max score 99), c61 (samples 10, max score 99, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with max score 99.

**Soft Minimax — Copeland**

- Soft Minimax dominates Copeland in 15 of 74 conditions; Copeland dominates Soft Minimax in 5: c2 (response to a move arithmetic), c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c21 (laziness 0.25), c39 (utility linear), c44 (width 1.4).
- VSE\*: Soft Minimax clearly better than Copeland in 62 of 74 conditions.
- WME\*: Copeland clearly better than Soft Minimax in 56 of 74 conditions; Soft Minimax better in 3: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c38 (strategic share 1.0, adaptive share 0.0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0.

**Smith//Score — Copeland**

- Smith//Score dominates Copeland in 9 of 74 conditions; Copeland dominates Smith//Score in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c6 (candidate dispersion 0.5), c21 (laziness 0.25), c22 (laziness 0.5).
- VSE\*: Smith//Score clearly better than Copeland in 66 of 74 conditions; Copeland better in 1: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic).
- WME\*: Copeland clearly better than Smith//Score in 64 of 74 conditions; Smith//Score better in 2: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c74 (samples 10, strategic share 1.0, adaptive share 0.0, exits 0, clones 0, search 10, individual voters 0, candidates vote 0, honesty cost none, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, individual voters 0.

