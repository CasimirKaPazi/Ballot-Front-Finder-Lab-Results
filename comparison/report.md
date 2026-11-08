# Ballot Front Finder Lab — comparison across runs

Folder: ../bffl-runs. 24 run or sweep folder(s), 43 results file(s); simulator version 2.1.0; analysis version 2.1.0. 44 distinct condition(s); 71 methods. Bootstrap: 1000 paired replicates per condition.

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

## Methods

Methods run in at least 22 of the 44 conditions, sorted by how often undominated, then near the front. Means are weighted by condition; worst: the lowest value over the conditions, with the condition.

| Method                | Group           | Conditions | Undominated | Pareto front | Near front (1 / 2) | Mean VSE\* | Mean WME\* | Mean sincerity | Worst VSE\* | Worst WME\* |
| --------------------- | --------------- | ---------- | ----------- | ------------ | ------------------ | ---------- | ---------- | -------------- | ----------- | ----------- |
| Schulze               | condorcet       | 43         | 72%         | 49%          | 95% / 95%          | 84.3       | 98.5       | 99.5           | 54.4 (c7)   | 91.1 (c4)   |
| Soft Minimax-D        | hybrid          | 42         | 69%         | 86%          | 98% / 100%         | 85.6       | 97.3       | 100.0          | 55.5 (c7)   | 94.5 (c40)  |
| Kemeny-Young          | condorcet       | 41         | 68%         | 44%          | 93% / 95%          | 83.6       | 98.5       | 99.6           | 54.0 (c7)   | 89.8 (c5)   |
| MMWV                  | minimax-family  | 41         | 68%         | 29%          | 93% / 95%          | 83.5       | 98.4       | 99.6           | 54.3 (c7)   | 89.2 (c4)   |
| Minimax               | minimax-family  | 41         | 66%         | 34%          | 93% / 95%          | 83.7       | 98.5       | 99.5           | 54.3 (c7)   | 90.8 (c4)   |
| Ranked pairs          | condorcet       | 43         | 65%         | 44%          | 91% / 95%          | 84.3       | 98.4       | 99.5           | 54.0 (c7)   | 90.8 (c4)   |
| Copeland              | condorcet       | 41         | 54%         | 34%          | 95% / 98%          | 83.9       | 98.7       | 99.6           | 53.4 (c7)   | 94.0 (c17)  |
| MMPO                  | minimax-family  | 41         | 51%         | 20%          | 93% / 95%          | 83.5       | 98.4       | 99.6           | 54.2 (c7)   | 88.5 (c4)   |
| BTR-score             | hybrid          | 42         | 50%         | 55%          | 93% / 95%          | 85.2       | 97.2       | 100.0          | 55.0 (c7)   | 93.4 (c18)  |
| Soft Minimax          | hybrid          | 42         | 36%         | 74%          | 98% / 100%         | 85.1       | 97.5       | 100.0          | 54.9 (c7)   | 94.7 (c18)  |
| Smith//Score          | condorcet-based | 42         | 33%         | 29%          | 98% / 100%         | 85.2       | 97.2       | 100.0          | 55.0 (c7)   | 93.8 (c18)  |
| Soft Minimax-40       | hybrid          | 42         | 31%         | 83%          | 98% / 100%         | 86.1       | 96.2       | 100.0          | 55.6 (c7)   | 92.2 (c40)  |
| Score runoff          | two-round       | 42         | 26%         | 62%          | 100% / 100%        | 86.4       | 95.7       | 99.9           | 59.8 (c7)   | 92.0 (c18)  |
| ARA                   | hybrid          | 42         | 10%         | 29%          | 95% / 98%          | 86.0       | 95.6       | 100.0          | 56.8 (c7)   | 90.1 (c18)  |
| MARS                  | hybrid          | 42         | 7%          | 43%          | 90% / 98%          | 85.7       | 96.3       | 100.0          | 57.4 (c7)   | 91.6 (c38)  |
| Approval runoff       | two-round       | 41         | 5%          | 10%          | 66% / 93%          | 85.3       | 93.1       | 99.9           | 53.1 (c18)  | 79.8 (c6)   |
| MARS-T                | hybrid          | 42         | 5%          | 21%          | 88% / 95%          | 85.7       | 96.1       | 100.0          | 57.4 (c7)   | 91.0 (c38)  |
| BTR-IRV               | condorcet-based | 41         | 2%          | 10%          | 80% / 93%          | 83.4       | 97.8       | 99.3           | 54.0 (c7)   | 91.0 (c18)  |
| ESR-MARS              | hybrid          | 42         | 2%          | 26%          | 86% / 93%          | 85.9       | 94.6       | 100.0          | 57.9 (c7)   | 88.9 (c38)  |
| STAR                  | hybrid          | 44         | 2%          | 32%          | 86% / 93%          | 86.4       | 94.6       | 99.9           | 59.6 (c7)   | 81.0 (c43)  |
| MJ                    | median-based    | 44         | 2%          | 5%           | 18% / 82%          | 85.5       | 92.7       | 100.0          | 55.2 (c7)   | 80.4 (c43)  |
| IMDS                  | rated           | 42         | 0%          | 7%           | 90% / 100%         | 84.9       | 96.9       | 100.0          | 54.6 (c7)   | 92.6 (c18)  |
| Delegated 321         | delegated       | 41         | 0%          | 0%           | 76% / 98%          | 85.9       | 92.8       | nan            | 56.8 (c7)   | 86.3 (c18)  |
| 321                   | hybrid          | 43         | 0%          | 5%           | 70% / 93%          | 86.0       | 92.0       | 99.9           | 56.8 (c7)   | 74.0 (c43)  |
| ESR                   | rated           | 42         | 0%          | 0%           | 67% / 93%          | 85.5       | 94.5       | 100.0          | 56.6 (c7)   | 89.1 (c6)   |
| DMS                   | rated           | 42         | 0%          | 5%           | 52% / 81%          | 85.4       | 91.7       | 100.0          | 57.6 (c18)  | 83.9 (c38)  |
| MAV                   | median-based    | 44         | 0%          | 2%           | 48% / 80%          | 85.6       | 92.1       | 100.0          | 55.2 (c7)   | 79.2 (c43)  |
| Approval cond. runoff | two-round       | 41         | 0%          | 0%           | 46% / 80%          | 84.7       | 92.2       | 99.9           | 52.7 (c18)  | 79.3 (c18)  |
| Score                 | rated           | 44         | 0%          | 2%           | 41% / 86%          | 85.6       | 91.7       | 100.0          | 59.1 (c7)   | 76.4 (c43)  |
| ER-IRV                | plurality-based | 41         | 0%          | 2%           | 10% / 61%          | 84.1       | 93.7       | 99.2           | 54.6 (c7)   | 78.1 (c6)   |
| TTR                   | two-round       | 41         | 0%          | 0%           | 5% / 20%           | 82.9       | 92.6       | 97.9           | 52.9 (c18)  | 75.4 (c18)  |
| Approval              | approval-based  | 43         | 0%          | 2%           | 5% / 58%           | 84.0       | 88.8       | 100.0          | 43.3 (c18)  | 69.6 (c18)  |
| Plurality             | plurality-based | 43         | 0%          | 2%           | 5% / 5%            | 73.1       | 73.6       | 98.7           | 18.2 (c6)   | 40.5 (c18)  |
| Asset                 | delegated       | 41         | 0%          | 2%           | 2% / 7%            | 80.5       | 84.0       | 98.0           | 52.1 (c7)   | 59.2 (c18)  |
| Borda                 | positional      | 43         | 0%          | 2%           | 2% / 12%           | 81.7       | 89.8       | 98.6           | 54.7 (c7)   | 64.9 (c43)  |
| Supplementary vote    | plurality-based | 41         | 0%          | 0%           | 0% / 0%            | 81.3       | 85.6       | 99.0           | 40.3 (c18)  | 58.7 (c18)  |
| IRV                   | plurality-based | 43         | 0%          | 0%           | 0% / 9%            | 81.0       | 89.0       | 98.4           | 52.2 (c6)   | 64.9 (c43)  |
| IRV-3                 | plurality-based | 41         | 0%          | 0%           | 0% / 7%            | 80.9       | 88.6       | 98.6           | 50.7 (c18)  | 69.4 (c6)   |

