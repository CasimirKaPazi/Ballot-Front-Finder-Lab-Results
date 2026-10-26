# Ballot Front Finder Lab — comparison across runs

Folder: ../bffl-runs. 20 run or sweep folder(s), 35 results file(s); simulator version 1.0.0; analysis version 1.0.0. 37 distinct condition(s) in 38 analysed parts (results files with other elections are replicates of their condition and share its weight); 66 methods. Bootstrap: 1000 paired replicates per condition.

How to read this:

- **Every condition counts once**, whatever the number of runs or elections behind it. Averages and shares are over the conditions listed below, and only over those a method was run in (column *conditions*); they describe this collection of runs, not all possible electorates.
- **Clearly better** and **dominates** are as in analyze.py's reports: the paired 95% interval of the difference within the condition excludes 0 (sincerity: and the difference is at least 1 point); dominance on VSE\*, WME\* and sincerity. **Near the front (1 / 2)**: no method in the condition is better by 1 (2) points on VSE\* and WME\* at once (point estimates), the guide lines of the arrow plots.
- Pair counts are over the conditions both methods were run in.

## Conditions

| Condition | Differs from the defaults | Elections | Methods | Inputs |
|---|---|---|---|---|
| c1 | aggressive share 0.0 | 1000 | 35 | aggressive-kind |
| c2 | aggressive share 1.0 | 1000 | 35 | aggressive-kind |
| c3 | defaults (replicate: 2000 elections, seed 1) | 2000 | 66 | base, base-rest |
| c4 | defaults (replicate: 200 elections, seed 1) | 200 | 29 | defaults |
| c5 | candidate dispersion 0.5 | 1000 | 35 | cand-dispersion |
| c6 | candidate dispersion 2.0 | 1000 | 35 | cand-dispersion |
| c7 | candidates 4 | 700 | 35 | candidates |
| c8 | candidates 5 | 700 | 35 | candidates |
| c9 | candidates 7 | 700 | 35 | candidates |
| c10 | candidates 8 | 700 | 35 | candidates |
| c11 | candidates 9 | 700 | 35 | candidates |
| c12 | candidates 10 | 700 | 35 | candidates |
| c13 | candidate dispersion 0.5, laziness 0.1, exits 1, clones 2, poll size 9 | 1000 | 35 | hard |
| c14 | laziness 0.1 | 1000 | 35 | laziness |
| c15 | laziness 0.5 | 1000 | 35 | laziness |
| c16 | first poll previous | 1000 | 35 | lockin |
| c17 | max score 9 | 2000 | 28 | max-score |
| c18 | max score 99 | 2000 | 28 | max-score |
| c19 | electorate model clusters | 1000 | 35 | models |
| c20 | electorate model field | 1000 | 35 | models |
| c21 | electorate model ks | 1000 | 35 | models |
| c22 | exits 1, clones 2 | 1000 | 35 | nomination |
| c23 | samples 5, poll size 9 | 1000 | 35 | poll-size |
| c24 | poll rounds 1 | 1000 | 35 | rounds |
| c25 | poll rounds 2 | 1000 | 35 | rounds |
| c26 | poll rounds 4 | 1000 | 35 | rounds |
| c27 | poll rounds 5 | 1000 | 35 | rounds |
| c28 | poll rounds 6 | 1000 | 35 | rounds |
| c29 | samples 5 | 1000 | 35 | samples |
| c30 | strategic share 0.5, adaptive share 0.0 | 1000 | 35 | strategic-share |
| c31 | strategic share 1.0, adaptive share 0.0 | 1000 | 35 | strategic-share |
| c32 | utility linear | 1000 | 35 | utility |
| c33 | utility quadratic | 1000 | 35 | utility |
| c34 | voters 49 | 1000 | 35 | voters |
| c35 | voters 40, electorate model ks-quinn, utility linear, poll rounds 1, strategic share 0.5, adaptive share 0.0, max score 10 | 15000 | 11 | vsesim |
| c36 | voters 40, electorate model ks-quinn, utility linear, poll rounds 1, strategic share 1.0, adaptive share 0.0, max score 10 | 15000 | 11 | vsesim |
| c37 | width 1.4 | 1000 | 35 | width |
| c38 | width 2.8 | 1000 | 35 | width |

## Methods

Methods run in at least 19 of the 37 conditions, sorted by how often undominated, then near the front. Means are weighted by condition; worst: the lowest value over the conditions, with the condition.

