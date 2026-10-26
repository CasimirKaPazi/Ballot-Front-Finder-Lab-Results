# Ballot Front Finder Lab — analysis

Input: vsesim/01-strategic=0.5, vsesim/02-strategic=1.0 (simulator version 1.0.0; analysis version 1.0.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

How to read this:

- The values are the result. The **front** is judged on VSE\*, WME\* and sincerity: the result with strategic voters, and how honest they can afford to be. The **Pareto front** uses the point estimates as they are: a method is on it if no other is at least as good on all three and better on one, however small the difference (sincerity: below 1 point counts as a tie). **Undominated** asks for clear differences instead (see Dominated by). The two need not agree: a noise-sized advantage on one axis keeps a method on the Pareto front, but does not protect it from a method clearly better on another axis; a method beaten by a hair on every axis leaves the Pareto front but can stay undominated. Honest VSE and WME depend more on modelling assumptions and are shown for context. **Dominated by** lists the methods that are clearly better on at least one of the three front axes and clearly worse on none, with the axes on which they are better in brackets. Clearly means the paired 95% interval of the difference excludes 0; smaller differences count as ties. Sincerity differences are measured very precisely, but below 1 point no voter is noticeably more honest, so they count as ties too, here and on the Pareto front. It is a pointer for interpretation, relative to the methods included, not a verdict.
- **Sincerity**: how far the learned ballots go against the voters' own preferences, within each method's ballot format: the utility-weighted pairs a ballot puts in the wrong order, relative to the most dishonest ballot the format allows (100 = no reversed preference, 0 = as far against one's preferences as the format permits). Ties (exaggeration, levelling) are not reversals. **Fidelity**: how well the ballots reproduce the voters' utility differences (100 = exactly, a tie loses half a pair, a mirrored pair all of it); it includes what the format cannot express. **(honest)** is the fidelity of the sincere ballots: the format's own loss; the drop from there to Fidelity is what strategy adds (docs/analysis.md, Sincerity and fidelity). Both are undefined (—) for TRASH and the Inverse-\* methods, whose honest ballot works against the voter's own interest; on the front they count as ties on sincerity.
- The intervals in the tables are for each value on its own. They are wide because elections differ a lot from each other. Comparisons between methods are much more precise, because all methods face the same elections: use the paired differences for those, not overlapping intervals.
- Two methods whose paired difference interval includes 0 are tied at this sample size; their order in the table is not a finding.
- **VSE** and **WME** are efficiencies with 0 = random ballot and 100 = the best candidate of each election (values above 100 cannot be reached). VSE measures total utility; WME the winner's worst head-to-head margin by sincere preferences (a refined Condorcet measure; every election counts; Minimax reaches 100 with honest ballots by construction). CE (Condorcet efficiency, elections with a Condorcet winner only, elected or not) is shown for comparison and not used for dominance. **Low** and **Low\***: the share of elections whose winner (the expected total utility of the winner lottery) lies below the median candidate's total utility, with honest and with learned ballots: how often a method elects a poor candidate, which the averages of VSE hide. Not used for dominance.
- The strategic axes (VSE\*, WME\*) give the result after the poll rounds: voters see polls of the previous round (the method's counts only) and answer them all at once (docs/model.md, Strategy engine). Under convergent learning (--converge) they describe a long-run state instead and depend on the group size.
- **Compromise, burial, push-over** are shares of voters whose learned ballot reverses a real preference, by kind (see strategy.strategy_types): compromise raises a liked second choice above the favourite, push-over raises a disliked candidate to the top, burial keeps the favourite on top and reverses a preference below it.

## Across conditions

![axes](axes-heatmap.png)

**Sweeps:** for each parameter varied around the base, the methods' learned values as a chain of arrows in the order of the parameter's values (light to dark). Parameters that only affect learning start each chain at the honest point; others show the honest values as a dashed chain. Parameters changed together (e.g. exits and clones) form one chain.

![sweep over strategic share](chain-strategic.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis). A condition near or above that row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 1.00 | 1.00 | 0.99 | 1.00 | 1.00 |
| c2: strategic share 1.0 | 1.00 | 1.00 | 0.97 | 0.86 | 0.97 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 87.3% | 91.6% | 1.1% | — |
| c2: strategic share 1.0 | 87.3% | 91.6% | 1.1% | — |

## c1: base

15000 elections (seeds 1), 13095 with a Condorcet winner. 40 voters, 6 candidates, model ks-quinn: Quinn's hierarchical model as in vse-sim's code (one offset per bundle), faction alpha 1.5, cut 0.2, linear utility; 1 poll round(s), 10 polls of 21 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.5, scores 0..10. Electorates: Condorcet winner in 87.3%; it also has the highest total utility in 91.6% of those; electing it loses 1.1% of VSE; favourite ÷ least liked: — (utility not positive). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 97.0 to 98.8; with the best VSE (100) WME 96.5 (nothing lower is possible there either); the most balanced point VSE 99.3 / WME 99.7. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 98.5 (at WME 99.5); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -60.7 / WME -44.8.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): STAR, Ranked pairs.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): STAR, Ranked pairs, Schulze.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| STAR | hybrid | 97.4 [97.2–97.5] | 94.0 [93.6–94.3] | 99.8 [99.8–99.8] | 86.1 | 97.0 | 97.7 [97.6–97.9] | 93.1 [92.7–93.4] | 88.6 [88.1–89.1] | 89.0 [88.4–89.5] | 0.8 [0.6–0.9] | 0.8 [0.6–0.9] | 0.1% | 2.6% | 0.0% | — |
| Ranked pairs | condorcet | 97.2 [97.0–97.4] | 97.8 [97.6–98.0] | 96.5 [96.4–96.5] | 77.6 | 82.7 | 97.9 [97.8–98.1] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.0 [95.7–96.3] | 0.3 [0.2–0.4] | 0.5 [0.4–0.6] | 8.6% | 11.6% | 1.7% | — |
| Schulze | condorcet | 97.2 [97.0–97.4] | 97.7 [97.6–97.9] | 96.4 [96.4–96.4] | 77.5 | 82.7 | 97.9 [97.8–98.1] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.0 [95.7–96.3] | 0.3 [0.2–0.4] | 0.5 [0.4–0.6] | 8.6% | 11.7% | 1.7% | — |
| Score | rated | 95.7 [95.5–96.0] | 89.0 [88.6–89.5] | 99.9 [99.9–99.9] | 82.5 | 97.0 | 95.2 [95.0–95.4] | 86.1 [85.6–86.5] | 76.8 [76.1–77.4] | 80.5 [79.8–81.1] | 2.0 [1.8–2.2] | 1.3 [1.1–1.5] | 0.0% | 1.7% | 0.0% | STAR (VSE\* WME\*) |
| MJ | median-based | 94.7 [94.5–94.9] | 86.1 [85.6–86.6] | 99.8 [99.8–99.8] | 85.3 | 97.0 | 93.1 [92.8–93.4] | 78.9 [78.2–79.5] | 69.4 [68.6–70.2] | 77.3 [76.6–78.0] | 1.5 [1.3–1.7] | 1.8 [1.5–2.0] | 0.1% | 2.6% | 0.0% | Score (VSE\* WME\*), STAR (VSE\* WME\*) |
| MAV | median-based | 94.2 [94.0–94.5] | 84.8 [84.3–85.4] | 99.8 [99.8–99.8] | 85.5 | 97.0 | 92.0 [91.7–92.3] | 76.1 [75.4–76.8] | 66.5 [65.7–67.3] | 75.8 [75.1–76.6] | 2.1 [1.9–2.3] | 2.0 [1.7–2.2] | 0.2% | 2.6% | 0.0% | Score (VSE\* WME\*), MJ (VSE\* WME\*), STAR (VSE\* WME\*) |
| 321 | hybrid | 94.1 [93.8–94.4] | 84.1 [83.5–84.7] | 99.6 [99.6–99.7] | 75.3 | 85.1 | 91.9 [91.6–92.2] | 73.5 [72.6–74.2] | 67.3 [66.5–68.1] | 75.7 [75.0–76.5] | 2.4 [2.2–2.7] | 1.9 [1.6–2.1] | 0.3% | 4.2% | 0.0% | Score (VSE\* WME\*), MJ (VSE\* WME\*), STAR (VSE\* WME\*) +1 more |
| Borda | positional | 93.8 [93.4–94.1] | 89.0 [88.5–89.5] | 95.2 [95.1–95.2] | 72.7 | 82.7 | 94.6 [94.3–95.0] | 92.5 [92.1–92.8] | 84.9 [84.3–85.4] | 81.4 [80.7–82.0] | 0.5 [0.4–0.6] | 1.2 [1.0–1.4] | 14.2% | 13.0% | 2.5% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity) +1 more |
| Approval | approval-based | 90.7 [90.3–91.1] | 80.8 [80.2–81.5] | 99.7 [99.7–99.7] | 58.3 | 61.2 | 88.7 [88.2–89.1] | 75.7 [75.0–76.3] | 67.2 [66.3–67.9] | 72.1 [71.4–72.9] | 5.8 [5.4–6.2] | 4.4 [4.1–4.7] | 0.1% | 3.5% | 0.0% | Score (VSE\* WME\*), MJ (VSE\* WME\*), MAV (VSE\* WME\*) +2 more |
| IRV | plurality-based | 90.1 [89.7–90.5] | 81.7 [81.1–82.3] | 95.8 [95.8–95.9] | 76.6 | 82.7 | 86.1 [85.6–86.5] | 72.9 [72.2–73.6] | 67.4 [66.6–68.2] | 76.0 [75.3–76.7] | 7.1 [6.7–7.5] | 5.1 [4.8–5.4] | 9.6% | 11.8% | 3.4% | Score (VSE\* WME\* sincerity), MJ (VSE\* WME\* sincerity), MAV (VSE\* WME\* sincerity) +5 more |
| Plurality | plurality-based | 70.0 [69.1–70.9] | 60.8 [59.9–61.6] | 97.7 [97.7–97.8] | 50.2 | 52.9 | 59.0 [58.0–60.2] | 53.0 [52.1–53.9] | 50.4 [49.6–51.2] | 55.9 [55.1–56.8] | 22.4 [21.7–23.0] | 16.8 [16.2–17.4] | 12.2% | 0.0% | 2.7% | Approval (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity), MJ (VSE\* WME\* sincerity) +3 more |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 13095 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 1905 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| STAR | 89.8% (90.8%) | 5.8% (4.8%) | 4.4% (4.4%) | 63.7% (59.4%) | 95.8% (95.0%) | 89.0% |
| Ranked pairs | 93.6% (100.0%) | 1.9% (0.0%) | 4.5% (0.0%) | 23.1% (0.0%) | 100.0% (100.0%) | 96.0% |
| Schulze | 93.5% (100.0%) | 1.8% (0.0%) | 4.7% (0.0%) | 22.2% (0.0%) | 100.0% (100.0%) | 96.0% |
| Score | 89.2% (90.8%) | 6.1% (4.8%) | 4.7% (4.4%) | 67.0% (59.4%) | 84.4% (80.7%) | 80.5% |
| MJ | 87.8% (90.8%) | 6.6% (4.8%) | 5.7% (4.4%) | 67.9% (59.4%) | 82.2% (71.0%) | 77.3% |
| MAV | 88.0% (90.8%) | 6.9% (4.8%) | 5.1% (4.4%) | 67.2% (59.4%) | 80.6% (67.9%) | 75.8% |
| 321 | 75.4% (65.2%) | 17.7% (20.8%) | 6.9% (14.0%) | 83.8% (77.0%) | 95.0% (93.6%) | 75.7% |
| Borda | 88.6% (100.0%) | 4.1% (0.0%) | 7.4% (0.0%) | 35.1% (0.0%) | 85.6% (84.9%) | 81.4% |
| IRV | 88.4% (100.0%) | 5.2% (0.0%) | 6.4% (0.0%) | 41.6% (0.0%) | 81.4% (67.4%) | 76.0% |