**Run in fewer conditions** (shares over few conditions say little; not in the pair matrix):

| Method | Group | Conditions | Undominated | Pareto front | Near front (1 / 2) | Mean VSE\* | Mean WME\* | Mean sincerity | Worst VSE\* | Worst WME\* |
|---|---|---|---|---|---|---|---|---|---|---|
| MMWVxScore | hybrid | 2 | 100% | 100% | 100% / 100% | 85.7 | 98.6 | 100.0 | 85.2 (c24) | 97.7 (c1) |
| Schulze-ER | condorcet | 1 | 100% | 0% | 100% / 100% | 84.4 | 99.3 | 99.9 | 84.4 (c1) | 99.3 (c1) |
| Minimax-ER | minimax-family | 1 | 100% | 0% | 100% / 100% | 84.3 | 99.3 | 99.9 | 84.3 (c1) | 99.3 (c1) |
| MMWV+Score | hybrid | 2 | 50% | 100% | 100% / 100% | 85.6 | 98.6 | 100.0 | 85.2 (c24) | 97.8 (c1) |
| Smith//Approval | condorcet-based | 2 | 50% | 0% | 100% / 100% | 85.6 | 98.5 | 100.0 | 84.9 (c24) | 97.6 (c1) |
| Soft Minimax-L | hybrid | 2 | 0% | 50% | 100% / 100% | 87.7 | 95.9 | 100.0 | 87.7 (c1) | 95.8 (c1) |
| IC-MARS | hybrid | 2 | 0% | 50% | 100% / 100% | 86.7 | 96.7 | 100.0 | 86.3 (c24) | 95.3 (c1) |
| ARA-C | hybrid | 2 | 0% | 0% | 100% / 100% | 86.3 | 96.6 | 100.0 | 86.0 (c24) | 95.6 (c1) |
| Score+ | hybrid | 2 | 0% | 0% | 100% / 100% | 86.0 | 96.8 | 100.0 | 85.8 (c24) | 96.3 (c1) |
| INS | rated | 2 | 0% | 0% | 100% / 100% | 85.4 | 98.0 | 100.0 | 84.9 (c24) | 97.3 (c1) |
| Black | condorcet-based | 1 | 0% | 100% | 100% / 100% | 84.6 | 98.8 | 99.7 | 84.6 (c1) | 98.8 (c1) |
| MMPOWV | minimax-family | 1 | 0% | 100% | 100% / 100% | 84.5 | 99.2 | 99.9 | 84.5 (c1) | 99.2 (c1) |
| Copeland//Margins | condorcet | 1 | 0% | 0% | 100% / 100% | 84.3 | 99.1 | 99.8 | 84.3 (c1) | 99.1 (c1) |
| Copeland//Margins-ER | condorcet | 1 | 0% | 0% | 100% / 100% | 84.3 | 99.1 | 99.8 | 84.3 (c1) | 99.1 (c1) |
| BTR-Borda | condorcet-based | 1 | 0% | 0% | 100% / 100% | 84.1 | 98.9 | 99.7 | 84.1 (c1) | 98.9 (c1) |
| Benham | condorcet-based | 1 | 0% | 0% | 100% / 100% | 84.1 | 98.3 | 99.7 | 84.1 (c1) | 98.3 (c1) |
| Nanson | condorcet-based | 1 | 0% | 0% | 100% / 100% | 84.1 | 99.2 | 99.9 | 84.1 (c1) | 99.2 (c1) |
| SICT | condorcet | 1 | 0% | 0% | 100% / 100% | 83.9 | 98.5 | 99.8 | 83.9 (c1) | 98.5 (c1) |
| Baldwin-ER | condorcet-based | 1 | 0% | 0% | 100% / 100% | 83.8 | 98.9 | 99.9 | 83.8 (c1) | 98.9 (c1) |
| Baldwin | condorcet-based | 1 | 0% | 0% | 100% / 100% | 83.8 | 98.8 | 99.9 | 83.8 (c1) | 98.8 (c1) |
| STLR | hybrid | 2 | 0% | 0% | 50% / 100% | 86.7 | 94.0 | 100.0 | 86.4 (c24) | 94.0 (c24) |
| IDS | rated | 2 | 0% | 0% | 0% / 100% | 86.5 | 93.4 | 100.0 | 86.4 (c1) | 93.3 (c24) |
| MDDA | approval-based | 1 | 0% | 0% | 0% / 100% | 86.2 | 90.5 | 100.0 | 86.2 (c1) | 90.5 (c1) |
| MAMPO | approval-based | 1 | 0% | 0% | 0% / 100% | 86.0 | 90.7 | 100.0 | 86.0 (c1) | 90.7 (c1) |
| DAC | plurality-based | 1 | 0% | 0% | 0% / 0% | 85.7 | 90.8 | 99.6 | 85.7 (c1) | 90.8 (c1) |
| DSC | plurality-based | 1 | 0% | 0% | 0% / 0% | 85.6 | 91.1 | 99.5 | 85.6 (c1) | 91.1 (c1) |
| ARA-Top | hybrid | 2 | 0% | 0% | 0% / 100% | 85.6 | 94.8 | 100.0 | 85.0 (c24) | 94.2 (c1) |
| Reciprocal score | rated | 2 | 0% | 0% | 0% / 50% | 85.6 | 92.4 | 99.9 | 85.3 (c24) | 91.9 (c24) |
| MCA | median-based | 1 | 0% | 0% | 0% / 0% | 84.9 | 91.0 | 100.0 | 84.9 (c1) | 91.0 (c1) |
| Dowdall | positional | 1 | 0% | 0% | 0% / 0% | 84.3 | 89.8 | 99.4 | 84.3 (c1) | 89.8 (c1) |
| Coombs | plurality-based | 1 | 0% | 0% | 0% / 100% | 83.1 | 96.5 | 99.3 | 83.1 (c1) | 96.5 (c1) |
| Borda-ER | positional | 1 | 0% | 0% | 0% / 0% | 81.9 | 91.0 | 99.2 | 81.9 (c1) | 91.0 (c1) |
| Asset-foresight | delegated | 1 | 0% | 0% | 0% / 0% | 80.6 | 87.3 | 99.3 | 80.6 (c1) | 87.3 (c1) |