| Method | Group | Conditions | Undominated | Pareto front | Near front (1 / 2) | Mean VSE\* | Mean WME\* | Mean sincerity | Worst VSE\* | Worst WME\* |
|---|---|---|---|---|---|---|---|---|---|---|
| Score runoff | two-round | 35 | 80% | 89% | 100% / 100% | 87.0 | 97.5 | 99.8 | 80.4 (c7) | 91.7 (c13) |
| Ranked pairs | condorcet | 35 | 73% | 66% | 100% / 100% | 86.2 | 98.8 | 97.6 | 80.2 (c7) | 93.8 (c36) |
| Kemeny-Young | condorcet | 33 | 68% | 32% | 97% / 100% | 85.5 | 98.9 | 97.8 | 81.0 (c5) | 94.9 (c13) |
| Schulze | condorcet | 35 | 67% | 40% | 100% / 100% | 86.2 | 98.7 | 97.6 | 79.9 (c7) | 93.6 (c36) |
| Minimax | minimax-family | 33 | 58% | 30% | 100% / 100% | 85.6 | 98.8 | 97.8 | 79.3 (c7) | 94.1 (c13) |
| MMWV | minimax-family | 33 | 55% | 27% | 97% / 100% | 85.4 | 98.8 | 98.2 | 79.0 (c7) | 94.6 (c15) |
| Soft Minimax-D | hybrid | 35 | 53% | 66% | 100% / 100% | 88.4 | 95.9 | 99.9 | 81.0 (c7) | 91.6 (c5) |
| Soft Minimax | hybrid | 35 | 46% | 80% | 100% / 100% | 88.0 | 96.3 | 99.9 | 82.2 (c7) | 91.7 (c5) |
| BTR-score | hybrid | 35 | 44% | 33% | 100% / 100% | 88.0 | 96.0 | 99.9 | 82.7 (c7) | 91.1 (c5) |
| MMPO | minimax-family | 33 | 44% | 38% | 94% / 100% | 85.4 | 98.7 | 98.2 | 78.3 (c7) | 93.9 (c15) |
| ARA | hybrid | 35 | 33% | 71% | 100% / 100% | 89.7 | 94.3 | 99.9 | 83.8 (c7) | 88.3 (c5) |
| Smith//Score | condorcet-based | 35 | 33% | 29% | 100% / 100% | 88.2 | 95.8 | 99.9 | 83.2 (c7) | 91.1 (c5) |
| MARS | hybrid | 35 | 31% | 46% | 97% / 100% | 88.9 | 95.3 | 99.9 | 83.4 (c31) | 89.1 (c5) |
| MARS-T | hybrid | 35 | 29% | 63% | 97% / 100% | 89.0 | 95.1 | 99.9 | 83.8 (c7) | 89.1 (c5) |
| STAR | hybrid | 37 | 19% | 32% | 100% / 100% | 88.9 | 95.2 | 99.9 | 82.0 (c7) | 89.5 (c5) |
| Geometric median | rated | 35 | 17% | 74% | 94% / 97% | 90.8 | 90.3 | 99.9 | 84.3 (c22) | 79.5 (c5) |
| Soft Minimax-40 | hybrid | 35 | 11% | 49% | 97% / 100% | 89.5 | 94.4 | 99.9 | 82.9 (c7) | 89.4 (c5) |
| DMS | rated | 35 | 6% | 34% | 91% / 100% | 90.3 | 90.0 | 99.9 | 84.3 (c31) | 77.7 (c5) |
| Approval runoff | two-round | 33 | 3% | 3% | 48% / 94% | 87.5 | 94.2 | 99.6 | 82.3 (c7) | 82.5 (c5) |
| ESR | rated | 35 | 3% | 43% | 100% / 100% | 89.8 | 93.1 | 99.9 | 84.3 (c7) | 84.4 (c5) |
| Score | rated | 37 | 1% | 34% | 92% / 100% | 90.2 | 91.4 | 99.9 | 84.2 (c13) | 83.7 (c5) |
| BTR-IRV | condorcet-based | 33 | 0% | 17% | 79% / 97% | 85.5 | 97.9 | 97.7 | 80.0 (c7) | 91.9 (c13) |
| Copeland | condorcet | 33 | 0% | 11% | 73% / 97% | 85.1 | 97.7 | 97.7 | 79.4 (c7) | 92.9 (c13) |
| 321 | hybrid | 37 | 0% | 0% | 45% / 88% | 89.2 | 90.0 | 99.8 | 83.8 (c13) | 79.4 (c5) |
| MAV | median-based | 37 | 0% | 3% | 39% / 78% | 89.4 | 89.1 | 99.9 | 83.3 (c7) | 73.7 (c5) |
| Borda | positional | 35 | 0% | 3% | 16% / 50% | 86.2 | 93.1 | 97.3 | 77.6 (c13) | 78.3 (c36) |
| MJ | median-based | 37 | 0% | 0% | 15% / 74% | 89.0 | 89.9 | 99.9 | 83.3 (c7) | 77.4 (c5) |
| Approval | approval-based | 35 | 0% | 0% | 7% / 10% | 87.7 | 85.8 | 99.9 | 79.7 (c13) | 65.6 (c5) |
| Approval cond. runoff | two-round | 33 | 0% | 0% | 6% / 73% | 87.6 | 92.5 | 99.7 | 81.4 (c7) | 79.7 (c13) |
| IRV-3 | plurality-based | 33 | 0% | 0% | 3% / 6% | 85.6 | 88.8 | 98.6 | 78.6 (c13) | 72.0 (c5) |
| TTR | two-round | 33 | 0% | 0% | 3% / 6% | 84.5 | 88.8 | 96.5 | 78.0 (c13) | 70.6 (c5) |
| IRV | plurality-based | 35 | 0% | 0% | 0% / 6% | 86.1 | 89.6 | 97.3 | 80.4 (c5) | 73.0 (c5) |
| ER-IRV | plurality-based | 33 | 0% | 0% | 0% / 6% | 86.0 | 90.4 | 98.0 | 81.1 (c7) | 74.0 (c5) |
| Supplementary vote | plurality-based | 33 | 0% | 0% | 0% / 3% | 83.6 | 82.8 | 97.6 | 76.4 (c5) | 59.7 (c5) |
| Plurality | plurality-based | 35 | 0% | 0% | 0% / 0% | 77.6 | 73.5 | 97.9 | 62.3 (c13) | 48.4 (c5) |