![strategy arrows](arrows-c1.png)

![strategy shifts](shift-c1.png)

![sincerity](sincerity-c1.png)

![fidelity](fidelity-c1.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c1.png)

## c2: strategic share 1.0

15000 elections (seeds 1), 13095 with a Condorcet winner. 40 voters, 6 candidates, model ks-quinn: Quinn's hierarchical model as in vse-sim's code (one offset per bundle), faction alpha 1.5, cut 0.2, linear utility; 1 poll round(s), 10 polls of 21 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 1.0, scores 0..10. Electorates: Condorcet winner in 87.3%; it also has the highest total utility in 91.6% of those; electing it loses 1.1% of VSE; favourite ÷ least liked: — (utility not positive). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 97.0 to 98.8; with the best VSE (100) WME 96.5 (nothing lower is possible there either); the most balanced point VSE 99.3 / WME 99.7. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 98.5 (at WME 99.5); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -60.7 / WME -44.8.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): STAR, Ranked pairs.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): STAR, Ranked pairs.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| STAR | hybrid | 96.0 [95.8–96.3] | 91.7 [91.2–92.1] | 99.5 [99.5–99.5] | 74.9 | 97.0 | 97.7 [97.6–97.9] | 93.1 [92.7–93.4] | 88.6 [88.1–89.1] | 86.0 [85.4–86.6] | 0.8 [0.6–0.9] | 1.3 [1.1–1.5] | 0.2% | 6.9% | 0.0% | — |
| Ranked pairs | condorcet | 95.3 [95.1–95.6] | 93.8 [93.5–94.2] | 93.2 [93.2–93.3] | 72.8 | 82.7 | 97.9 [97.8–98.1] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 90.8 [90.3–91.2] | 0.3 [0.2–0.4] | 1.1 [1.0–1.3] | 16.1% | 23.2% | 3.1% | — |
| Schulze | condorcet | 95.2 [95.0–95.5] | 93.6 [93.2–94.0] | 93.2 [93.1–93.2] | 72.6 | 82.7 | 97.9 [97.8–98.1] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 90.5 [90.0–90.9] | 0.3 [0.2–0.4] | 1.2 [1.0–1.3] | 16.2% | 23.5% | 3.1% | Ranked pairs (WME\*) |
| Score | rated | 95.0 [94.8–95.3] | 89.4 [88.9–89.8] | 99.8 [99.8–99.8] | 67.2 | 97.0 | 95.2 [95.0–95.4] | 86.1 [85.6–86.5] | 76.8 [76.1–77.4] | 82.1 [81.4–82.7] | 2.0 [1.8–2.2] | 1.4 [1.3–1.6] | 0.0% | 3.3% | 0.0% | STAR (VSE\* WME\*) |
| MJ | median-based | 94.8 [94.5–95.1] | 87.7 [87.2–88.3] | 99.6 [99.5–99.6] | 72.6 | 97.0 | 93.1 [92.8–93.4] | 78.9 [78.2–79.5] | 69.4 [68.6–70.2] | 80.5 [79.8–81.1] | 1.5 [1.3–1.7] | 1.7 [1.5–1.9] | 0.4% | 5.6% | 0.0% | STAR (VSE\* WME\*), Score (WME\*) |
| MAV | median-based | 94.4 [94.1–94.6] | 86.6 [86.1–87.1] | 99.5 [99.5–99.6] | 73.0 | 97.0 | 92.0 [91.7–92.3] | 76.1 [75.4–76.8] | 66.5 [65.7–67.3] | 79.5 [78.9–80.2] | 2.1 [1.9–2.3] | 2.2 [1.9–2.4] | 0.4% | 5.8% | 0.0% | Score (VSE\* WME\*), MJ (VSE\* WME\*), STAR (VSE\* WME\*) |
| 321 | hybrid | 92.6 [92.2–93.0] | 84.6 [84.0–85.2] | 99.3 [99.2–99.3] | 65.2 | 85.1 | 91.9 [91.6–92.2] | 73.5 [72.6–74.2] | 67.3 [66.5–68.1] | 77.0 [76.3–77.8] | 2.4 [2.2–2.7] | 2.6 [2.3–2.8] | 0.8% | 8.8% | 0.0% | Score (VSE\* WME\*), MJ (VSE\* WME\*), MAV (VSE\* WME\*) +1 more |
| Approval | approval-based | 90.6 [90.2–91.0] | 81.7 [81.1–82.3] | 99.4 [99.4–99.4] | 55.3 | 61.2 | 88.7 [88.2–89.1] | 75.7 [75.0–76.3] | 67.2 [66.3–67.9] | 73.4 [72.7–74.2] | 5.8 [5.4–6.2] | 3.9 [3.6–4.2] | 0.2% | 7.3% | 0.0% | Score (VSE\* WME\*), MJ (VSE\* WME\*), MAV (VSE\* WME\*) +2 more |
| IRV | plurality-based | 89.6 [89.1–90.0] | 81.8 [81.2–82.4] | 92.3 [92.2–92.3] | 71.1 | 82.7 | 86.1 [85.6–86.5] | 72.9 [72.2–73.6] | 67.4 [66.6–68.2] | 76.6 [75.9–77.3] | 7.1 [6.7–7.5] | 5.3 [4.9–5.6] | 19.2% | 22.2% | 6.3% | Score (VSE\* WME\* sincerity), MJ (VSE\* WME\* sincerity), MAV (VSE\* WME\* sincerity) +5 more |
| Borda | positional | 85.8 [85.0–86.4] | 78.3 [77.5–79.1] | 91.0 [90.9–91.0] | 61.4 | 82.7 | 94.6 [94.3–95.0] | 92.5 [92.1–92.8] | 84.9 [84.3–85.4] | 76.1 [75.3–76.8] | 0.5 [0.4–0.6] | 3.2 [3.0–3.5] | 28.6% | 24.4% | 4.9% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), IRV (VSE\* WME\* sincerity) +6 more |
| Plurality | plurality-based | 74.3 [73.5–75.1] | 63.5 [62.6–64.3] | 95.5 [95.4–95.6] | 47.5 | 52.9 | 59.0 [58.0–60.2] | 53.0 [52.1–53.9] | 50.4 [49.6–51.2] | 58.6 [57.9–59.5] | 22.4 [21.7–23.0] | 14.4 [13.9–14.9] | 26.4% | 0.0% | 5.3% | Approval (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity), MJ (VSE\* WME\* sincerity) +3 more |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 13095 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 1905 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| STAR | 85.5% (90.8%) | 7.3% (4.8%) | 7.1% (4.4%) | 57.8% (59.4%) | 95.7% (95.0%) | 86.0% |
| Ranked pairs | 86.3% (100.0%) | 4.1% (0.0%) | 9.6% (0.0%) | 28.3% (0.0%) | 100.0% (100.0%) | 90.8% |
| Schulze | 86.1% (100.0%) | 4.3% (0.0%) | 9.6% (0.0%) | 27.6% (0.0%) | 100.0% (100.0%) | 90.5% |
| Score | 86.3% (90.8%) | 7.6% (4.8%) | 6.1% (4.4%) | 55.4% (59.4%) | 88.7% (80.7%) | 82.1% |
| MJ | 83.4% (90.8%) | 8.9% (4.8%) | 7.7% (4.4%) | 64.0% (59.4%) | 88.4% (71.0%) | 80.5% |
| MAV | 83.4% (90.8%) | 9.3% (4.8%) | 7.4% (4.4%) | 64.3% (59.4%) | 87.0% (67.9%) | 79.5% |
| 321 | 76.3% (65.2%) | 16.8% (20.8%) | 6.9% (14.0%) | 80.9% (77.0%) | 96.3% (93.6%) | 77.0% |
| IRV | 80.3% (100.0%) | 10.0% (0.0%) | 9.7% (0.0%) | 49.2% (0.0%) | 85.9% (67.4%) | 76.6% |
| Borda | 79.2% (100.0%) | 7.2% (0.0%) | 13.6% (0.0%) | 37.6% (0.0%) | 83.6% (84.9%) | 76.1% |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

Scatter and chain plots leave out Random pair runoff, Anti-plurality, TRASH, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