## Who dominates whom (the 12 leading methods)

Row dominates column in this many of the conditions both were run in (shared conditions after the slash).

| | Schulze | Soft Minimax-D | Kemeny-Young | MMWV | Minimax | Ranked pairs | Copeland | MMPO | BTR-score | Soft Minimax | Smith//Score | Soft Minimax-40 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Schulze** | — | 3/41 | 2/41 | 5/41 | 2/41 | 7/43 | 12/41 | 6/41 | 6/41 | 6/41 | 6/41 | 2/41 |
| **Soft Minimax-D** | 6/41 | — | 5/41 | 4/41 | 5/41 | 6/41 | 2/41 | 5/41 | 13/42 | 16/42 | 11/42 | 23/42 |
| **Kemeny-Young** | 2/41 | 2/41 | — | 5/41 | 4/41 | 1/41 | 9/41 | 5/41 | 6/41 | 5/41 | 6/41 | 1/41 |
| **MMWV** | 1/41 | 2/41 | 1/41 | — | 1/41 | 4/41 | 10/41 | 4/41 | 5/41 | 6/41 | 4/41 | 2/41 |
| **Minimax** | 1/41 | 3/41 | 3/41 | 6/41 | — | 5/41 | 11/41 | 10/41 | 6/41 | 7/41 | 7/41 | 2/41 |
| **Ranked pairs** | 3/43 | 3/41 | 6/41 | 3/41 | 4/41 | — | 11/41 | 8/41 | 6/41 | 7/41 | 5/41 | 1/41 |
| **Copeland** | 2/41 | 3/41 | 2/41 | 3/41 | 2/41 | 3/41 | — | 6/41 | 5/41 | 5/41 | 4/41 | 2/41 |
| **MMPO** | 3/41 | 1/41 | 1/41 | 0/41 | 1/41 | 4/41 | 7/41 | — | 4/41 | 6/41 | 4/41 | 2/41 |
| **BTR-score** | 3/41 | 2/42 | 3/41 | 3/41 | 4/41 | 3/41 | 2/41 | 4/41 | — | 5/42 | 6/42 | 10/42 |
| **Soft Minimax** | 5/41 | 7/42 | 6/41 | 6/41 | 5/41 | 6/41 | 6/41 | 7/41 | 11/42 | — | 14/42 | 8/42 |
| **Smith//Score** | 3/41 | 3/42 | 4/41 | 3/41 | 4/41 | 4/41 | 2/41 | 4/41 | 3/42 | 3/42 | — | 9/42 |
| **Soft Minimax-40** | 3/41 | 1/42 | 3/41 | 3/41 | 3/41 | 3/41 | 2/41 | 3/41 | 5/42 | 0/42 | 3/42 | — |

## Clear dominance (beatpath)

An arrow A → B: A dominates B in at least 50% of the conditions both were run in (--beat-share), and more often than B dominates A. Arrows implied by a chain of others are left out, so a method is dominated at this level by everything above it along the arrows. Methods of the main table only.

![clear dominance](beatpath.png)

Dominated by none at this level: Schulze, Soft Minimax-D, Kemeny-Young, MMWV, Minimax, Ranked pairs, Copeland, MMPO, BTR-score, Soft Minimax, Smith//Score.

- **Schulze** → BTR-IRV (90%)
- **Soft Minimax-D** → Soft Minimax-40 (55%), Score runoff (57%), ARA (81%), MARS (83%), MARS-T (88%), IMDS (71%)
- **Kemeny-Young** → BTR-IRV (90%)
- **MMWV** → BTR-IRV (90%)
- **Minimax** → BTR-IRV (90%)
- **Ranked pairs** → BTR-IRV (88%)
- **Copeland** → BTR-IRV (78%)
- **MMPO** → BTR-IRV (90%)
- **BTR-score** → ARA (50%), MARS (69%), MARS-T (69%), IMDS (52%)
- **Soft Minimax** → IMDS (69%)
- **Smith//Score** → MARS (64%), MARS-T (62%)
- **Soft Minimax-40** → ESR-MARS (81%), STAR (81%)
- **Score runoff** → ESR-MARS (69%), ESR (79%)
- **ARA** → ESR-MARS (62%), ESR (71%)
- **MARS** → ESR-MARS (79%), STAR (57%)
- **Approval runoff** → Approval cond. runoff (61%)
- **MARS-T** → ESR-MARS (74%), ESR (79%)
- **BTR-IRV** → ER-IRV (56%)
- **ESR-MARS** → Approval runoff (68%), MJ (74%), Delegated 321 (63%), 321 (68%), MAV (79%), Score (86%), ER-IRV (80%)
- **STAR** → ESR (64%)
- **MJ** → DMS (50%)
- **IMDS** → ESR (52%)
- **Delegated 321** → DMS (51%)
- **321** → TTR (85%), Approval (88%)
- **ESR** → Approval runoff (68%), MJ (62%), Delegated 321 (61%), 321 (66%), MAV (83%), Score (86%), ER-IRV (66%)
- **DMS** → TTR (61%), Approval (68%)
- **MAV** → TTR (83%), Approval (91%)
- **Approval cond. runoff** → TTR (59%), Approval (90%)
- **Score** → TTR (63%), Approval (72%)
- **ER-IRV** → TTR (73%), Approval (80%)
- **TTR** → Borda (54%), IRV (61%)
- **Approval** → Borda (56%), IRV (51%)
- **Asset** → Plurality (85%)
- **Borda** → Supplementary vote (61%)
- **Supplementary vote** → Asset (56%)
- **IRV** → IRV-3 (73%)
- **IRV-3** → Supplementary vote (68%)