**Run in fewer conditions** (shares over few conditions say little; not in the pair matrix):

| Method | Group | Conditions | Undominated | Pareto front | Near front (1 / 2) | Mean VSE\* | Mean WME\* | Mean sincerity | Worst VSE\* | Worst WME\* |
|---|---|---|---|---|---|---|---|---|---|---|
| Copeland//Margins | condorcet | 1 | 100% | 0% | 100% / 100% | 83.3 | 99.2 | 97.8 | 83.3 (c3) | 99.2 (c3) |
| Nanson | condorcet-based | 1 | 100% | 0% | 100% / 100% | 83.1 | 99.2 | 97.9 | 83.1 (c3) | 99.2 (c3) |
| Schulze-ER | condorcet | 1 | 100% | 0% | 100% / 100% | 83.1 | 99.1 | 98.3 | 83.1 (c3) | 99.1 (c3) |
| Minimax-ER | minimax-family | 1 | 100% | 0% | 100% / 100% | 83.0 | 99.2 | 98.3 | 83.0 (c3) | 99.2 (c3) |
| IC-MARS | hybrid | 3 | 33% | 0% | 100% / 100% | 87.2 | 96.0 | 99.9 | 86.2 (c18) | 94.4 (c3) |
| MMWVxScore | hybrid | 3 | 33% | 33% | 100% / 100% | 85.3 | 97.6 | 99.9 | 83.8 (c18) | 96.0 (c3) |
| MMWV+Score | hybrid | 3 | 33% | 67% | 100% / 100% | 85.2 | 97.7 | 99.9 | 83.7 (c18) | 96.3 (c3) |
| Soft Minimax-L | hybrid | 3 | 0% | 67% | 100% / 100% | 88.9 | 94.0 | 99.9 | 88.6 (c3) | 93.9 (c17) |
| INS | rated | 3 | 0% | 0% | 100% / 100% | 85.2 | 96.9 | 99.9 | 83.8 (c18) | 95.7 (c3) |
| Condorcet-random-drop | condorcet | 1 | 0% | 100% | 100% / 100% | 84.0 | 98.2 | 98.2 | 84.0 (c3) | 98.2 (c3) |
| SICT | condorcet | 1 | 0% | 0% | 100% / 100% | 83.6 | 97.4 | 98.2 | 83.6 (c3) | 97.4 (c3) |
| Benham | condorcet-based | 1 | 0% | 0% | 100% / 100% | 83.3 | 98.0 | 97.7 | 83.3 (c3) | 98.0 (c3) |
| Black | condorcet-based | 1 | 0% | 0% | 100% / 100% | 83.2 | 98.7 | 97.9 | 83.2 (c3) | 98.7 (c3) |
| Baldwin-ER | condorcet-based | 1 | 0% | 0% | 100% / 100% | 83.2 | 98.9 | 98.3 | 83.2 (c3) | 98.9 (c3) |
| Baldwin | condorcet-based | 1 | 0% | 0% | 100% / 100% | 83.1 | 99.0 | 97.9 | 83.1 (c3) | 99.0 (c3) |
| MMPOWV | minimax-family | 1 | 0% | 0% | 100% / 100% | 82.9 | 99.0 | 98.3 | 82.9 (c3) | 99.0 (c3) |
| BTR-Borda | condorcet-based | 1 | 0% | 0% | 100% / 100% | 82.9 | 98.8 | 97.9 | 82.9 (c3) | 98.8 (c3) |
| Coombs | plurality-based | 1 | 0% | 0% | 100% / 100% | 82.8 | 98.0 | 98.0 | 82.8 (c3) | 98.0 (c3) |
| Copeland//Margins-ER | condorcet | 1 | 0% | 0% | 100% / 100% | 82.7 | 98.5 | 98.2 | 82.7 (c3) | 98.5 (c3) |
| IDS | rated | 3 | 0% | 33% | 67% / 100% | 88.6 | 91.2 | 99.9 | 88.1 (c18) | 90.9 (c3) |
| Score+ | hybrid | 3 | 0% | 0% | 67% / 100% | 86.1 | 95.9 | 99.8 | 85.2 (c18) | 95.1 (c3) |
| ARA-Top | hybrid | 3 | 0% | 0% | 33% / 67% | 87.0 | 93.4 | 99.9 | 86.0 (c18) | 92.4 (c3) |
| Reciprocal score | rated | 3 | 0% | 0% | 0% / 67% | 88.2 | 89.4 | 99.6 | 87.6 (c18) | 88.7 (c18) |
| Dowdall | positional | 1 | 0% | 0% | 0% / 100% | 87.9 | 89.3 | 98.0 | 87.9 (c3) | 89.3 (c3) |
| MDDA | approval-based | 1 | 0% | 0% | 0% / 0% | 87.3 | 86.6 | 99.9 | 87.3 (c3) | 86.6 (c3) |
| MAMPO | approval-based | 1 | 0% | 0% | 0% / 0% | 87.0 | 86.8 | 99.9 | 87.0 (c3) | 86.8 (c3) |
| DSC | plurality-based | 1 | 0% | 0% | 0% / 0% | 86.3 | 85.7 | 98.2 | 86.3 (c3) | 85.7 (c3) |
| DAC | plurality-based | 1 | 0% | 0% | 0% / 0% | 86.1 | 88.2 | 98.4 | 86.1 (c3) | 88.2 (c3) |
| Borda-ER | positional | 1 | 0% | 0% | 0% / 100% | 84.6 | 94.8 | 98.0 | 84.6 (c3) | 94.8 (c3) |
| STLR | hybrid | 3 | 0% | 0% | 0% / 67% | 84.5 | 95.6 | 99.9 | 83.8 (c18) | 94.9 (c3) |
| MCA | median-based | 3 | 0% | 0% | 0% / 0% | 84.0 | 91.4 | 100.0 | 84.0 (c3) | 91.4 (c3) |

