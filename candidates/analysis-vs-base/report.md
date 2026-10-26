# Ballot Front Finder Lab — analysis

Input: base, candidates/01-cands=4, candidates/02-cands=5, candidates/03-cands=7, candidates/04-cands=8, candidates/05-cands=9, candidates/06-cands=10 (simulator version 1.0.0; analysis version 1.0.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

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

![sweep over candidates](chain-cands.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis). A condition near or above that row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 0.99 | 1.00 | 0.97 | 0.99 | 0.99 |
| c2: candidates 4 | 0.89 | 0.99 | 0.93 | 0.95 | 0.93 |
| c3: candidates 5 | 0.96 | 0.99 | 0.94 | 0.98 | 0.97 |
| c4: candidates 7 | 0.96 | 1.00 | 0.95 | 0.98 | 0.96 |
| c5: candidates 8 | 0.99 | 0.99 | 0.95 | 0.97 | 0.96 |
| c6: candidates 9 | 0.95 | 0.99 | 0.96 | 0.97 | 0.95 |
| c7: candidates 10 | 0.95 | 0.99 | 0.94 | 0.98 | 0.94 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 97.6% | 68.6% | 16.2% | 8.0 |
| c2: candidates 4 | 98.9% | 73.1% | 19.6% | 5.2 |
| c3: candidates 5 | 98.0% | 73.3% | 17.5% | 7.0 |
| c4: candidates 7 | 97.1% | 65.1% | 17.2% | 9.8 |
| c5: candidates 8 | 96.6% | 59.3% | 17.8% | 10.8 |
| c6: candidates 9 | 95.0% | 61.7% | 15.9% | 12.0 |
| c7: candidates 10 | 94.7% | 60.9% | 15.6% | 12.9 |

## c1: base

2000 elections (seeds 1), 1952 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 97.6%; it also has the highest total utility in 68.6% of those; electing it loses 16.2% of VSE; favourite ÷ least liked 8.0 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 83.1 to 83.4; with the best VSE (100) WME 77.0 (nothing lower is possible there either); the most balanced point VSE 94.4 / WME 95.9. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 90.5 (at WME 92.6); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -60.1 / WME -53.7.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, DMS, ESR, ARA, Soft Minimax-40, MARS-T, Smith//Score, BTR-score, Score runoff, Copeland, Ranked pairs, Kemeny-Young.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Geometric median, ARA, MARS-T, Smith//Score, Soft Minimax-D, BTR-score, Soft Minimax, Score runoff, Ranked pairs, Kemeny-Young, Schulze.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 90.3 [89.1–91.5] | 90.4 [89.3–91.5] | 99.9 [99.9–99.9] | 90.1 | 94.1 | 91.2 [90.1–92.4] | 90.1 [88.9–91.3] | 79.3 [77.4–81.2] | 79.7 [78.0–81.5] | 2.5 [1.8–3.1] | 2.9 [2.1–3.5] | 0.1% | 1.2% | 0.0% | — |
| DMS | rated | 89.5 [88.1–90.8] | 90.9 [89.8–92.0] | 99.9 [99.9–100.0] | 89.7 | 94.1 | 90.3 [89.1–91.5] | 89.5 [88.2–90.7] | 79.1 [77.2–80.9] | 80.8 [79.1–82.6] | 3.0 [2.3–3.9] | 2.9 [2.2–3.6] | 0.0% | 0.8% | 0.0% | ESR (WME\*), ARA (WME\*) |
| ESR | rated | 89.1 [87.8–90.4] | 93.2 [92.2–94.1] | 99.9 [99.9–99.9] | 90.0 | 94.1 | 89.5 [88.2–90.8] | 93.6 [92.7–94.5] | 85.1 [83.5–86.7] | 84.6 [83.0–86.1] | 2.9 [2.1–3.6] | 2.9 [2.1–3.6] | 0.1% | 1.0% | 0.0% | ARA (WME\*), Soft Minimax-40 (WME\*) |
| MAV | median-based | 88.8 [87.4–90.1] | 90.1 [88.9–91.2] | 99.9 [99.9–99.9] | 89.8 | 94.1 | 89.0 [87.6–90.3] | 87.6 [86.2–88.8] | 77.1 [75.2–78.9] | 80.0 [78.1–81.7] | 2.6 [2.0–3.4] | 2.8 [2.1–3.5] | 0.0% | 1.0% | 0.0% | Score (WME\*), ESR (WME\*), Geometric median (VSE\*) +4 more |
| ARA | hybrid | 88.7 [87.5–90.0] | 94.6 [93.9–95.4] | 99.9 [99.9–99.9] | 90.1 | 94.1 | 89.3 [88.1–90.5] | 94.6 [93.8–95.4] | 85.9 [84.4–87.5] | 86.4 [85.0–87.8] | 2.5 [1.8–3.1] | 2.8 [2.1–3.5] | 0.0% | 1.0% | 0.0% | — |
| Soft Minimax-40 | hybrid | 88.5 [87.1–89.9] | 94.8 [94.0–95.5] | 99.9 [99.9–99.9] | 90.3 | 94.1 | 89.1 [87.8–90.3] | 94.5 [93.6–95.2] | 85.7 [84.1–87.1] | 86.3 [84.9–87.8] | 2.7 [2.0–3.4] | 2.7 [2.0–3.5] | 0.0% | 1.0% | 0.0% | MARS-T (WME\*) |
| Score | rated | 88.4 [87.0–89.7] | 92.2 [91.1–93.1] | 100.0 [99.9–100.0] | 89.7 | 94.1 | 90.2 [89.0–91.4] | 92.7 [91.8–93.7] | 83.0 [81.3–84.7] | 82.2 [80.5–83.9] | 2.8 [2.1–3.5] | 3.1 [2.4–4.0] | 0.0% | 0.8% | 0.0% | ESR (WME\*), MARS (WME\*), MARS-T (WME\*) +3 more |
| 321 | hybrid | 88.3 [86.9–89.7] | 91.6 [90.5–92.7] | 99.9 [99.9–99.9] | 82.9 | 85.8 | 88.8 [87.4–90.2] | 90.5 [89.3–91.6] | 81.2 [79.4–82.9] | 82.7 [81.0–84.3] | 2.8 [2.1–3.5] | 2.9 [2.1–3.6] | 0.1% | 1.7% | 0.0% | Smith//Score (WME\*), ESR (WME\*), MARS (WME\*) +4 more |
| MJ | median-based | 88.2 [86.8–89.5] | 90.8 [89.6–91.8] | 99.9 [99.9–99.9] | 89.8 | 94.1 | 88.8 [87.4–90.0] | 89.5 [88.3–90.7] | 79.8 [78.0–81.5] | 80.8 [79.0–82.5] | 2.4 [1.7–3.0] | 2.4 [1.8–3.0] | 0.0% | 1.0% | 0.0% | Smith//Score (WME\*), Score (WME\*), DMS (VSE\*) +7 more |
| MARS-T | hybrid | 87.9 [86.5–89.3] | 95.5 [94.8–96.2] | 99.9 [99.9–100.0] | 90.0 | 94.1 | 88.2 [86.9–89.6] | 95.5 [94.8–96.2] | 87.3 [85.7–88.8] | 87.3 [85.9–88.8] | 3.0 [2.3–3.8] | 3.1 [2.4–4.0] | 0.0% | 0.8% | 0.0% | — |
| MARS | hybrid | 87.3 [85.8–88.8] | 95.4 [94.6–96.1] | 99.9 [99.9–100.0] | 90.1 | 94.1 | 87.9 [86.6–89.3] | 95.6 [94.9–96.3] | 87.6 [86.1–89.1] | 87.1 [85.6–88.5] | 3.1 [2.4–4.0] | 2.9 [2.2–3.7] | 0.0% | 0.8% | 0.0% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax (WME\*) +2 more |
| Smith//Score | condorcet-based | 87.3 [85.8–88.7] | 96.1 [95.4–96.7] | 99.9 [99.9–99.9] | 90.4 | 94.1 | 87.1 [85.6–88.6] | 96.2 [95.5–96.8] | 89.1 [87.7–90.5] | 88.8 [87.5–90.2] | 3.1 [2.4–3.9] | 2.9 [2.2–3.6] | 0.1% | 1.0% | 0.0% | — |
| Soft Minimax-D | hybrid | 87.2 [85.7–88.7] | 95.9 [95.2–96.6] | 99.9 [99.9–100.0] | 90.3 | 94.1 | 87.4 [85.9–88.8] | 95.9 [95.2–96.6] | 88.4 [87.1–89.8] | 88.5 [87.1–89.9] | 3.1 [2.4–3.9] | 3.0 [2.2–3.8] | 0.1% | 0.9% | 0.0% | — |
| Approval | approval-based | 87.0 [85.4–88.6] | 87.5 [86.2–88.7] | 99.9 [99.9–99.9] | 69.8 | 71.0 | 86.0 [84.3–87.6] | 83.5 [82.0–85.0] | 73.3 [71.3–75.2] | 77.4 [75.6–79.1] | 5.1 [4.2–6.0] | 4.1 [3.2–5.0] | 0.0% | 1.3% | 0.0% | Score (VSE\* WME\*), MAV (VSE\* WME\*), DMS (VSE\* WME\*) +15 more |
| STAR | hybrid | 86.8 [85.4–88.3] | 95.5 [94.8–96.2] | 99.9 [99.9–99.9] | 90.1 | 94.1 | 87.9 [86.5–89.2] | 95.7 [95.0–96.5] | 88.3 [86.9–89.7] | 87.9 [86.5–89.3] | 2.8 [2.1–3.5] | 3.5 [2.6–4.2] | 0.1% | 1.2% | 0.0% | Smith//Score (WME\*), BTR-score (WME\*), MARS-T (VSE\*) +1 more |
| BTR-score | hybrid | 86.7 [85.1–88.2] | 96.3 [95.6–96.9] | 99.9 [99.9–99.9] | 90.4 | 94.1 | 87.0 [85.5–88.4] | 96.2 [95.6–96.9] | 89.3 [87.9–90.6] | 89.2 [87.8–90.5] | 3.1 [2.4–3.9] | 3.2 [2.5–4.0] | 0.1% | 1.0% | 0.0% | — |
| Soft Minimax | hybrid | 86.5 [84.9–88.0] | 96.1 [95.5–96.8] | 99.9 [99.9–99.9] | 90.5 | 94.1 | 86.9 [85.3–88.3] | 96.3 [95.6–96.9] | 89.4 [88.1–90.8] | 89.3 [88.0–90.6] | 3.2 [2.5–4.0] | 3.5 [2.7–4.3] | 0.1% | 1.0% | 0.0% | — |
| Approval runoff | two-round | 86.1 [84.6–87.6] | 94.6 [93.7–95.5] | 99.6 [99.6–99.7] | 69.0 | 71.0 | 86.2 [84.8–87.6] | 93.4 [92.3–94.4] | 90.3 [89.0–91.6] | 91.3 [90.1–92.5] | 3.1 [2.3–3.8] | 3.3 [2.5–4.0] | 0.2% | 2.9% | 0.1% | Smith//Score (VSE\* WME\*), Soft Minimax-D (VSE\* WME\*), Score runoff (WME\*) +6 more |
| Approval cond. runoff | two-round | 85.9 [84.3–87.5] | 93.7 [92.8–94.6] | 99.7 [99.7–99.7] | 69.3 | 71.0 | 87.2 [85.8–88.6] | 91.8 [90.7–92.9] | 86.2 [84.6–87.6] | 88.0 [86.5–89.3] | 3.0 [2.3–3.8] | 3.1 [2.5–3.9] | 0.1% | 2.3% | 0.1% | Smith//Score (VSE\* WME\*), MARS (VSE\* WME\*), MARS-T (VSE\* WME\*) +9 more |
| IRV-3 | plurality-based | 85.6 [83.9–87.2] | 91.0 [89.9–92.1] | 98.7 [98.7–98.8] | 75.9 | 78.3 | 85.3 [83.6–86.9] | 86.8 [85.4–88.1] | 80.5 [78.8–82.2] | 84.4 [82.8–86.0] | 4.3 [3.5–5.2] | 3.8 [2.9–4.5] | 5.3% | 7.5% | 1.4% | Smith//Score (VSE\* WME\* sincerity), ESR (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity) +16 more |
| Borda | positional | 85.5 [84.1–87.0] | 95.7 [95.0–96.4] | 97.7 [97.7–97.8] | 75.9 | 79.7 | 80.9 [79.0–82.9] | 95.4 [94.6–96.1] | 86.9 [85.3–88.5] | 87.3 [85.9–88.8] | 3.8 [3.0–4.6] | 3.2 [2.5–4.0] | 6.0% | 13.2% | 0.9% | Smith//Score (VSE\* sincerity), STAR (VSE\* sincerity), Score runoff (WME\* sincerity) +6 more |
| ER-IRV | plurality-based | 85.3 [83.7–86.9] | 91.3 [90.1–92.5] | 98.0 [98.0–98.1] | 76.1 | 79.7 | 85.4 [83.8–87.1] | 87.7 [86.3–89.0] | 83.3 [81.7–84.9] | 86.8 [85.4–88.3] | 3.8 [2.9–4.6] | 3.5 [2.7–4.3] | 1.4% | 17.6% | 0.3% | Smith//Score (VSE\* WME\* sincerity), ESR (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity) +17 more |
| Score runoff | two-round | 85.2 [83.6–86.7] | 98.0 [97.5–98.5] | 99.9 [99.8–99.9] | 89.9 | 94.1 | 85.3 [83.8–86.7] | 98.3 [97.9–98.8] | 96.5 [95.7–97.4] | 96.0 [95.1–96.9] | 3.0 [2.3–3.8] | 3.5 [2.8–4.3] | 0.2% | 1.5% | 0.0% | — |
| IRV | plurality-based | 84.9 [83.2–86.6] | 90.8 [89.6–92.0] | 97.6 [97.5–97.6] | 76.1 | 79.7 | 85.4 [83.8–87.1] | 87.7 [86.3–89.0] | 83.3 [81.7–84.9] | 86.1 [84.6–87.6] | 3.8 [2.9–4.6] | 3.7 [2.9–4.5] | 5.2% | 13.6% | 1.6% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity) +18 more |
| Supplementary vote | plurality-based | 84.3 [82.5–86.0] | 84.3 [82.7–85.8] | 97.7 [97.6–97.7] | 76.2 | 79.7 | 80.6 [78.4–82.7] | 79.6 [77.9–81.3] | 70.0 [68.1–72.0] | 74.2 [72.3–76.2] | 7.5 [6.5–8.6] | 5.7 [4.7–6.8] | 5.7% | 15.1% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +30 more |
| Copeland | condorcet | 83.6 [81.9–85.1] | 98.2 [97.8–98.6] | 97.8 [97.7–97.8] | 76.3 | 79.7 | 82.9 [81.2–84.5] | 99.5 [99.4–99.7] | 100.0 [100.0–100.0] | 96.4 [95.6–97.2] | 3.8 [3.0–4.6] | 3.3 [2.5–4.0] | 5.2% | 13.3% | 0.9% | Score runoff (VSE\* sincerity), Ranked pairs (WME\*), Schulze (WME\*) +4 more |
| Ranked pairs | condorcet | 83.5 [81.8–85.1] | 99.3 [99.0–99.6] | 97.9 [97.8–97.9] | 76.4 | 79.7 | 83.3 [81.5–84.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.0 [97.3–98.6] | 3.9 [3.1–4.7] | 4.2 [3.4–5.1] | 5.2% | 13.0% | 0.9% | — |
| Kemeny-Young | condorcet | 83.4 [81.7–85.1] | 99.4 [99.2–99.6] | 97.9 [97.8–97.9] | 76.4 | 79.7 | 83.1 [81.4–84.7] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 97.8 [97.2–98.5] | 4.0 [3.1–4.8] | 4.0 [3.2–4.9] | 5.1% | 12.9% | 0.9% | — |
| TTR | two-round | 83.3 [81.4–85.2] | 90.4 [89.2–91.7] | 96.7 [96.5–96.9] | 63.2 | 66.5 | 83.8 [81.9–85.7] | 84.8 [83.2–86.4] | 80.4 [78.7–82.0] | 85.9 [84.3–87.4] | 5.0 [4.0–5.9] | 5.0 [4.1–6.0] | 6.2% | 0.0% | 3.5% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +28 more |
| Schulze | condorcet | 83.2 [81.5–84.9] | 99.4 [99.1–99.6] | 97.9 [97.8–97.9] | 76.4 | 79.7 | 83.3 [81.6–84.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.8 [97.1–98.4] | 3.9 [3.1–4.7] | 4.0 [3.2–4.9] | 5.2% | 13.0% | 0.8% | — |
| BTR-IRV | condorcet-based | 83.2 [81.5–84.9] | 98.2 [97.8–98.7] | 97.8 [97.7–97.8] | 76.3 | 79.7 | 83.0 [81.3–84.6] | 99.7 [99.5–99.9] | 100.0 [100.0–100.0] | 95.5 [94.6–96.4] | 4.0 [3.2–4.9] | 4.0 [3.2–4.9] | 5.3% | 13.3% | 1.0% | Score runoff (VSE\* sincerity), Ranked pairs (WME\*), Schulze (WME\*) +4 more |
| MMWV | minimax-family | 83.1 [81.4–84.7] | 99.0 [98.7–99.3] | 98.3 [98.2–98.3] | 76.5 | 79.7 | 83.3 [81.6–84.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.1 [96.3–97.8] | 3.9 [3.1–4.7] | 4.1 [3.2–5.0] | 0.1% | 17.4% | 0.0% | Schulze (WME\*), Kemeny-Young (WME\*), Minimax (WME\*) |
| Minimax | minimax-family | 82.9 [81.2–84.7] | 99.4 [99.2–99.6] | 97.9 [97.8–97.9] | 76.4 | 79.7 | 83.3 [81.6–84.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.9 [97.2–98.5] | 3.9 [3.1–4.7] | 4.3 [3.5–5.2] | 5.2% | 13.1% | 0.8% | Ranked pairs (VSE\*), Kemeny-Young (VSE\*) |
| MMPO | minimax-family | 82.9 [81.1–84.5] | 99.1 [98.7–99.3] | 98.3 [98.2–98.3] | 76.5 | 79.7 | 83.3 [81.6–84.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.1 [96.3–97.8] | 3.9 [3.1–4.7] | 4.2 [3.3–5.1] | 0.1% | 17.5% | 0.0% | Kemeny-Young (WME\*), Minimax (WME\*) |
| Plurality | plurality-based | 79.8 [77.8–81.9] | 76.9 [75.0–78.7] | 98.3 [98.2–98.4] | 64.4 | 66.5 | 69.8 [67.0–72.7] | 69.0 [66.8–71.1] | 61.7 [59.6–63.9] | 68.1 [66.2–70.1] | 11.5 [10.0–13.0] | 7.3 [6.2–8.5] | 7.1% | 0.0% | 1.5% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +30 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 51.1% |
| Supplementary vote | 48.2% |
| Approval | 46.3% |
| Geometric median | 45.7% |
| IRV-3 | 45.3% |
| DMS | 44.9% |
| IRV | 44.9% |
| ESR | 44.7% |
| TTR | 44.5% |
| ER-IRV | 44.1% |
| Score | 44.0% |
| MARS | 43.8% |
| MARS-T | 43.7% |
| Soft Minimax-D | 43.5% |
| Approval cond. runoff | 43.4% |
| BTR-IRV | 43.3% |
| STAR | 43.3% |
| Approval runoff | 43.3% |
| ARA | 43.2% |
| Soft Minimax-40 | 43.0% |
| MAV | 42.9% |
| 321 | 42.9% |
| Smith//Score | 42.9% |
| BTR-score | 42.8% |
| Ranked pairs | 42.8% |
| Score runoff | 42.7% |
| Schulze | 42.7% |
| Minimax | 42.6% |
| MJ | 42.5% |
| Kemeny-Young | 42.5% |
| MMPO | 42.1% |
| Soft Minimax | 42.1% |
| MMWV | 42.0% |
| Copeland | 41.9% |
| Borda | 41.2% |

**Manipulation** (from honest ballots, see manipulation.py; after John Huang's votesim): for each candidate X other than the honest winner T, the voters who prefer X to T try the naive exaggerations of their ballot type for X against T, everyone else votes sincerely. One-sided: the share of elections in which some such coalition gains, and the VSE when the worst successful manipulation for the electorate is recorded (the honest result where none succeeds). Two-sided: T's supporters answer with whichever exaggeration for T counters it best; the share in which a coalition still gains, and the VSE recorded the same way. Compare VSE in the main table (honest).

| Method | Manipulable (one-sided) | VSE (one-sided) | Manipulable (two-sided) | VSE (two-sided) |
|---|---|---|---|---|
| TTR | 27.7 [25.7–29.6]% | 70.8 [68.3–73.1] | 21.9 [20.1–23.7]% | 74.5 [72.2–76.7] |
| Soft Minimax | 40.8 [38.6–43.0]% | 70.5 [68.3–72.5] | 12.5 [11.1–14.0]% | 81.0 [79.0–82.8] |
| Approval runoff | 38.5 [36.3–40.6]% | 70.3 [67.9–72.6] | 11.9 [10.4–13.4]% | 79.9 [78.1–81.8] |
| IRV-3 | 37.5 [35.3–39.6]% | 64.3 [61.7–67.0] | 21.9 [20.1–23.6]% | 74.1 [71.7–76.5] |
| Score runoff | 51.0 [48.8–53.2]% | 64.2 [61.9–66.7] | 5.7 [4.7–6.7]% | 81.1 [79.1–83.0] |
| BTR-score | 50.0 [47.9–52.2]% | 63.2 [60.7–65.6] | 13.0 [11.5–14.5]% | 80.4 [78.4–82.3] |
| Smith//Score | 50.6 [48.4–52.8]% | 61.0 [58.5–63.4] | 13.0 [11.6–14.5]% | 80.4 [78.4–82.2] |
| Soft Minimax-D | 57.1 [55.0–59.4]% | 59.6 [57.3–61.8] | 13.4 [11.9–14.8]% | 80.3 [78.3–82.2] |
| Soft Minimax-40 | 57.6 [55.5–59.7]% | 59.5 [57.2–61.8] | 16.4 [14.8–18.1]% | 80.0 [78.1–81.9] |
| Schulze | 57.1 [54.8–59.4]% | 57.0 [54.3–59.4] | 2.4 [1.8–3.0]% | 81.6 [79.7–83.3] |
| Minimax | 57.1 [54.8–59.4]% | 56.6 [54.0–59.1] | 2.4 [1.8–3.0]% | 81.6 [79.7–83.3] |
| STAR | 58.3 [56.1–60.6]% | 56.3 [53.6–58.9] | 13.8 [12.3–15.3]% | 80.3 [78.2–82.1] |
| Ranked pairs | 58.1 [55.9–60.4]% | 56.3 [53.6–58.7] | 2.4 [1.8–3.0]% | 81.4 [79.5–83.2] |
| MMWV | 57.1 [54.8–59.5]% | 56.2 [53.6–58.7] | 2.4 [1.8–3.0]% | 81.5 [79.6–83.3] |
| MMPO | 57.1 [54.8–59.5]% | 56.2 [53.5–58.6] | 2.4 [1.8–3.0]% | 81.2 [79.3–83.0] |
| Kemeny-Young | 60.2 [58.0–62.4]% | 53.9 [51.3–56.5] | 2.4 [1.8–3.0]% | 81.4 [79.5–83.2] |
| Approval cond. runoff | 64.2 [62.2–66.3]% | 53.5 [51.0–55.9] | 16.2 [14.6–17.9]% | 79.1 [77.2–81.1] |
| BTR-IRV | 59.9 [57.7–62.0]% | 52.5 [49.9–55.0] | 2.4 [1.8–3.0]% | 81.3 [79.3–83.1] |
| Geometric median | 67.8 [65.9–69.9]% | 51.3 [49.0–53.7] | 15.6 [14.0–17.2]% | 83.0 [81.1–84.9] |
| ARA | 67.8 [65.8–69.9]% | 50.3 [47.8–52.9] | 16.1 [14.5–17.7]% | 79.9 [77.9–81.8] |
| IRV | 53.3 [51.2–55.6]% | 48.1 [45.0–51.4] | 19.4 [17.6–21.1]% | 69.2 [66.4–72.0] |
| ER-IRV | 54.5 [52.4–56.7]% | 46.1 [43.0–49.4] | 19.3 [17.6–21.1]% | 68.9 [66.0–71.8] |
| Supplementary vote | 50.3 [48.0–52.5]% | 45.9 [42.6–49.4] | 32.4 [30.3–34.3]% | 65.2 [62.4–68.0] |
| Copeland | 73.4 [71.5–75.3]% | 44.3 [41.9–46.7] | 2.4 [1.8–3.0]% | 82.5 [80.7–84.1] |
| MARS | 69.3 [67.3–71.3]% | 44.3 [41.5–47.1] | 14.5 [13.0–16.1]% | 79.7 [77.7–81.7] |
| MARS-T | 70.5 [68.5–72.5]% | 42.6 [39.8–45.5] | 14.8 [13.3–16.5]% | 79.7 [77.7–81.7] |
| 321 | 72.1 [70.2–74.0]% | 42.6 [39.7–45.1] | 20.8 [19.1–22.7]% | 78.4 [76.3–80.4] |
| ESR | 68.0 [66.0–70.2]% | 41.7 [38.9–44.5] | 17.1 [15.4–18.9]% | 79.2 [77.1–81.2] |
| MAV | 75.0 [73.1–76.8]% | 37.9 [35.2–40.5] | 24.8 [23.0–26.8]% | 77.2 [75.0–79.3] |
| MJ | 76.6 [74.8–78.5]% | 36.4 [33.7–39.2] | 22.1 [20.3–24.1]% | 78.3 [76.3–80.2] |
| Approval | 77.0 [75.1–78.8]% | 26.5 [23.2–29.8] | 28.9 [27.1–31.1]% | 71.7 [69.1–74.0] |
| DMS | 86.2 [84.6–87.6]% | 13.9 [10.9–16.9] | 22.8 [20.9–24.8]% | 77.5 [75.2–79.6] |
| Score | 87.8 [86.3–89.1]% | 9.2 [6.3–12.2] | 19.4 [17.5–21.2]% | 78.7 [76.6–80.7] |
| Borda | 93.0 [92.0–94.1]% | -9.8 [-13.2–-6.4] | 15.2 [13.6–16.9]% | 73.9 [71.5–76.2] |
| Plurality | 72.5 [70.6–74.5]% | -10.3 [-14.7–-5.7] | 40.5 [38.2–42.5]% | 53.8 [50.6–57.0] |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 1952 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 48 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 87.7% (88.1%) | 8.8% (9.1%) | 3.5% (2.8%) | 62.5% (64.6%) | 87.7% (86.7%) | 79.7% |
| DMS | 87.6% (88.1%) | 9.3% (9.1%) | 3.1% (2.8%) | 66.7% (64.6%) | 87.9% (86.2%) | 80.8% |
| ESR | 87.6% (88.1%) | 9.7% (9.1%) | 2.7% (2.8%) | 62.5% (64.6%) | 94.2% (94.0%) | 84.6% |
| MAV | 87.4% (88.1%) | 9.7% (9.1%) | 2.9% (2.8%) | 68.8% (64.6%) | 86.8% (83.3%) | 80.0% |
| ARA | 87.6% (88.1%) | 9.4% (9.1%) | 3.0% (2.8%) | 66.7% (64.6%) | 95.2% (93.7%) | 86.4% |
| Soft Minimax-40 | 87.9% (88.1%) | 8.8% (9.1%) | 3.3% (2.8%) | 60.4% (64.6%) | 95.8% (94.4%) | 86.3% |
| Score | 87.6% (88.1%) | 9.6% (9.1%) | 2.9% (2.8%) | 64.6% (64.6%) | 89.8% (90.0%) | 82.2% |
| 321 | 81.9% (81.2%) | 14.7% (15.4%) | 3.4% (3.3%) | 87.5% (83.3%) | 96.8% (95.6%) | 82.7% |
| MJ | 87.8% (88.1%) | 9.1% (9.1%) | 3.1% (2.8%) | 75.0% (64.6%) | 87.6% (85.3%) | 80.8% |
| MARS-T | 88.2% (88.1%) | 8.9% (9.1%) | 2.9% (2.8%) | 58.3% (64.6%) | 96.2% (96.5%) | 87.3% |
| MARS | 87.7% (88.1%) | 9.4% (9.1%) | 2.9% (2.8%) | 56.2% (64.6%) | 97.0% (97.0%) | 87.1% |
| Smith//Score | 87.6% (88.1%) | 8.8% (9.1%) | 3.7% (2.8%) | 64.6% (64.6%) | 100.0% (100.0%) | 88.8% |
| Soft Minimax-D | 88.3% (88.1%) | 8.7% (9.1%) | 3.0% (2.8%) | 56.2% (64.6%) | 99.1% (99.2%) | 88.5% |
| STAR | 87.1% (88.1%) | 9.3% (9.1%) | 3.6% (2.8%) | 62.5% (64.6%) | 99.0% (98.8%) | 87.9% |
| BTR-score | 88.0% (88.1%) | 8.8% (9.1%) | 3.3% (2.8%) | 62.5% (64.6%) | 100.0% (100.0%) | 89.2% |
| Soft Minimax | 88.0% (88.1%) | 8.9% (9.1%) | 3.1% (2.8%) | 66.7% (64.6%) | 100.0% (100.0%) | 89.3% |
| IRV-3 | 86.8% (88.2%) | 8.5% (7.5%) | 4.7% (4.3%) | 47.9% (33.3%) | 93.6% (89.8%) | 84.4% |
| Borda | 95.3% (100.0%) | 2.5% (0.0%) | 2.2% (0.0%) | 54.2% (0.0%) | 89.2% (86.9%) | 87.3% |
| ER-IRV | 95.1% (100.0%) | 2.7% (0.0%) | 2.3% (0.0%) | 43.8% (0.0%) | 87.5% (83.3%) | 86.8% |
| IRV | 95.3% (100.0%) | 3.4% (0.0%) | 1.3% (0.0%) | 45.8% (0.0%) | 86.6% (83.3%) | 86.1% |
| Supplementary vote | 93.8% (100.0%) | 4.6% (0.0%) | 1.7% (0.0%) | 37.5% (0.0%) | 76.2% (70.0%) | 74.2% |
| Copeland | 95.5% (100.0%) | 1.8% (0.0%) | 2.6% (0.0%) | 33.3% (0.0%) | 100.0% (100.0%) | 96.4% |
| Ranked pairs | 97.0% (100.0%) | 1.5% (0.0%) | 1.5% (0.0%) | 27.1% (0.0%) | 100.0% (100.0%) | 98.0% |
| Kemeny-Young | 96.9% (100.0%) | 1.7% (0.0%) | 1.4% (0.0%) | 31.2% (0.0%) | 100.0% (100.0%) | 97.8% |
| Schulze | 96.9% (100.0%) | 1.5% (0.0%) | 1.5% (0.0%) | 31.2% (0.0%) | 100.0% (100.0%) | 97.8% |
| BTR-IRV | 94.9% (100.0%) | 2.8% (0.0%) | 2.3% (0.0%) | 31.2% (0.0%) | 100.0% (100.0%) | 95.5% |
| MMWV | 95.3% (100.0%) | 1.6% (0.0%) | 3.1% (0.0%) | 25.0% (0.0%) | 100.0% (100.0%) | 97.1% |
| Minimax | 97.0% (100.0%) | 1.5% (0.0%) | 1.5% (0.0%) | 31.2% (0.0%) | 100.0% (100.0%) | 97.9% |
| MMPO | 95.3% (100.0%) | 1.5% (0.0%) | 3.2% (0.0%) | 22.9% (0.0%) | 99.9% (100.0%) | 97.1% |

![strategy arrows](arrows-c1.png)

![strategy shifts](shift-c1.png)

![sincerity](sincerity-c1.png)

![fidelity](fidelity-c1.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c1.png)

## c2: candidates 4

700 elections (seeds 1), 692 with a Condorcet winner. 99 voters, 4 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 98.9%; it also has the highest total utility in 73.1% of those; electing it loses 19.6% of VSE; favourite ÷ least liked 5.2 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 78.3 to 79.1; with the best VSE (100) WME 76.4 (nothing lower is possible there either); the most balanced point VSE 93.5 / WME 95.4. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 84.6 (at WME 95.6); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -76.6 / WME -77.1.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, DMS, Score, MARS, MARS-T, Smith//Score, Soft Minimax-40, Soft Minimax, Kemeny-Young, Score runoff, Schulze, Minimax.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Smith//Score, Score runoff.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 86.9 [83.8–89.8] | 92.8 [90.9–94.4] | 99.9 [99.9–99.9] | 91.8 | 96.0 | 86.6 [83.8–89.4] | 93.1 [91.2–94.7] | 87.7 [85.3–90.0] | 87.1 [84.5–89.5] | 4.7 [3.3–6.3] | 4.4 [3.0–6.0] | 0.2% | 1.0% | 0.0% | Score (WME\*), DMS (WME\*), MARS (WME\*) |
| DMS | rated | 85.7 [82.4–88.6] | 94.3 [92.6–95.8] | 100.0 [99.9–100.0] | 91.4 | 96.0 | 85.8 [82.8–88.6] | 92.7 [90.8–94.5] | 86.6 [83.9–89.0] | 88.4 [86.1–90.6] | 4.9 [3.4–6.4] | 4.9 [3.3–6.6] | 0.0% | 0.4% | 0.0% | Smith//Score (WME\*), Score (WME\*), BTR-score (WME\*) +1 more |
| Score | rated | 84.8 [81.5–87.9] | 95.4 [94.0–96.5] | 100.0 [99.9–100.0] | 91.3 | 96.0 | 84.3 [81.3–87.3] | 94.7 [93.1–95.9] | 88.2 [85.7–90.5] | 89.4 [87.1–91.5] | 5.3 [3.6–7.0] | 4.6 [3.0–6.1] | 0.0% | 0.4% | 0.0% | Smith//Score (WME\*), MARS (WME\*), MARS-T (WME\*) +2 more |
| MARS | hybrid | 84.7 [81.3–87.8] | 96.9 [95.7–97.8] | 99.9 [99.9–99.9] | 91.5 | 96.0 | 83.7 [80.3–86.9] | 97.3 [96.3–98.1] | 92.8 [90.8–94.6] | 92.3 [90.3–93.9] | 5.7 [4.0–7.6] | 5.4 [3.7–7.3] | 0.1% | 0.8% | 0.0% | Soft Minimax (WME\*) |
| ESR | rated | 84.3 [80.7–87.6] | 94.4 [92.6–95.9] | 99.9 [99.9–99.9] | 91.5 | 96.0 | 85.0 [81.7–88.0] | 95.2 [93.7–96.6] | 90.4 [88.2–92.5] | 90.1 [87.9–92.2] | 5.3 [3.6–7.1] | 5.6 [3.9–7.4] | 0.0% | 0.6% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), BTR-score (WME\*) +4 more |
| 321 | hybrid | 84.2 [80.8–87.4] | 95.0 [93.5–96.4] | 99.9 [99.8–99.9] | 86.9 | 90.3 | 84.2 [80.9–87.4] | 94.0 [92.4–95.5] | 88.5 [86.1–90.8] | 89.7 [87.4–91.9] | 5.4 [3.7–7.1] | 5.4 [3.7–7.3] | 0.1% | 1.3% | 0.0% | Smith//Score (WME\*), STAR (WME\*), BTR-score (WME\*) +4 more |
| ARA | hybrid | 83.8 [80.2–87.1] | 95.5 [93.9–96.8] | 99.9 [99.9–99.9] | 91.7 | 96.0 | 83.9 [80.3–87.0] | 96.0 [94.5–97.1] | 91.3 [89.2–93.2] | 91.6 [89.6–93.6] | 5.3 [3.6–7.0] | 5.6 [4.0–7.4] | 0.0% | 0.6% | 0.0% | Smith//Score (WME\*), MARS-T (WME\*), Soft Minimax (WME\*) +1 more |
| MARS-T | hybrid | 83.8 [80.2–87.0] | 97.1 [96.1–97.9] | 99.9 [99.9–100.0] | 91.5 | 96.0 | 83.7 [80.3–86.9] | 97.3 [96.3–98.1] | 92.8 [90.8–94.6] | 92.5 [90.5–94.4] | 5.7 [4.0–7.6] | 6.1 [4.4–8.0] | 0.0% | 0.7% | 0.0% | MARS (VSE\*) |
| MAV | median-based | 83.3 [79.2–86.9] | 92.8 [90.8–94.5] | 99.9 [99.9–100.0] | 91.4 | 96.0 | 85.3 [81.6–88.5] | 90.0 [87.7–92.0] | 84.8 [82.3–87.3] | 87.4 [85.0–89.7] | 4.9 [3.3–6.6] | 5.7 [4.0–7.6] | 0.1% | 0.5% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), Score (WME\*) +9 more |
| MJ | median-based | 83.3 [79.5–86.8] | 93.3 [91.3–95.1] | 99.9 [99.9–100.0] | 91.3 | 96.0 | 84.6 [80.9–87.9] | 91.6 [89.4–93.6] | 87.1 [84.8–89.5] | 88.3 [86.0–90.8] | 5.0 [3.4–6.9] | 5.7 [4.0–7.6] | 0.0% | 0.5% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), Score (WME\*) +9 more |
| Smith//Score | condorcet-based | 83.2 [79.5–86.5] | 97.4 [96.1–98.3] | 99.9 [99.9–99.9] | 91.8 | 96.0 | 82.2 [78.4–85.6] | 97.5 [96.4–98.4] | 94.2 [92.5–95.8] | 94.4 [92.6–96.0] | 5.6 [3.9–7.4] | 5.7 [4.0–7.6] | 0.0% | 1.1% | 0.0% | — |
| Soft Minimax-40 | hybrid | 82.9 [79.3–86.3] | 97.4 [96.5–98.2] | 99.9 [99.9–99.9] | 91.8 | 96.0 | 83.6 [79.9–86.8] | 96.5 [95.3–97.5] | 92.1 [90.1–93.9] | 93.0 [91.1–94.7] | 5.6 [3.9–7.4] | 6.0 [4.3–7.9] | 0.1% | 1.0% | 0.0% | MARS (VSE\*) |
| BTR-score | hybrid | 82.7 [78.7–86.1] | 96.7 [95.5–97.8] | 99.9 [99.8–99.9] | 91.8 | 96.0 | 81.9 [77.9–85.4] | 97.7 [96.6–98.5] | 94.7 [92.9–96.2] | 93.9 [91.9–95.5] | 5.4 [3.7–7.3] | 5.7 [4.0–7.7] | 0.1% | 1.2% | 0.0% | Smith//Score (WME\*), Score runoff (WME\*), Soft Minimax (WME\*) |
| Approval | approval-based | 82.6 [78.8–86.1] | 90.0 [87.8–92.1] | 99.9 [99.9–99.9] | 76.4 | 77.8 | 82.7 [78.7–86.4] | 86.6 [84.0–88.9] | 81.0 [78.3–83.7] | 84.4 [81.9–86.9] | 6.7 [4.9–8.7] | 6.1 [4.4–8.0] | 0.0% | 0.7% | 0.0% | DMS (VSE\* WME\*), Geometric median (VSE\* WME\*), Smith//Score (WME\*) +15 more |
| Supplementary vote | plurality-based | 82.4 [78.2–85.8] | 91.9 [89.7–93.8] | 96.9 [96.8–97.0] | 78.1 | 82.9 | 82.0 [78.3–85.3] | 89.9 [87.5–92.2] | 87.0 [84.2–89.3] | 89.1 [86.8–91.2] | 6.3 [4.4–8.1] | 6.6 [4.7–8.6] | 3.5% | 16.9% | 1.4% | DMS (VSE\* WME\* sincerity), Smith//Score (WME\* sincerity), Approval runoff (WME\* sincerity) +26 more |
| Approval runoff | two-round | 82.3 [78.6–85.8] | 96.2 [94.7–97.5] | 99.2 [99.1–99.4] | 74.5 | 77.8 | 83.2 [79.3–86.6] | 95.3 [93.6–96.9] | 94.0 [92.2–95.7] | 95.0 [93.2–96.5] | 5.3 [3.7–7.1] | 5.9 [4.1–7.6] | 0.3% | 3.8% | 0.2% | Score runoff (WME\*), Soft Minimax (WME\*) |
| Borda | positional | 82.3 [78.3–85.8] | 97.0 [95.7–98.0] | 96.7 [96.5–96.9] | 77.6 | 82.9 | 75.2 [70.5–79.3] | 94.9 [93.4–96.3] | 89.0 [86.7–91.3] | 92.8 [91.0–94.7] | 5.9 [4.1–7.7] | 5.4 [3.7–7.1] | 4.0% | 17.8% | 1.3% | Score runoff (WME\* sincerity), MARS (VSE\* sincerity), Ranked pairs (WME\*) +11 more |
| Soft Minimax | hybrid | 82.2 [78.4–85.7] | 97.8 [96.7–98.6] | 99.9 [99.8–99.9] | 92.0 | 96.0 | 81.4 [77.4–84.9] | 97.7 [96.7–98.5] | 94.9 [93.2–96.3] | 94.9 [93.2–96.4] | 5.9 [4.1–7.9] | 5.7 [4.0–7.6] | 0.0% | 1.1% | 0.0% | Score runoff (WME\*) |
| STAR | hybrid | 82.0 [78.0–85.5] | 96.5 [95.2–97.6] | 99.8 [99.8–99.9] | 91.6 | 96.0 | 82.0 [78.1–85.6] | 96.9 [95.8–98.1] | 93.6 [91.8–95.4] | 92.9 [90.8–94.7] | 5.6 [3.9–7.3] | 6.0 [4.3–8.0] | 0.1% | 1.4% | 0.1% | Score (VSE\*), Score runoff (WME\*), MARS (VSE\*) +2 more |
| IRV-3 | plurality-based | 81.8 [77.6–85.5] | 94.6 [92.9–96.2] | 96.8 [96.6–96.9] | 77.9 | 82.9 | 81.6 [77.4–85.3] | 92.0 [89.5–94.2] | 91.1 [89.0–93.1] | 92.9 [91.0–94.7] | 6.9 [5.0–8.9] | 5.9 [4.1–7.7] | 3.5% | 15.9% | 1.8% | MARS (VSE\* WME\* sincerity), Smith//Score (WME\* sincerity), Approval runoff (WME\* sincerity) +19 more |
| Approval cond. runoff | two-round | 81.4 [77.2–85.2] | 92.2 [89.8–94.1] | 99.6 [99.5–99.7] | 75.6 | 77.8 | 84.7 [80.6–88.0] | 93.4 [91.5–95.1] | 89.7 [87.4–91.9] | 88.9 [86.5–91.2] | 5.3 [3.7–7.1] | 6.4 [4.7–8.3] | 0.1% | 2.1% | 0.2% | DMS (VSE\* WME\*), Smith//Score (WME\*), Approval runoff (WME\*) +12 more |
| ER-IRV | plurality-based | 81.1 [77.1–84.9] | 94.0 [91.9–95.9] | 97.4 [97.3–97.5] | 77.5 | 82.9 | 81.6 [77.4–85.3] | 92.0 [89.5–94.2] | 91.1 [89.0–93.1] | 93.1 [91.2–94.9] | 6.9 [5.0–8.9] | 6.7 [4.9–8.7] | 0.9% | 18.0% | 0.5% | MARS (VSE\* WME\* sincerity), Smith//Score (WME\* sincerity), Approval runoff (WME\* sincerity) +25 more |
| Kemeny-Young | condorcet | 81.0 [77.3–84.4] | 99.0 [98.3–99.6] | 97.1 [97.0–97.3] | 78.3 | 82.9 | 78.7 [74.4–82.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.0 [97.0–99.0] | 6.3 [4.6–8.3] | 5.9 [4.1–7.7] | 3.3% | 16.2% | 1.1% | Score runoff (sincerity) |
| Soft Minimax-D | hybrid | 81.0 [77.0–84.6] | 97.4 [96.5–98.3] | 99.9 [99.8–99.9] | 91.8 | 96.0 | 83.0 [79.2–86.3] | 97.5 [96.4–98.3] | 93.9 [91.8–95.5] | 94.4 [92.7–96.1] | 5.7 [4.0–7.6] | 6.0 [4.3–7.9] | 0.1% | 1.2% | 0.0% | Smith//Score (VSE\*), Score runoff (WME\*), MARS (VSE\*) +1 more |
| IRV | plurality-based | 80.9 [76.5–84.7] | 94.2 [92.3–96.0] | 96.8 [96.6–96.9] | 78.0 | 82.9 | 81.6 [77.4–85.3] | 92.0 [89.5–94.2] | 91.1 [89.0–93.1] | 93.1 [91.3–95.0] | 6.9 [5.0–8.9] | 6.3 [4.6–8.1] | 3.4% | 15.8% | 1.8% | MARS (VSE\* WME\* sincerity), MARS-T (VSE\* WME\* sincerity), Smith//Score (WME\* sincerity) +23 more |
| Score runoff | two-round | 80.4 [76.2–84.1] | 98.9 [98.3–99.5] | 99.7 [99.6–99.7] | 91.1 | 96.0 | 80.3 [76.2–83.8] | 98.5 [97.6–99.2] | 98.1 [97.1–99.0] | 97.8 [96.6–98.8] | 5.3 [3.6–7.0] | 6.1 [4.4–8.1] | 0.2% | 1.9% | 0.2% | — |
| TTR | two-round | 80.3 [76.4–84.1] | 94.0 [91.8–96.1] | 96.4 [95.9–96.8] | 68.6 | 72.5 | 81.7 [77.7–85.4] | 91.7 [89.3–93.9] | 90.8 [88.6–92.8] | 93.5 [91.5–95.3] | 6.3 [4.4–8.1] | 7.0 [5.0–8.9] | 4.3% | 0.0% | 4.1% | Smith//Score (VSE\* WME\* sincerity), MARS (VSE\* WME\* sincerity), MARS-T (VSE\* WME\* sincerity) +26 more |
| Ranked pairs | condorcet | 80.2 [76.2–83.8] | 99.0 [98.2–99.6] | 97.2 [97.0–97.3] | 78.3 | 82.9 | 78.7 [74.4–82.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.2 [97.2–99.2] | 6.3 [4.6–8.3] | 6.3 [4.6–8.3] | 3.1% | 16.1% | 1.1% | Score runoff (sincerity) |
| BTR-IRV | condorcet-based | 80.0 [75.8–83.7] | 97.8 [96.7–98.8] | 97.0 [96.9–97.2] | 78.1 | 82.9 | 80.3 [76.2–83.8] | 99.9 [99.7–100.0] | 100.0 [100.0–100.0] | 96.7 [95.5–98.0] | 6.3 [4.4–8.3] | 6.6 [4.7–8.6] | 3.3% | 16.9% | 1.1% | Smith//Score (VSE\* sincerity), Approval runoff (VSE\* sincerity), STAR (VSE\* sincerity) +12 more |
| Schulze | condorcet | 79.9 [75.9–83.6] | 99.2 [98.6–99.8] | 97.2 [97.0–97.3] | 78.3 | 82.9 | 78.7 [74.4–82.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.3 [97.3–99.2] | 6.3 [4.6–8.3] | 6.7 [4.9–8.7] | 3.2% | 16.2% | 1.0% | Kemeny-Young (VSE\*), Score runoff (sincerity) |
| Copeland | condorcet | 79.4 [75.4–83.1] | 97.2 [95.9–98.3] | 97.1 [97.0–97.2] | 78.2 | 82.9 | 79.1 [75.0–82.8] | 99.6 [99.3–99.9] | 100.0 [100.0–100.0] | 96.6 [95.2–97.8] | 6.0 [4.1–7.9] | 6.1 [4.4–8.1] | 3.2% | 17.2% | 1.0% | Smith//Score (VSE\* sincerity), Approval runoff (VSE\* sincerity), STAR (VSE\* sincerity) +13 more |
| Minimax | minimax-family | 79.3 [75.0–83.1] | 99.3 [98.8–99.8] | 97.2 [97.0–97.3] | 78.3 | 82.9 | 78.7 [74.4–82.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.3 [97.4–99.2] | 6.3 [4.6–8.3] | 6.7 [4.9–8.7] | 3.2% | 16.2% | 1.0% | Score runoff (sincerity) |
| MMWV | minimax-family | 79.0 [74.6–82.8] | 99.2 [98.4–99.7] | 97.6 [97.4–97.7] | 77.9 | 82.9 | 78.7 [74.4–82.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.3 [97.3–99.1] | 6.3 [4.6–8.3] | 6.3 [4.6–8.3] | 0.2% | 18.7% | 0.1% | Score runoff (VSE\* sincerity) |
| Plurality | plurality-based | 78.6 [74.3–82.8] | 85.9 [83.0–88.4] | 98.6 [98.5–98.7] | 70.6 | 72.5 | 75.7 [71.4–80.1] | 80.6 [77.5–83.8] | 75.8 [72.8–79.2] | 80.6 [77.7–83.6] | 9.3 [7.3–11.3] | 7.7 [5.7–9.9] | 5.1% | 0.0% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity) +17 more |
| MMPO | minimax-family | 78.3 [74.0–82.5] | 98.5 [97.3–99.6] | 97.6 [97.4–97.7] | 77.9 | 82.9 | 78.7 [74.4–82.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.0 [97.0–98.9] | 6.3 [4.6–8.3] | 6.1 [4.3–8.1] | 0.2% | 18.8% | 0.1% | Smith//Score (VSE\* sincerity), Score runoff (VSE\* sincerity), MARS-T (VSE\* sincerity) +4 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 60.8% |
| Approval | 57.6% |
| Supplementary vote | 56.5% |
| MAV | 56.2% |
| ESR | 56.1% |
| ER-IRV | 56.0% |
| DMS | 56.0% |
| TTR | 56.0% |
| IRV | 56.0% |
| Geometric median | 55.9% |
| Score | 55.9% |
| IRV-3 | 55.7% |
| MARS-T | 55.5% |
| MJ | 55.5% |
| BTR-IRV | 55.3% |
| ARA | 55.2% |
| MARS | 55.2% |
| Approval cond. runoff | 55.1% |
| 321 | 55.1% |
| Approval runoff | 54.9% |
| Score runoff | 54.9% |
| Smith//Score | 54.9% |
| STAR | 54.8% |
| BTR-score | 54.7% |
| Soft Minimax-D | 54.5% |
| MMWV | 54.5% |
| Soft Minimax-40 | 54.5% |
| MMPO | 54.5% |
| Kemeny-Young | 54.5% |
| Minimax | 54.3% |
| Copeland | 54.2% |
| Schulze | 54.1% |
| Ranked pairs | 54.0% |
| Borda | 53.9% |
| Soft Minimax | 53.5% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 692 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 8 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 93.2% (93.6%) | 5.2% (4.0%) | 1.6% (2.3%) | 25.0% (50.0%) | 91.8% (91.8%) | 87.1% |
| DMS | 93.4% (93.6%) | 4.3% (4.0%) | 2.3% (2.3%) | 25.0% (50.0%) | 92.6% (89.9%) | 88.4% |
| Score | 93.5% (93.6%) | 4.2% (4.0%) | 2.3% (2.3%) | 62.5% (50.0%) | 93.2% (91.5%) | 89.4% |
| MARS | 93.9% (93.6%) | 4.2% (4.0%) | 1.9% (2.3%) | 50.0% (50.0%) | 96.9% (97.2%) | 92.3% |
| ESR | 93.4% (93.6%) | 4.6% (4.0%) | 2.0% (2.3%) | 50.0% (50.0%) | 95.5% (95.4%) | 90.1% |
| 321 | 89.3% (87.9%) | 8.7% (9.7%) | 2.0% (2.5%) | 50.0% (50.0%) | 98.8% (97.9%) | 89.7% |
| ARA | 94.5% (93.6%) | 3.6% (4.0%) | 1.9% (2.3%) | 37.5% (50.0%) | 95.2% (95.0%) | 91.6% |
| MARS-T | 93.6% (93.6%) | 3.9% (4.0%) | 2.5% (2.3%) | 75.0% (50.0%) | 95.9% (97.2%) | 92.5% |
| MAV | 93.5% (93.6%) | 4.2% (4.0%) | 2.3% (2.3%) | 50.0% (50.0%) | 91.3% (89.3%) | 87.4% |
| MJ | 93.4% (93.6%) | 4.8% (4.0%) | 1.9% (2.3%) | 62.5% (50.0%) | 91.5% (90.3%) | 88.3% |
| Smith//Score | 93.9% (93.6%) | 4.2% (4.0%) | 1.9% (2.3%) | 0.0% (50.0%) | 100.0% (100.0%) | 94.4% |
| Soft Minimax-40 | 94.2% (93.6%) | 3.8% (4.0%) | 2.0% (2.3%) | 37.5% (50.0%) | 96.6% (96.8%) | 93.0% |
| BTR-score | 92.8% (93.6%) | 4.2% (4.0%) | 3.0% (2.3%) | 12.5% (50.0%) | 100.0% (100.0%) | 93.9% |
| Supplementary vote | 97.0% (100.0%) | 2.2% (0.0%) | 0.9% (0.0%) | 37.5% (0.0%) | 90.2% (87.0%) | 89.1% |
| Borda | 95.4% (100.0%) | 1.6% (0.0%) | 3.0% (0.0%) | 50.0% (0.0%) | 94.4% (89.0%) | 92.8% |
| Soft Minimax | 93.9% (93.6%) | 4.0% (4.0%) | 2.0% (2.3%) | 25.0% (50.0%) | 100.0% (100.0%) | 94.9% |
| STAR | 92.5% (93.6%) | 4.9% (4.0%) | 2.6% (2.3%) | 37.5% (50.0%) | 100.0% (99.4%) | 92.9% |
| IRV-3 | 96.7% (100.0%) | 2.7% (0.0%) | 0.6% (0.0%) | 25.0% (0.0%) | 94.1% (91.1%) | 92.9% |
| ER-IRV | 97.0% (100.0%) | 1.4% (0.0%) | 1.6% (0.0%) | 37.5% (0.0%) | 94.0% (91.1%) | 93.1% |
| Kemeny-Young | 97.0% (100.0%) | 1.2% (0.0%) | 1.9% (0.0%) | 12.5% (0.0%) | 100.0% (100.0%) | 98.0% |
| Soft Minimax-D | 93.4% (93.6%) | 3.3% (4.0%) | 3.3% (2.3%) | 75.0% (50.0%) | 99.4% (99.4%) | 94.4% |
| IRV | 97.4% (100.0%) | 2.3% (0.0%) | 0.3% (0.0%) | 50.0% (0.0%) | 93.4% (91.1%) | 93.1% |
| Ranked pairs | 97.8% (100.0%) | 0.9% (0.0%) | 1.3% (0.0%) | 37.5% (0.0%) | 100.0% (100.0%) | 98.2% |
| BTR-IRV | 96.2% (100.0%) | 1.9% (0.0%) | 1.9% (0.0%) | 37.5% (0.0%) | 100.0% (100.0%) | 96.7% |
| Schulze | 97.7% (100.0%) | 0.7% (0.0%) | 1.6% (0.0%) | 37.5% (0.0%) | 100.0% (100.0%) | 98.3% |
| Copeland | 95.7% (100.0%) | 1.4% (0.0%) | 2.9% (0.0%) | 12.5% (0.0%) | 100.0% (100.0%) | 96.6% |
| Minimax | 97.5% (100.0%) | 1.0% (0.0%) | 1.4% (0.0%) | 37.5% (0.0%) | 100.0% (100.0%) | 98.3% |
| MMWV | 97.3% (100.0%) | 1.0% (0.0%) | 1.7% (0.0%) | 25.0% (0.0%) | 100.0% (100.0%) | 98.3% |
| MMPO | 97.0% (100.0%) | 1.3% (0.0%) | 1.7% (0.0%) | 25.0% (0.0%) | 99.9% (100.0%) | 98.0% |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

## c3: candidates 5

700 elections (seeds 1), 686 with a Condorcet winner. 99 voters, 5 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 98.0%; it also has the highest total utility in 73.3% of those; electing it loses 17.5% of VSE; favourite ÷ least liked 7.0 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 82.0 to 82.3; with the best VSE (100) WME 79.8 (nothing lower is possible there either); the most balanced point VSE 94.7 / WME 95.7. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 87.6 (at WME 93.8); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -68.1 / WME -63.5.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, DMS, Soft Minimax-40, ARA, MARS, MARS-T, STAR, Soft Minimax, BTR-score, BTR-IRV, Score runoff, Kemeny-Young, MMPO.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): STAR, Smith//Score, Soft Minimax, BTR-score, Score runoff, Kemeny-Young, MMWV, Minimax, MMPO.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 88.1 [84.9–90.9] | 91.7 [89.8–93.5] | 99.9 [99.9–99.9] | 90.7 | 94.9 | 88.9 [85.9–91.6] | 90.6 [88.7–92.6] | 82.2 [79.2–84.9] | 83.8 [80.8–86.5] | 4.0 [2.6–5.6] | 4.4 [2.9–6.0] | 0.1% | 1.3% | 0.0% | ESR (WME\*), MARS (WME\*), MARS-T (WME\*) +2 more |
| DMS | rated | 88.0 [85.2–90.4] | 92.7 [90.9–94.3] | 99.9 [99.9–100.0] | 90.3 | 94.9 | 89.0 [86.0–91.5] | 91.5 [89.5–93.4] | 83.5 [80.5–86.3] | 85.0 [82.0–87.8] | 4.1 [2.7–5.7] | 4.7 [3.1–6.4] | 0.0% | 0.7% | 0.0% | ESR (WME\*), MARS (WME\*), ARA (WME\*) +1 more |
| Soft Minimax-40 | hybrid | 87.2 [84.1–90.0] | 94.4 [92.8–95.7] | 99.9 [99.9–99.9] | 90.7 | 94.9 | 86.9 [83.9–89.9] | 94.3 [92.8–95.8] | 87.4 [84.7–89.8] | 87.4 [84.7–89.8] | 4.7 [3.1–6.1] | 4.7 [3.1–6.3] | 0.1% | 1.1% | 0.0% | STAR (WME\*) |
| MAV | median-based | 87.0 [83.7–89.7] | 90.1 [88.0–92.1] | 99.9 [99.9–100.0] | 90.4 | 94.9 | 85.6 [82.2–88.5] | 89.1 [86.9–91.0] | 80.6 [77.6–83.6] | 81.9 [78.9–84.9] | 5.3 [3.7–7.0] | 4.7 [3.3–6.4] | 0.0% | 0.9% | 0.0% | Smith//Score (WME\*), Score (WME\*), MJ (WME\*) +9 more |
| ARA | hybrid | 86.8 [83.5–89.6] | 94.5 [92.9–95.9] | 99.9 [99.9–99.9] | 90.7 | 94.9 | 86.6 [83.7–89.2] | 94.5 [93.0–95.9] | 88.0 [85.4–90.4] | 88.0 [85.6–90.4] | 4.9 [3.3–6.4] | 4.9 [3.3–6.4] | 0.1% | 0.9% | 0.0% | Smith//Score (WME\*), STAR (WME\*), BTR-score (WME\*) +2 more |
| MARS | hybrid | 86.6 [83.5–89.5] | 95.2 [94.0–96.5] | 99.9 [99.9–99.9] | 90.6 | 94.9 | 86.9 [84.0–89.5] | 95.5 [94.2–96.7] | 89.4 [86.9–91.7] | 88.6 [86.2–90.9] | 4.6 [3.1–6.1] | 4.9 [3.1–6.6] | 0.0% | 0.7% | 0.0% | STAR (WME\*), Soft Minimax (WME\*) |
| Score | rated | 86.6 [83.6–89.2] | 92.2 [90.2–93.8] | 99.9 [99.9–100.0] | 90.1 | 94.9 | 88.0 [85.1–90.3] | 92.9 [91.2–94.4] | 84.6 [81.7–87.2] | 84.2 [81.0–86.8] | 4.0 [2.7–5.4] | 4.9 [3.4–6.4] | 0.0% | 0.7% | 0.0% | Smith//Score (WME\*), DMS (VSE\*), ESR (WME\*) +8 more |
| ESR | rated | 86.5 [83.6–89.1] | 93.9 [92.4–95.3] | 99.9 [99.9–99.9] | 90.5 | 94.9 | 87.4 [84.1–90.3] | 93.5 [91.9–95.0] | 86.4 [83.5–88.9] | 86.3 [83.5–88.8] | 4.7 [3.3–6.3] | 5.7 [4.0–7.3] | 0.1% | 0.9% | 0.0% | Smith//Score (WME\*), STAR (WME\*), BTR-score (WME\*) +4 more |
| MJ | median-based | 86.2 [83.1–89.2] | 91.6 [89.7–93.3] | 99.9 [99.9–99.9] | 90.2 | 94.9 | 86.6 [83.5–89.5] | 90.9 [88.9–92.7] | 82.1 [78.9–84.9] | 82.5 [79.2–85.1] | 4.4 [3.0–6.0] | 5.3 [3.6–7.0] | 0.0% | 0.8% | 0.0% | Smith//Score (WME\*), ESR (WME\*), Geometric median (VSE\*) +8 more |
| MARS-T | hybrid | 86.0 [82.9–88.9] | 95.3 [94.0–96.5] | 99.9 [99.9–100.0] | 90.6 | 94.9 | 87.0 [84.1–89.6] | 95.4 [94.1–96.7] | 89.2 [86.8–91.6] | 88.6 [86.2–90.9] | 4.4 [3.0–5.9] | 5.3 [3.7–6.9] | 0.0% | 0.7% | 0.0% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax (WME\*) |
| STAR | hybrid | 85.8 [82.8–88.7] | 96.4 [95.1–97.5] | 99.8 [99.8–99.9] | 90.6 | 94.9 | 86.0 [83.1–88.8] | 95.3 [94.1–96.6] | 89.7 [87.5–91.8] | 90.6 [88.3–92.7] | 4.6 [3.0–6.1] | 5.0 [3.4–6.6] | 0.1% | 1.5% | 0.0% | — |
| 321 | hybrid | 85.7 [82.6–88.3] | 92.2 [90.4–94.1] | 99.8 [99.8–99.9] | 84.4 | 87.8 | 86.5 [83.5–89.2] | 90.6 [88.5–92.5] | 84.0 [81.4–86.6] | 85.2 [82.5–87.9] | 5.3 [3.7–6.9] | 5.4 [3.9–7.1] | 0.1% | 1.7% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), Approval cond. runoff (WME\*) +11 more |
| Approval | approval-based | 85.4 [82.0–88.3] | 88.7 [86.5–91.0] | 99.9 [99.9–99.9] | 72.3 | 73.7 | 84.5 [80.8–87.8] | 85.5 [83.0–88.2] | 77.8 [74.6–80.8] | 80.7 [77.6–83.5] | 6.7 [5.0–8.7] | 5.9 [4.1–7.7] | 0.0% | 1.1% | 0.0% | DMS (VSE\* WME\*), Geometric median (VSE\* WME\*), Smith//Score (WME\*) +14 more |
| Borda | positional | 85.3 [82.1–88.1] | 95.6 [94.4–96.7] | 97.2 [97.1–97.3] | 76.3 | 80.9 | 79.5 [75.7–83.0] | 94.5 [93.0–95.7] | 85.9 [83.2–88.2] | 87.8 [85.3–90.2] | 5.6 [3.9–7.3] | 5.3 [3.6–7.1] | 5.2% | 15.4% | 1.0% | BTR-IRV (WME\*), Smith//Score (sincerity), Approval runoff (sincerity) +9 more |
| Smith//Score | condorcet-based | 84.9 [81.7–88.0] | 96.2 [95.1–97.3] | 99.9 [99.9–99.9] | 90.9 | 94.9 | 86.2 [83.5–89.0] | 96.1 [94.9–97.2] | 90.8 [88.7–92.8] | 90.7 [88.7–92.9] | 5.1 [3.4–6.9] | 5.7 [4.0–7.4] | 0.1% | 1.0% | 0.0% | — |
| Soft Minimax | hybrid | 84.7 [81.7–87.7] | 96.4 [95.3–97.5] | 99.9 [99.9–99.9] | 91.0 | 94.9 | 85.4 [82.6–88.2] | 96.0 [94.9–97.2] | 90.7 [88.7–92.6] | 90.7 [88.6–92.8] | 5.4 [3.7–7.1] | 5.7 [3.9–7.4] | 0.1% | 1.0% | 0.0% | — |
| BTR-score | hybrid | 84.7 [81.5–87.8] | 96.5 [95.4–97.6] | 99.9 [99.9–99.9] | 90.8 | 94.9 | 85.3 [82.6–88.2] | 96.0 [94.9–97.2] | 90.5 [88.4–92.5] | 91.0 [89.0–93.0] | 5.4 [3.7–7.1] | 5.6 [3.7–7.3] | 0.1% | 1.2% | 0.0% | — |
| Soft Minimax-D | hybrid | 84.7 [81.5–87.7] | 96.0 [94.9–97.2] | 99.9 [99.9–99.9] | 90.8 | 94.9 | 86.5 [83.6–89.3] | 95.7 [94.3–96.9] | 89.9 [87.8–92.2] | 90.3 [88.0–92.5] | 5.0 [3.3–6.6] | 5.6 [3.9–7.1] | 0.1% | 1.0% | 0.0% | Score runoff (WME\*), MARS (VSE\*) |
| Approval cond. runoff | two-round | 83.7 [80.5–86.9] | 94.4 [92.7–95.9] | 99.6 [99.6–99.7] | 71.6 | 73.7 | 84.4 [81.1–87.4] | 93.3 [91.5–94.9] | 89.5 [87.2–91.8] | 90.7 [88.5–92.9] | 5.3 [3.7–7.0] | 6.0 [4.3–7.7] | 0.1% | 2.6% | 0.0% | Smith//Score (WME\*), ESR (VSE\*), STAR (WME\*) +8 more |
| Approval runoff | two-round | 83.6 [80.3–86.7] | 95.5 [94.0–96.9] | 99.4 [99.4–99.5] | 71.1 | 73.7 | 83.2 [79.9–86.4] | 95.1 [93.5–96.5] | 93.1 [91.2–94.8] | 92.9 [91.0–94.8] | 5.6 [4.0–7.3] | 6.0 [4.3–7.7] | 0.2% | 3.7% | 0.1% | STAR (VSE\*), Score runoff (WME\*), MARS (VSE\*) +3 more |
| IRV | plurality-based | 83.5 [80.2–86.8] | 92.7 [90.7–94.4] | 97.2 [97.1–97.3] | 76.8 | 80.9 | 82.4 [78.9–85.6] | 90.9 [88.7–92.9] | 87.4 [84.7–89.8] | 89.2 [86.8–91.5] | 7.1 [5.3–9.0] | 6.1 [4.3–8.0] | 4.6% | 14.7% | 1.6% | MARS (VSE\* WME\* sincerity), Smith//Score (WME\* sincerity), Approval runoff (WME\* sincerity) +23 more |
| Supplementary vote | plurality-based | 83.3 [79.4–86.7] | 90.8 [88.9–92.6] | 97.3 [97.2–97.4] | 76.8 | 80.9 | 81.3 [77.5–84.8] | 84.8 [82.0–87.3] | 78.2 [75.2–81.0] | 83.6 [80.9–86.2] | 8.0 [6.0–10.1] | 7.1 [5.3–9.1] | 5.0% | 16.0% | 1.3% | DMS (VSE\* WME\* sincerity), ESR (VSE\* WME\* sincerity), MARS (VSE\* WME\* sincerity) +27 more |
| BTR-IRV | condorcet-based | 83.0 [79.5–86.3] | 98.6 [98.0–99.2] | 97.5 [97.3–97.6] | 76.9 | 80.9 | 82.6 [79.5–85.6] | 99.9 [99.7–100.0] | 100.0 [100.0–100.0] | 96.8 [95.5–98.0] | 6.3 [4.4–8.0] | 6.0 [4.3–7.9] | 4.6% | 14.9% | 1.0% | MMPO (WME\*), Score runoff (sincerity) |
| ER-IRV | plurality-based | 82.5 [79.2–85.9] | 92.8 [90.9–94.6] | 97.7 [97.6–97.8] | 76.7 | 80.9 | 82.4 [78.9–85.6] | 90.9 [88.7–92.9] | 87.4 [84.7–89.8] | 89.1 [86.8–91.4] | 7.1 [5.3–9.0] | 6.1 [4.4–7.9] | 1.2% | 18.3% | 0.4% | Smith//Score (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity), MARS (VSE\* WME\* sincerity) +24 more |
| Score runoff | two-round | 82.4 [78.9–85.8] | 98.0 [97.0–98.8] | 99.8 [99.7–99.8] | 90.3 | 94.9 | 82.8 [79.5–85.9] | 98.0 [97.2–98.7] | 95.8 [94.4–97.2] | 96.0 [94.4–97.4] | 4.9 [3.3–6.3] | 5.7 [4.0–7.3] | 0.3% | 1.7% | 0.1% | — |
| IRV-3 | plurality-based | 82.3 [78.7–85.8] | 91.7 [89.6–93.6] | 98.2 [98.1–98.3] | 77.3 | 80.5 | 82.5 [78.9–85.9] | 90.5 [88.2–92.5] | 86.1 [83.5–88.6] | 87.3 [84.5–89.6] | 7.0 [5.1–8.9] | 6.9 [4.9–8.7] | 4.4% | 10.6% | 1.6% | Smith//Score (VSE\* WME\* sincerity), ESR (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity) +25 more |
| Kemeny-Young | condorcet | 82.2 [78.7–85.5] | 99.3 [98.8–99.7] | 97.6 [97.5–97.7] | 77.1 | 80.9 | 81.8 [78.7–85.0] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 98.1 [97.0–99.1] | 6.7 [4.9–8.4] | 6.4 [4.6–8.3] | 4.4% | 14.3% | 1.0% | — |
| MMWV | minimax-family | 82.2 [79.1–85.3] | 99.3 [98.9–99.6] | 98.0 [97.8–98.1] | 77.1 | 80.9 | 82.1 [79.0–85.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.0 [97.0–99.0] | 6.6 [4.7–8.3] | 5.9 [4.0–7.6] | 0.1% | 18.3% | 0.0% | — |
| Ranked pairs | condorcet | 82.0 [78.6–85.4] | 98.9 [98.2–99.5] | 97.5 [97.4–97.7] | 77.1 | 80.9 | 81.9 [78.9–85.1] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.7 [96.4–98.7] | 6.6 [4.7–8.3] | 6.4 [4.6–8.3] | 4.4% | 14.4% | 0.9% | Score runoff (sincerity) |
| Schulze | condorcet | 81.9 [78.2–85.4] | 98.9 [98.2–99.5] | 97.6 [97.5–97.7] | 77.1 | 80.9 | 82.1 [79.0–85.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.8 [96.7–98.8] | 6.6 [4.7–8.3] | 6.4 [4.6–8.1] | 4.4% | 14.5% | 0.9% | Score runoff (sincerity) |
| TTR | two-round | 81.8 [78.5–85.4] | 91.8 [89.7–93.9] | 96.4 [96.1–96.8] | 65.0 | 68.6 | 81.5 [78.0–84.8] | 88.7 [86.2–90.9] | 86.6 [84.2–88.9] | 89.3 [86.9–91.6] | 7.1 [5.3–9.0] | 6.9 [5.0–8.7] | 5.3% | 0.0% | 3.8% | STAR (VSE\* WME\* sincerity), MARS (VSE\* WME\* sincerity), MARS-T (VSE\* WME\* sincerity) +27 more |
| Minimax | minimax-family | 81.7 [78.1–85.2] | 99.0 [98.2–99.6] | 97.6 [97.5–97.7] | 77.1 | 80.9 | 82.1 [79.0–85.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.3 [97.2–99.1] | 6.6 [4.7–8.3] | 6.3 [4.4–8.1] | 4.4% | 14.5% | 0.9% | — |
| MMPO | minimax-family | 81.6 [78.3–84.9] | 99.3 [98.9–99.7] | 97.9 [97.8–98.0] | 77.1 | 80.9 | 82.1 [79.0–85.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.2 [97.1–99.1] | 6.6 [4.7–8.3] | 5.9 [4.0–7.6] | 0.1% | 18.3% | 0.0% | — |
| Copeland | condorcet | 80.7 [77.3–84.0] | 97.7 [96.9–98.5] | 97.5 [97.4–97.6] | 77.0 | 80.9 | 81.1 [78.0–84.3] | 99.6 [99.4–99.8] | 100.0 [100.0–100.0] | 95.9 [94.4–97.2] | 6.7 [4.7–8.4] | 6.3 [4.4–8.0] | 4.6% | 15.2% | 0.9% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +5 more |
| Plurality | plurality-based | 78.3 [73.7–82.1] | 79.8 [76.7–82.5] | 98.4 [98.3–98.5] | 66.5 | 68.6 | 71.7 [66.7–76.2] | 73.4 [69.8–76.9] | 68.9 [65.4–72.2] | 73.5 [70.0–76.5] | 12.7 [10.1–15.3] | 10.1 [8.0–12.4] | 6.7% | 0.0% | 1.4% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +27 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 55.2% |
| Supplementary vote | 50.9% |
| Approval | 49.9% |
| IRV | 49.3% |
| Geometric median | 49.1% |
| IRV-3 | 49.0% |
| Score | 48.7% |
| ESR | 48.3% |
| ER-IRV | 48.3% |
| MAV | 48.2% |
| DMS | 48.0% |
| Approval runoff | 47.9% |
| Approval cond. runoff | 47.9% |
| MARS-T | 47.8% |
| TTR | 47.7% |
| ARA | 47.7% |
| MJ | 47.7% |
| MARS | 47.7% |
| BTR-score | 47.7% |
| Smith//Score | 47.6% |
| BTR-IRV | 47.6% |
| Soft Minimax-D | 47.5% |
| STAR | 47.5% |
| Ranked pairs | 47.4% |
| Score runoff | 47.2% |
| Soft Minimax-40 | 47.2% |
| Kemeny-Young | 47.1% |
| Schulze | 46.9% |
| Minimax | 46.9% |
| MMWV | 46.8% |
| 321 | 46.8% |
| Copeland | 46.8% |
| MMPO | 46.7% |
| Soft Minimax | 46.6% |
| Borda | 45.1% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 686 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 14 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 88.6% (88.9%) | 8.0% (7.7%) | 3.4% (3.4%) | 78.6% (85.7%) | 89.8% (88.7%) | 83.8% |
| DMS | 89.4% (88.9%) | 7.9% (7.7%) | 2.8% (3.4%) | 78.6% (85.7%) | 90.7% (87.1%) | 85.0% |
| Soft Minimax-40 | 88.5% (88.9%) | 8.0% (7.7%) | 3.5% (3.4%) | 78.6% (85.7%) | 94.7% (93.7%) | 87.4% |
| MAV | 89.1% (88.9%) | 7.3% (7.7%) | 3.6% (3.4%) | 85.7% (85.7%) | 88.0% (86.2%) | 81.9% |
| ARA | 89.5% (88.9%) | 7.0% (7.7%) | 3.5% (3.4%) | 64.3% (85.7%) | 92.8% (92.7%) | 88.0% |
| MARS | 89.5% (88.9%) | 7.3% (7.7%) | 3.2% (3.4%) | 85.7% (85.7%) | 95.4% (96.6%) | 88.6% |
| Score | 88.9% (88.9%) | 7.6% (7.7%) | 3.5% (3.4%) | 78.6% (85.7%) | 89.5% (89.1%) | 84.2% |
| ESR | 88.3% (88.9%) | 6.7% (7.7%) | 5.0% (3.4%) | 78.6% (85.7%) | 94.1% (93.0%) | 86.3% |
| MJ | 89.1% (88.9%) | 7.7% (7.7%) | 3.2% (3.4%) | 92.9% (85.7%) | 90.3% (87.6%) | 82.5% |
| MARS-T | 89.4% (88.9%) | 6.9% (7.7%) | 3.8% (3.4%) | 78.6% (85.7%) | 95.4% (96.0%) | 88.6% |
| STAR | 88.9% (88.9%) | 6.9% (7.7%) | 4.2% (3.4%) | 85.7% (85.7%) | 99.5% (97.9%) | 90.6% |
| 321 | 85.4% (84.1%) | 11.8% (13.1%) | 2.8% (2.8%) | 85.7% (92.9%) | 97.3% (97.2%) | 85.2% |
| Borda | 95.0% (100.0%) | 2.3% (0.0%) | 2.6% (0.0%) | 35.7% (0.0%) | 89.8% (85.9%) | 87.8% |
| Smith//Score | 89.4% (88.9%) | 7.6% (7.7%) | 3.1% (3.4%) | 85.7% (85.7%) | 100.0% (100.0%) | 90.7% |
| Soft Minimax | 88.9% (88.9%) | 7.4% (7.7%) | 3.6% (3.4%) | 78.6% (85.7%) | 100.0% (100.0%) | 90.7% |
| BTR-score | 89.4% (88.9%) | 7.6% (7.7%) | 3.1% (3.4%) | 92.9% (85.7%) | 100.0% (100.0%) | 91.0% |
| Soft Minimax-D | 89.1% (88.9%) | 7.4% (7.7%) | 3.5% (3.4%) | 78.6% (85.7%) | 98.2% (99.1%) | 90.3% |
| IRV | 96.2% (100.0%) | 3.4% (0.0%) | 0.4% (0.0%) | 21.4% (0.0%) | 89.3% (87.4%) | 89.2% |
| Supplementary vote | 95.3% (100.0%) | 3.2% (0.0%) | 1.5% (0.0%) | 14.3% (0.0%) | 84.6% (78.2%) | 83.6% |
| BTR-IRV | 96.1% (100.0%) | 0.7% (0.0%) | 3.2% (0.0%) | 14.3% (0.0%) | 100.0% (100.0%) | 96.8% |
| ER-IRV | 95.0% (100.0%) | 2.8% (0.0%) | 2.2% (0.0%) | 28.6% (0.0%) | 90.8% (87.4%) | 89.1% |
| IRV-3 | 91.5% (94.9%) | 5.1% (2.8%) | 3.4% (2.3%) | 57.1% (14.3%) | 92.0% (90.6%) | 87.3% |
| Kemeny-Young | 97.4% (100.0%) | 1.5% (0.0%) | 1.2% (0.0%) | 35.7% (0.0%) | 100.0% (100.0%) | 98.1% |
| MMWV | 96.2% (100.0%) | 1.2% (0.0%) | 2.6% (0.0%) | 21.4% (0.0%) | 100.0% (100.0%) | 98.0% |
| Ranked pairs | 96.8% (100.0%) | 1.2% (0.0%) | 2.0% (0.0%) | 28.6% (0.0%) | 100.0% (100.0%) | 97.7% |
| Schulze | 96.8% (100.0%) | 1.0% (0.0%) | 2.2% (0.0%) | 14.3% (0.0%) | 100.0% (100.0%) | 97.8% |
| Minimax | 96.9% (100.0%) | 0.7% (0.0%) | 2.3% (0.0%) | 21.4% (0.0%) | 100.0% (100.0%) | 98.3% |
| MMPO | 96.2% (100.0%) | 0.9% (0.0%) | 2.9% (0.0%) | 14.3% (0.0%) | 100.0% (100.0%) | 98.2% |
| Copeland | 94.8% (100.0%) | 1.7% (0.0%) | 3.5% (0.0%) | 21.4% (0.0%) | 100.0% (100.0%) | 95.9% |

![strategy arrows](arrows-c3.png)

![strategy shifts](shift-c3.png)

![sincerity](sincerity-c3.png)

![fidelity](fidelity-c3.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c3.png)

## c4: candidates 7

700 elections (seeds 1), 680 with a Condorcet winner. 99 voters, 7 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 97.1%; it also has the highest total utility in 65.1% of those; electing it loses 17.2% of VSE; favourite ÷ least liked 9.8 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 81.3 to 82.6; with the best VSE (100) WME 76.4 (nothing lower is possible there either); the most balanced point VSE 93.7 / WME 95.7. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 89.9 (at WME 92.7); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -55.2 / WME -49.5.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, DMS, ARA, STAR, Smith//Score, Soft Minimax-D, BTR-score, Soft Minimax, Score runoff, BTR-IRV, Kemeny-Young.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): ARA, Soft Minimax-40, STAR, Soft Minimax, Score runoff, Kemeny-Young, Schulze, MMWV, Ranked pairs.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 89.4 [87.4–91.4] | 88.9 [86.9–90.8] | 99.9 [99.9–100.0] | 89.7 | 93.4 | 89.7 [87.7–91.8] | 88.7 [86.6–90.6] | 77.9 [74.7–80.9] | 78.2 [75.0–81.3] | 3.4 [2.1–4.7] | 3.1 [1.9–4.4] | 0.1% | 1.1% | 0.0% | Score (WME\*), ESR (WME\*), ARA (WME\*) +2 more |
| MAV | median-based | 89.3 [87.1–91.4] | 87.7 [85.5–89.9] | 99.9 [99.9–100.0] | 89.3 | 93.4 | 89.6 [87.4–91.8] | 85.1 [82.7–87.5] | 74.3 [71.1–77.5] | 77.1 [73.9–80.1] | 2.6 [1.4–3.7] | 2.9 [1.7–4.0] | 0.0% | 1.1% | 0.0% | Score (WME\*), ESR (WME\*), ARA (WME\*) +2 more |
| DMS | rated | 89.2 [87.3–91.1] | 89.4 [87.3–91.2] | 100.0 [100.0–100.0] | 89.2 | 93.4 | 89.2 [87.0–91.3] | 87.9 [85.8–90.1] | 76.2 [72.9–79.6] | 78.4 [75.4–81.4] | 3.3 [2.0–4.6] | 3.3 [2.1–4.6] | 0.0% | 0.8% | 0.0% | Score (WME\*), ESR (WME\*), ARA (WME\*) +1 more |
| ARA | hybrid | 89.0 [87.0–91.0] | 94.4 [93.1–95.7] | 99.9 [99.9–100.0] | 89.6 | 93.4 | 89.3 [87.2–91.3] | 93.9 [92.5–95.2] | 84.9 [82.1–87.5] | 85.4 [82.9–88.0] | 2.6 [1.6–3.7] | 2.4 [1.3–3.6] | 0.1% | 0.8% | 0.0% | — |
| Score | rated | 88.9 [86.7–90.9] | 91.1 [89.4–92.8] | 100.0 [99.9–100.0] | 89.1 | 93.4 | 89.5 [87.5–91.4] | 92.3 [90.6–93.8] | 82.5 [79.8–85.2] | 80.7 [77.7–83.6] | 2.7 [1.6–3.9] | 2.7 [1.6–3.9] | 0.0% | 0.8% | 0.0% | STAR (WME\*), ARA (WME\*), Soft Minimax-40 (WME\*) |
| Soft Minimax-40 | hybrid | 88.8 [86.7–90.8] | 94.4 [93.1–95.6] | 100.0 [99.9–100.0] | 89.8 | 93.4 | 89.6 [87.7–91.5] | 93.7 [92.3–95.1] | 84.7 [82.0–87.3] | 85.6 [82.9–88.1] | 2.7 [1.6–4.0] | 3.1 [1.9–4.4] | 0.1% | 0.9% | 0.0% | — |
| ESR | rated | 88.3 [86.0–90.4] | 92.2 [90.6–93.8] | 99.9 [99.9–100.0] | 89.5 | 93.4 | 88.4 [86.3–90.5] | 93.1 [91.5–94.5] | 83.8 [80.8–86.5] | 83.0 [80.0–85.7] | 3.3 [2.0–4.6] | 3.3 [2.0–4.6] | 0.1% | 0.8% | 0.0% | STAR (WME\*), MARS (WME\*), ARA (WME\*) +1 more |
| MJ | median-based | 88.2 [86.0–90.4] | 89.1 [87.1–91.0] | 99.9 [99.9–99.9] | 89.2 | 93.4 | 89.2 [87.0–91.2] | 87.2 [84.9–89.3] | 77.2 [74.1–80.2] | 77.8 [74.7–80.9] | 2.7 [1.6–3.9] | 3.1 [1.9–4.4] | 0.0% | 1.1% | 0.0% | Smith//Score (WME\*), Approval cond. runoff (WME\*), Score (WME\*) +7 more |
| 321 | hybrid | 88.0 [85.7–90.2] | 90.9 [88.8–92.6] | 99.9 [99.9–99.9] | 81.5 | 84.3 | 88.1 [85.8–90.2] | 89.1 [87.2–91.1] | 78.6 [75.7–81.8] | 81.0 [78.0–84.0] | 3.1 [2.0–4.3] | 3.1 [1.9–4.4] | 0.2% | 1.5% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), Approval cond. runoff (WME\*) +8 more |
| STAR | hybrid | 87.4 [85.2–89.6] | 95.8 [94.6–96.9] | 99.9 [99.9–100.0] | 89.6 | 93.4 | 87.1 [84.8–89.1] | 95.6 [94.5–96.9] | 88.3 [85.8–90.8] | 88.5 [86.2–90.8] | 2.9 [1.7–4.1] | 3.3 [2.0–4.6] | 0.1% | 1.0% | 0.0% | — |
| MARS | hybrid | 87.0 [84.8–89.3] | 95.5 [94.3–96.6] | 100.0 [99.9–100.0] | 89.5 | 93.4 | 87.8 [85.8–89.9] | 95.6 [94.4–96.8] | 87.8 [85.3–90.2] | 87.4 [84.8–89.9] | 3.1 [1.9–4.4] | 3.1 [1.9–4.4] | 0.1% | 0.9% | 0.0% | Score runoff (WME\*), Soft Minimax (WME\*) |
| Smith//Score | condorcet-based | 86.7 [84.5–89.0] | 95.8 [94.6–96.9] | 100.0 [99.9–100.0] | 89.8 | 93.4 | 86.8 [84.5–89.1] | 96.1 [95.0–97.2] | 89.1 [86.9–91.4] | 89.0 [86.6–91.3] | 3.3 [2.0–4.6] | 3.6 [2.3–4.9] | 0.0% | 0.9% | 0.0% | Score runoff (WME\*), Soft Minimax (WME\*) |
| Approval cond. runoff | two-round | 86.7 [84.3–88.8] | 93.1 [91.4–94.7] | 99.8 [99.8–99.8] | 67.1 | 68.5 | 85.5 [83.1–87.8] | 91.3 [89.5–93.1] | 84.9 [82.3–87.6] | 87.3 [84.7–89.8] | 3.0 [1.7–4.3] | 3.1 [1.9–4.3] | 0.1% | 2.0% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), STAR (WME\*) +8 more |
| Soft Minimax-D | hybrid | 86.4 [83.8–89.0] | 95.8 [94.6–96.9] | 100.0 [99.9–100.0] | 89.7 | 93.4 | 87.6 [85.5–89.7] | 96.0 [94.9–97.1] | 88.5 [86.1–91.0] | 88.8 [86.4–91.2] | 3.1 [2.0–4.4] | 3.0 [1.9–4.3] | 0.0% | 0.8% | 0.0% | Score runoff (WME\*), Soft Minimax (WME\*) |
| MARS-T | hybrid | 86.3 [84.2–88.5] | 95.7 [94.7–96.8] | 100.0 [99.9–100.0] | 89.5 | 93.4 | 87.8 [85.7–89.9] | 95.5 [94.3–96.6] | 87.5 [85.1–89.9] | 87.5 [85.0–90.0] | 3.1 [1.9–4.4] | 3.0 [1.7–4.3] | 0.1% | 0.9% | 0.0% | Score runoff (WME\*), Soft Minimax (WME\*) |
| BTR-score | hybrid | 86.1 [83.7–88.4] | 96.1 [95.0–97.2] | 99.9 [99.9–100.0] | 89.8 | 93.4 | 86.6 [84.2–88.8] | 96.3 [95.3–97.3] | 89.5 [87.3–91.8] | 89.3 [86.9–91.6] | 3.3 [2.0–4.6] | 3.3 [2.0–4.6] | 0.1% | 1.0% | 0.0% | Score runoff (WME\*), Soft Minimax (WME\*) |
| Soft Minimax | hybrid | 86.0 [83.6–88.4] | 96.9 [95.9–97.9] | 100.0 [100.0–100.0] | 89.9 | 93.4 | 86.1 [83.7–88.4] | 96.6 [95.6–97.6] | 89.9 [87.7–92.1] | 90.4 [88.2–92.6] | 3.4 [2.1–4.7] | 3.3 [2.0–4.6] | 0.1% | 0.6% | 0.0% | — |
| Approval runoff | two-round | 86.0 [83.7–88.1] | 94.8 [93.3–96.2] | 99.8 [99.7–99.8] | 67.0 | 68.5 | 84.9 [82.4–87.2] | 93.1 [91.5–94.7] | 88.5 [86.0–90.8] | 90.2 [88.0–92.6] | 3.0 [1.7–4.3] | 3.1 [1.9–4.6] | 0.2% | 2.3% | 0.1% | Score runoff (WME\*), ARA (VSE\*), Soft Minimax (WME\*) +1 more |
| Score runoff | two-round | 85.5 [83.1–87.7] | 97.3 [96.3–98.3] | 99.9 [99.9–99.9] | 89.4 | 93.4 | 85.2 [82.7–87.4] | 98.3 [97.6–99.0] | 95.8 [94.1–97.3] | 94.3 [92.5–96.0] | 2.9 [1.7–4.1] | 3.0 [1.7–4.3] | 0.2% | 1.2% | 0.0% | — |
| Approval | approval-based | 85.4 [82.5–88.2] | 84.9 [82.5–87.1] | 99.9 [99.9–99.9] | 67.4 | 68.5 | 85.1 [81.9–88.0] | 81.7 [79.0–84.2] | 70.1 [66.8–73.5] | 73.0 [69.7–76.2] | 5.4 [3.9–7.1] | 4.9 [3.4–6.4] | 0.0% | 1.3% | 0.0% | Score (VSE\* WME\*), MJ (VSE\* WME\*), MAV (VSE\* WME\*) +17 more |
| Borda | positional | 85.1 [82.7–87.5] | 93.0 [91.5–94.5] | 98.0 [97.9–98.1] | 75.3 | 78.8 | 82.0 [79.0–84.7] | 94.6 [93.4–95.8] | 85.0 [82.3–87.7] | 83.8 [80.8–86.5] | 3.1 [1.9–4.4] | 3.1 [1.9–4.4] | 6.5% | 11.5% | 0.9% | STAR (VSE\* WME\* sincerity), Smith//Score (WME\* sincerity), Score (VSE\* sincerity) +18 more |
| IRV | plurality-based | 84.2 [81.5–86.9] | 90.1 [88.1–92.1] | 97.9 [97.8–98.0] | 75.7 | 78.8 | 84.7 [81.9–87.4] | 86.1 [83.6–88.3] | 79.0 [75.9–82.0] | 84.3 [81.6–87.0] | 4.1 [2.7–5.6] | 3.7 [2.3–5.0] | 5.5% | 12.8% | 1.4% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +24 more |
| IRV-3 | plurality-based | 84.0 [81.2–86.7] | 88.4 [86.2–90.5] | 99.0 [98.9–99.1] | 74.1 | 76.0 | 82.8 [79.7–85.8] | 85.0 [82.4–87.3] | 75.7 [72.6–79.0] | 80.2 [77.3–83.1] | 5.4 [3.9–7.0] | 4.4 [3.0–5.9] | 5.6% | 5.6% | 1.3% | Smith//Score (VSE\* WME\*), Approval cond. runoff (VSE\* WME\*), Score (VSE\* WME\*) +24 more |
| ER-IRV | plurality-based | 83.4 [80.4–86.3] | 89.4 [87.2–91.5] | 98.3 [98.3–98.4] | 75.7 | 78.8 | 84.7 [81.9–87.4] | 86.1 [83.6–88.3] | 79.0 [75.9–82.0] | 83.1 [80.3–85.9] | 4.1 [2.7–5.6] | 5.0 [3.6–6.6] | 1.2% | 17.2% | 0.3% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +25 more |
| BTR-IRV | condorcet-based | 83.3 [80.6–85.8] | 98.6 [98.0–99.2] | 98.1 [98.0–98.2] | 75.8 | 78.8 | 82.3 [79.6–85.0] | 99.9 [99.7–100.0] | 100.0 [100.0–100.0] | 96.6 [95.3–97.9] | 4.4 [2.9–5.9] | 3.9 [2.4–5.1] | 5.7% | 12.1% | 0.9% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +2 more |
| Kemeny-Young | condorcet | 82.9 [80.2–85.5] | 99.7 [99.4–99.9] | 98.2 [98.2–98.3] | 76.0 | 78.8 | 82.4 [79.6–85.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.8 [97.8–99.5] | 4.3 [2.7–5.7] | 4.4 [2.9–5.9] | 5.4% | 11.8% | 0.8% | — |
| Copeland | condorcet | 82.7 [80.1–85.3] | 98.0 [97.2–98.7] | 98.2 [98.1–98.3] | 75.9 | 78.8 | 82.0 [79.4–84.8] | 99.8 [99.6–99.9] | 100.0 [100.0–100.0] | 95.7 [94.0–97.0] | 4.4 [2.9–5.9] | 3.9 [2.4–5.1] | 5.4% | 11.7% | 0.8% | Score runoff (VSE\* sincerity), Ranked pairs (WME\*), Schulze (WME\*) +4 more |
| Minimax | minimax-family | 82.7 [79.9–85.3] | 99.1 [98.5–99.6] | 98.2 [98.1–98.2] | 76.0 | 78.8 | 82.0 [79.2–84.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.9 [96.8–99.0] | 4.4 [2.9–5.9] | 4.1 [2.7–5.4] | 5.5% | 11.8% | 0.9% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) |
| Schulze | condorcet | 82.7 [79.9–85.4] | 99.4 [98.9–99.8] | 98.2 [98.1–98.3] | 76.0 | 78.8 | 82.2 [79.4–84.9] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.4 [97.3–99.2] | 4.4 [2.9–5.9] | 4.1 [2.7–5.6] | 5.5% | 11.7% | 0.8% | — |
| MMPO | minimax-family | 82.6 [80.0–85.3] | 99.2 [98.8–99.6] | 98.5 [98.4–98.6] | 76.0 | 78.8 | 82.0 [79.2–84.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.4 [96.2–98.5] | 4.4 [2.9–5.9] | 4.0 [2.6–5.4] | 0.1% | 17.3% | 0.0% | Kemeny-Young (WME\*) |
| MMWV | minimax-family | 82.5 [79.8–85.2] | 99.4 [99.0–99.7] | 98.5 [98.4–98.6] | 76.0 | 78.8 | 82.0 [79.2–84.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.6 [96.3–98.7] | 4.4 [2.9–5.9] | 4.0 [2.6–5.4] | 0.1% | 17.2% | 0.0% | — |
| Ranked pairs | condorcet | 82.5 [79.6–85.3] | 99.6 [99.3–99.8] | 98.2 [98.1–98.3] | 76.0 | 78.8 | 82.2 [79.4–84.9] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.4 [97.3–99.3] | 4.6 [3.0–6.0] | 4.6 [3.0–6.0] | 5.5% | 11.7% | 0.8% | — |
| TTR | two-round | 81.2 [77.9–84.3] | 89.7 [87.5–91.9] | 96.9 [96.6–97.1] | 61.7 | 64.7 | 81.7 [78.6–84.7] | 83.9 [81.3–86.5] | 78.3 [75.1–81.4] | 84.4 [81.7–87.1] | 5.7 [4.0–7.4] | 5.3 [3.7–7.0] | 6.6% | 0.0% | 3.4% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +28 more |
| Supplementary vote | plurality-based | 80.2 [76.8–83.5] | 80.9 [78.0–83.5] | 98.0 [97.9–98.1] | 75.8 | 78.8 | 76.4 [72.6–79.9] | 76.7 [73.6–79.7] | 66.2 [62.5–69.6] | 70.3 [66.9–73.5] | 8.6 [6.6–10.6] | 6.6 [4.7–8.4] | 6.1% | 14.3% | 1.2% | Smith//Score (VSE\* WME\* sincerity), IRV-3 (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity) +29 more |
| Plurality | plurality-based | 75.5 [71.6–79.0] | 72.7 [69.3–75.8] | 98.3 [98.2–98.4] | 62.6 | 64.7 | 64.1 [59.3–68.3] | 62.9 [59.3–66.6] | 55.0 [51.3–58.6] | 62.8 [59.2–66.4] | 13.7 [11.3–16.1] | 9.6 [7.6–11.7] | 7.9% | 0.0% | 1.5% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +30 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 49.1% |
| Supplementary vote | 45.9% |
| Approval | 43.5% |
| IRV-3 | 42.4% |
| DMS | 42.1% |
| Geometric median | 42.0% |
| IRV | 41.5% |
| TTR | 41.1% |
| ESR | 41.0% |
| ER-IRV | 40.9% |
| Score | 40.9% |
| Approval cond. runoff | 40.7% |
| MARS | 40.6% |
| STAR | 40.4% |
| BTR-IRV | 40.4% |
| 321 | 40.4% |
| MARS-T | 40.2% |
| Smith//Score | 40.0% |
| Soft Minimax-D | 40.0% |
| Approval runoff | 39.9% |
| BTR-score | 39.9% |
| MAV | 39.8% |
| MMPO | 39.6% |
| Soft Minimax-40 | 39.5% |
| Schulze | 39.4% |
| Ranked pairs | 39.3% |
| MMWV | 39.3% |
| Minimax | 39.3% |
| Kemeny-Young | 39.3% |
| ARA | 39.3% |
| Soft Minimax | 39.0% |
| MJ | 38.9% |
| Score runoff | 38.7% |
| Copeland | 38.1% |
| Borda | 36.5% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 680 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 20 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 86.6% (88.7%) | 10.4% (8.7%) | 2.9% (2.6%) | 65.0% (60.0%) | 87.7% (85.8%) | 78.2% |
| MAV | 87.9% (88.7%) | 9.4% (8.7%) | 2.6% (2.6%) | 65.0% (60.0%) | 84.1% (80.4%) | 77.1% |
| DMS | 87.4% (88.7%) | 10.3% (8.7%) | 2.4% (2.6%) | 60.0% (60.0%) | 86.7% (84.3%) | 78.4% |
| ARA | 88.4% (88.7%) | 9.4% (8.7%) | 2.2% (2.6%) | 65.0% (60.0%) | 93.3% (93.1%) | 85.4% |
| Score | 86.6% (88.7%) | 10.4% (8.7%) | 2.9% (2.6%) | 60.0% (60.0%) | 88.9% (90.7%) | 80.7% |
| Soft Minimax-40 | 89.0% (88.7%) | 8.7% (8.7%) | 2.4% (2.6%) | 65.0% (60.0%) | 93.4% (93.8%) | 85.6% |
| ESR | 86.8% (88.7%) | 10.4% (8.7%) | 2.8% (2.6%) | 55.0% (60.0%) | 93.6% (93.5%) | 83.0% |
| MJ | 87.8% (88.7%) | 10.0% (8.7%) | 2.2% (2.6%) | 70.0% (60.0%) | 86.2% (82.3%) | 77.8% |
| 321 | 81.2% (80.3%) | 16.3% (14.7%) | 2.5% (5.0%) | 95.0% (85.0%) | 96.8% (93.4%) | 81.0% |
| STAR | 88.2% (88.7%) | 8.8% (8.7%) | 2.9% (2.6%) | 60.0% (60.0%) | 98.4% (98.7%) | 88.5% |
| MARS | 88.8% (88.7%) | 8.5% (8.7%) | 2.6% (2.6%) | 75.0% (60.0%) | 97.0% (97.8%) | 87.4% |
| Smith//Score | 88.5% (88.7%) | 9.0% (8.7%) | 2.5% (2.6%) | 65.0% (60.0%) | 100.0% (100.0%) | 89.0% |
| Soft Minimax-D | 89.0% (88.7%) | 9.1% (8.7%) | 1.9% (2.6%) | 60.0% (60.0%) | 99.1% (99.3%) | 88.8% |
| MARS-T | 89.3% (88.7%) | 8.2% (8.7%) | 2.5% (2.6%) | 65.0% (60.0%) | 95.9% (97.1%) | 87.5% |
| BTR-score | 88.7% (88.7%) | 9.4% (8.7%) | 1.9% (2.6%) | 55.0% (60.0%) | 100.0% (100.0%) | 89.3% |
| Soft Minimax | 89.0% (88.7%) | 8.7% (8.7%) | 2.4% (2.6%) | 65.0% (60.0%) | 100.0% (100.0%) | 90.4% |
| Borda | 95.9% (100.0%) | 2.2% (0.0%) | 1.9% (0.0%) | 45.0% (0.0%) | 84.4% (85.0%) | 83.8% |
| IRV | 95.4% (100.0%) | 2.8% (0.0%) | 1.8% (0.0%) | 45.0% (0.0%) | 85.7% (79.0%) | 84.3% |
| IRV-3 | 84.4% (83.8%) | 12.1% (11.3%) | 3.5% (4.9%) | 80.0% (65.0%) | 92.3% (88.7%) | 80.2% |
| ER-IRV | 95.0% (100.0%) | 2.4% (0.0%) | 2.6% (0.0%) | 30.0% (0.0%) | 84.1% (79.0%) | 83.1% |
| BTR-IRV | 96.6% (100.0%) | 1.9% (0.0%) | 1.5% (0.0%) | 20.0% (0.0%) | 100.0% (100.0%) | 96.6% |
| Kemeny-Young | 98.2% (100.0%) | 0.6% (0.0%) | 1.2% (0.0%) | 10.0% (0.0%) | 100.0% (100.0%) | 98.8% |
| Copeland | 94.7% (100.0%) | 2.4% (0.0%) | 2.9% (0.0%) | 10.0% (0.0%) | 100.0% (100.0%) | 95.7% |
| Minimax | 96.8% (100.0%) | 1.0% (0.0%) | 2.2% (0.0%) | 30.0% (0.0%) | 100.0% (100.0%) | 97.9% |
| Schulze | 97.2% (100.0%) | 0.7% (0.0%) | 2.1% (0.0%) | 25.0% (0.0%) | 100.0% (100.0%) | 98.4% |
| MMPO | 96.3% (100.0%) | 1.8% (0.0%) | 1.9% (0.0%) | 5.0% (0.0%) | 99.9% (100.0%) | 97.4% |
| MMWV | 96.2% (100.0%) | 1.5% (0.0%) | 2.4% (0.0%) | 0.0% (0.0%) | 100.0% (100.0%) | 97.6% |
| Ranked pairs | 97.8% (100.0%) | 0.9% (0.0%) | 1.3% (0.0%) | 25.0% (0.0%) | 100.0% (100.0%) | 98.4% |
| Supplementary vote | 94.6% (100.0%) | 3.8% (0.0%) | 1.6% (0.0%) | 50.0% (0.0%) | 71.5% (66.2%) | 70.3% |

![strategy arrows](arrows-c4.png)

![strategy shifts](shift-c4.png)

![sincerity](sincerity-c4.png)

![fidelity](fidelity-c4.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c4.png)

## c5: candidates 8

700 elections (seeds 1), 676 with a Condorcet winner. 99 voters, 8 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 96.6%; it also has the highest total utility in 59.3% of those; electing it loses 17.8% of VSE; favourite ÷ least liked 10.8 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 81.3 to 81.7; with the best VSE (100) WME 75.3 (nothing lower is possible there either); the most balanced point VSE 94.4 / WME 95.4. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 92.5 (at WME 90.6); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -52.0 / WME -43.8.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, Score, ESR, ARA, MARS-T, MARS, STAR, Smith//Score, Soft Minimax-D, Soft Minimax, Score runoff, Copeland, Schulze, Minimax, Ranked pairs.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): MARS-T, Soft Minimax-40, Smith//Score, Soft Minimax-D, Score runoff, Schulze, Minimax, MMWV, Ranked pairs.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 92.0 [90.3–93.8] | 87.5 [85.4–89.4] | 100.0 [99.9–100.0] | 89.2 | 92.8 | 92.1 [90.3–93.8] | 86.9 [84.9–88.8] | 70.0 [66.5–73.3] | 70.7 [67.4–74.2] | 0.7 [0.1–1.4] | 1.0 [0.3–1.9] | 0.1% | 1.0% | 0.0% | Score (WME\*), ESR (WME\*), ARA (WME\*) |
| Score | rated | 91.1 [89.6–92.7] | 90.3 [88.6–91.8] | 100.0 [100.0–100.0] | 88.7 | 92.8 | 92.6 [91.1–94.0] | 90.3 [88.6–92.0] | 74.9 [71.7–78.0] | 74.8 [71.5–77.9] | 0.4 [0.0–1.0] | 0.9 [0.3–1.6] | 0.0% | 0.8% | 0.0% | MARS-T (WME\*), Soft Minimax-40 (WME\*) |
| MAV | median-based | 91.1 [89.3–92.7] | 86.5 [84.5–88.5] | 99.9 [99.9–100.0] | 88.8 | 92.8 | 90.4 [88.5–92.0] | 83.3 [81.1–85.6] | 65.8 [62.4–69.5] | 69.5 [66.1–72.8] | 0.9 [0.3–1.6] | 0.9 [0.3–1.6] | 0.0% | 1.1% | 0.0% | Score (WME\*), ESR (WME\*), MARS-T (WME\*) +2 more |
| ESR | rated | 90.7 [88.9–92.4] | 90.5 [88.7–92.2] | 99.9 [99.9–99.9] | 89.0 | 92.8 | 91.2 [89.4–92.8] | 90.8 [89.0–92.5] | 76.6 [73.4–79.8] | 76.4 [73.4–79.5] | 0.7 [0.1–1.4] | 1.1 [0.4–2.0] | 0.1% | 1.0% | 0.0% | MARS (WME\*), MARS-T (WME\*), Soft Minimax-40 (WME\*) |
| ARA | hybrid | 90.5 [88.9–92.1] | 91.3 [89.8–93.0] | 100.0 [99.9–100.0] | 89.2 | 92.8 | 91.3 [89.7–92.7] | 92.1 [90.6–93.6] | 78.4 [75.3–81.5] | 77.9 [74.9–81.3] | 0.3 [0.0–0.7] | 0.6 [0.0–1.1] | 0.1% | 1.0% | 0.0% | MARS (WME\*), MARS-T (WME\*) |
| DMS | rated | 90.5 [88.8–92.3] | 87.4 [85.3–89.3] | 100.0 [100.0–100.0] | 88.8 | 92.8 | 92.2 [90.8–93.6] | 84.2 [82.0–86.5] | 67.2 [63.7–70.7] | 72.0 [68.6–75.4] | 0.7 [0.1–1.4] | 1.3 [0.6–2.1] | 0.0% | 0.9% | 0.0% | Score (WME\*), ESR (WME\*), Geometric median (VSE\*) +4 more |
| MJ | median-based | 90.5 [88.7–92.1] | 88.1 [86.1–89.9] | 99.9 [99.9–100.0] | 88.8 | 92.8 | 89.8 [88.0–91.6] | 85.3 [82.9–87.5] | 69.1 [65.4–72.4] | 72.6 [69.4–76.0] | 1.3 [0.6–2.1] | 1.0 [0.4–1.7] | 0.0% | 1.0% | 0.0% | Score (WME\*), ESR (WME\*), Geometric median (VSE\*) +4 more |
| MARS-T | hybrid | 90.4 [88.6–92.1] | 92.8 [91.2–94.3] | 100.0 [99.9–100.0] | 89.0 | 92.8 | 90.1 [88.2–91.9] | 93.5 [92.0–94.9] | 80.8 [77.8–83.7] | 80.0 [77.0–82.9] | 0.9 [0.3–1.6] | 1.0 [0.3–1.7] | 0.0% | 1.0% | 0.0% | — |
| Soft Minimax-40 | hybrid | 90.2 [88.3–92.0] | 92.3 [90.7–93.8] | 100.0 [99.9–100.0] | 89.4 | 92.8 | 90.2 [88.1–92.0] | 91.9 [90.3–93.4] | 77.5 [74.4–80.6] | 78.7 [75.7–81.5] | 1.0 [0.3–1.9] | 1.0 [0.3–1.9] | 0.0% | 1.1% | 0.0% | — |
| 321 | hybrid | 89.5 [87.6–91.4] | 88.0 [86.0–89.9] | 99.9 [99.9–99.9] | 80.5 | 83.1 | 90.6 [89.0–92.3] | 87.5 [85.6–89.5] | 72.6 [69.5–76.0] | 74.1 [71.0–77.3] | 0.7 [0.1–1.4] | 1.1 [0.4–2.0] | 0.1% | 1.8% | 0.0% | Score (VSE\* WME\*), Smith//Score (WME\*), Approval cond. runoff (WME\*) +7 more |
| MARS | hybrid | 89.2 [87.2–91.0] | 93.3 [91.8–94.8] | 100.0 [100.0–100.0] | 89.1 | 92.8 | 89.8 [87.8–91.6] | 93.7 [92.3–95.1] | 81.4 [78.2–84.4] | 81.4 [78.5–84.3] | 1.1 [0.4–2.0] | 1.1 [0.4–2.0] | 0.0% | 0.8% | 0.0% | Smith//Score (WME\*) |
| Approval | approval-based | 88.6 [86.0–90.6] | 82.7 [80.2–85.0] | 99.9 [99.9–99.9] | 66.1 | 67.1 | 88.2 [85.7–90.5] | 79.4 [76.7–82.0] | 63.0 [59.4–66.5] | 66.8 [63.2–70.2] | 2.1 [1.1–3.3] | 1.9 [1.0–2.9] | 0.0% | 1.3% | 0.0% | Score (VSE\* WME\*), MAV (VSE\* WME\*), Geometric median (VSE\* WME\*) +16 more |
| STAR | hybrid | 88.3 [86.2–90.2] | 93.7 [92.3–95.1] | 99.9 [99.9–99.9] | 89.2 | 92.8 | 88.9 [86.8–90.6] | 94.0 [92.7–95.4] | 82.2 [79.2–85.1] | 82.0 [79.0–84.8] | 1.1 [0.4–2.0] | 1.3 [0.6–2.1] | 0.1% | 1.3% | 0.0% | MARS-T (VSE\*) |
| Approval cond. runoff | two-round | 88.0 [85.8–90.0] | 92.1 [90.4–93.5] | 99.8 [99.8–99.9] | 65.9 | 67.1 | 89.8 [88.1–91.5] | 90.9 [89.1–92.5] | 80.4 [77.4–83.1] | 81.8 [78.8–84.5] | 1.0 [0.4–1.9] | 1.1 [0.4–2.0] | 0.1% | 2.0% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), ESR (VSE\*) +7 more |
| Smith//Score | condorcet-based | 87.9 [85.6–89.9] | 94.3 [93.0–95.5] | 100.0 [99.9–100.0] | 89.3 | 92.8 | 87.8 [85.5–89.8] | 94.7 [93.4–95.9] | 83.4 [80.3–86.2] | 83.4 [80.7–86.3] | 1.1 [0.4–2.0] | 1.3 [0.6–2.1] | 0.0% | 0.9% | 0.0% | — |
| Soft Minimax-D | hybrid | 87.8 [85.5–89.8] | 94.5 [93.2–95.7] | 100.0 [100.0–100.0] | 89.4 | 92.8 | 88.4 [86.2–90.3] | 93.8 [92.5–95.2] | 82.0 [78.8–85.0] | 83.4 [80.7–86.2] | 1.1 [0.4–2.0] | 1.7 [0.9–2.7] | 0.0% | 0.9% | 0.0% | — |
| IRV-3 | plurality-based | 87.0 [84.8–89.2] | 84.4 [82.1–86.7] | 99.2 [99.1–99.2] | 73.0 | 74.6 | 85.3 [82.5–87.9] | 80.7 [78.2–83.3] | 64.5 [61.1–67.8] | 68.7 [65.3–72.1] | 3.7 [2.4–5.1] | 3.3 [2.0–4.6] | 6.1% | 4.3% | 1.2% | Score (VSE\* WME\*), MJ (VSE\* WME\*), DMS (VSE\* WME\*) +17 more |
| Approval runoff | two-round | 87.0 [84.7–89.1] | 93.7 [92.2–95.1] | 99.8 [99.8–99.8] | 65.8 | 67.1 | 88.9 [87.0–90.7] | 92.7 [91.0–94.2] | 84.0 [81.1–86.4] | 85.5 [82.9–88.1] | 1.0 [0.4–1.9] | 1.4 [0.6–2.4] | 0.1% | 2.3% | 0.0% | Score runoff (WME\*), MARS (VSE\*), MARS-T (VSE\*) +1 more |
| Soft Minimax | hybrid | 86.8 [84.3–88.9] | 94.8 [93.5–96.0] | 100.0 [99.9–100.0] | 89.5 | 92.8 | 86.8 [84.4–89.0] | 94.7 [93.5–95.9] | 83.7 [80.8–86.5] | 83.7 [80.9–86.4] | 1.3 [0.6–2.1] | 1.4 [0.6–2.4] | 0.1% | 0.8% | 0.0% | Smith//Score (VSE\*), Score runoff (WME\*) |
| ER-IRV | plurality-based | 86.7 [84.5–88.6] | 88.6 [86.7–90.4] | 98.5 [98.5–98.6] | 75.2 | 77.9 | 86.2 [83.9–88.4] | 83.6 [81.3–86.0] | 73.2 [70.1–76.4] | 78.0 [75.0–81.1] | 1.7 [0.9–2.7] | 1.6 [0.7–2.6] | 1.5% | 15.9% | 0.3% | MARS (VSE\* WME\* sincerity), MARS-T (VSE\* WME\* sincerity), ARA (VSE\* WME\* sincerity) +16 more |
| BTR-score | hybrid | 86.6 [84.2–88.9] | 94.1 [92.7–95.4] | 99.9 [99.9–100.0] | 89.4 | 92.8 | 87.2 [84.8–89.3] | 94.7 [93.4–95.9] | 83.7 [80.8–86.4] | 83.4 [80.6–86.2] | 1.3 [0.6–2.1] | 1.9 [1.0–3.0] | 0.1% | 1.1% | 0.0% | Smith//Score (VSE\*), STAR (VSE\*), Score runoff (WME\*) +3 more |
| IRV | plurality-based | 86.3 [84.0–88.4] | 88.2 [86.0–90.3] | 98.1 [98.0–98.2] | 75.2 | 77.9 | 86.2 [83.9–88.4] | 83.6 [81.3–86.0] | 73.2 [70.1–76.4] | 78.5 [75.4–81.5] | 1.7 [0.9–2.7] | 1.6 [0.7–2.6] | 6.1% | 11.7% | 1.4% | ESR (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity), MARS (VSE\* WME\* sincerity) +16 more |
| Score runoff | two-round | 86.1 [83.8–88.2] | 97.4 [96.5–98.2] | 99.9 [99.9–99.9] | 89.0 | 92.8 | 86.3 [84.2–88.3] | 97.9 [97.0–98.6] | 93.8 [91.9–95.6] | 93.5 [91.5–95.2] | 1.7 [0.9–2.7] | 1.7 [0.9–2.7] | 0.2% | 1.3% | 0.0% | — |
| TTR | two-round | 84.1 [81.6–86.4] | 85.2 [82.6–87.7] | 97.4 [97.2–97.6] | 61.2 | 63.6 | 82.3 [79.6–84.7] | 80.0 [77.1–82.9] | 69.7 [66.2–72.9] | 75.0 [71.5–78.4] | 4.0 [2.7–5.6] | 2.9 [1.7–4.1] | 6.8% | 0.0% | 2.6% | Smith//Score (VSE\* WME\* sincerity), ER-IRV (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +23 more |
| Borda | positional | 82.1 [79.2–84.9] | 92.1 [90.5–93.6] | 98.3 [98.2–98.4] | 75.0 | 77.9 | 79.4 [76.1–82.3] | 94.9 [93.7–95.9] | 83.7 [80.9–86.5] | 79.0 [75.9–82.2] | 2.4 [1.3–3.7] | 2.6 [1.4–3.7] | 6.7% | 9.6% | 0.8% | Smith//Score (VSE\* WME\* sincerity), Score runoff (VSE\* WME\* sincerity), BTR-score (VSE\* WME\* sincerity) +19 more |
| Copeland | condorcet | 82.0 [79.3–84.4] | 98.4 [97.9–98.9] | 98.4 [98.3–98.4] | 75.4 | 77.9 | 81.0 [78.1–83.5] | 99.6 [99.4–99.8] | 100.0 [100.0–100.0] | 95.4 [93.9–96.8] | 3.4 [2.1–4.9] | 3.1 [2.0–4.6] | 5.8% | 9.7% | 0.8% | Schulze (WME\*), Kemeny-Young (WME\*), Minimax (WME\*) +2 more |
| BTR-IRV | condorcet-based | 81.8 [78.8–84.5] | 98.3 [97.6–98.9] | 98.3 [98.3–98.4] | 75.4 | 77.9 | 81.4 [78.4–84.0] | 99.6 [99.4–99.8] | 100.0 [100.0–100.0] | 94.7 [92.9–96.4] | 3.4 [2.1–4.9] | 2.4 [1.3–3.6] | 5.9% | 10.2% | 0.9% | Score runoff (VSE\* sincerity), Ranked pairs (WME\*), Schulze (WME\*) +4 more |
| Schulze | condorcet | 81.7 [78.8–84.1] | 99.3 [99.0–99.6] | 98.4 [98.4–98.5] | 75.6 | 77.9 | 81.5 [78.6–84.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.1 [95.7–98.3] | 3.3 [2.0–4.7] | 3.3 [2.0–4.7] | 5.8% | 10.0% | 0.7% | — |
| Minimax | minimax-family | 81.4 [78.6–84.0] | 99.4 [99.0–99.6] | 98.4 [98.4–98.5] | 75.6 | 77.9 | 81.5 [78.6–84.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.2 [96.0–98.3] | 3.3 [2.0–4.7] | 3.1 [2.0–4.4] | 5.7% | 9.9% | 0.7% | — |
| Supplementary vote | plurality-based | 81.3 [78.3–84.4] | 76.5 [73.7–79.3] | 98.3 [98.3–98.4] | 75.4 | 77.9 | 75.2 [71.1–79.0] | 68.9 [65.8–72.2] | 54.4 [50.6–58.1] | 60.3 [56.7–64.0] | 8.7 [6.7–10.9] | 5.3 [3.6–7.0] | 6.5% | 13.4% | 1.0% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +30 more |
| MMPO | minimax-family | 81.1 [78.4–83.7] | 99.2 [98.8–99.6] | 98.7 [98.6–98.8] | 75.6 | 77.9 | 81.5 [78.6–84.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.6 [95.4–97.9] | 3.3 [2.0–4.7] | 3.0 [1.9–4.4] | 0.2% | 15.5% | 0.0% | Ranked pairs (WME\*) |
| Kemeny-Young | condorcet | 81.0 [78.2–83.6] | 99.3 [98.9–99.6] | 98.4 [98.4–98.5] | 75.5 | 77.9 | 81.0 [78.0–83.6] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 97.3 [96.1–98.5] | 3.4 [2.1–4.9] | 3.0 [1.9–4.3] | 5.7% | 10.0% | 0.7% | Ranked pairs (WME\*) |
| MMWV | minimax-family | 81.0 [78.2–83.6] | 99.3 [98.9–99.6] | 98.7 [98.7–98.8] | 75.6 | 77.9 | 81.5 [78.6–84.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.4 [96.3–98.5] | 3.3 [2.0–4.7] | 3.1 [2.0–4.6] | 0.2% | 15.4% | 0.0% | — |
| Ranked pairs | condorcet | 80.2 [77.1–82.9] | 99.6 [99.4–99.8] | 98.4 [98.4–98.5] | 75.5 | 77.9 | 81.0 [78.0–83.6] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 98.1 [97.0–99.0] | 3.4 [2.1–4.9] | 3.7 [2.4–5.1] | 5.8% | 9.9% | 0.7% | — |
| Plurality | plurality-based | 76.9 [73.4–80.7] | 66.9 [63.4–70.5] | 98.2 [98.1–98.4] | 61.6 | 63.6 | 64.6 [58.9–69.6] | 57.9 [54.2–61.7] | 46.8 [43.1–50.6] | 53.6 [49.9–57.5] | 12.1 [9.9–14.6] | 6.7 [4.9–8.6] | 8.4% | 0.0% | 1.6% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +31 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 46.7% |
| Supplementary vote | 43.4% |
| IRV-3 | 40.0% |
| IRV | 39.8% |
| Approval | 39.7% |
| Geometric median | 39.0% |
| DMS | 38.5% |
| TTR | 38.3% |
| Score | 38.1% |
| MARS-T | 37.9% |
| ER-IRV | 37.7% |
| ESR | 37.7% |
| Approval runoff | 37.3% |
| Approval cond. runoff | 37.1% |
| Soft Minimax-D | 37.0% |
| MARS | 37.0% |
| BTR-score | 36.9% |
| MAV | 36.9% |
| Smith//Score | 36.8% |
| 321 | 36.7% |
| STAR | 36.7% |
| Soft Minimax-40 | 36.6% |
| ARA | 36.5% |
| BTR-IRV | 36.4% |
| MJ | 36.2% |
| Soft Minimax | 36.1% |
| Score runoff | 36.1% |
| Schulze | 35.7% |
| Kemeny-Young | 35.5% |
| Ranked pairs | 35.5% |
| Minimax | 35.4% |
| MMPO | 35.0% |
| MMWV | 34.8% |
| Copeland | 34.7% |
| Borda | 33.6% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 676 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 24 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 81.8% (82.1%) | 13.5% (13.6%) | 4.7% (4.3%) | 62.5% (70.8%) | 84.1% (82.7%) | 70.7% |
| Score | 82.1% (82.1%) | 13.6% (13.6%) | 4.3% (4.3%) | 70.8% (70.8%) | 86.0% (86.9%) | 74.8% |
| MAV | 80.6% (82.1%) | 14.3% (13.6%) | 5.0% (4.3%) | 70.8% (70.8%) | 82.2% (76.7%) | 69.5% |
| ESR | 81.8% (82.1%) | 14.3% (13.6%) | 3.8% (4.3%) | 70.8% (70.8%) | 90.9% (90.8%) | 76.4% |
| ARA | 82.0% (82.1%) | 13.9% (13.6%) | 4.1% (4.3%) | 66.7% (70.8%) | 91.8% (90.9%) | 77.9% |
| DMS | 80.8% (82.1%) | 14.8% (13.6%) | 4.4% (4.3%) | 66.7% (70.8%) | 84.9% (79.5%) | 72.0% |
| MJ | 81.5% (82.1%) | 14.2% (13.6%) | 4.3% (4.3%) | 66.7% (70.8%) | 82.7% (78.9%) | 72.6% |
| MARS-T | 80.6% (82.1%) | 14.5% (13.6%) | 4.9% (4.3%) | 70.8% (70.8%) | 93.9% (95.9%) | 80.0% |
| Soft Minimax-40 | 82.4% (82.1%) | 13.5% (13.6%) | 4.1% (4.3%) | 70.8% (70.8%) | 92.6% (92.0%) | 78.7% |
| 321 | 73.8% (73.2%) | 22.5% (21.4%) | 3.7% (5.3%) | 100.0% (100.0%) | 95.0% (92.6%) | 74.1% |
| MARS | 81.7% (82.1%) | 14.5% (13.6%) | 3.8% (4.3%) | 83.3% (70.8%) | 96.6% (97.0%) | 81.4% |
| STAR | 81.7% (82.1%) | 13.6% (13.6%) | 4.7% (4.3%) | 75.0% (70.8%) | 98.0% (97.7%) | 82.0% |
| Smith//Score | 81.8% (82.1%) | 13.5% (13.6%) | 4.7% (4.3%) | 66.7% (70.8%) | 100.0% (100.0%) | 83.4% |
| Soft Minimax-D | 81.5% (82.1%) | 13.9% (13.6%) | 4.6% (4.3%) | 62.5% (70.8%) | 99.5% (98.8%) | 83.4% |
| IRV-3 | 71.2% (70.6%) | 21.3% (21.9%) | 7.5% (7.5%) | 95.8% (87.5%) | 93.7% (88.7%) | 68.7% |
| Soft Minimax | 81.8% (82.1%) | 13.9% (13.6%) | 4.3% (4.3%) | 70.8% (70.8%) | 100.0% (100.0%) | 83.7% |
| ER-IRV | 93.9% (100.0%) | 3.4% (0.0%) | 2.7% (0.0%) | 37.5% (0.0%) | 79.5% (73.2%) | 78.0% |
| BTR-score | 81.5% (82.1%) | 13.8% (13.6%) | 4.7% (4.3%) | 75.0% (70.8%) | 100.0% (100.0%) | 83.4% |
| IRV | 93.0% (100.0%) | 4.3% (0.0%) | 2.7% (0.0%) | 33.3% (0.0%) | 80.2% (73.2%) | 78.5% |
| Borda | 93.6% (100.0%) | 4.4% (0.0%) | 1.9% (0.0%) | 54.2% (0.0%) | 81.7% (83.7%) | 79.0% |
| Copeland | 93.8% (100.0%) | 2.4% (0.0%) | 3.8% (0.0%) | 33.3% (0.0%) | 100.0% (100.0%) | 95.4% |
| BTR-IRV | 94.2% (100.0%) | 2.2% (0.0%) | 3.6% (0.0%) | 29.2% (0.0%) | 100.0% (100.0%) | 94.7% |
| Schulze | 96.0% (100.0%) | 2.2% (0.0%) | 1.8% (0.0%) | 33.3% (0.0%) | 100.0% (100.0%) | 97.1% |
| Minimax | 95.9% (100.0%) | 2.1% (0.0%) | 2.1% (0.0%) | 37.5% (0.0%) | 100.0% (100.0%) | 97.2% |
| Supplementary vote | 92.2% (100.0%) | 5.8% (0.0%) | 2.1% (0.0%) | 41.7% (0.0%) | 63.5% (54.4%) | 60.3% |
| MMPO | 93.6% (100.0%) | 1.9% (0.0%) | 4.4% (0.0%) | 12.5% (0.0%) | 100.0% (100.0%) | 96.6% |
| Kemeny-Young | 95.7% (100.0%) | 1.8% (0.0%) | 2.5% (0.0%) | 33.3% (0.0%) | 100.0% (100.0%) | 97.3% |
| MMWV | 94.1% (100.0%) | 1.5% (0.0%) | 4.4% (0.0%) | 20.8% (0.0%) | 100.0% (100.0%) | 97.4% |
| Ranked pairs | 96.4% (100.0%) | 1.5% (0.0%) | 2.1% (0.0%) | 25.0% (0.0%) | 100.0% (100.0%) | 98.1% |

![strategy arrows](arrows-c5.png)

![strategy shifts](shift-c5.png)

![sincerity](sincerity-c5.png)

![fidelity](fidelity-c5.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c5.png)

## c6: candidates 9

700 elections (seeds 1), 665 with a Condorcet winner. 99 voters, 9 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 95.0%; it also has the highest total utility in 61.7% of those; electing it loses 15.9% of VSE; favourite ÷ least liked 12.0 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 82.5 to 83.3; with the best VSE (100) WME 77.2 (nothing lower is possible there either); the most balanced point VSE 94.4 / WME 95.8. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 93.6 (at WME 90.2); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -49.5 / WME -39.7.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, Score, Soft Minimax-40, ARA, MARS-T, STAR, Soft Minimax-D, Smith//Score, BTR-score, Soft Minimax, Score runoff, BTR-IRV, Ranked pairs, Minimax, MMPO.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Soft Minimax-40, ARA, MARS, MARS-T, Score runoff, Ranked pairs, Schulze, Minimax, Kemeny-Young, MMWV.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 92.4 [90.7–94.0] | 86.3 [84.2–88.2] | 100.0 [99.9–100.0] | 88.9 | 92.4 | 93.2 [91.5–94.7] | 86.2 [84.1–88.1] | 70.5 [67.1–74.0] | 70.7 [67.4–74.1] | 1.9 [1.0–2.9] | 1.7 [0.9–2.7] | 0.1% | 1.0% | 0.0% | Score (WME\*), ESR (WME\*), MARS (WME\*) +2 more |
| Score | rated | 92.2 [90.8–93.6] | 89.9 [88.1–91.6] | 100.0 [100.0–100.0] | 88.3 | 92.4 | 93.0 [91.6–94.4] | 90.6 [89.0–92.1] | 76.1 [73.0–79.4] | 76.5 [73.3–79.8] | 1.6 [0.9–2.4] | 1.3 [0.6–2.1] | 0.0% | 0.8% | 0.0% | ARA (WME\*), Soft Minimax-40 (WME\*) |
| DMS | rated | 92.0 [90.5–93.4] | 86.0 [83.8–88.0] | 100.0 [99.9–100.0] | 88.5 | 92.4 | 91.3 [89.4–93.1] | 80.3 [77.9–82.8] | 64.7 [61.1–68.1] | 71.7 [68.4–74.9] | 1.7 [0.9–2.7] | 1.7 [0.9–2.7] | 0.0% | 0.8% | 0.0% | Score (WME\*), ESR (WME\*), MARS (WME\*) +3 more |
| MAV | median-based | 91.8 [90.2–93.3] | 86.6 [84.5–88.8] | 100.0 [99.9–100.0] | 88.6 | 92.4 | 91.4 [89.8–93.0] | 83.6 [81.2–85.9] | 68.1 [64.5–71.5] | 71.4 [67.7–74.9] | 1.0 [0.4–1.9] | 1.1 [0.4–2.0] | 0.0% | 1.0% | 0.0% | Score (WME\*), ESR (WME\*), MARS (WME\*) +3 more |
| ESR | rated | 91.6 [90.1–93.2] | 89.7 [87.9–91.3] | 100.0 [99.9–100.0] | 88.8 | 92.4 | 92.6 [91.2–94.0] | 91.7 [90.2–93.2] | 78.9 [76.0–81.9] | 76.3 [73.3–79.6] | 1.1 [0.4–1.9] | 1.4 [0.6–2.3] | 0.1% | 0.8% | 0.0% | MARS (WME\*), MARS-T (WME\*), ARA (WME\*) +1 more |
| Soft Minimax-40 | hybrid | 91.6 [89.6–93.3] | 92.4 [90.9–93.8] | 100.0 [100.0–100.0] | 89.1 | 92.4 | 92.0 [90.4–93.5] | 92.1 [90.6–93.4] | 80.1 [77.1–83.0] | 81.0 [78.1–83.9] | 1.3 [0.4–2.1] | 1.0 [0.3–1.9] | 0.0% | 0.8% | 0.0% | — |
| ARA | hybrid | 91.6 [90.0–93.2] | 93.0 [91.7–94.2] | 100.0 [99.9–100.0] | 88.8 | 92.4 | 92.2 [90.7–93.7] | 92.6 [91.2–93.8] | 80.6 [77.7–83.4] | 81.2 [78.3–84.1] | 1.0 [0.3–1.7] | 1.4 [0.7–2.3] | 0.0% | 1.0% | 0.0% | — |
| 321 | hybrid | 91.3 [89.7–92.7] | 87.8 [85.8–89.9] | 99.9 [99.9–99.9] | 79.7 | 82.1 | 90.5 [88.8–92.1] | 86.3 [84.0–88.4] | 73.5 [70.1–76.8] | 76.2 [73.1–79.6] | 1.3 [0.6–2.1] | 1.3 [0.4–2.0] | 0.1% | 1.5% | 0.0% | Score (WME\*), ESR (WME\*), STAR (WME\*) +4 more |
| MARS | hybrid | 91.1 [89.5–92.6] | 92.9 [91.6–94.3] | 100.0 [100.0–100.0] | 88.9 | 92.4 | 91.5 [89.9–93.1] | 93.5 [92.2–94.7] | 82.3 [79.3–85.0] | 80.6 [77.5–83.7] | 1.0 [0.3–1.7] | 1.3 [0.6–2.1] | 0.0% | 0.7% | 0.0% | — |
| MARS-T | hybrid | 90.9 [89.3–92.5] | 93.3 [91.9–94.6] | 100.0 [100.0–100.0] | 88.7 | 92.4 | 91.8 [90.3–93.4] | 93.0 [91.7–94.3] | 81.6 [78.7–84.4] | 81.3 [78.3–84.3] | 1.0 [0.3–1.7] | 1.4 [0.6–2.3] | 0.0% | 0.7% | 0.0% | — |
| MJ | median-based | 90.3 [88.3–92.0] | 87.6 [85.5–89.5] | 100.0 [99.9–100.0] | 88.4 | 92.4 | 90.3 [88.4–92.0] | 85.4 [83.2–87.5] | 71.0 [67.6–74.4] | 74.1 [70.8–77.3] | 1.3 [0.6–2.1] | 1.6 [0.9–2.6] | 0.0% | 1.0% | 0.0% | Score (VSE\* WME\*), Smith//Score (WME\*), Approval runoff (WME\*) +11 more |
| STAR | hybrid | 90.1 [88.2–91.8] | 93.7 [92.3–95.0] | 100.0 [99.9–100.0] | 89.0 | 92.4 | 90.8 [89.2–92.5] | 93.5 [92.2–94.7] | 82.5 [79.7–85.3] | 83.1 [80.1–85.9] | 1.1 [0.4–2.0] | 1.4 [0.6–2.3] | 0.1% | 0.9% | 0.0% | ARA (VSE\*) |
| Approval | approval-based | 89.9 [87.8–91.8] | 80.4 [78.0–82.8] | 99.9 [99.9–99.9] | 65.0 | 66.0 | 88.2 [85.8–90.6] | 78.4 [75.9–80.8] | 62.9 [59.2–66.4] | 65.6 [62.2–69.0] | 3.6 [2.3–4.9] | 2.6 [1.4–3.7] | 0.0% | 1.3% | 0.0% | Score (VSE\* WME\*), MAV (VSE\* WME\*), DMS (VSE\* WME\*) +15 more |
| Soft Minimax-D | hybrid | 89.4 [87.5–91.3] | 94.1 [92.8–95.4] | 100.0 [100.0–100.0] | 89.1 | 92.4 | 89.8 [87.9–91.6] | 94.3 [93.1–95.5] | 84.1 [81.2–86.7] | 83.5 [80.6–86.3] | 1.3 [0.6–2.1] | 1.7 [0.7–2.6] | 0.0% | 0.7% | 0.0% | MARS-T (VSE\*), ARA (VSE\*) |
| Smith//Score | condorcet-based | 88.9 [86.8–90.8] | 94.1 [92.8–95.3] | 100.0 [100.0–100.0] | 89.1 | 92.4 | 89.2 [87.1–91.1] | 94.4 [93.2–95.6] | 84.4 [81.8–87.1] | 84.1 [81.4–86.6] | 1.6 [0.7–2.6] | 1.3 [0.6–2.1] | 0.0% | 0.7% | 0.0% | STAR (VSE\*), MARS-T (VSE\*), ARA (VSE\*) |
| Approval cond. runoff | two-round | 88.8 [86.9–90.6] | 92.0 [90.3–93.6] | 99.9 [99.8–99.9] | 64.9 | 66.0 | 88.6 [86.4–90.7] | 89.4 [87.5–91.3] | 79.9 [76.9–82.9] | 83.3 [80.5–85.9] | 2.3 [1.3–3.4] | 1.7 [0.9–2.7] | 0.1% | 1.8% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), BTR-score (WME\*) +6 more |
| BTR-score | hybrid | 88.5 [86.5–90.4] | 94.4 [93.2–95.5] | 100.0 [100.0–100.0] | 89.1 | 92.4 | 89.0 [86.9–90.8] | 94.7 [93.7–95.9] | 84.9 [82.3–87.4] | 84.5 [82.1–87.2] | 1.6 [0.7–2.6] | 1.1 [0.4–2.0] | 0.0% | 0.8% | 0.0% | STAR (VSE\*), Score runoff (WME\*) |
| Approval runoff | two-round | 88.4 [86.6–90.2] | 93.2 [91.5–94.7] | 99.9 [99.8–99.9] | 64.8 | 66.0 | 87.9 [85.8–90.0] | 90.7 [88.7–92.4] | 83.5 [80.5–86.1] | 87.1 [84.4–89.5] | 2.4 [1.4–3.6] | 1.9 [0.9–2.9] | 0.1% | 1.9% | 0.0% | STAR (VSE\*), Score runoff (WME\*), MARS (VSE\*) +3 more |
| Soft Minimax | hybrid | 88.1 [85.9–90.0] | 94.8 [93.6–95.9] | 100.0 [100.0–100.0] | 89.2 | 92.4 | 88.1 [85.9–90.1] | 95.1 [94.0–96.2] | 85.4 [82.8–87.9] | 85.0 [82.5–87.4] | 1.6 [0.7–2.6] | 1.6 [0.7–2.4] | 0.1% | 0.7% | 0.0% | Smith//Score (VSE\*), Score runoff (WME\*), Soft Minimax-D (VSE\*) |
| Score runoff | two-round | 87.1 [85.2–89.1] | 96.9 [95.9–97.8] | 99.9 [99.9–100.0] | 88.7 | 92.4 | 87.8 [85.9–89.5] | 97.1 [96.2–98.0] | 93.6 [91.7–95.4] | 93.4 [91.4–95.2] | 1.3 [0.4–2.1] | 1.4 [0.6–2.3] | 0.1% | 0.9% | 0.0% | — |
| IRV-3 | plurality-based | 86.3 [83.6–88.6] | 81.9 [79.5–84.3] | 99.3 [99.2–99.3] | 72.3 | 73.7 | 84.5 [81.6–87.0] | 77.8 [75.1–80.5] | 63.4 [59.8–67.0] | 67.1 [63.6–70.8] | 3.9 [2.4–5.4] | 3.1 [1.9–4.4] | 6.7% | 3.5% | 1.1% | Smith//Score (VSE\* WME\*), Approval cond. runoff (VSE\* WME\*), Score (VSE\* WME\*) +21 more |
| ER-IRV | plurality-based | 85.6 [83.2–87.8] | 85.5 [83.1–87.7] | 98.7 [98.6–98.7] | 75.1 | 77.5 | 85.9 [83.7–88.0] | 80.8 [78.2–83.4] | 71.4 [68.0–74.8] | 76.4 [73.2–79.5] | 2.0 [1.0–3.1] | 2.1 [1.1–3.1] | 1.4% | 16.4% | 0.2% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +18 more |
| IRV | plurality-based | 85.5 [83.0–87.6] | 86.3 [83.9–88.7] | 98.4 [98.3–98.5] | 75.1 | 77.5 | 85.9 [83.7–88.0] | 80.8 [78.2–83.4] | 71.4 [68.0–74.8] | 76.8 [73.6–79.9] | 2.0 [1.0–3.1] | 1.7 [0.9–2.7] | 6.1% | 10.9% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +18 more |
| BTR-IRV | condorcet-based | 83.8 [81.3–86.0] | 98.4 [97.8–98.9] | 98.6 [98.5–98.6] | 75.3 | 77.5 | 83.2 [80.8–85.5] | 99.6 [99.2–99.8] | 100.0 [100.0–100.0] | 95.6 [94.0–97.1] | 3.0 [1.9–4.4] | 2.6 [1.4–3.7] | 6.4% | 9.4% | 0.7% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +1 more |
| Borda | positional | 83.7 [81.1–86.2] | 92.7 [91.3–94.1] | 98.5 [98.5–98.6] | 74.8 | 77.5 | 83.4 [80.9–85.9] | 95.5 [94.4–96.4] | 85.9 [83.4–88.6] | 79.6 [76.7–82.8] | 1.6 [0.7–2.6] | 2.9 [1.7–4.1] | 6.9% | 8.7% | 0.7% | Score runoff (VSE\* WME\* sincerity), BTR-score (VSE\* WME\* sincerity), Soft Minimax (VSE\* WME\* sincerity) +17 more |
| Ranked pairs | condorcet | 83.5 [80.9–85.8] | 99.3 [98.9–99.6] | 98.7 [98.6–98.7] | 75.5 | 77.5 | 82.9 [80.5–85.3] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.1 [95.8–98.4] | 2.7 [1.6–4.0] | 2.9 [1.7–4.1] | 6.3% | 9.2% | 0.6% | — |
| TTR | two-round | 83.3 [80.9–85.6] | 82.1 [79.3–84.8] | 97.4 [97.2–97.6] | 60.3 | 62.6 | 80.8 [77.8–83.7] | 74.8 [71.6–77.7] | 64.9 [61.2–68.4] | 72.9 [69.5–76.2] | 4.4 [3.0–6.0] | 2.7 [1.6–4.0] | 7.3% | 0.0% | 2.6% | Smith//Score (VSE\* WME\* sincerity), ER-IRV (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +29 more |
| Schulze | condorcet | 83.3 [80.8–85.6] | 99.3 [98.8–99.6] | 98.7 [98.6–98.7] | 75.5 | 77.5 | 82.9 [80.4–85.3] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.0 [95.7–98.2] | 2.7 [1.6–4.0] | 2.7 [1.6–3.9] | 6.1% | 8.9% | 0.6% | — |
| Minimax | minimax-family | 83.1 [80.6–85.5] | 99.3 [98.8–99.7] | 98.7 [98.6–98.7] | 75.5 | 77.5 | 82.9 [80.4–85.3] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.1 [95.8–98.3] | 2.7 [1.6–4.0] | 2.7 [1.6–3.9] | 6.1% | 8.8% | 0.7% | — |
| Kemeny-Young | condorcet | 82.8 [80.3–85.3] | 99.3 [98.9–99.6] | 98.6 [98.6–98.7] | 75.5 | 77.5 | 82.7 [80.3–85.1] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.4 [96.2–98.5] | 2.7 [1.6–4.0] | 3.0 [1.7–4.4] | 6.3% | 9.1% | 0.6% | — |
| Copeland | condorcet | 82.4 [79.9–84.9] | 98.3 [97.6–98.9] | 98.5 [98.5–98.6] | 75.2 | 77.5 | 82.4 [79.7–84.8] | 99.4 [99.1–99.7] | 100.0 [100.0–100.0] | 95.9 [94.2–97.2] | 3.0 [1.7–4.3] | 3.0 [1.7–4.3] | 6.6% | 9.2% | 0.8% | Ranked pairs (VSE\* WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +4 more |
| MMWV | minimax-family | 82.4 [79.8–84.9] | 99.3 [98.9–99.6] | 98.8 [98.8–98.9] | 75.4 | 77.5 | 82.9 [80.4–85.3] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.0 [95.6–98.2] | 2.7 [1.6–4.0] | 2.7 [1.6–3.9] | 0.1% | 15.3% | 0.0% | — |
| MMPO | minimax-family | 82.0 [79.2–84.6] | 99.3 [98.9–99.6] | 98.8 [98.8–98.9] | 75.4 | 77.5 | 82.9 [80.4–85.3] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.5 [95.0–97.7] | 2.7 [1.6–4.0] | 2.9 [1.7–4.1] | 0.1% | 15.5% | 0.0% | Ranked pairs (VSE\*), Schulze (VSE\*) |
| Supplementary vote | plurality-based | 81.2 [78.0–84.3] | 73.3 [70.3–76.2] | 98.5 [98.5–98.6] | 75.3 | 77.5 | 73.8 [69.6–77.8] | 67.5 [64.2–70.6] | 54.8 [51.1–58.6] | 58.6 [54.9–62.3] | 10.3 [8.1–12.7] | 6.0 [4.3–7.7] | 7.2% | 13.2% | 1.1% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +29 more |
| Plurality | plurality-based | 76.8 [73.3–80.6] | 64.7 [61.3–67.9] | 98.3 [98.2–98.4] | 60.7 | 62.6 | 61.2 [56.3–66.2] | 54.5 [50.7–58.0] | 43.7 [39.9–47.3] | 50.6 [46.9–54.4] | 15.0 [12.3–17.7] | 7.6 [5.6–9.7] | 9.0% | 0.0% | 1.4% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +31 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 45.8% |
| Supplementary vote | 43.7% |
| IRV-3 | 40.4% |
| Approval | 38.2% |
| Geometric median | 37.8% |
| TTR | 37.4% |
| ER-IRV | 37.3% |
| IRV | 37.1% |
| DMS | 37.0% |
| Score | 36.3% |
| ESR | 35.9% |
| MARS-T | 35.8% |
| Approval runoff | 35.7% |
| ARA | 35.7% |
| MARS | 35.4% |
| Smith//Score | 35.3% |
| Approval cond. runoff | 35.3% |
| Soft Minimax-D | 35.3% |
| Score runoff | 35.2% |
| MAV | 35.2% |
| BTR-score | 35.1% |
| Soft Minimax-40 | 35.0% |
| STAR | 34.7% |
| MMPO | 34.6% |
| MMWV | 34.4% |
| BTR-IRV | 34.3% |
| Soft Minimax | 34.3% |
| Copeland | 34.3% |
| MJ | 34.2% |
| 321 | 34.1% |
| Ranked pairs | 34.0% |
| Kemeny-Young | 34.0% |
| Minimax | 33.4% |
| Schulze | 33.4% |
| Borda | 32.2% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 665 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 35 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 82.4% (83.2%) | 13.4% (11.9%) | 4.2% (5.0%) | 68.6% (71.4%) | 83.5% (82.2%) | 70.7% |
| Score | 82.7% (83.2%) | 14.1% (11.9%) | 3.2% (5.0%) | 68.6% (71.4%) | 87.8% (88.1%) | 76.5% |
| DMS | 82.6% (83.2%) | 13.7% (11.9%) | 3.8% (5.0%) | 54.3% (71.4%) | 84.1% (75.5%) | 71.7% |
| MAV | 82.7% (83.2%) | 13.7% (11.9%) | 3.6% (5.0%) | 57.1% (71.4%) | 82.8% (77.9%) | 71.4% |
| ESR | 82.1% (83.2%) | 13.7% (11.9%) | 4.2% (5.0%) | 62.9% (71.4%) | 90.7% (92.3%) | 76.3% |
| Soft Minimax-40 | 83.2% (83.2%) | 13.2% (11.9%) | 3.6% (5.0%) | 62.9% (71.4%) | 94.2% (93.2%) | 81.0% |
| ARA | 83.9% (83.2%) | 13.7% (11.9%) | 2.4% (5.0%) | 62.9% (71.4%) | 91.7% (91.6%) | 81.2% |
| 321 | 76.5% (75.0%) | 19.4% (21.7%) | 4.1% (3.3%) | 74.3% (85.7%) | 95.6% (91.8%) | 76.2% |
| MARS | 82.1% (83.2%) | 13.4% (11.9%) | 4.5% (5.0%) | 68.6% (71.4%) | 96.3% (96.8%) | 80.6% |
| MARS-T | 82.3% (83.2%) | 14.1% (11.9%) | 3.6% (5.0%) | 77.1% (71.4%) | 94.9% (96.0%) | 81.3% |
| MJ | 83.2% (83.2%) | 13.5% (11.9%) | 3.3% (5.0%) | 68.6% (71.4%) | 83.5% (78.5%) | 74.1% |
| STAR | 82.7% (83.2%) | 12.9% (11.9%) | 4.4% (5.0%) | 65.7% (71.4%) | 98.3% (97.7%) | 83.1% |
| Soft Minimax-D | 83.2% (83.2%) | 12.8% (11.9%) | 4.1% (5.0%) | 62.9% (71.4%) | 98.0% (99.1%) | 83.5% |
| Smith//Score | 82.7% (83.2%) | 13.7% (11.9%) | 3.6% (5.0%) | 71.4% (71.4%) | 100.0% (100.0%) | 84.1% |
| BTR-score | 82.9% (83.2%) | 12.8% (11.9%) | 4.4% (5.0%) | 65.7% (71.4%) | 100.0% (100.0%) | 84.5% |
| Soft Minimax | 83.3% (83.2%) | 12.5% (11.9%) | 4.2% (5.0%) | 62.9% (71.4%) | 100.0% (100.0%) | 85.0% |
| IRV-3 | 69.3% (68.6%) | 25.3% (24.1%) | 5.4% (7.4%) | 77.1% (82.9%) | 92.9% (86.5%) | 67.1% |
| ER-IRV | 93.5% (100.0%) | 4.2% (0.0%) | 2.3% (0.0%) | 25.7% (2.9%) | 77.1% (71.4%) | 76.4% |
| IRV | 92.8% (100.0%) | 3.8% (0.0%) | 3.5% (0.0%) | 28.6% (2.9%) | 79.5% (71.4%) | 76.8% |
| BTR-IRV | 94.9% (100.0%) | 2.1% (0.0%) | 3.0% (0.0%) | 20.0% (2.9%) | 100.0% (100.0%) | 95.6% |
| Borda | 91.7% (100.0%) | 3.2% (0.0%) | 5.1% (0.0%) | 37.1% (2.9%) | 82.8% (86.0%) | 79.6% |
| Ranked pairs | 95.2% (100.0%) | 1.8% (0.0%) | 3.0% (0.0%) | 25.7% (2.9%) | 100.0% (100.0%) | 97.1% |
| Schulze | 95.5% (100.0%) | 1.8% (0.0%) | 2.7% (0.0%) | 20.0% (2.9%) | 100.0% (100.0%) | 97.0% |
| Minimax | 95.8% (100.0%) | 2.1% (0.0%) | 2.1% (0.0%) | 25.7% (2.9%) | 100.0% (100.0%) | 97.1% |
| Kemeny-Young | 95.8% (100.0%) | 2.1% (0.0%) | 2.1% (0.0%) | 28.6% (2.9%) | 100.0% (100.0%) | 97.4% |
| Copeland | 95.2% (100.0%) | 2.3% (0.0%) | 2.6% (0.0%) | 25.7% (2.9%) | 100.0% (100.0%) | 95.9% |
| MMWV | 94.4% (100.0%) | 2.4% (0.0%) | 3.2% (0.0%) | 14.3% (2.9%) | 100.0% (100.0%) | 97.0% |
| MMPO | 94.1% (100.0%) | 2.4% (0.0%) | 3.5% (0.0%) | 14.3% (2.9%) | 100.0% (100.0%) | 96.5% |
| Supplementary vote | 93.1% (100.0%) | 4.8% (0.0%) | 2.1% (0.0%) | 45.7% (2.9%) | 62.6% (54.7%) | 58.6% |

![strategy arrows](arrows-c6.png)

![strategy shifts](shift-c6.png)

![sincerity](sincerity-c6.png)

![fidelity](fidelity-c6.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c6.png)

## c7: candidates 10

700 elections (seeds 1), 663 with a Condorcet winner. 99 voters, 10 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 94.7%; it also has the highest total utility in 60.9% of those; electing it loses 15.6% of VSE; favourite ÷ least liked 12.9 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 82.3 to 83.0; with the best VSE (100) WME 77.1 (nothing lower is possible there either); the most balanced point VSE 94.1 / WME 95.8. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 91.9 (at WME 89.4); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -47.1 / WME -38.1.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, ARA, MARS-T, BTR-score, Soft Minimax, Score runoff, Copeland, MMPO, Schulze.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): ARA, MARS-T, MARS, Soft Minimax-D, Approval runoff, BTR-score, Score runoff, MMPO, Schulze, Minimax, MMWV, Kemeny-Young, Ranked pairs.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 91.6 [90.0–93.3] | 85.5 [83.1–87.7] | 100.0 [100.0–100.0] | 88.5 | 91.9 | 92.1 [90.5–93.7] | 85.2 [82.9–87.6] | 70.4 [66.9–74.1] | 71.2 [67.7–74.6] | 2.0 [1.0–3.1] | 2.3 [1.3–3.4] | 0.1% | 0.8% | 0.0% | ESR (WME\*), ARA (WME\*) |
| DMS | rated | 91.5 [89.8–93.1] | 84.4 [82.0–86.6] | 100.0 [100.0–100.0] | 88.2 | 91.9 | 90.8 [89.0–92.6] | 81.2 [78.6–83.7] | 65.5 [62.0–69.3] | 68.3 [64.9–72.1] | 2.7 [1.6–4.0] | 2.0 [1.0–3.0] | 0.0% | 0.7% | 0.0% | Score (WME\*), ESR (WME\*), ARA (WME\*) |
| ARA | hybrid | 91.1 [89.2–92.8] | 91.1 [89.3–92.7] | 100.0 [100.0–100.0] | 88.5 | 91.9 | 91.0 [89.3–92.7] | 90.9 [89.1–92.6] | 78.4 [75.2–81.4] | 80.2 [77.0–83.2] | 1.1 [0.4–2.0] | 1.3 [0.4–2.1] | 0.0% | 0.8% | 0.0% | — |
| ESR | rated | 90.3 [88.3–92.0] | 90.1 [88.2–91.8] | 100.0 [100.0–100.0] | 88.3 | 91.9 | 91.7 [90.2–93.2] | 89.5 [87.6–91.3] | 77.6 [74.5–80.8] | 77.8 [74.6–80.9] | 1.3 [0.4–2.1] | 1.6 [0.7–2.6] | 0.1% | 0.9% | 0.0% | MARS-T (WME\*), Soft Minimax-40 (WME\*) |
| Score | rated | 90.2 [88.4–92.0] | 89.0 [87.2–90.9] | 100.0 [100.0–100.0] | 88.0 | 91.9 | 92.0 [90.6–93.4] | 89.3 [87.4–91.1] | 76.8 [73.6–80.0] | 75.0 [71.8–78.2] | 1.3 [0.6–2.1] | 1.6 [0.7–2.6] | 0.0% | 0.8% | 0.0% | MARS-T (WME\*), ARA (WME\*), Soft Minimax-40 (WME\*) |
| 321 | hybrid | 89.8 [87.9–91.6] | 86.9 [84.9–88.8] | 99.9 [99.9–99.9] | 78.8 | 81.1 | 90.5 [88.7–92.2] | 84.7 [82.5–86.8] | 70.6 [67.3–74.1] | 73.2 [69.8–76.4] | 1.4 [0.6–2.3] | 1.6 [0.7–2.6] | 0.1% | 1.4% | 0.0% | Approval runoff (WME\*), Approval cond. runoff (WME\*), Score (WME\*) +9 more |
| MARS-T | hybrid | 89.4 [87.5–91.3] | 93.0 [91.5–94.4] | 100.0 [100.0–100.0] | 88.4 | 91.9 | 89.6 [87.7–91.5] | 92.8 [91.2–94.3] | 83.2 [80.3–85.9] | 83.3 [80.3–86.0] | 1.6 [0.7–2.6] | 1.3 [0.6–2.3] | 0.0% | 0.6% | 0.0% | — |
| Soft Minimax-40 | hybrid | 89.4 [87.4–91.4] | 92.5 [90.9–93.8] | 100.0 [100.0–100.0] | 88.6 | 91.9 | 90.9 [89.0–92.6] | 91.4 [89.7–92.9] | 79.8 [76.6–82.8] | 81.7 [78.6–84.5] | 1.3 [0.6–2.1] | 1.6 [0.7–2.6] | 0.0% | 0.7% | 0.0% | BTR-score (WME\*) |
| Approval | approval-based | 89.0 [86.9–91.0] | 81.6 [79.2–84.1] | 99.9 [99.9–100.0] | 63.9 | 64.8 | 87.9 [85.5–90.3] | 77.3 [74.5–79.7] | 61.2 [57.6–64.9] | 66.1 [62.7–70.0] | 3.6 [2.3–5.0] | 2.4 [1.4–3.6] | 0.0% | 1.2% | 0.0% | DMS (VSE\* WME\*), Geometric median (VSE\* WME\*), ARA (VSE\* WME\*) +15 more |
| MAV | median-based | 89.0 [86.9–90.8] | 84.1 [81.4–86.5] | 100.0 [100.0–100.0] | 88.2 | 91.9 | 89.8 [87.9–91.4] | 80.7 [78.1–83.3] | 65.7 [61.8–69.4] | 70.4 [67.0–73.9] | 1.3 [0.6–2.1] | 1.7 [0.9–2.7] | 0.0% | 0.9% | 0.0% | ARA (VSE\* WME\*), Smith//Score (WME\*), Approval runoff (WME\*) +13 more |
| MARS | hybrid | 88.8 [86.8–90.8] | 93.0 [91.3–94.4] | 100.0 [100.0–100.0] | 88.5 | 91.9 | 89.7 [87.8–91.5] | 92.8 [91.1–94.2] | 83.2 [80.1–85.9] | 82.7 [79.8–85.6] | 1.6 [0.7–2.6] | 1.6 [0.7–2.6] | 0.0% | 0.7% | 0.0% | — |
| Soft Minimax-D | hybrid | 88.8 [86.7–90.7] | 93.0 [91.4–94.5] | 100.0 [100.0–100.0] | 88.7 | 91.9 | 89.1 [87.3–91.0] | 93.3 [91.7–94.8] | 84.5 [81.7–87.1] | 83.0 [79.9–85.6] | 1.4 [0.6–2.4] | 1.9 [0.9–2.9] | 0.0% | 0.7% | 0.0% | — |
| Approval runoff | two-round | 88.7 [86.9–90.6] | 92.7 [91.2–94.3] | 99.9 [99.8–99.9] | 63.7 | 64.8 | 88.6 [86.5–90.4] | 90.2 [88.5–91.9] | 82.0 [79.0–84.7] | 85.7 [83.3–88.3] | 2.7 [1.6–4.0] | 1.6 [0.7–2.6] | 0.2% | 2.0% | 0.0% | — |
| Approval cond. runoff | two-round | 88.6 [86.6–90.5] | 91.7 [90.1–93.4] | 99.9 [99.9–99.9] | 63.8 | 64.8 | 88.8 [86.7–90.6] | 89.1 [87.3–91.0] | 79.5 [76.5–82.7] | 83.4 [80.5–86.2] | 2.7 [1.6–4.0] | 1.9 [0.9–3.0] | 0.1% | 1.9% | 0.0% | Approval runoff (WME\*), ARA (VSE\*), Soft Minimax (WME\*) |
| MJ | median-based | 88.6 [86.5–90.4] | 86.1 [83.7–88.2] | 100.0 [100.0–100.0] | 88.2 | 91.9 | 88.2 [86.2–90.2] | 84.8 [82.5–87.0] | 70.4 [66.8–74.2] | 71.9 [68.3–75.2] | 1.4 [0.6–2.3] | 1.4 [0.6–2.3] | 0.0% | 0.9% | 0.0% | ARA (VSE\* WME\*), Smith//Score (WME\*), Approval runoff (WME\*) +13 more |
| BTR-score | hybrid | 88.4 [86.4–90.3] | 93.5 [92.0–95.0] | 100.0 [100.0–100.0] | 88.7 | 91.9 | 88.0 [86.0–90.2] | 93.4 [91.9–94.8] | 84.5 [81.6–87.0] | 85.0 [82.3–87.7] | 1.6 [0.7–2.4] | 1.6 [0.7–2.6] | 0.1% | 0.7% | 0.0% | — |
| STAR | hybrid | 88.2 [86.0–90.2] | 93.1 [91.6–94.6] | 100.0 [100.0–100.0] | 88.5 | 91.9 | 90.1 [88.3–91.9] | 92.0 [90.4–93.7] | 82.2 [79.2–85.1] | 84.2 [81.3–86.7] | 1.4 [0.6–2.4] | 1.4 [0.6–2.4] | 0.1% | 0.9% | 0.0% | Score runoff (WME\*), Soft Minimax (WME\*) |
| Smith//Score | condorcet-based | 87.8 [85.7–89.9] | 93.4 [91.8–94.9] | 100.0 [100.0–100.0] | 88.7 | 91.9 | 88.5 [86.6–90.6] | 93.0 [91.3–94.4] | 83.8 [80.8–86.4] | 85.1 [82.3–87.7] | 1.7 [0.7–2.7] | 1.7 [0.9–2.7] | 0.0% | 0.6% | 0.0% | Score runoff (WME\*), MARS-T (VSE\*), Soft Minimax (WME\*) |
| Soft Minimax | hybrid | 87.3 [85.2–89.4] | 94.3 [92.8–95.6] | 100.0 [100.0–100.0] | 88.8 | 91.9 | 87.7 [85.6–89.8] | 93.7 [92.2–95.1] | 84.8 [82.0–87.3] | 85.6 [82.9–88.2] | 1.7 [0.9–2.7] | 2.0 [1.0–3.1] | 0.0% | 0.7% | 0.0% | Score runoff (WME\*), BTR-score (VSE\*) |
| Score runoff | two-round | 87.0 [84.9–88.9] | 96.0 [94.8–97.0] | 99.9 [99.9–100.0] | 88.4 | 91.9 | 87.7 [85.7–89.6] | 96.5 [95.4–97.5] | 92.2 [90.1–94.2] | 91.5 [89.4–93.6] | 2.0 [1.0–3.0] | 1.9 [0.9–2.9] | 0.1% | 1.0% | 0.0% | — |
| ER-IRV | plurality-based | 86.8 [84.6–88.9] | 87.2 [85.1–89.4] | 98.8 [98.7–98.8] | 74.4 | 76.7 | 86.5 [84.4–88.8] | 82.1 [79.6–84.5] | 70.9 [67.6–74.4] | 76.1 [73.1–79.4] | 2.6 [1.4–3.7] | 2.1 [1.1–3.1] | 1.6% | 16.0% | 0.3% | Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity), ESR (VSE\* WME\* sincerity) +15 more |
| Borda | positional | 85.6 [83.3–87.8] | 94.2 [92.9–95.3] | 98.7 [98.6–98.7] | 74.2 | 76.7 | 82.4 [79.8–84.8] | 95.9 [95.0–96.9] | 85.1 [82.4–87.9] | 81.9 [79.1–84.7] | 1.3 [0.6–2.1] | 1.3 [0.6–2.1] | 7.3% | 7.6% | 0.7% | Smith//Score (VSE\* sincerity), Approval runoff (VSE\* sincerity), STAR (VSE\* sincerity) +7 more |
| IRV-3 | plurality-based | 85.1 [82.6–87.6] | 79.4 [76.8–81.9] | 99.3 [99.3–99.4] | 71.2 | 72.4 | 83.6 [80.8–86.2] | 76.6 [73.9–79.2] | 60.1 [56.5–63.8] | 63.3 [59.7–67.0] | 4.9 [3.4–6.4] | 3.9 [2.4–5.3] | 7.0% | 2.7% | 1.0% | Smith//Score (VSE\* WME\*), Approval (VSE\* WME\*), Approval runoff (VSE\* WME\*) +27 more |
| IRV | plurality-based | 85.0 [82.7–87.4] | 85.1 [82.7–87.4] | 98.6 [98.5–98.6] | 74.6 | 76.7 | 86.5 [84.4–88.8] | 82.1 [79.6–84.5] | 70.9 [67.6–74.4] | 74.5 [71.2–77.7] | 2.6 [1.4–3.7] | 2.7 [1.6–3.9] | 6.2% | 10.4% | 1.2% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +21 more |
| TTR | two-round | 83.7 [81.3–86.1] | 82.7 [80.3–85.1] | 97.5 [97.3–97.7] | 59.8 | 61.9 | 79.9 [77.2–82.9] | 73.7 [70.7–76.9] | 63.6 [60.2–67.3] | 72.3 [69.0–75.8] | 5.4 [3.7–7.0] | 3.9 [2.4–5.3] | 7.7% | 0.0% | 2.4% | Smith//Score (VSE\* WME\* sincerity), ER-IRV (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +28 more |
| Copeland | condorcet | 83.5 [81.2–85.8] | 98.4 [97.9–98.8] | 98.7 [98.7–98.8] | 74.7 | 76.7 | 83.1 [80.9–85.4] | 99.4 [99.0–99.7] | 100.0 [100.0–100.0] | 95.2 [93.7–96.7] | 2.1 [1.0–3.3] | 1.7 [0.7–2.7] | 6.6% | 8.0% | 0.7% | Schulze (WME\*), Minimax (WME\*), MMWV (WME\*) +1 more |
| BTR-IRV | condorcet-based | 83.2 [80.7–85.6] | 98.1 [97.5–98.7] | 98.7 [98.6–98.7] | 74.7 | 76.7 | 83.3 [81.0–85.7] | 99.6 [99.3–99.8] | 100.0 [100.0–100.0] | 94.7 [92.9–96.3] | 2.4 [1.3–3.6] | 2.7 [1.4–4.0] | 6.5% | 8.8% | 0.8% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +3 more |
| MMPO | minimax-family | 83.1 [80.7–85.5] | 99.2 [98.8–99.5] | 99.0 [98.9–99.0] | 74.7 | 76.7 | 82.6 [80.4–85.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.0 [95.7–98.2] | 2.7 [1.4–3.9] | 2.7 [1.4–3.9] | 0.1% | 14.6% | 0.0% | — |
| Schulze | condorcet | 83.0 [80.5–85.3] | 99.4 [99.0–99.7] | 98.7 [98.7–98.8] | 74.8 | 76.7 | 82.6 [80.4–85.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.4 [96.1–98.6] | 2.7 [1.4–3.9] | 2.7 [1.6–3.9] | 6.5% | 8.4% | 0.6% | — |
| Minimax | minimax-family | 82.8 [80.4–85.2] | 99.4 [99.0–99.6] | 98.7 [98.7–98.8] | 74.8 | 76.7 | 82.6 [80.4–85.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.4 [96.2–98.6] | 2.7 [1.4–3.9] | 2.7 [1.6–4.0] | 6.5% | 8.3% | 0.6% | — |
| MMWV | minimax-family | 82.8 [80.3–85.3] | 99.2 [98.8–99.5] | 99.0 [98.9–99.0] | 74.7 | 76.7 | 82.6 [80.4–85.0] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.4 [96.2–98.5] | 2.7 [1.4–3.9] | 2.7 [1.4–3.9] | 0.1% | 14.6% | 0.0% | — |
| Kemeny-Young | condorcet | 82.3 [79.8–84.8] | 99.3 [98.9–99.6] | 98.8 [98.7–98.8] | 74.8 | 76.7 | 82.7 [80.3–85.1] | 99.9 [99.7–100.0] | 100.0 [100.0–100.0] | 97.3 [95.9–98.4] | 2.7 [1.6–4.0] | 2.9 [1.7–4.1] | 6.5% | 8.3% | 0.7% | — |
| Ranked pairs | condorcet | 82.3 [79.9–84.8] | 99.1 [98.7–99.6] | 98.7 [98.7–98.8] | 74.8 | 76.7 | 82.7 [80.4–85.1] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.6 [95.2–97.9] | 2.7 [1.4–3.9] | 2.9 [1.7–4.0] | 6.6% | 8.2% | 0.7% | — |
| Supplementary vote | plurality-based | 80.1 [76.7–83.5] | 72.7 [69.6–75.6] | 98.6 [98.6–98.7] | 74.6 | 76.7 | 70.1 [66.0–74.5] | 64.8 [61.5–68.0] | 49.7 [45.9–53.4] | 56.3 [52.7–60.0] | 10.1 [7.9–12.3] | 6.0 [4.1–7.7] | 7.5% | 12.6% | 1.0% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +29 more |
| Plurality | plurality-based | 75.0 [71.1–78.6] | 64.2 [60.6–67.5] | 98.3 [98.2–98.4] | 60.1 | 61.9 | 56.2 [51.0–61.7] | 52.6 [48.9–56.5] | 42.1 [38.5–45.6] | 48.7 [45.0–52.4] | 14.0 [11.4–16.4] | 7.6 [5.6–9.3] | 9.2% | 0.0% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +31 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 44.4% |
| Supplementary vote | 42.4% |
| IRV-3 | 38.9% |
| Approval | 37.3% |
| TTR | 36.7% |
| ER-IRV | 36.1% |
| Geometric median | 36.0% |
| DMS | 36.0% |
| IRV | 35.8% |
| Approval cond. runoff | 34.9% |
| Score | 34.8% |
| ESR | 34.8% |
| Approval runoff | 34.5% |
| MARS-T | 33.7% |
| 321 | 33.6% |
| ARA | 33.5% |
| STAR | 33.5% |
| MARS | 33.4% |
| BTR-score | 33.4% |
| Soft Minimax-40 | 33.2% |
| BTR-IRV | 33.2% |
| Soft Minimax-D | 33.1% |
| Score runoff | 33.0% |
| MAV | 32.7% |
| Smith//Score | 32.6% |
| Soft Minimax | 32.6% |
| Kemeny-Young | 32.4% |
| Schulze | 32.2% |
| Ranked pairs | 32.1% |
| Minimax | 32.1% |
| MMWV | 32.0% |
| MMPO | 32.0% |
| MJ | 31.9% |
| Copeland | 31.6% |
| Borda | 30.5% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 663 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 37 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 82.5% (82.8%) | 13.0% (12.7%) | 4.5% (4.5%) | 75.7% (67.6%) | 82.0% (80.4%) | 71.2% |
| DMS | 82.1% (82.8%) | 13.4% (12.7%) | 4.5% (4.5%) | 64.9% (67.6%) | 79.6% (74.6%) | 68.3% |
| ARA | 83.0% (82.8%) | 12.4% (12.7%) | 4.7% (4.5%) | 56.8% (67.6%) | 90.4% (88.2%) | 80.2% |
| ESR | 83.1% (82.8%) | 11.6% (12.7%) | 5.3% (4.5%) | 75.7% (67.6%) | 90.3% (89.1%) | 77.8% |
| Score | 82.4% (82.8%) | 12.2% (12.7%) | 5.4% (4.5%) | 67.6% (67.6%) | 84.5% (85.6%) | 75.0% |
| 321 | 75.0% (72.5%) | 21.3% (22.2%) | 3.8% (5.3%) | 83.8% (86.5%) | 94.3% (92.3%) | 73.2% |
| MARS-T | 83.1% (82.8%) | 11.6% (12.7%) | 5.3% (4.5%) | 67.6% (67.6%) | 94.9% (95.7%) | 83.3% |
| Soft Minimax-40 | 83.1% (82.8%) | 12.1% (12.7%) | 4.8% (4.5%) | 78.4% (67.6%) | 92.9% (90.3%) | 81.7% |
| MAV | 83.0% (82.8%) | 12.2% (12.7%) | 4.8% (4.5%) | 70.3% (67.6%) | 76.9% (73.9%) | 70.4% |
| MARS | 83.3% (82.8%) | 12.4% (12.7%) | 4.4% (4.5%) | 73.0% (67.6%) | 95.2% (96.2%) | 82.7% |
| Soft Minimax-D | 82.5% (82.8%) | 11.3% (12.7%) | 6.2% (4.5%) | 70.3% (67.6%) | 98.0% (98.8%) | 83.0% |
| MJ | 82.7% (82.8%) | 12.7% (12.7%) | 4.7% (4.5%) | 73.0% (67.6%) | 76.6% (75.1%) | 71.9% |
| BTR-score | 83.7% (82.8%) | 11.6% (12.7%) | 4.7% (4.5%) | 73.0% (67.6%) | 100.0% (100.0%) | 85.0% |
| STAR | 82.7% (82.8%) | 12.1% (12.7%) | 5.3% (4.5%) | 75.7% (67.6%) | 98.1% (97.3%) | 84.2% |
| Smith//Score | 84.0% (82.8%) | 12.1% (12.7%) | 3.9% (4.5%) | 73.0% (67.6%) | 100.0% (100.0%) | 85.1% |
| Soft Minimax | 83.9% (82.8%) | 12.1% (12.7%) | 4.1% (4.5%) | 67.6% (67.6%) | 100.0% (100.0%) | 85.6% |
| ER-IRV | 92.8% (100.0%) | 2.6% (0.0%) | 4.7% (0.0%) | 35.1% (0.0%) | 77.4% (70.9%) | 76.1% |
| Borda | 91.9% (100.0%) | 3.0% (0.0%) | 5.1% (0.0%) | 32.4% (0.0%) | 84.7% (85.1%) | 81.9% |
| IRV-3 | 64.3% (63.0%) | 27.0% (26.4%) | 8.7% (10.6%) | 86.5% (89.2%) | 91.2% (88.6%) | 63.3% |
| IRV | 92.6% (100.0%) | 3.8% (0.0%) | 3.6% (0.0%) | 37.8% (0.0%) | 75.7% (70.9%) | 74.5% |
| Copeland | 93.4% (100.0%) | 1.8% (0.0%) | 4.8% (0.0%) | 35.1% (0.0%) | 100.0% (100.0%) | 95.2% |
| BTR-IRV | 93.4% (100.0%) | 2.3% (0.0%) | 4.4% (0.0%) | 43.2% (0.0%) | 100.0% (100.0%) | 94.7% |
| MMPO | 95.2% (100.0%) | 1.5% (0.0%) | 3.3% (0.0%) | 21.6% (0.0%) | 99.8% (100.0%) | 97.0% |
| Schulze | 95.9% (100.0%) | 1.4% (0.0%) | 2.7% (0.0%) | 27.0% (0.0%) | 100.0% (100.0%) | 97.4% |
| Minimax | 95.8% (100.0%) | 1.5% (0.0%) | 2.7% (0.0%) | 32.4% (0.0%) | 100.0% (100.0%) | 97.4% |
| MMWV | 95.3% (100.0%) | 1.4% (0.0%) | 3.3% (0.0%) | 18.9% (0.0%) | 100.0% (100.0%) | 97.4% |
| Kemeny-Young | 95.6% (100.0%) | 2.0% (0.0%) | 2.4% (0.0%) | 27.0% (0.0%) | 100.0% (100.0%) | 97.3% |
| Ranked pairs | 95.0% (100.0%) | 2.0% (0.0%) | 3.0% (0.0%) | 29.7% (0.0%) | 100.0% (100.0%) | 96.6% |
| Supplementary vote | 93.8% (100.0%) | 4.1% (0.0%) | 2.1% (0.0%) | 43.2% (0.0%) | 57.2% (49.7%) | 56.3% |

![strategy arrows](arrows-c7.png)

![strategy shifts](shift-c7.png)

![sincerity](sincerity-c7.png)

![fidelity](fidelity-c7.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c7.png)

Scatter and chain plots leave out Random pair runoff, Anti-plurality, TRASH, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