## Map of the methods

How alike the methods are in what they do: the distance between two methods is the share of elections in which they elect different winners, averaged over the conditions (each counting once). The map lays these distances out in two dimensions (classical multidimensional scaling) with honest (open) and learned ballots (filled) in one layout, an arrow from each method's honest to its strategic behaviour. The two dimensions show 38% of the spread; methods close together elect the same winner in most elections, whatever their family. Coordinates in method-map.csv.

![map of the methods](method-map.png)

## Requested pairs

The pairs asked for with --pairs, in that order: how often each method is ahead, and where the other one is.

**Approval — Plurality**

- Approval dominates Plurality in 42 of 43 conditions.
- VSE\*: Approval clearly better than Plurality in 41 of 43 conditions; Plurality better in 1: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic).
- WME\*: Approval clearly better than Plurality in 43 of 43 conditions.

**Supplementary vote — Plurality**

- Supplementary vote dominates Plurality in 39 of 41 conditions.
- VSE\*: Supplementary vote clearly better than Plurality in 39 of 41 conditions; Plurality better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- WME\*: Supplementary vote clearly better than Plurality in 39 of 41 conditions; Plurality better in 1: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic).

**TTR — Supplementary vote**

- TTR dominates Supplementary vote in 34 of 41 conditions.
- VSE\*: TTR clearly better than Supplementary vote in 14 of 41 conditions; Supplementary vote better in 1: c38 (strategic share 1.0, adaptive share 0.0).
- WME\*: TTR clearly better than Supplementary vote in 41 of 41 conditions.

**IRV — TTR**

- TTR dominates IRV in 25 of 41 conditions; IRV dominates TTR in 4: c12 (candidates 10), c13 (coordination share 0.0), c20 (honesty cost 1.0), c36 (samples 20).
- VSE\*: TTR clearly better than IRV in 16 of 41 conditions; IRV better in 2: c12 (candidates 10), c13 (coordination share 0.0).
- WME\*: TTR clearly better than IRV in 26 of 41 conditions; IRV better in 3: c13 (coordination share 0.0), c20 (honesty cost 1.0), c36 (samples 20).

**IRV-3 — TTR**

- TTR dominates IRV-3 in 32 of 41 conditions; IRV-3 dominates TTR in 1: c13 (coordination share 0.0).
- VSE\*: TTR clearly better than IRV-3 in 18 of 41 conditions; IRV-3 better in 1: c13 (coordination share 0.0).
- WME\*: TTR clearly better than IRV-3 in 37 of 41 conditions.

**Approval cond. runoff — TTR**

- Approval cond. runoff dominates TTR in 24 of 41 conditions; TTR dominates Approval cond. runoff in 1: c9 (candidate shift 1.0).
- VSE\*: Approval cond. runoff clearly better than TTR in 28 of 41 conditions.
- WME\*: TTR clearly better than Approval cond. runoff in 13 of 41 conditions; Approval cond. runoff better in 4: c12 (candidates 10), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c26 (electorate model field), c28 (exits 0, clones 0).

**Approval cond. runoff — Approval**

- Approval cond. runoff dominates Approval in 37 of 41 conditions; Approval dominates Approval cond. runoff in 2: c19 (honesty cost 0.1), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Approval cond. runoff clearly better than Approval in 9 of 41 conditions; Approval better in 2: c19 (honesty cost 0.1), c38 (strategic share 1.0, adaptive share 0.0).
- WME\*: Approval cond. runoff clearly better than Approval in 37 of 41 conditions.

**Approval runoff — Approval cond. runoff**

- Approval runoff dominates Approval cond. runoff in 25 of 41 conditions.
- VSE\*: Approval runoff clearly better than Approval cond. runoff in 9 of 41 conditions.
- WME\*: Approval runoff clearly better than Approval cond. runoff in 23 of 41 conditions.

**BTR-score — IRV**

- BTR-score dominates IRV in 41 of 41 conditions.
- VSE\*: BTR-score clearly better than IRV in 38 of 41 conditions.
- WME\*: BTR-score clearly better than IRV in 40 of 41 conditions.

**IMDS — INS**

- No clear difference on any of the front axes in any of the 2 conditions both were run in.

**BTR-score — Smith//Score**

- BTR-score dominates Smith//Score in 6 of 42 conditions; Smith//Score dominates BTR-score in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c8 (candidate shift 0.5).
- VSE\*: BTR-score clearly better than Smith//Score in 4 of 42 conditions; Smith//Score better in 2: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c8 (candidate shift 0.5).
- WME\*: BTR-score clearly better than Smith//Score in 2 of 42 conditions; Smith//Score better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

## Exceptions

Pairs of leading methods where each is ahead somewhere: how often each is ahead, and the conditions where the less usual outcome happens, with the settings they share. Pairs asked for with --pairs are above, not repeated here.

**Schulze — Soft Minimax-D**

- Soft Minimax-D dominates Schulze in 6 of 41 conditions; Schulze dominates Soft Minimax-D in 3: c21 (laziness 0.25), c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than Schulze in 37 of 41 conditions.
- WME\*: Schulze clearly better than Soft Minimax-D in 35 of 41 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Schulze — Kemeny-Young**

- Schulze dominates Kemeny-Young in 2 of 41 conditions; Kemeny-Young dominates Schulze in 2: c27 (electorate model ks), c34 (poll rounds 5).
- VSE\*: Schulze clearly better than Kemeny-Young in 2 of 41 conditions; Kemeny-Young better in 2: c27 (electorate model ks), c34 (poll rounds 5).
- WME\*: Schulze clearly better than Kemeny-Young in 1 of 41 conditions.