## Who dominates whom (the 12 leading methods)

Row dominates column in this many of the conditions both were run in (shared conditions after the slash).

| | Score runoff | Ranked pairs | Kemeny-Young | Schulze | Minimax | MMWV | Soft Minimax-D | Soft Minimax | BTR-score | MMPO | ARA | Smith//Score |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Score runoff** | — | 7.5/33 | 6.5/33 | 7.5/33 | 6.5/33 | 8.5/33 | 8.5/35 | 14/35 | 13.5/35 | 9/33 | 2.5/35 | 15/35 |
| **Ranked pairs** | 1/33 | — | 4/33 | 2/35 | 3.5/33 | 3/33 | 0/33 | 1/33 | 0/33 | 6/33 | 0/33 | 0/33 |
| **Kemeny-Young** | 1/33 | 0/33 | — | 1/33 | 1.5/33 | 3.5/33 | 0/33 | 1/33 | 0/33 | 5.5/33 | 0/33 | 0/33 |
| **Schulze** | 1/33 | 0/35 | 3/33 | — | 4/33 | 4.5/33 | 0/33 | 0/33 | 0/33 | 6/33 | 0/33 | 0/33 |
| **Minimax** | 1/33 | 1/33 | 2/33 | 0/33 | — | 3.5/33 | 0/33 | 0/33 | 0/33 | 4.5/33 | 0/33 | 0/33 |
| **MMWV** | 0/33 | 2/33 | 2/33 | 3/33 | 3/33 | — | 0/33 | 0/33 | 0/33 | 4/33 | 0/33 | 0/33 |
| **Soft Minimax-D** | 3/35 | 1/33 | 1/33 | 0/33 | 0/33 | 2/33 | — | 9/35 | 6/35 | 3/33 | 11/35 | 4/35 |
| **Soft Minimax** | 4/35 | 1/33 | 2/33 | 2/33 | 3/33 | 3/33 | 7/35 | — | 5/35 | 4/33 | 11/35 | 9/35 |
| **BTR-score** | 1/35 | 1/33 | 1/33 | 1/33 | 1/33 | 2/33 | 1/35 | 2/35 | — | 2/33 | 10/35 | 5/35 |
| **MMPO** | 0/33 | 1/33 | 4/33 | 3/33 | 3/33 | 2/33 | 0/33 | 0/33 | 0/33 | — | 0/33 | 0/33 |
| **ARA** | 0/35 | 0/33 | 1/33 | 0/33 | 0/33 | 1/33 | 2/35 | 1/35 | 0/35 | 2/33 | — | 3/35 |
| **Smith//Score** | 0/35 | 1/33 | 1/33 | 0/33 | 0/33 | 3/33 | 1/35 | 6/35 | 4/35 | 4/33 | 10/35 | — |

## Clear dominance (beatpath)

An arrow A → B: A dominates B in at least 50% of the conditions both were run in (--beat-share), and more often than B dominates A. Arrows implied by a chain of others are left out, so a method is dominated at this level by everything above it along the arrows. Methods of the main table only.

![clear dominance](beatpath.png)

Dominated by none at this level: Score runoff, Ranked pairs, Kemeny-Young, Schulze, Minimax, MMWV, Soft Minimax-D, Soft Minimax, BTR-score, MMPO, ARA, Smith//Score, MARS-T, Soft Minimax-40.

- **Score runoff** → Approval runoff (91%), BTR-IRV (76%), Copeland (79%)
- **Ranked pairs** → BTR-IRV (83%), Copeland (91%)
- **Kemeny-Young** → BTR-IRV (65%), Copeland (86%)
- **Schulze** → BTR-IRV (74%), Copeland (86%)
- **Minimax** → BTR-IRV (68%), Copeland (86%)
- **MMWV** → BTR-IRV (68%), Copeland (83%)
- **Soft Minimax-D** → Approval runoff (68%), Score (50%), 321 (77%)
- **Soft Minimax** → MARS (57%), STAR (63%)
- **BTR-score** → Approval runoff (59%), 321 (67%)
- **MMPO** → BTR-IRV (65%), Copeland (76%)
- **ARA** → Approval runoff (71%), ESR (63%)
- **Smith//Score** → Approval runoff (59%), Score (51%), 321 (80%)
- **MARS** → Approval runoff (59%), ESR (57%)
- **MARS-T** → Approval runoff (70%), ESR (54%)
- **STAR** → Score (59%), 321 (88%), Approval cond. runoff (86%)
- **Geometric median** → MAV (69%), MJ (83%)
- **Soft Minimax-40** → Approval runoff (73%), ESR (54%)
- **DMS** → MAV (54%), MJ (57%)
- **Approval runoff** → MAV (50%), Approval cond. runoff (74%)
- **ESR** → Geometric median (71%), Score (54%), 321 (83%), Approval cond. runoff (79%)
- **Score** → DMS (57%)
- **BTR-IRV** → Borda (86%)
- **Copeland** → Borda (77%)
- **321** → Approval (90%), IRV-3 (97%), IRV (94%), ER-IRV (86%)
- **MAV** → Approval (81%), IRV-3 (91%), IRV (86%), ER-IRV (74%)
- **Borda** → IRV-3 (52%), IRV (73%), ER-IRV (68%)
- **MJ** → Approval (90%), IRV-3 (97%), IRV (94%), ER-IRV (86%)
- **Approval** → Supplementary vote (97%)
- **Approval cond. runoff** → Borda (59%), MJ (64%)
- **IRV-3** → TTR (88%), Supplementary vote (97%)
- **IRV** → TTR (77%), Supplementary vote (100%)
- **ER-IRV** → TTR (83%), Supplementary vote (97%)
- **Supplementary vote** → Plurality (88%)

