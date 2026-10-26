# Ballot Front Finder Lab — analysis

Input: defaults (simulator version 1.0.0; analysis version 1.0.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

How to read this:

- The values are the result. The **front** is judged on VSE\*, WME\* and sincerity: the result with strategic voters, and how honest they can afford to be. The **Pareto front** uses the point estimates as they are: a method is on it if no other is at least as good on all three and better on one, however small the difference (sincerity: below 1 point counts as a tie). **Undominated** asks for clear differences instead (see Dominated by). The two need not agree: a noise-sized advantage on one axis keeps a method on the Pareto front, but does not protect it from a method clearly better on another axis; a method beaten by a hair on every axis leaves the Pareto front but can stay undominated. Honest VSE and WME depend more on modelling assumptions and are shown for context. **Dominated by** lists the methods that are clearly better on at least one of the three front axes and clearly worse on none, with the axes on which they are better in brackets. Clearly means the paired 95% interval of the difference excludes 0; smaller differences count as ties. Sincerity differences are measured very precisely, but below 1 point no voter is noticeably more honest, so they count as ties too, here and on the Pareto front. It is a pointer for interpretation, relative to the methods included, not a verdict.
- **Sincerity**: how far the learned ballots go against the voters' own preferences, within each method's ballot format: the utility-weighted pairs a ballot puts in the wrong order, relative to the most dishonest ballot the format allows (100 = no reversed preference, 0 = as far against one's preferences as the format permits). Ties (exaggeration, levelling) are not reversals. **Fidelity**: how well the ballots reproduce the voters' utility differences (100 = exactly, a tie loses half a pair, a mirrored pair all of it); it includes what the format cannot express. **(honest)** is the fidelity of the sincere ballots: the format's own loss; the drop from there to Fidelity is what strategy adds (docs/analysis.md, Sincerity and fidelity). Both are undefined (—) for TRASH and the Inverse-\* methods, whose honest ballot works against the voter's own interest; on the front they count as ties on sincerity.
- The intervals in the tables are for each value on its own. They are wide because elections differ a lot from each other. Comparisons between methods are much more precise, because all methods face the same elections: use the paired differences for those, not overlapping intervals.
- Two methods whose paired difference interval includes 0 are tied at this sample size; their order in the table is not a finding.
- **VSE** and **WME** are efficiencies with 0 = random ballot and 100 = the best candidate of each election (values above 100 cannot be reached). VSE measures total utility; WME the winner's worst head-to-head margin by sincere preferences (a refined Condorcet measure; every election counts; Minimax reaches 100 with honest ballots by construction). CE (Condorcet efficiency, elections with a Condorcet winner only, elected or not) is shown for comparison and not used for dominance. **Low** and **Low\***: the share of elections whose winner (the expected total utility of the winner lottery) lies below the median candidate's total utility, with honest and with learned ballots: how often a method elects a poor candidate, which the averages of VSE hide. Not used for dominance.
- The strategic axes (VSE\*, WME\*) give the result after the poll rounds: voters see polls of the previous round (the method's counts only) and answer them all at once (docs/model.md, Strategy engine). Under convergent learning (--converge) they describe a long-run state instead and depend on the group size.
- **Compromise, burial, push-over** are shares of voters whose learned ballot reverses a real preference, by kind (see strategy.strategy_types): compromise raises a liked second choice above the favourite, push-over raises a disliked candidate to the top, burial keeps the favourite on top and reverses a preference below it.

## c1: single condition

200 elections (seeds 1), 198 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 99.0%; it also has the highest total utility in 70.2% of those; electing it loses 16.7% of VSE; favourite ÷ least liked 8.5 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 82.8; with the best VSE (100) WME 77.5 (nothing lower is possible there either); the most balanced point VSE 93.5 / WME 95.2. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 89.8 (at WME 91.5); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -57.8 / WME -54.7.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Score, ARA, Smith//Score, Score runoff, BTR-IRV, Ranked pairs, MMPO.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Score, Score runoff, MMPO.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Score | rated | 89.1 [84.3–92.9] | 93.2 [90.3–96.0] | 100.0 [99.9–100.0] | 89.8 | 94.1 | 89.2 [84.6–92.7] | 92.5 [89.1–95.7] | 84.6 [79.3–89.5] | 84.8 [79.6–89.7] | 4.0 [1.5–6.5] | 4.5 [2.0–7.5] | 0.0% | 0.9% | 0.0% | — |
| Approval | approval-based | 88.5 [83.7–92.4] | 88.4 [84.0–92.4] | 99.9 [99.8–99.9] | 69.3 | 70.8 | 86.9 [81.8–91.3] | 85.7 [80.8–90.2] | 76.3 [69.9–82.1] | 78.8 [73.1–84.1] | 5.0 [2.0–8.0] | 4.5 [2.0–7.5] | 0.0% | 1.6% | 0.0% | Smith//Score (WME\*), Approval cond. runoff (WME\*), Score (WME\*) +6 more |
| MAV | median-based | 87.3 [82.1–91.5] | 88.9 [84.7–92.8] | 99.9 [99.9–100.0] | 89.8 | 94.1 | 88.7 [83.7–92.8] | 89.1 [84.8–92.9] | 79.5 [73.5–84.9] | 79.3 [73.4–84.8] | 4.0 [1.5–7.0] | 5.0 [2.5–8.0] | 0.0% | 0.9% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), Score (WME\*) +6 more |
| Approval cond. runoff | two-round | 86.6 [81.4–91.0] | 92.1 [88.4–95.7] | 99.7 [99.6–99.8] | 68.9 | 70.8 | 87.5 [82.3–91.5] | 92.2 [88.5–95.7] | 88.9 [84.5–93.2] | 87.9 [83.6–92.7] | 5.0 [2.0–8.0] | 4.5 [2.0–7.5] | 0.1% | 2.6% | 0.0% | Score runoff (WME\*) |
| ARA | hybrid | 86.4 [81.2–90.9] | 94.2 [91.5–96.8] | 99.9 [99.9–100.0] | 90.1 | 94.1 | 87.4 [82.4–91.5] | 93.3 [90.2–96.1] | 86.6 [82.0–91.2] | 87.6 [83.0–92.1] | 4.0 [1.5–7.0] | 4.5 [2.0–7.5] | 0.1% | 1.2% | 0.0% | Score (VSE\*), Score runoff (WME\*) |
| MJ | median-based | 86.2 [80.9–90.8] | 88.5 [84.1–92.6] | 99.9 [99.9–100.0] | 89.8 | 94.1 | 86.7 [81.2–91.0] | 91.0 [86.9–94.5] | 82.3 [76.9–87.4] | 79.3 [73.5–84.8] | 3.0 [1.0–5.5] | 3.5 [1.5–6.0] | 0.0% | 1.0% | 0.0% | Score (VSE\* WME\*), Smith//Score (WME\*), Approval runoff (WME\*) +6 more |
| Supplementary vote | plurality-based | 86.2 [81.2–90.4] | 86.2 [81.2–90.9] | 97.8 [97.7–98.0] | 76.2 | 79.6 | 84.1 [78.1–89.3] | 82.5 [76.5–87.8] | 75.5 [69.0–81.4] | 79.0 [73.2–84.5] | 6.5 [3.5–10.0] | 5.5 [2.5–8.5] | 5.8% | 15.5% | 1.2% | Score (VSE\* WME\* sincerity), Smith//Score (WME\* sincerity), Approval runoff (WME\* sincerity) +19 more |
| Smith//Score | condorcet-based | 86.1 [80.6–90.5] | 95.8 [93.2–97.9] | 99.9 [99.9–100.0] | 90.4 | 94.1 | 86.6 [81.3–90.8] | 95.3 [92.7–97.4] | 88.4 [83.8–92.5] | 88.4 [83.8–92.5] | 4.5 [1.5–7.5] | 5.0 [2.0–8.0] | 0.1% | 1.2% | 0.0% | Score (VSE\*) |
| Soft Minimax-D | hybrid | 85.9 [80.9–90.2] | 94.9 [92.1–97.1] | 100.0 [99.9–100.0] | 90.3 | 94.1 | 86.0 [80.6–90.5] | 95.0 [92.3–97.2] | 87.4 [82.8–91.5] | 86.6 [81.7–91.2] | 5.0 [2.0–8.0] | 4.0 [1.5–6.5] | 0.1% | 0.8% | 0.0% | Score (VSE\*), Score runoff (WME\*) |
| 321 | hybrid | 85.9 [80.8–90.1] | 87.8 [82.7–92.2] | 99.9 [99.8–99.9] | 82.9 | 85.8 | 88.3 [83.6–92.2] | 86.8 [82.2–91.0] | 78.0 [72.1–83.8] | 79.8 [74.2–85.3] | 3.0 [1.0–5.5] | 5.0 [2.5–8.0] | 0.2% | 1.6% | 0.0% | Score (VSE\* WME\*), Smith//Score (WME\*), Approval runoff (WME\*) +7 more |
| BTR-score | hybrid | 85.9 [80.5–90.5] | 95.6 [92.8–97.9] | 99.9 [99.9–100.0] | 90.4 | 94.1 | 86.2 [81.0–90.5] | 95.4 [92.8–97.4] | 88.4 [83.7–92.4] | 89.6 [85.6–93.6] | 5.0 [2.0–8.0] | 5.0 [2.0–8.0] | 0.1% | 1.0% | 0.0% | Score (VSE\*), Score runoff (WME\*) |
| STAR | hybrid | 85.3 [79.8–90.0] | 94.7 [91.9–97.0] | 99.9 [99.9–99.9] | 90.1 | 94.1 | 86.6 [81.3–90.7] | 94.6 [91.8–97.0] | 86.4 [81.3–90.9] | 86.4 [81.5–90.9] | 4.0 [1.5–6.5] | 5.0 [2.0–8.0] | 0.2% | 1.4% | 0.0% | Score (VSE\*), Score runoff (WME\*) |
| MARS | hybrid | 85.2 [79.8–89.7] | 95.2 [92.5–97.4] | 99.9 [99.9–100.0] | 90.1 | 94.1 | 87.6 [82.5–91.7] | 95.0 [92.4–97.3] | 88.1 [83.4–92.4] | 87.9 [83.4–92.4] | 4.5 [2.0–7.5] | 5.0 [2.0–8.0] | 0.0% | 0.9% | 0.0% | Score (VSE\*), Score runoff (WME\*) |
| TTR | two-round | 85.0 [79.6–89.9] | 89.6 [85.0–93.7] | 97.2 [96.8–97.6] | 63.4 | 66.2 | 86.3 [80.9–90.8] | 87.2 [82.0–91.4] | 83.3 [77.8–88.4] | 86.6 [81.7–91.1] | 5.5 [2.5–8.5] | 5.5 [2.5–8.5] | 6.5% | 0.0% | 2.8% | MMWV (WME\* sincerity), MMPO (WME\* sincerity), Smith//Score (WME\* sincerity) +20 more |
| Approval runoff | two-round | 84.6 [79.5–89.4] | 93.5 [90.0–96.6] | 99.6 [99.5–99.7] | 68.6 | 70.8 | 86.1 [80.7–90.5] | 93.4 [89.7–96.7] | 90.9 [86.9–94.7] | 90.9 [86.8–94.5] | 5.0 [2.0–8.0] | 6.0 [3.0–9.0] | 0.2% | 3.3% | 0.1% | Score (VSE\*), Score runoff (WME\*) |
| Score runoff | two-round | 84.6 [79.0–89.4] | 98.2 [96.5–99.6] | 99.9 [99.8–99.9] | 89.8 | 94.1 | 84.7 [79.2–89.2] | 98.2 [96.6–99.5] | 97.0 [94.4–99.0] | 97.5 [95.0–99.5] | 5.5 [2.5–8.5] | 6.0 [3.0–9.5] | 0.2% | 1.4% | 0.0% | — |
| IRV | plurality-based | 84.1 [78.5–89.1] | 91.4 [87.4–95.1] | 97.5 [97.3–97.7] | 75.9 | 79.6 | 85.6 [80.4–90.1] | 88.1 [83.1–92.4] | 85.4 [80.2–90.0] | 87.0 [82.0–91.2] | 6.0 [3.0–9.5] | 5.5 [2.5–8.5] | 5.5% | 14.5% | 1.6% | Smith//Score (WME\* sincerity), Approval (VSE\* sincerity), Score (VSE\* sincerity) +20 more |
| IRV-3 | plurality-based | 83.3 [77.6–88.2] | 90.7 [86.6–94.7] | 98.8 [98.6–98.9] | 75.6 | 77.9 | 84.5 [79.0–89.2] | 87.0 [81.9–91.5] | 83.2 [78.3–88.3] | 85.6 [80.7–90.4] | 7.0 [3.5–10.5] | 7.0 [4.0–10.5] | 5.6% | 7.1% | 1.3% | ARA (VSE\* WME\* sincerity), Smith//Score (WME\* sincerity), Approval (VSE\* sincerity) +19 more |
| Copeland | condorcet | 83.0 [77.2–87.9] | 97.7 [95.8–99.2] | 97.9 [97.7–98.0] | 76.2 | 79.6 | 83.0 [77.2–88.1] | 99.8 [99.5–100.0] | 100.0 [100.0–100.0] | 95.9 [93.1–98.1] | 7.0 [3.5–10.5] | 6.5 [3.5–10.0] | 5.4% | 13.0% | 0.9% | Ranked pairs (WME\*), MMPO (WME\*), Smith//Score (sincerity) +3 more |
| ER-IRV | plurality-based | 82.9 [76.9–88.5] | 93.3 [89.9–96.3] | 98.0 [97.8–98.2] | 75.9 | 79.6 | 85.6 [80.4–90.1] | 88.1 [83.1–92.4] | 85.4 [80.2–90.0] | 89.4 [84.9–93.5] | 6.0 [3.0–9.5] | 5.0 [2.5–8.0] | 1.3% | 18.9% | 0.2% | Score (VSE\* sincerity), Score runoff (WME\* sincerity), Ranked pairs (WME\*) +15 more |
| BTR-IRV | condorcet-based | 82.8 [76.9–87.8] | 99.1 [98.0–99.8] | 97.8 [97.6–98.0] | 76.2 | 79.6 | 83.2 [77.3–88.3] | 99.9 [99.8–100.0] | 100.0 [100.0–100.0] | 96.5 [93.9–98.5] | 7.0 [3.5–10.5] | 7.0 [3.5–10.5] | 5.6% | 13.3% | 1.0% | Score runoff (sincerity) |
| Ranked pairs | condorcet | 82.6 [76.7–87.7] | 99.3 [98.3–99.9] | 97.9 [97.8–98.1] | 76.3 | 79.6 | 82.8 [77.0–88.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.0 [94.5–99.0] | 7.0 [3.5–10.5] | 7.0 [3.5–10.5] | 5.4% | 13.3% | 0.8% | Score runoff (sincerity) |
| Borda | positional | 82.1 [76.1–87.6] | 94.1 [90.8–96.8] | 97.7 [97.6–97.8] | 75.7 | 79.6 | 78.9 [72.8–84.8] | 95.0 [92.2–97.2] | 85.9 [80.7–90.5] | 85.6 [80.7–90.4] | 7.0 [3.5–10.5] | 6.0 [3.0–9.5] | 6.5% | 13.8% | 0.9% | Score (VSE\* sincerity), Score runoff (WME\* sincerity), Ranked pairs (WME\*) +16 more |
| Kemeny-Young | condorcet | 81.9 [75.8–87.1] | 99.1 [97.9–99.8] | 97.9 [97.8–98.1] | 76.3 | 79.6 | 82.8 [77.0–88.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.5 [93.9–99.0] | 7.0 [3.5–10.5] | 7.0 [3.5–10.5] | 5.3% | 13.2% | 0.8% | Score runoff (VSE\* sincerity) |
| MMWV | minimax-family | 81.7 [75.2–87.0] | 99.2 [98.2–99.8] | 98.2 [98.1–98.4] | 76.4 | 79.6 | 82.8 [77.0–88.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.0 [93.0–98.5] | 7.0 [3.5–10.5] | 7.0 [3.5–10.5] | 0.1% | 18.1% | 0.0% | Score runoff (VSE\* sincerity) |
| Schulze | condorcet | 81.6 [75.7–86.7] | 99.0 [97.9–99.8] | 98.0 [97.8–98.1] | 76.3 | 79.6 | 82.8 [77.0–88.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.0 [93.4–98.5] | 7.0 [3.5–10.5] | 7.0 [3.5–10.5] | 5.5% | 13.3% | 0.8% | Score runoff (VSE\* sincerity) |
| Minimax | minimax-family | 81.4 [75.5–86.6] | 99.0 [97.9–99.8] | 97.9 [97.8–98.0] | 76.3 | 79.6 | 82.8 [77.0–88.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.0 [93.4–98.5] | 7.0 [3.5–10.5] | 7.0 [3.5–10.5] | 5.6% | 13.3% | 0.8% | Score runoff (VSE\* sincerity) |
| MMPO | minimax-family | 81.4 [75.2–87.0] | 99.6 [99.4–99.9] | 98.2 [98.1–98.4] | 76.4 | 79.6 | 82.8 [77.0–88.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.2 [93.4–98.5] | 7.0 [3.5–10.5] | 7.0 [3.5–10.5] | 0.1% | 18.1% | 0.0% | — |
| Plurality | plurality-based | 77.8 [70.7–84.6] | 80.7 [75.2–85.6] | 98.4 [98.3–98.6] | 64.2 | 66.2 | 69.2 [59.2–78.1] | 71.8 [65.2–78.1] | 64.9 [58.5–71.5] | 71.2 [65.1–77.8] | 11.5 [7.5–16.0] | 8.5 [4.5–12.0] | 7.3% | 0.0% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +24 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 51.6% |
| Supplementary vote | 49.4% |
| Approval | 49.0% |
| IRV | 47.9% |
| Score | 46.6% |
| ER-IRV | 46.5% |
| IRV-3 | 46.2% |
| Approval runoff | 46.0% |
| Approval cond. runoff | 45.9% |
| TTR | 45.6% |
| MAV | 45.3% |
| 321 | 45.3% |
| MARS | 45.1% |
| BTR-score | 44.9% |
| Soft Minimax-D | 44.9% |
| ARA | 44.9% |
| Schulze | 44.7% |
| Minimax | 44.7% |
| STAR | 44.6% |
| BTR-IRV | 44.3% |
| Ranked pairs | 44.2% |
| MJ | 44.1% |
| Score runoff | 44.0% |
| Smith//Score | 43.9% |
| Kemeny-Young | 43.5% |
| MMPO | 43.3% |
| MMWV | 43.1% |
| Copeland | 43.1% |
| Borda | 43.1% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 198 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 2 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Score | 87.4% (86.9%) | 10.1% (10.1%) | 2.5% (3.0%) | 50.0% (50.0%) | 91.8% (92.2%) | 84.8% |
| MAV | 87.9% (86.9%) | 10.6% (10.1%) | 1.5% (3.0%) | 50.0% (50.0%) | 83.7% (82.6%) | 79.3% |
| ARA | 88.4% (86.9%) | 9.1% (10.1%) | 2.5% (3.0%) | 100.0% (50.0%) | 95.4% (94.3%) | 87.6% |
| MJ | 87.4% (86.9%) | 10.6% (10.1%) | 2.0% (3.0%) | 100.0% (50.0%) | 85.7% (86.0%) | 79.3% |
| Supplementary vote | 96.0% (100.0%) | 2.5% (0.0%) | 1.5% (0.0%) | 50.0% (0.0%) | 80.4% (75.5%) | 79.0% |
| Smith//Score | 87.4% (86.9%) | 8.6% (10.1%) | 4.0% (3.0%) | 50.0% (50.0%) | 100.0% (100.0%) | 88.4% |
| Soft Minimax-D | 87.4% (86.9%) | 9.1% (10.1%) | 3.5% (3.0%) | 100.0% (50.0%) | 99.0% (99.5%) | 86.6% |
| 321 | 81.3% (80.8%) | 16.2% (15.7%) | 2.5% (3.5%) | 100.0% (50.0%) | 96.4% (96.4%) | 79.8% |
| BTR-score | 88.4% (86.9%) | 8.6% (10.1%) | 3.0% (3.0%) | 50.0% (50.0%) | 100.0% (100.0%) | 89.6% |
| STAR | 85.9% (86.9%) | 9.6% (10.1%) | 4.5% (3.0%) | 50.0% (50.0%) | 99.5% (97.9%) | 86.4% |
| MARS | 86.9% (86.9%) | 9.6% (10.1%) | 3.5% (3.0%) | 50.0% (50.0%) | 96.9% (96.6%) | 87.9% |
| IRV | 93.9% (100.0%) | 4.0% (0.0%) | 2.0% (0.0%) | 50.0% (0.0%) | 89.3% (85.4%) | 87.0% |
| IRV-3 | 88.9% (91.4%) | 5.6% (3.5%) | 5.6% (5.1%) | 50.0% (50.0%) | 92.6% (89.4%) | 85.6% |
| Copeland | 94.9% (100.0%) | 3.0% (0.0%) | 2.0% (0.0%) | 50.0% (0.0%) | 100.0% (100.0%) | 95.9% |
| ER-IRV | 93.9% (100.0%) | 4.0% (0.0%) | 2.0% (0.0%) | 0.0% (0.0%) | 89.4% (85.4%) | 89.4% |
| BTR-IRV | 95.5% (100.0%) | 3.0% (0.0%) | 1.5% (0.0%) | 50.0% (0.0%) | 100.0% (100.0%) | 96.5% |
| Ranked pairs | 96.0% (100.0%) | 2.5% (0.0%) | 1.5% (0.0%) | 0.0% (0.0%) | 100.0% (100.0%) | 97.0% |
| Borda | 93.9% (100.0%) | 3.5% (0.0%) | 2.5% (0.0%) | 100.0% (0.0%) | 87.4% (85.9%) | 85.6% |
| Kemeny-Young | 95.5% (100.0%) | 3.0% (0.0%) | 1.5% (0.0%) | 50.0% (0.0%) | 100.0% (100.0%) | 96.5% |
| MMWV | 94.4% (100.0%) | 2.0% (0.0%) | 3.5% (0.0%) | 0.0% (0.0%) | 100.0% (100.0%) | 96.0% |
| Schulze | 94.9% (100.0%) | 3.5% (0.0%) | 1.5% (0.0%) | 0.0% (0.0%) | 100.0% (100.0%) | 96.0% |
| Minimax | 94.9% (100.0%) | 3.5% (0.0%) | 1.5% (0.0%) | 50.0% (0.0%) | 100.0% (100.0%) | 96.0% |
| MMPO | 95.5% (100.0%) | 2.0% (0.0%) | 2.5% (0.0%) | 0.0% (0.0%) | 100.0% (100.0%) | 96.2% |

![strategy arrows](arrows-c1.png)

![strategy shifts](shift-c1.png)

![sincerity](sincerity-c1.png)

![fidelity](fidelity-c1.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c1.png)

Scatter and chain plots leave out Random pair runoff, Anti-plurality, TRASH, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