**Schulze — MMWV**

- Schulze dominates MMWV in 5 of 41 conditions; MMWV dominates Schulze in 1: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Schulze clearly better than MMWV in 3 of 41 conditions.
- WME\*: Schulze clearly better than MMWV in 2 of 41 conditions.

**Schulze — Minimax**

- Schulze dominates Minimax in 2 of 41 conditions; Minimax dominates Schulze in 1: c22 (laziness 0.5).
- VSE\*: Schulze clearly better than Minimax in 1 of 41 conditions.
- WME\*: Schulze clearly better than Minimax in 1 of 41 conditions; Minimax better in 1: c22 (laziness 0.5).

**Schulze — Ranked pairs**

- Schulze dominates Ranked pairs in 7 of 43 conditions; Ranked pairs dominates Schulze in 3: c3 (campaigns 1.0, response to a move arithmetic), c31 (poll rounds 1), c34 (poll rounds 5).
- VSE\*: Ranked pairs clearly better than Schulze in 3 of 43 conditions; Schulze better in 2: c42 (voters 40, electorate model ks-quinn, utility linear, poll rounds 1, strategic share 0.5, adaptive share 0.0, max score 10, exits 0, clones 0, candidates vote 0), c43 (voters 40, electorate model ks-quinn, utility linear, poll rounds 1, strategic share 1.0, adaptive share 0.0, max score 10, exits 0, clones 0, candidates vote 0), all with voters 40, electorate model ks-quinn, utility linear, poll rounds 1, adaptive share 0.0, max score 10, exits 0, clones 0, candidates vote 0.
- WME\*: Schulze clearly better than Ranked pairs in 7 of 43 conditions.

**Schulze — Copeland**

- Schulze dominates Copeland in 12 of 41 conditions; Copeland dominates Schulze in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- VSE\*: Schulze clearly better than Copeland in 5 of 41 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- WME\*: Schulze clearly better than Copeland in 12 of 41 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Schulze — MMPO**

- Schulze dominates MMPO in 6 of 41 conditions; MMPO dominates Schulze in 3: c9 (candidate shift 1.0), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Schulze clearly better than MMPO in 5 of 41 conditions; MMPO better in 2: c9 (candidate shift 1.0), c10 (candidates 4).
- WME\*: Schulze clearly better than MMPO in 3 of 41 conditions.

**Schulze — BTR-score**

- Schulze dominates BTR-score in 6 of 41 conditions; BTR-score dominates Schulze in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: BTR-score clearly better than Schulze in 34 of 41 conditions.
- WME\*: Schulze clearly better than BTR-score in 38 of 41 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Schulze — Soft Minimax**

- Schulze dominates Soft Minimax in 6 of 41 conditions; Soft Minimax dominates Schulze in 5: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c26 (electorate model field).
- VSE\*: Soft Minimax clearly better than Schulze in 31 of 41 conditions.
- WME\*: Schulze clearly better than Soft Minimax in 33 of 41 conditions; Soft Minimax better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Schulze — Smith//Score**

- Schulze dominates Smith//Score in 6 of 41 conditions; Smith//Score dominates Schulze in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: Smith//Score clearly better than Schulze in 34 of 41 conditions.
- WME\*: Schulze clearly better than Smith//Score in 38 of 41 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Schulze — Soft Minimax-40**

- Soft Minimax-40 dominates Schulze in 3 of 41 conditions; Schulze dominates Soft Minimax-40 in 2: c7 (candidate dispersion 2.0), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-40 clearly better than Schulze in 39 of 41 conditions.
- WME\*: Schulze clearly better than Soft Minimax-40 in 38 of 41 conditions; Soft Minimax-40 better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Soft Minimax-D — Kemeny-Young**

- Soft Minimax-D dominates Kemeny-Young in 5 of 41 conditions; Kemeny-Young dominates Soft Minimax-D in 2: c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than Kemeny-Young in 38 of 41 conditions.
- WME\*: Kemeny-Young clearly better than Soft Minimax-D in 36 of 41 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Soft Minimax-D — MMWV**

- Soft Minimax-D dominates MMWV in 4 of 41 conditions; MMWV dominates Soft Minimax-D in 2: c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than MMWV in 39 of 41 conditions.
- WME\*: MMWV clearly better than Soft Minimax-D in 37 of 41 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Soft Minimax-D — Minimax**

- Soft Minimax-D dominates Minimax in 5 of 41 conditions; Minimax dominates Soft Minimax-D in 3: c21 (laziness 0.25), c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than Minimax in 36 of 41 conditions.
- WME\*: Minimax clearly better than Soft Minimax-D in 35 of 41 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Soft Minimax-D — Ranked pairs**

- Soft Minimax-D dominates Ranked pairs in 6 of 41 conditions; Ranked pairs dominates Soft Minimax-D in 3: c21 (laziness 0.25), c22 (laziness 0.5), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than Ranked pairs in 37 of 41 conditions.
- WME\*: Ranked pairs clearly better than Soft Minimax-D in 35 of 41 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Soft Minimax-D — Copeland**

- Copeland dominates Soft Minimax-D in 3 of 41 conditions; Soft Minimax-D dominates Copeland in 2: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c38 (strategic share 1.0, adaptive share 0.0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: Soft Minimax-D clearly better than Copeland in 36 of 41 conditions; Copeland better in 1: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic).
- WME\*: Copeland clearly better than Soft Minimax-D in 38 of 41 conditions; Soft Minimax-D better in 1: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).

**Soft Minimax-D — MMPO**

- Soft Minimax-D dominates MMPO in 5 of 41 conditions; MMPO dominates Soft Minimax-D in 1: c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-D clearly better than MMPO in 40 of 41 conditions.
- WME\*: MMPO clearly better than Soft Minimax-D in 36 of 41 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Soft Minimax-D — BTR-score**

- Soft Minimax-D dominates BTR-score in 13 of 42 conditions; BTR-score dominates Soft Minimax-D in 2: c39 (utility linear), c41 (voters 199).
- VSE\*: Soft Minimax-D clearly better than BTR-score in 9 of 42 conditions.
- WME\*: Soft Minimax-D clearly better than BTR-score in 6 of 42 conditions; BTR-score better in 3: c39 (utility linear), c40 (utility quadratic), c41 (voters 199).

**Soft Minimax-D — Soft Minimax**