## Requested pairs

The pairs asked for with --pairs, in that order: how often each method is ahead, and where the other one is.

**Approval cond. runoff — TTR**

- Approval cond. runoff dominates TTR in 33 of 33 conditions.
- VSE\*: Approval cond. runoff clearly better than TTR in 26.5 of 33 conditions.
- WME\*: Approval cond. runoff clearly better than TTR in 27.5 of 33 conditions.

**Approval — Plurality**

- Approval dominates Plurality in 35 of 35 conditions.
- VSE\*: Approval clearly better than Plurality in 34 of 35 conditions.
- WME\*: Approval clearly better than Plurality in 35 of 35 conditions.

**ER-IRV — IRV**

- ER-IRV dominates IRV in 4 of 33 conditions; IRV dominates ER-IRV in 1: c14 (laziness 0.1).
- VSE\*: ER-IRV clearly better than IRV in 3 of 33 conditions; IRV better in 1: c14 (laziness 0.1).
- WME\*: ER-IRV clearly better than IRV in 2 of 33 conditions.

**BTR-score — IRV**

- BTR-score dominates IRV in 33 of 33 conditions.
- VSE\*: BTR-score clearly better than IRV in 17.5 of 33 conditions.
- WME\*: BTR-score clearly better than IRV in 32 of 33 conditions.

**Score+ — Score**

- VSE\*: Score clearly better than Score+ in 3 of 3 conditions.
- WME\*: Score+ clearly better than Score in 3 of 3 conditions.

**Approval cond. runoff — Approval**

- Approval cond. runoff dominates Approval in 29 of 33 conditions.
- VSE\*: Approval cond. runoff clearly better than Approval in 3 of 33 conditions; Approval better in 1: c2 (aggressive share 1.0).
- WME\*: Approval cond. runoff clearly better than Approval in 30 of 33 conditions.

**Approval runoff — Approval cond. runoff**

- Approval runoff dominates Approval cond. runoff in 24.5 of 33 conditions; Approval cond. runoff dominates Approval runoff in 1: c24 (poll rounds 1).
- VSE\*: Approval runoff clearly better than Approval cond. runoff in 1 of 33 conditions; Approval cond. runoff better in 1: c24 (poll rounds 1).
- WME\*: Approval runoff clearly better than Approval cond. runoff in 24.5 of 33 conditions.

**IRV-3 — TTR**

- IRV-3 dominates TTR in 29 of 33 conditions.
- VSE\*: IRV-3 clearly better than TTR in 7.5 of 33 conditions.
- WME\*: TTR clearly better than IRV-3 in 2 of 33 conditions; IRV-3 better in 1: c20 (electorate model field).

**IRV — TTR**

- IRV dominates TTR in 25.5 of 33 conditions; TTR dominates IRV in 1: c6 (candidate dispersion 2.0).
- VSE\*: IRV clearly better than TTR in 13.5 of 33 conditions.
- WME\*: IRV clearly better than TTR in 12 of 33 conditions.

**TTR — Supplementary vote**

- TTR dominates Supplementary vote in 15.5 of 33 conditions.
- VSE\*: TTR clearly better than Supplementary vote in 8 of 33 conditions.
- WME\*: TTR clearly better than Supplementary vote in 31.5 of 33 conditions.

**Supplementary vote — Plurality**

- Supplementary vote dominates Plurality in 29 of 33 conditions.
- VSE\*: Supplementary vote clearly better than Plurality in 32 of 33 conditions.
- WME\*: Supplementary vote clearly better than Plurality in 33 of 33 conditions.

**Smith//Score — BTR-score**

- BTR-score dominates Smith//Score in 5 of 35 conditions; Smith//Score dominates BTR-score in 4: c7 (candidates 4), c10 (candidates 8), c15 (laziness 0.5), c34 (voters 49).
- VSE\*: Smith//Score clearly better than BTR-score in 4 of 35 conditions; BTR-score better in 1: c38 (width 2.8).
- WME\*: BTR-score clearly better than Smith//Score in 5 of 35 conditions; Smith//Score better in 1: c7 (candidates 4).

**MARS-T — MARS**

- MARS dominates MARS-T in 5 of 35 conditions; MARS-T dominates MARS in 2: c1 (aggressive share 0.0), c14 (laziness 0.1).
- VSE\*: MARS-T clearly better than MARS in 2 of 35 conditions; MARS better in 1: c7 (candidates 4).
- WME\*: MARS clearly better than MARS-T in 4 of 35 conditions.