- Soft Minimax-D dominates Soft Minimax in 16 of 42 conditions; Soft Minimax dominates Soft Minimax-D in 7: c7 (candidate dispersion 2.0), c8 (candidate shift 0.5), c19 (honesty cost 0.1), c27 (electorate model ks), c37 (strategic share 0.5, adaptive share 0.0), c38 (strategic share 1.0, adaptive share 0.0), c41 (voters 199).
- VSE\*: Soft Minimax-D clearly better than Soft Minimax in 27 of 42 conditions; Soft Minimax better in 1: c24 (max score 99).
- WME\*: Soft Minimax clearly better than Soft Minimax-D in 18 of 42 conditions; Soft Minimax-D better in 1: c24 (max score 99).

**Soft Minimax-D — Smith//Score**

- Soft Minimax-D dominates Smith//Score in 11 of 42 conditions; Smith//Score dominates Soft Minimax-D in 3: c9 (candidate shift 1.0), c39 (utility linear), c41 (voters 199).
- VSE\*: Soft Minimax-D clearly better than Smith//Score in 9 of 42 conditions.
- WME\*: Smith//Score clearly better than Soft Minimax-D in 4 of 42 conditions; Soft Minimax-D better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c25 (electorate model clusters).

**Soft Minimax-D — Soft Minimax-40**

- Soft Minimax-D dominates Soft Minimax-40 in 23 of 42 conditions; Soft Minimax-40 dominates Soft Minimax-D in 1: c22 (laziness 0.5).
- VSE\*: Soft Minimax-40 clearly better than Soft Minimax-D in 16 of 42 conditions.
- WME\*: Soft Minimax-D clearly better than Soft Minimax-40 in 38 of 42 conditions.

**Kemeny-Young — MMWV**

- Kemeny-Young dominates MMWV in 5 of 41 conditions; MMWV dominates Kemeny-Young in 1: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Kemeny-Young clearly better than MMWV in 2 of 41 conditions.
- WME\*: Kemeny-Young clearly better than MMWV in 3 of 41 conditions.

**Kemeny-Young — Minimax**

- Kemeny-Young dominates Minimax in 4 of 41 conditions; Minimax dominates Kemeny-Young in 3: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c28 (exits 0, clones 0), c41 (voters 199).
- VSE\*: Minimax clearly better than Kemeny-Young in 3 of 41 conditions; Kemeny-Young better in 2: c27 (electorate model ks), c34 (poll rounds 5).
- WME\*: Kemeny-Young clearly better than Minimax in 2 of 41 conditions; Minimax better in 1: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic).

**Kemeny-Young — Ranked pairs**

- Ranked pairs dominates Kemeny-Young in 6 of 41 conditions; Kemeny-Young dominates Ranked pairs in 1: c36 (samples 20).
- VSE\*: Ranked pairs clearly better than Kemeny-Young in 4 of 41 conditions.
- WME\*: Ranked pairs clearly better than Kemeny-Young in 2 of 41 conditions; Kemeny-Young better in 1: c36 (samples 20).

**Kemeny-Young — Copeland**

- Kemeny-Young dominates Copeland in 9 of 41 conditions; Copeland dominates Kemeny-Young in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- VSE\*: Copeland clearly better than Kemeny-Young in 2 of 41 conditions.
- WME\*: Kemeny-Young clearly better than Copeland in 9 of 41 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Kemeny-Young — MMPO**

- Kemeny-Young dominates MMPO in 5 of 41 conditions; MMPO dominates Kemeny-Young in 1: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Kemeny-Young clearly better than MMPO in 1 of 41 conditions.
- WME\*: Kemeny-Young clearly better than MMPO in 4 of 41 conditions.

**Kemeny-Young — BTR-score**

- Kemeny-Young dominates BTR-score in 6 of 41 conditions; BTR-score dominates Kemeny-Young in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: BTR-score clearly better than Kemeny-Young in 33 of 41 conditions.
- WME\*: Kemeny-Young clearly better than BTR-score in 38 of 41 conditions; BTR-score better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Kemeny-Young — Soft Minimax**

- Soft Minimax dominates Kemeny-Young in 6 of 41 conditions; Kemeny-Young dominates Soft Minimax in 5: c7 (candidate dispersion 2.0), c21 (laziness 0.25), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c44 (width 1.4).
- VSE\*: Soft Minimax clearly better than Kemeny-Young in 32 of 41 conditions.
- WME\*: Kemeny-Young clearly better than Soft Minimax in 33 of 41 conditions; Soft Minimax better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Kemeny-Young — Smith//Score**

- Kemeny-Young dominates Smith//Score in 6 of 41 conditions; Smith//Score dominates Kemeny-Young in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Smith//Score clearly better than Kemeny-Young in 33 of 41 conditions.
- WME\*: Kemeny-Young clearly better than Smith//Score in 37 of 41 conditions; Smith//Score better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Kemeny-Young — Soft Minimax-40**

- Soft Minimax-40 dominates Kemeny-Young in 3 of 41 conditions; Kemeny-Young dominates Soft Minimax-40 in 1: c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-40 clearly better than Kemeny-Young in 40 of 41 conditions.
- WME\*: Kemeny-Young clearly better than Soft Minimax-40 in 38 of 41 conditions; Soft Minimax-40 better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**MMWV — Minimax**

- Minimax dominates MMWV in 6 of 41 conditions; MMWV dominates Minimax in 1: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Minimax clearly better than MMWV in 4 of 41 conditions.
- WME\*: Minimax clearly better than MMWV in 2 of 41 conditions.

**MMWV — Ranked pairs**

- MMWV dominates Ranked pairs in 4 of 41 conditions; Ranked pairs dominates MMWV in 3: c3 (campaigns 1.0, response to a move arithmetic), c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c34 (poll rounds 5).
- VSE\*: Ranked pairs clearly better than MMWV in 2 of 41 conditions.
- WME\*: MMWV clearly better than Ranked pairs in 3 of 41 conditions; Ranked pairs better in 1: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic).

**MMWV — Copeland**

- MMWV dominates Copeland in 10 of 41 conditions; Copeland dominates MMWV in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0).
- VSE\*: Copeland clearly better than MMWV in 3 of 41 conditions; MMWV better in 2: c1 (defaults), c38 (strategic share 1.0, adaptive share 0.0).
- WME\*: MMWV clearly better than Copeland in 9 of 41 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**MMWV — BTR-score**

- MMWV dominates BTR-score in 5 of 41 conditions; BTR-score dominates MMWV in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: BTR-score clearly better than MMWV in 36 of 41 conditions.
- WME\*: MMWV clearly better than BTR-score in 38 of 41 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**MMWV — Soft Minimax**

- MMWV dominates Soft Minimax in 6 of 41 conditions; Soft Minimax dominates MMWV in 6: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c19 (honesty cost 0.1), c26 (electorate model field).
- VSE\*: Soft Minimax clearly better than MMWV in 31 of 41 conditions.
- WME\*: MMWV clearly better than Soft Minimax in 32 of 41 conditions; Soft Minimax better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**MMWV — Smith//Score**

- MMWV dominates Smith//Score in 4 of 41 conditions; Smith//Score dominates MMWV in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: Smith//Score clearly better than MMWV in 36 of 41 conditions.
- WME\*: MMWV clearly better than Smith//Score in 37 of 41 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**MMWV — Soft Minimax-40**

- Soft Minimax-40 dominates MMWV in 3 of 41 conditions; MMWV dominates Soft Minimax-40 in 2: c7 (candidate dispersion 2.0), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-40 clearly better than MMWV in 39 of 41 conditions.
- WME\*: MMWV clearly better than Soft Minimax-40 in 38 of 41 conditions; Soft Minimax-40 better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Minimax — Ranked pairs**

- Minimax dominates Ranked pairs in 5 of 41 conditions; Ranked pairs dominates Minimax in 4: c3 (campaigns 1.0, response to a move arithmetic), c25 (electorate model clusters), c31 (poll rounds 1), c34 (poll rounds 5).
- VSE\*: Ranked pairs clearly better than Minimax in 3 of 41 conditions; Minimax better in 1: c41 (voters 199).
- WME\*: Minimax clearly better than Ranked pairs in 4 of 41 conditions; Ranked pairs better in 1: c25 (electorate model clusters).

**Minimax — Copeland**

- Minimax dominates Copeland in 11 of 41 conditions; Copeland dominates Minimax in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- VSE\*: Minimax clearly better than Copeland in 3 of 41 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- WME\*: Minimax clearly better than Copeland in 10 of 41 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Minimax — MMPO**

- Minimax dominates MMPO in 10 of 41 conditions; MMPO dominates Minimax in 1: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Minimax clearly better than MMPO in 9 of 41 conditions.
- WME\*: Minimax clearly better than MMPO in 3 of 41 conditions.

**Minimax — BTR-score**

- Minimax dominates BTR-score in 6 of 41 conditions; BTR-score dominates Minimax in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: BTR-score clearly better than Minimax in 34 of 41 conditions.
- WME\*: Minimax clearly better than BTR-score in 37 of 41 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Minimax — Soft Minimax**

- Minimax dominates Soft Minimax in 7 of 41 conditions; Soft Minimax dominates Minimax in 5: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c26 (electorate model field).
- VSE\*: Soft Minimax clearly better than Minimax in 31 of 41 conditions.
- WME\*: Minimax clearly better than Soft Minimax in 34 of 41 conditions; Soft Minimax better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Minimax — Smith//Score**

- Minimax dominates Smith//Score in 7 of 41 conditions; Smith//Score dominates Minimax in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Smith//Score clearly better than Minimax in 32 of 41 conditions.
- WME\*: Minimax clearly better than Smith//Score in 37 of 41 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Minimax — Soft Minimax-40**

- Soft Minimax-40 dominates Minimax in 3 of 41 conditions; Minimax dominates Soft Minimax-40 in 2: c7 (candidate dispersion 2.0), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-40 clearly better than Minimax in 39 of 41 conditions.
- WME\*: Minimax clearly better than Soft Minimax-40 in 38 of 41 conditions; Soft Minimax-40 better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Ranked pairs — Copeland**

- Ranked pairs dominates Copeland in 11 of 41 conditions; Copeland dominates Ranked pairs in 3: c2 (response to a move arithmetic), c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with response to a move arithmetic.
- VSE\*: Ranked pairs clearly better than Copeland in 3 of 41 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- WME\*: Ranked pairs clearly better than Copeland in 10 of 41 conditions; Copeland better in 3: c2 (response to a move arithmetic), c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with response to a move arithmetic.

**Ranked pairs — MMPO**

- Ranked pairs dominates MMPO in 8 of 41 conditions; MMPO dominates Ranked pairs in 4: c13 (coordination share 0.0), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c20 (honesty cost 1.0), c35 (samples 10).
- VSE\*: Ranked pairs clearly better than MMPO in 6 of 41 conditions.
- WME\*: Ranked pairs clearly better than MMPO in 3 of 41 conditions; MMPO better in 3: c13 (coordination share 0.0), c20 (honesty cost 1.0), c35 (samples 10).

**Ranked pairs — BTR-score**

- Ranked pairs dominates BTR-score in 6 of 41 conditions; BTR-score dominates Ranked pairs in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: BTR-score clearly better than Ranked pairs in 34 of 41 conditions.
- WME\*: Ranked pairs clearly better than BTR-score in 38 of 41 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Ranked pairs — Soft Minimax**

- Ranked pairs dominates Soft Minimax in 7 of 41 conditions; Soft Minimax dominates Ranked pairs in 6: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c6 (candidate dispersion 0.5), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c26 (electorate model field).
- VSE\*: Soft Minimax clearly better than Ranked pairs in 30 of 41 conditions.
- WME\*: Ranked pairs clearly better than Soft Minimax in 33 of 41 conditions; Soft Minimax better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Ranked pairs — Smith//Score**

- Ranked pairs dominates Smith//Score in 5 of 41 conditions; Smith//Score dominates Ranked pairs in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Smith//Score clearly better than Ranked pairs in 33 of 41 conditions.
- WME\*: Ranked pairs clearly better than Smith//Score in 36 of 41 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Ranked pairs — Soft Minimax-40**

- Soft Minimax-40 dominates Ranked pairs in 3 of 41 conditions; Ranked pairs dominates Soft Minimax-40 in 1: c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-40 clearly better than Ranked pairs in 40 of 41 conditions.
- WME\*: Ranked pairs clearly better than Soft Minimax-40 in 38 of 41 conditions; Soft Minimax-40 better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**Copeland — MMPO**

- MMPO dominates Copeland in 7 of 41 conditions; Copeland dominates MMPO in 6: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c11 (candidates 8), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c21 (laziness 0.25), c22 (laziness 0.5).
- VSE\*: Copeland clearly better than MMPO in 6 of 41 conditions; MMPO better in 1: c38 (strategic share 1.0, adaptive share 0.0).
- WME\*: MMPO clearly better than Copeland in 6 of 41 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.