## Exceptions

Pairs of leading methods where each is ahead somewhere: how often each is ahead, and the conditions where the less usual outcome happens, with the settings they share. Pairs asked for with --pairs are above, not repeated here.

**Score runoff — Ranked pairs**

- Score runoff dominates Ranked pairs in 7.5 of 33 conditions; Ranked pairs dominates Score runoff in 1: c1 (aggressive share 0.0).
- VSE\*: Score runoff clearly better than Ranked pairs in 15.5 of 33 conditions.
- WME\*: Ranked pairs clearly better than Score runoff in 25.5 of 33 conditions; Score runoff better in 1: c15 (laziness 0.5).

**Score runoff — Kemeny-Young**

- Score runoff dominates Kemeny-Young in 6.5 of 33 conditions; Kemeny-Young dominates Score runoff in 1: c1 (aggressive share 0.0).
- VSE\*: Score runoff clearly better than Kemeny-Young in 19 of 33 conditions.
- WME\*: Kemeny-Young clearly better than Score runoff in 26.5 of 33 conditions; Score runoff better in 2: c15 (laziness 0.5), c31 (strategic share 1.0, adaptive share 0.0).

**Score runoff — Schulze**

- Score runoff dominates Schulze in 7.5 of 33 conditions; Schulze dominates Score runoff in 1: c1 (aggressive share 0.0).
- VSE\*: Score runoff clearly better than Schulze in 17 of 33 conditions.
- WME\*: Schulze clearly better than Score runoff in 25.5 of 33 conditions; Score runoff better in 1: c15 (laziness 0.5).

**Score runoff — Minimax**

- Score runoff dominates Minimax in 6.5 of 33 conditions; Minimax dominates Score runoff in 1: c1 (aggressive share 0.0).
- VSE\*: Score runoff clearly better than Minimax in 18 of 33 conditions.
- WME\*: Minimax clearly better than Score runoff in 26.5 of 33 conditions; Score runoff better in 1: c15 (laziness 0.5).

**Score runoff — MMWV**

- Score runoff dominates MMWV in 8.5 of 33 conditions.
- VSE\*: Score runoff clearly better than MMWV in 22 of 33 conditions.
- WME\*: MMWV clearly better than Score runoff in 24.5 of 33 conditions; Score runoff better in 2: c15 (laziness 0.5), c31 (strategic share 1.0, adaptive share 0.0).

**Score runoff — Soft Minimax-D**

- Score runoff dominates Soft Minimax-D in 8.5 of 35 conditions; Soft Minimax-D dominates Score runoff in 3: c17 (max score 9), c18 (max score 99), c22 (exits 1, clones 2).
- VSE\*: Soft Minimax-D clearly better than Score runoff in 22.5 of 35 conditions.
- WME\*: Score runoff clearly better than Soft Minimax-D in 30 of 35 conditions; Soft Minimax-D better in 2: c18 (max score 99), c22 (exits 1, clones 2).

**Score runoff — Soft Minimax**

- Score runoff dominates Soft Minimax in 14 of 35 conditions; Soft Minimax dominates Score runoff in 4: c22 (exits 1, clones 2), c30 (strategic share 0.5, adaptive share 0.0), c31 (strategic share 1.0, adaptive share 0.0), c37 (width 1.4).
- VSE\*: Soft Minimax clearly better than Score runoff in 15 of 35 conditions.
- WME\*: Score runoff clearly better than Soft Minimax in 26 of 35 conditions; Soft Minimax better in 1: c22 (exits 1, clones 2).

**Score runoff — BTR-score**

- Score runoff dominates BTR-score in 13.5 of 35 conditions; BTR-score dominates Score runoff in 1: c22 (exits 1, clones 2).
- VSE\*: BTR-score clearly better than Score runoff in 15.5 of 35 conditions; Score runoff better in 1: c18 (max score 99).
- WME\*: Score runoff clearly better than BTR-score in 29 of 35 conditions; BTR-score better in 2: c18 (max score 99), c22 (exits 1, clones 2).

**Score runoff — MMPO**

- Score runoff dominates MMPO in 9 of 33 conditions.
- VSE\*: Score runoff clearly better than MMPO in 23 of 33 conditions.
- WME\*: MMPO clearly better than Score runoff in 24 of 33 conditions; Score runoff better in 1: c15 (laziness 0.5).

**Score runoff — Smith//Score**

- Score runoff dominates Smith//Score in 15 of 35 conditions.
- VSE\*: Smith//Score clearly better than Score runoff in 15.5 of 35 conditions; Score runoff better in 1: c18 (max score 99).
- WME\*: Score runoff clearly better than Smith//Score in 30.5 of 35 conditions; Smith//Score better in 1: c18 (max score 99).

**Ranked pairs — Minimax**

- Ranked pairs dominates Minimax in 3.5 of 33 conditions; Minimax dominates Ranked pairs in 1: c38 (width 2.8).
- VSE\*: Minimax clearly better than Ranked pairs in 2 of 33 conditions; Ranked pairs better in 1.5: c3 (defaults (replicate: 2000 elections, seed 1); counts 0.5), c22 (exits 1, clones 2).
- WME\*: Ranked pairs clearly better than Minimax in 3 of 33 conditions.