**Copeland — BTR-score**

- Copeland dominates BTR-score in 5 of 41 conditions; BTR-score dominates Copeland in 2: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c38 (strategic share 1.0, adaptive share 0.0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: BTR-score clearly better than Copeland in 34 of 41 conditions; Copeland better in 2: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), all with strategic share 1.0, adaptive share 0.0, response to a move arithmetic.
- WME\*: Copeland clearly better than BTR-score in 39 of 41 conditions; BTR-score better in 1: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).

**Copeland — Soft Minimax**

- Soft Minimax dominates Copeland in 6 of 41 conditions; Copeland dominates Soft Minimax in 5: c2 (response to a move arithmetic), c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c21 (laziness 0.25), c39 (utility linear), c44 (width 1.4).
- VSE\*: Soft Minimax clearly better than Copeland in 32 of 41 conditions.
- WME\*: Copeland clearly better than Soft Minimax in 32 of 41 conditions; Soft Minimax better in 2: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c38 (strategic share 1.0, adaptive share 0.0), all with strategic share 1.0, adaptive share 0.0.

**Copeland — Smith//Score**

- Copeland dominates Smith//Score in 4 of 41 conditions; Smith//Score dominates Copeland in 2: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c38 (strategic share 1.0, adaptive share 0.0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: Smith//Score clearly better than Copeland in 34 of 41 conditions; Copeland better in 1: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic).
- WME\*: Copeland clearly better than Smith//Score in 38 of 41 conditions; Smith//Score better in 1: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).

**Copeland — Soft Minimax-40**

- Copeland dominates Soft Minimax-40 in 2 of 41 conditions; Soft Minimax-40 dominates Copeland in 2: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c38 (strategic share 1.0, adaptive share 0.0), all with strategic share 1.0, adaptive share 0.0.
- VSE\*: Soft Minimax-40 clearly better than Copeland in 38 of 41 conditions; Copeland better in 1: c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic).
- WME\*: Copeland clearly better than Soft Minimax-40 in 39 of 41 conditions; Soft Minimax-40 better in 1: c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).

**MMPO — BTR-score**

- MMPO dominates BTR-score in 4 of 41 conditions; BTR-score dominates MMPO in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: BTR-score clearly better than MMPO in 36 of 41 conditions.
- WME\*: MMPO clearly better than BTR-score in 37 of 41 conditions; BTR-score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**MMPO — Soft Minimax**

- Soft Minimax dominates MMPO in 7 of 41 conditions; MMPO dominates Soft Minimax in 6: c6 (candidate dispersion 0.5), c7 (candidate dispersion 2.0), c9 (candidate shift 1.0), c38 (strategic share 1.0, adaptive share 0.0), c39 (utility linear), c44 (width 1.4).
- VSE\*: Soft Minimax clearly better than MMPO in 32 of 41 conditions.
- WME\*: MMPO clearly better than Soft Minimax in 32 of 41 conditions; Soft Minimax better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**MMPO — Smith//Score**

- MMPO dominates Smith//Score in 4 of 41 conditions; Smith//Score dominates MMPO in 4: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c10 (candidates 4), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0).
- VSE\*: Smith//Score clearly better than MMPO in 37 of 41 conditions.
- WME\*: MMPO clearly better than Smith//Score in 37 of 41 conditions; Smith//Score better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**MMPO — Soft Minimax-40**

- Soft Minimax-40 dominates MMPO in 3 of 41 conditions; MMPO dominates Soft Minimax-40 in 2: c7 (candidate dispersion 2.0), c38 (strategic share 1.0, adaptive share 0.0).
- VSE\*: Soft Minimax-40 clearly better than MMPO in 39 of 41 conditions.
- WME\*: MMPO clearly better than Soft Minimax-40 in 38 of 41 conditions; Soft Minimax-40 better in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c5 (strategic share 1.0, adaptive share 0.0, campaigns 1.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), all with strategic share 1.0, adaptive share 0.0.

**BTR-score — Soft Minimax**

- Soft Minimax dominates BTR-score in 11 of 42 conditions; BTR-score dominates Soft Minimax in 5: c9 (candidate shift 1.0), c13 (coordination share 0.0), c20 (honesty cost 1.0), c32 (poll rounds 2), c39 (utility linear).
- VSE\*: BTR-score clearly better than Soft Minimax in 6 of 42 conditions; Soft Minimax better in 2: c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c24 (max score 99).
- WME\*: Soft Minimax clearly better than BTR-score in 12 of 42 conditions; BTR-score better in 1: c24 (max score 99).

**BTR-score — Soft Minimax-40**

- BTR-score dominates Soft Minimax-40 in 10 of 42 conditions; Soft Minimax-40 dominates BTR-score in 5: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c6 (candidate dispersion 0.5), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c18 (candidate dispersion 0.5, laziness 0.25, poll error 4.0), c22 (laziness 0.5).
- VSE\*: Soft Minimax-40 clearly better than BTR-score in 29 of 42 conditions.
- WME\*: BTR-score clearly better than Soft Minimax-40 in 35 of 42 conditions; Soft Minimax-40 better in 1: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic).

**Soft Minimax — Smith//Score**

- Soft Minimax dominates Smith//Score in 14 of 42 conditions; Smith//Score dominates Soft Minimax in 3: c9 (candidate shift 1.0), c15 (coordination share 0.5), c44 (width 1.4).
- VSE\*: Smith//Score clearly better than Soft Minimax in 5 of 42 conditions; Soft Minimax better in 1: c24 (max score 99).
- WME\*: Soft Minimax clearly better than Smith//Score in 16 of 42 conditions; Smith//Score better in 1: c24 (max score 99).

**Smith//Score — Soft Minimax-40**

- Smith//Score dominates Soft Minimax-40 in 9 of 42 conditions; Soft Minimax-40 dominates Smith//Score in 3: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic), c17 (strategic share 1.0, adaptive share 0.0, search 1, individual voters 0), c22 (laziness 0.5).
- VSE\*: Soft Minimax-40 clearly better than Smith//Score in 28 of 42 conditions.
- WME\*: Smith//Score clearly better than Soft Minimax-40 in 35 of 42 conditions; Soft Minimax-40 better in 1: c4 (strategic share 1.0, adaptive share 0.0, response to a move arithmetic).