**Ranked pairs — MMWV**

- Ranked pairs dominates MMWV in 3 of 33 conditions; MMWV dominates Ranked pairs in 2: c30 (strategic share 0.5, adaptive share 0.0), c31 (strategic share 1.0, adaptive share 0.0), all with adaptive share 0.0.
- VSE\*: Ranked pairs clearly better than MMWV in 1 of 33 conditions.
- WME\*: Ranked pairs clearly better than MMWV in 2 of 33 conditions.

**Ranked pairs — Soft Minimax**

- Ranked pairs dominates Soft Minimax in 1 of 33 conditions; Soft Minimax dominates Ranked pairs in 1: c31 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax clearly better than Ranked pairs in 26 of 33 conditions.
- WME\*: Ranked pairs clearly better than Soft Minimax in 32 of 33 conditions.

**Ranked pairs — MMPO**

- Ranked pairs dominates MMPO in 6 of 33 conditions; MMPO dominates Ranked pairs in 1: c31 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Ranked pairs clearly better than MMPO in 3 of 33 conditions.
- WME\*: Ranked pairs clearly better than MMPO in 4 of 33 conditions.

**Kemeny-Young — Schulze**

- Schulze dominates Kemeny-Young in 3 of 33 conditions; Kemeny-Young dominates Schulze in 1: c7 (candidates 4).
- VSE\*: Kemeny-Young clearly better than Schulze in 1 of 33 conditions; Schulze better in 1: c30 (strategic share 0.5, adaptive share 0.0).
- WME\*: Schulze clearly better than Kemeny-Young in 2 of 33 conditions.

**Kemeny-Young — Minimax**

- Minimax dominates Kemeny-Young in 2 of 33 conditions; Kemeny-Young dominates Minimax in 1.5: c3 (defaults (replicate: 2000 elections, seed 1); counts 0.5), c9 (candidates 7).
- VSE\*: Kemeny-Young clearly better than Minimax in 0.5 of 33 conditions.
- WME\*: Minimax clearly better than Kemeny-Young in 2 of 33 conditions; Kemeny-Young better in 1: c9 (candidates 7).

**Kemeny-Young — MMWV**

- Kemeny-Young dominates MMWV in 3.5 of 33 conditions; MMWV dominates Kemeny-Young in 2: c30 (strategic share 0.5, adaptive share 0.0), c31 (strategic share 1.0, adaptive share 0.0), all with adaptive share 0.0.
- VSE\*: Kemeny-Young clearly better than MMWV in 2 of 33 conditions.
- WME\*: Kemeny-Young clearly better than MMWV in 2.5 of 33 conditions.

**Kemeny-Young — Soft Minimax**

- Soft Minimax dominates Kemeny-Young in 2 of 33 conditions; Kemeny-Young dominates Soft Minimax in 1: c1 (aggressive share 0.0).
- VSE\*: Soft Minimax clearly better than Kemeny-Young in 26 of 33 conditions.
- WME\*: Kemeny-Young clearly better than Soft Minimax in 31 of 33 conditions; Soft Minimax better in 1: c31 (strategic share 1.0, adaptive share 0.0).

**Kemeny-Young — MMPO**

- Kemeny-Young dominates MMPO in 5.5 of 33 conditions; MMPO dominates Kemeny-Young in 4: c19 (electorate model clusters), c23 (samples 5, poll size 9), c30 (strategic share 0.5, adaptive share 0.0), c31 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Kemeny-Young clearly better than MMPO in 4 of 33 conditions; MMPO better in 1: c19 (electorate model clusters).
- WME\*: Kemeny-Young clearly better than MMPO in 2.5 of 33 conditions; MMPO better in 1: c23 (samples 5, poll size 9).

**Schulze — MMWV**

- Schulze dominates MMWV in 4.5 of 33 conditions; MMWV dominates Schulze in 3: c13 (candidate dispersion 0.5, laziness 0.1, exits 1, clones 2, poll size 9), c30 (strategic share 0.5, adaptive share 0.0), c31 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Schulze clearly better than MMWV in 2 of 33 conditions.
- WME\*: Schulze clearly better than MMWV in 2.5 of 33 conditions; MMWV better in 1: c13 (candidate dispersion 0.5, laziness 0.1, exits 1, clones 2, poll size 9).

**Schulze — MMPO**

- Schulze dominates MMPO in 6 of 33 conditions; MMPO dominates Schulze in 3: c23 (samples 5, poll size 9), c30 (strategic share 0.5, adaptive share 0.0), c31 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Schulze clearly better than MMPO in 4 of 33 conditions.
- WME\*: Schulze clearly better than MMPO in 3 of 33 conditions; MMPO better in 1: c23 (samples 5, poll size 9).

**Minimax — MMWV**

- Minimax dominates MMWV in 3.5 of 33 conditions; MMWV dominates Minimax in 3: c13 (candidate dispersion 0.5, laziness 0.1, exits 1, clones 2, poll size 9), c30 (strategic share 0.5, adaptive share 0.0), c31 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Minimax clearly better than MMWV in 2 of 33 conditions.
- WME\*: Minimax clearly better than MMWV in 2.5 of 33 conditions; MMWV better in 1: c13 (candidate dispersion 0.5, laziness 0.1, exits 1, clones 2, poll size 9).

**Minimax — MMPO**

- Minimax dominates MMPO in 4.5 of 33 conditions; MMPO dominates Minimax in 3: c23 (samples 5, poll size 9), c30 (strategic share 0.5, adaptive share 0.0), c31 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Minimax clearly better than MMPO in 3 of 33 conditions.
- WME\*: Minimax clearly better than MMPO in 2.5 of 33 conditions; MMPO better in 1: c23 (samples 5, poll size 9).

**MMWV — Soft Minimax**

- Soft Minimax dominates MMWV in 3 of 33 conditions.
- VSE\*: Soft Minimax clearly better than MMWV in 29 of 33 conditions.
- WME\*: MMWV clearly better than Soft Minimax in 30 of 33 conditions; Soft Minimax better in 1: c31 (strategic share 1.0, adaptive share 0.0).

**MMWV — MMPO**

- MMWV dominates MMPO in 4 of 33 conditions; MMPO dominates MMWV in 2: c19 (electorate model clusters), c34 (voters 49).
- VSE\*: MMWV clearly better than MMPO in 4 of 33 conditions; MMPO better in 2: c19 (electorate model clusters), c34 (voters 49).
- WME\*: MMWV clearly better than MMPO in 1 of 33 conditions.

**Soft Minimax-D — Soft Minimax**

- Soft Minimax-D dominates Soft Minimax in 9 of 35 conditions; Soft Minimax dominates Soft Minimax-D in 7: c9 (candidates 7), c15 (laziness 0.5), c24 (poll rounds 1), c27 (poll rounds 5), c29 (samples 5), c30 (strategic share 0.5, adaptive share 0.0), c31 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than Soft Minimax in 10 of 35 conditions; Soft Minimax better in 1: c18 (max score 99).
- WME\*: Soft Minimax clearly better than Soft Minimax-D in 9 of 35 conditions; Soft Minimax-D better in 2: c17 (max score 9), c18 (max score 99).

**Soft Minimax-D — BTR-score**

- Soft Minimax-D dominates BTR-score in 6 of 35 conditions; BTR-score dominates Soft Minimax-D in 1: c31 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than BTR-score in 6 of 35 conditions.
- WME\*: BTR-score clearly better than Soft Minimax-D in 1 of 35 conditions.

**Soft Minimax-D — ARA**

- Soft Minimax-D dominates ARA in 11 of 35 conditions; ARA dominates Soft Minimax-D in 2: c11 (candidates 9), c22 (exits 1, clones 2).
- VSE\*: ARA clearly better than Soft Minimax-D in 17.5 of 35 conditions; Soft Minimax-D better in 1: c38 (width 2.8).
- WME\*: Soft Minimax-D clearly better than ARA in 26.5 of 35 conditions.

**Soft Minimax-D — Smith//Score**

- Soft Minimax-D dominates Smith//Score in 4 of 35 conditions; Smith//Score dominates Soft Minimax-D in 1: c7 (candidates 4).
- VSE\*: Soft Minimax-D clearly better than Smith//Score in 4 of 35 conditions; Smith//Score better in 1: c7 (candidates 4).

**Soft Minimax — BTR-score**

- Soft Minimax dominates BTR-score in 5 of 35 conditions; BTR-score dominates Soft Minimax in 2: c12 (candidates 10), c38 (width 2.8).
- VSE\*: Soft Minimax clearly better than BTR-score in 2 of 35 conditions; BTR-score better in 2: c12 (candidates 10), c38 (width 2.8).
- WME\*: Soft Minimax clearly better than BTR-score in 5 of 35 conditions; BTR-score better in 2: c17 (max score 9), c18 (max score 99).

**Soft Minimax — MMPO**

- Soft Minimax dominates MMPO in 4 of 33 conditions.
- VSE\*: Soft Minimax clearly better than MMPO in 28 of 33 conditions.
- WME\*: MMPO clearly better than Soft Minimax in 29 of 33 conditions; Soft Minimax better in 1: c31 (strategic share 1.0, adaptive share 0.0).

**Soft Minimax — ARA**

- Soft Minimax dominates ARA in 11 of 35 conditions; ARA dominates Soft Minimax in 1: c6 (candidate dispersion 2.0).
- VSE\*: ARA clearly better than Soft Minimax in 23 of 35 conditions.
- WME\*: Soft Minimax clearly better than ARA in 33 of 35 conditions.

**Soft Minimax — Smith//Score**

- Soft Minimax dominates Smith//Score in 9 of 35 conditions; Smith//Score dominates Soft Minimax in 6: c1 (aggressive share 0.0), c2 (aggressive share 1.0), c10 (candidates 8), c11 (candidates 9), c20 (electorate model field), c34 (voters 49).
- VSE\*: Smith//Score clearly better than Soft Minimax in 6 of 35 conditions; Soft Minimax better in 1: c18 (max score 99).
- WME\*: Soft Minimax clearly better than Smith//Score in 9 of 35 conditions; Smith//Score better in 1: c18 (max score 99).

**ARA — Smith//Score**

- Smith//Score dominates ARA in 10 of 35 conditions; ARA dominates Smith//Score in 3: c6 (candidate dispersion 2.0), c11 (candidates 9), c22 (exits 1, clones 2).
- VSE\*: ARA clearly better than Smith//Score in 19.5 of 35 conditions.
- WME\*: Smith//Score clearly better than ARA in 26.5 of 35 conditions.

