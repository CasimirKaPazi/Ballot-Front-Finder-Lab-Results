# Ballot Front Finder Lab — analysis

Input: base, width/01-width=1.4, width/02-width=2.8, utility/01-utility=linear, utility/02-utility=quadratic (simulator version 1.0.0; analysis version 1.0.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

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

![sweep over width](chain-width.png)

![sweep over utility](chain-utility.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis). A condition near or above that row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 0.99 | 1.00 | 0.97 | 0.99 | 0.99 |
| c2: width 1.4 | 0.87 | 1.00 | 0.84 | 0.98 | 0.98 |
| c3: width 2.8 | 0.58 | 0.99 | 0.58 | 0.97 | 0.96 |
| c4: utility linear | 0.42 | 0.98 | 0.41 | 0.95 | 0.99 |
| c5: utility quadratic | 0.45 | 0.91 | 0.48 | 0.85 | 0.98 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 97.6% | 68.6% | 16.2% | 8.0 |
| c2: width 1.4 | 97.7% | 77.3% | 6.7% | 2.8 |
| c3: width 2.8 | 97.7% | 83.0% | 3.9% | 1.7 |
| c4: utility linear | 97.7% | 87.3% | 2.6% | — |
| c5: utility quadratic | 97.7% | 83.1% | 4.1% | — |

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

## c2: width 1.4

1000 elections (seeds 1), 977 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 1.4; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 97.7%; it also has the highest total utility in 77.3% of those; electing it loses 6.7% of VSE; favourite ÷ least liked 2.8 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 92.9 to 93.1; with the best VSE (100) WME 87.2 (nothing lower is possible there either); the most balanced point VSE 97.0 / WME 98.3. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 95.1 (at WME 94.9); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -65.1 / WME -53.2.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, ESR, Soft Minimax-40, ARA, MARS-T, BTR-score, Soft Minimax-D, Soft Minimax, Kemeny-Young, Score runoff.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): MARS-T, Soft Minimax-D, Soft Minimax, Kemeny-Young, Schulze, Minimax, Ranked pairs.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 95.7 [94.6–96.7] | 91.7 [90.2–93.1] | 99.9 [99.9–99.9] | 90.0 | 94.0 | 96.0 [95.0–96.9] | 91.6 [90.0–93.0] | 82.6 [80.1–85.0] | 82.6 [80.1–85.0] | 1.3 [0.6–2.0] | 1.4 [0.7–2.1] | 0.1% | 1.2% | 0.0% | ESR (WME\*), ARA (WME\*) |
| ESR | rated | 95.2 [94.1–96.1] | 95.0 [93.7–96.0] | 99.9 [99.9–99.9] | 89.8 | 94.0 | 94.8 [93.7–95.8] | 95.4 [94.4–96.4] | 88.8 [86.8–90.6] | 87.8 [85.8–89.8] | 1.8 [1.0–2.7] | 1.5 [0.8–2.3] | 0.1% | 0.8% | 0.0% | MARS-T (WME\*), ARA (WME\*), Soft Minimax-40 (WME\*) |
| Soft Minimax-40 | hybrid | 94.7 [93.6–95.8] | 95.9 [94.9–96.8] | 99.9 [99.9–100.0] | 90.0 | 94.0 | 95.0 [94.0–96.1] | 96.1 [95.1–96.9] | 88.5 [86.4–90.5] | 89.2 [87.1–91.0] | 1.4 [0.7–2.1] | 1.6 [0.8–2.4] | 0.1% | 0.8% | 0.0% | BTR-score (WME\*), MARS (WME\*), MARS-T (WME\*) |
| ARA | hybrid | 94.7 [93.7–95.7] | 96.3 [95.4–97.1] | 99.9 [99.9–100.0] | 89.9 | 94.0 | 94.7 [93.5–95.7] | 96.6 [95.8–97.3] | 89.8 [87.8–91.5] | 89.6 [87.6–91.4] | 1.1 [0.5–1.8] | 1.4 [0.7–2.1] | 0.1% | 0.7% | 0.0% | Smith//Score (WME\*), Soft Minimax-D (WME\*) |
| DMS | rated | 94.6 [93.5–95.7] | 92.2 [90.7–93.6] | 100.0 [99.9–100.0] | 89.5 | 94.0 | 95.4 [94.3–96.3] | 91.8 [90.2–93.2] | 83.8 [81.3–86.0] | 84.0 [81.7–86.5] | 1.5 [0.8–2.3] | 1.3 [0.7–2.0] | 0.0% | 0.6% | 0.0% | Smith//Score (WME\*), ESR (WME\*), STAR (WME\*) +7 more |
| MARS-T | hybrid | 94.5 [93.3–95.6] | 96.8 [95.9–97.6] | 99.9 [99.9–100.0] | 89.8 | 94.0 | 94.6 [93.4–95.6] | 97.1 [96.3–97.8] | 90.4 [88.5–92.2] | 90.7 [88.8–92.5] | 1.9 [1.1–2.7] | 1.5 [0.8–2.3] | 0.0% | 0.8% | 0.0% | — |
| Score | rated | 94.5 [93.5–95.6] | 93.0 [91.6–94.3] | 100.0 [100.0–100.0] | 89.3 | 94.0 | 95.2 [94.1–96.1] | 94.1 [92.9–95.1] | 86.3 [84.1–88.3] | 84.3 [82.0–86.5] | 1.7 [0.9–2.5] | 1.6 [0.8–2.4] | 0.0% | 0.6% | 0.0% | Smith//Score (WME\*), ESR (WME\*), Geometric median (VSE\*) +8 more |
| BTR-score | hybrid | 94.2 [93.0–95.3] | 97.1 [96.2–97.8] | 99.9 [99.9–100.0] | 90.2 | 94.0 | 94.2 [93.1–95.4] | 97.6 [96.9–98.2] | 91.9 [90.2–93.5] | 91.4 [89.6–92.9] | 1.5 [0.8–2.2] | 1.4 [0.7–2.1] | 0.1% | 0.8% | 0.0% | Soft Minimax (WME\*) |
| Soft Minimax-D | hybrid | 94.2 [92.9–95.3] | 97.4 [96.7–98.0] | 99.9 [99.9–100.0] | 90.1 | 94.0 | 94.3 [93.1–95.4] | 97.5 [96.9–98.1] | 91.4 [89.5–93.0] | 91.2 [89.4–92.9] | 1.7 [0.9–2.5] | 2.2 [1.4–3.2] | 0.1% | 0.7% | 0.0% | — |
| MARS | hybrid | 94.1 [92.9–95.3] | 96.9 [96.2–97.7] | 100.0 [99.9–100.0] | 89.9 | 94.0 | 94.4 [93.4–95.6] | 97.2 [96.5–97.8] | 90.6 [88.7–92.3] | 90.2 [88.3–92.0] | 1.9 [1.1–2.7] | 1.7 [1.0–2.5] | 0.1% | 0.6% | 0.0% | Soft Minimax (WME\*) |
| MAV | median-based | 94.1 [92.9–95.2] | 90.4 [88.8–91.9] | 99.9 [99.9–100.0] | 89.7 | 94.0 | 93.6 [92.4–94.8] | 87.2 [85.2–89.0] | 77.4 [74.7–79.8] | 81.0 [78.3–83.5] | 1.6 [0.9–2.4] | 1.5 [0.8–2.3] | 0.0% | 0.8% | 0.0% | ESR (VSE\* WME\*), Geometric median (VSE\* WME\*), Smith//Score (WME\*) +12 more |
| Smith//Score | condorcet-based | 94.1 [92.9–95.3] | 97.2 [96.4–97.9] | 99.9 [99.9–100.0] | 90.2 | 94.0 | 94.4 [93.2–95.5] | 97.5 [96.8–98.1] | 91.7 [89.9–93.3] | 91.2 [89.4–92.9] | 1.7 [0.9–2.5] | 1.6 [0.9–2.4] | 0.1% | 0.8% | 0.0% | Soft Minimax (WME\*) |
| STAR | hybrid | 94.0 [92.8–95.2] | 97.1 [96.2–97.8] | 99.9 [99.9–99.9] | 90.0 | 94.0 | 94.4 [93.3–95.5] | 97.2 [96.5–97.9] | 90.9 [89.0–92.7] | 90.9 [89.0–92.6] | 1.6 [0.9–2.3] | 1.8 [1.0–2.6] | 0.1% | 0.9% | 0.0% | Soft Minimax (WME\*) |
| Soft Minimax | hybrid | 93.8 [92.6–95.0] | 97.7 [97.0–98.3] | 100.0 [99.9–100.0] | 90.3 | 94.0 | 94.1 [93.0–95.3] | 97.8 [97.1–98.3] | 92.2 [90.5–93.8] | 92.1 [90.4–93.8] | 1.5 [0.8–2.2] | 1.6 [0.9–2.3] | 0.1% | 0.6% | 0.0% | — |
| 321 | hybrid | 93.6 [92.3–94.8] | 91.6 [89.9–93.0] | 99.9 [99.9–99.9] | 82.1 | 85.2 | 93.5 [92.2–94.6] | 90.2 [88.6–91.8] | 81.4 [79.1–83.8] | 83.0 [80.6–85.2] | 1.7 [1.0–2.5] | 1.7 [1.0–2.6] | 0.2% | 1.4% | 0.0% | ESR (VSE\* WME\*), ARA (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +11 more |
| MJ | median-based | 93.6 [92.3–94.7] | 91.3 [89.7–92.8] | 99.9 [99.9–100.0] | 89.7 | 94.0 | 93.7 [92.6–94.8] | 89.5 [87.8–91.3] | 80.5 [77.9–83.0] | 82.6 [80.2–84.9] | 1.5 [0.8–2.3] | 1.8 [1.0–2.6] | 0.0% | 0.9% | 0.0% | ESR (VSE\* WME\*), ARA (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +12 more |
| Borda | positional | 93.4 [92.1–94.6] | 95.1 [94.0–96.1] | 97.7 [97.7–97.8] | 78.2 | 82.3 | 90.5 [88.5–92.1] | 95.3 [94.2–96.2] | 86.6 [84.4–88.7] | 86.6 [84.4–88.7] | 1.5 [0.8–2.2] | 1.4 [0.7–2.2] | 6.5% | 11.5% | 0.8% | ARA (VSE\* WME\* sincerity), Smith//Score (WME\* sincerity), ESR (VSE\* sincerity) +16 more |
| Approval cond. runoff | two-round | 93.3 [91.9–94.6] | 94.7 [93.5–95.8] | 99.8 [99.8–99.8] | 65.4 | 66.8 | 93.5 [92.2–94.6] | 93.0 [91.4–94.3] | 88.2 [85.9–90.3] | 89.6 [87.7–91.5] | 1.6 [0.9–2.4] | 1.6 [0.8–2.5] | 0.2% | 1.8% | 0.0% | MARS-T (VSE\* WME\*), ARA (VSE\* WME\*), Smith//Score (WME\*) +9 more |
| Approval runoff | two-round | 93.2 [91.9–94.5] | 96.0 [94.9–97.1] | 99.7 [99.6–99.7] | 65.2 | 66.8 | 93.2 [92.0–94.5] | 95.1 [93.8–96.2] | 92.8 [91.1–94.3] | 93.7 [92.2–95.2] | 1.7 [0.9–2.5] | 1.6 [0.9–2.4] | 0.3% | 2.4% | 0.0% | Soft Minimax-D (VSE\* WME\*), ESR (VSE\*), Score runoff (WME\*) +5 more |
| Kemeny-Young | condorcet | 93.0 [91.7–94.2] | 99.7 [99.4–99.8] | 98.0 [97.9–98.1] | 79.1 | 82.3 | 92.8 [91.5–94.2] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 98.3 [97.4–99.0] | 1.8 [1.0–2.7] | 1.6 [0.8–2.4] | 5.3% | 10.9% | 0.8% | — |
| Score runoff | two-round | 92.9 [91.6–94.1] | 98.4 [97.7–99.0] | 99.9 [99.9–99.9] | 89.8 | 94.0 | 92.9 [91.7–94.1] | 98.9 [98.5–99.3] | 98.0 [97.1–98.8] | 97.6 [96.6–98.6] | 1.9 [1.1–2.7] | 1.9 [1.1–2.8] | 0.2% | 1.1% | 0.0% | Soft Minimax (VSE\*) |
| Schulze | condorcet | 92.8 [91.6–94.1] | 99.6 [99.3–99.8] | 98.0 [97.9–98.0] | 79.0 | 82.3 | 93.0 [91.8–94.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.5 [97.7–99.2] | 1.6 [0.8–2.5] | 1.8 [1.0–2.7] | 5.4% | 11.0% | 0.8% | — |
| Minimax | minimax-family | 92.8 [91.5–94.1] | 99.5 [99.2–99.7] | 98.0 [97.9–98.0] | 79.1 | 82.3 | 93.0 [91.8–94.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.4 [97.5–99.1] | 1.6 [0.8–2.5] | 1.8 [1.0–2.7] | 5.4% | 11.0% | 0.8% | — |
| Ranked pairs | condorcet | 92.8 [91.5–94.0] | 99.6 [99.4–99.8] | 98.0 [97.9–98.0] | 79.1 | 82.3 | 93.0 [91.8–94.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.9 [96.9–98.7] | 1.7 [0.9–2.6] | 1.8 [1.0–2.7] | 5.3% | 10.8% | 0.8% | — |
| Approval | approval-based | 92.5 [90.9–93.9] | 88.8 [86.9–90.5] | 99.9 [99.9–99.9] | 65.7 | 66.8 | 90.7 [88.7–92.5] | 86.1 [84.1–88.2] | 76.9 [74.2–79.6] | 79.3 [76.7–81.8] | 3.9 [2.7–5.2] | 2.8 [1.8–3.9] | 0.0% | 1.2% | 0.0% | Smith//Score (VSE\* WME\*), Score (VSE\* WME\*), MAV (VSE\* WME\*) +16 more |
| BTR-IRV | condorcet-based | 92.4 [91.0–93.7] | 98.7 [98.3–99.2] | 97.9 [97.8–98.0] | 78.9 | 82.3 | 92.7 [91.4–93.9] | 99.8 [99.7–99.9] | 100.0 [100.0–100.0] | 97.1 [96.1–98.1] | 1.8 [1.0–2.7] | 2.1 [1.2–3.1] | 5.5% | 11.3% | 0.9% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +3 more |
| MMWV | minimax-family | 92.3 [91.0–93.6] | 99.2 [98.8–99.6] | 98.3 [98.2–98.4] | 79.2 | 82.3 | 93.0 [91.8–94.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.0 [97.1–98.8] | 1.6 [0.8–2.5] | 2.0 [1.1–2.9] | 0.1% | 15.7% | 0.0% | Kemeny-Young (VSE\* WME\*), Schulze (VSE\*), Minimax (VSE\*) |
| MMPO | minimax-family | 92.0 [90.6–93.4] | 99.5 [99.2–99.8] | 98.3 [98.2–98.4] | 79.2 | 82.3 | 93.0 [91.8–94.2] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.3 [97.5–99.1] | 1.6 [0.8–2.5] | 2.2 [1.3–3.1] | 0.1% | 15.6% | 0.0% | Schulze (VSE\*), Kemeny-Young (VSE\*), Minimax (VSE\*) |
| Copeland | condorcet | 92.0 [90.6–93.3] | 98.8 [98.2–99.2] | 98.0 [97.9–98.0] | 79.0 | 82.3 | 92.5 [91.1–93.7] | 99.6 [99.4–99.8] | 100.0 [100.0–100.0] | 97.4 [96.3–98.3] | 1.8 [1.0–2.7] | 1.9 [1.1–2.8] | 5.4% | 10.9% | 0.8% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +3 more |
| IRV | plurality-based | 91.5 [89.9–92.8] | 91.8 [90.3–93.3] | 97.7 [97.6–97.8] | 78.7 | 82.3 | 90.7 [89.0–92.2] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 88.1 [86.1–90.0] | 3.2 [2.2–4.4] | 2.7 [1.7–3.7] | 5.5% | 11.6% | 1.4% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +25 more |
| ER-IRV | plurality-based | 91.1 [89.4–92.5] | 91.7 [90.0–93.4] | 98.1 [98.1–98.2] | 78.8 | 82.3 | 90.7 [89.0–92.2] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 87.9 [85.9–89.9] | 3.2 [2.2–4.4] | 3.2 [2.1–4.4] | 1.3% | 15.7% | 0.3% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +25 more |
| IRV-3 | plurality-based | 90.6 [89.0–92.2] | 90.3 [88.5–91.9] | 98.9 [98.9–99.0] | 74.0 | 76.0 | 89.8 [87.9–91.4] | 87.1 [85.0–89.1] | 82.2 [79.7–84.3] | 84.8 [82.4–86.9] | 3.8 [2.7–5.0] | 2.9 [1.9–4.0] | 5.5% | 6.0% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity), DMS (VSE\* WME\* sincerity) +25 more |
| TTR | two-round | 89.2 [87.3–91.1] | 89.0 [87.1–90.9] | 97.0 [96.8–97.3] | 58.4 | 61.3 | 88.8 [87.0–90.7] | 84.3 [82.1–86.5] | 80.8 [78.3–83.1] | 85.0 [82.8–87.2] | 3.9 [2.7–5.1] | 3.8 [2.6–5.0] | 6.5% | 0.0% | 3.3% | MMWV (VSE\* WME\* sincerity), MMPO (VSE\* WME\* sincerity), Smith//Score (VSE\* WME\* sincerity) +29 more |
| Supplementary vote | plurality-based | 88.7 [86.8–90.5] | 85.2 [83.2–87.3] | 97.8 [97.7–97.8] | 78.8 | 82.3 | 82.9 [80.2–85.7] | 79.4 [76.9–81.9] | 71.9 [69.0–74.7] | 76.7 [74.0–79.3] | 7.3 [5.7–9.0] | 5.1 [3.9–6.5] | 6.3% | 13.2% | 1.2% | Smith//Score (VSE\* WME\* sincerity), IRV-3 (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity) +30 more |
| Plurality | plurality-based | 83.1 [80.4–85.7] | 78.1 [75.5–80.5] | 98.5 [98.5–98.6] | 59.4 | 61.3 | 70.4 [66.4–74.1] | 69.6 [66.6–72.7] | 63.7 [60.8–66.9] | 69.7 [66.7–72.4] | 12.9 [10.9–14.9] | 8.1 [6.5–9.8] | 7.8% | 0.0% | 1.4% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +30 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 48.3% |
| Supplementary vote | 45.2% |
| TTR | 41.6% |
| Approval | 41.2% |
| IRV-3 | 41.1% |
| Geometric median | 40.7% |
| IRV | 40.2% |
| ER-IRV | 40.0% |
| ESR | 40.0% |
| DMS | 39.8% |
| Approval runoff | 39.4% |
| Score | 39.3% |
| Approval cond. runoff | 39.3% |
| MAV | 39.1% |
| Soft Minimax-40 | 39.1% |
| BTR-IRV | 39.0% |
| MARS-T | 39.0% |
| BTR-score | 38.9% |
| MARS | 38.9% |
| Smith//Score | 38.9% |
| STAR | 38.7% |
| ARA | 38.6% |
| 321 | 38.6% |
| Score runoff | 38.5% |
| Soft Minimax-D | 38.4% |
| MJ | 38.2% |
| Soft Minimax | 38.2% |
| Kemeny-Young | 38.0% |
| Ranked pairs | 38.0% |
| Schulze | 38.0% |
| Minimax | 38.0% |
| MMWV | 37.6% |
| MMPO | 37.4% |
| Copeland | 37.2% |
| Borda | 36.8% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 977 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 23 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 89.7% (90.8%) | 7.0% (6.3%) | 3.4% (2.9%) | 52.2% (47.8%) | 89.9% (88.3%) | 82.6% |
| ESR | 90.2% (90.8%) | 7.7% (6.3%) | 2.1% (2.9%) | 60.9% (47.8%) | 95.7% (95.3%) | 87.8% |
| Soft Minimax-40 | 91.2% (90.8%) | 6.1% (6.3%) | 2.7% (2.9%) | 43.5% (47.8%) | 95.9% (95.5%) | 89.2% |
| ARA | 90.7% (90.8%) | 6.6% (6.3%) | 2.8% (2.9%) | 60.9% (47.8%) | 95.0% (94.3%) | 89.6% |
| DMS | 90.8% (90.8%) | 7.0% (6.3%) | 2.3% (2.9%) | 60.9% (47.8%) | 88.5% (87.8%) | 84.0% |
| MARS-T | 91.0% (90.8%) | 6.7% (6.3%) | 2.4% (2.9%) | 65.2% (47.8%) | 96.0% (97.0%) | 90.7% |
| Score | 90.7% (90.8%) | 7.1% (6.3%) | 2.3% (2.9%) | 65.2% (47.8%) | 89.2% (90.3%) | 84.3% |
| BTR-score | 90.6% (90.8%) | 6.9% (6.3%) | 2.6% (2.9%) | 60.9% (47.8%) | 100.0% (100.0%) | 91.4% |
| Soft Minimax-D | 90.6% (90.8%) | 6.1% (6.3%) | 3.3% (2.9%) | 56.5% (47.8%) | 99.8% (99.4%) | 91.2% |
| MARS | 90.4% (90.8%) | 7.0% (6.3%) | 2.7% (2.9%) | 65.2% (47.8%) | 97.0% (97.6%) | 90.2% |
| MAV | 90.2% (90.8%) | 7.2% (6.3%) | 2.7% (2.9%) | 65.2% (47.8%) | 86.9% (82.7%) | 81.0% |
| Smith//Score | 90.4% (90.8%) | 7.2% (6.3%) | 2.5% (2.9%) | 56.5% (47.8%) | 100.0% (100.0%) | 91.2% |
| STAR | 91.0% (90.8%) | 6.7% (6.3%) | 2.4% (2.9%) | 52.2% (47.8%) | 99.2% (99.2%) | 90.9% |
| Soft Minimax | 91.0% (90.8%) | 6.6% (6.3%) | 2.5% (2.9%) | 69.6% (47.8%) | 100.0% (100.0%) | 92.1% |
| 321 | 82.7% (82.5%) | 13.8% (14.0%) | 3.5% (3.5%) | 87.0% (82.6%) | 97.0% (95.3%) | 83.0% |
| MJ | 90.2% (90.8%) | 7.5% (6.3%) | 2.4% (2.9%) | 73.9% (47.8%) | 88.2% (85.2%) | 82.6% |
| Borda | 95.7% (100.0%) | 2.5% (0.0%) | 1.8% (0.0%) | 52.2% (0.0%) | 87.9% (86.6%) | 86.6% |
| Kemeny-Young | 97.9% (100.0%) | 1.2% (0.0%) | 0.9% (0.0%) | 17.4% (0.0%) | 100.0% (100.0%) | 98.3% |
| Schulze | 98.1% (100.0%) | 1.0% (0.0%) | 0.9% (0.0%) | 30.4% (0.0%) | 100.0% (100.0%) | 98.5% |
| Minimax | 98.0% (100.0%) | 1.1% (0.0%) | 0.9% (0.0%) | 30.4% (0.0%) | 100.0% (100.0%) | 98.4% |
| Ranked pairs | 97.7% (100.0%) | 1.4% (0.0%) | 0.8% (0.0%) | 21.7% (0.0%) | 100.0% (100.0%) | 97.9% |
| BTR-IRV | 96.6% (100.0%) | 1.6% (0.0%) | 1.7% (0.0%) | 26.1% (0.0%) | 100.0% (100.0%) | 97.1% |
| MMWV | 96.6% (100.0%) | 1.0% (0.0%) | 2.4% (0.0%) | 26.1% (0.0%) | 100.0% (100.0%) | 98.0% |
| MMPO | 97.1% (100.0%) | 0.9% (0.0%) | 1.9% (0.0%) | 21.7% (0.0%) | 100.0% (100.0%) | 98.3% |
| Copeland | 96.9% (100.0%) | 1.6% (0.0%) | 1.4% (0.0%) | 34.8% (0.0%) | 100.0% (100.0%) | 97.4% |
| IRV | 96.0% (100.0%) | 3.1% (0.0%) | 0.9% (0.0%) | 39.1% (0.0%) | 88.5% (84.8%) | 88.1% |
| ER-IRV | 96.0% (100.0%) | 2.8% (0.0%) | 1.2% (0.0%) | 39.1% (0.0%) | 88.5% (84.8%) | 87.9% |
| IRV-3 | 87.6% (89.9%) | 6.7% (5.6%) | 5.7% (4.5%) | 52.2% (34.8%) | 93.5% (90.5%) | 84.8% |
| Supplementary vote | 95.3% (100.0%) | 3.5% (0.0%) | 1.2% (0.0%) | 56.5% (0.0%) | 77.4% (71.9%) | 76.7% |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

## c3: width 2.8

1000 elections (seeds 1), 977 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 2.8; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 97.7%; it also has the highest total utility in 83.0% of those; electing it loses 3.9% of VSE; favourite ÷ least liked 1.7 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 95.8 to 95.9; with the best VSE (100) WME 92.3 (nothing lower is possible there either); the most balanced point VSE 98.2 / WME 99.0. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 97.1 (at WME 97.6); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -68.2 / WME -53.2.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, Soft Minimax-D, Soft Minimax, Minimax, Score runoff.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Soft Minimax-D, BTR-score, Minimax, Schulze, Kemeny-Young, Score runoff.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 97.0 [96.1–97.7] | 93.3 [92.0–94.5] | 99.9 [99.9–99.9] | 90.0 | 94.0 | 97.2 [96.4–97.8] | 92.6 [91.2–93.9] | 83.3 [81.0–85.5] | 84.6 [82.3–86.8] | 0.6 [0.2–1.1] | 0.9 [0.3–1.5] | 0.1% | 1.1% | 0.0% | Smith//Score (WME\*), ESR (WME\*), STAR (WME\*) +7 more |
| Soft Minimax-D | hybrid | 96.9 [96.1–97.6] | 97.6 [96.8–98.2] | 99.9 [99.9–100.0] | 90.1 | 94.0 | 96.9 [96.1–97.6] | 97.8 [97.1–98.3] | 91.4 [89.5–93.0] | 91.8 [90.1–93.4] | 0.5 [0.1–1.0] | 0.5 [0.1–1.0] | 0.1% | 0.6% | 0.0% | — |
| MARS | hybrid | 96.9 [96.1–97.6] | 96.8 [95.9–97.6] | 100.0 [99.9–100.0] | 89.9 | 94.0 | 97.1 [96.3–97.8] | 97.6 [96.9–98.2] | 90.9 [89.0–92.6] | 90.0 [88.0–91.7] | 0.5 [0.1–0.9] | 0.8 [0.3–1.4] | 0.1% | 0.6% | 0.0% | Soft Minimax-D (WME\*) |
| Soft Minimax-40 | hybrid | 96.9 [96.0–97.7] | 96.3 [95.3–97.0] | 99.9 [99.9–100.0] | 90.0 | 94.0 | 97.1 [96.3–97.7] | 96.5 [95.7–97.2] | 89.2 [87.1–91.0] | 89.0 [86.9–90.9] | 0.7 [0.2–1.2] | 0.7 [0.3–1.3] | 0.1% | 0.8% | 0.0% | Smith//Score (WME\*), STAR (WME\*), BTR-score (WME\*) +2 more |
| BTR-score | hybrid | 96.8 [96.0–97.5] | 97.5 [96.7–98.1] | 100.0 [99.9–100.0] | 90.2 | 94.0 | 96.8 [95.9–97.5] | 97.9 [97.3–98.4] | 91.9 [90.1–93.3] | 91.7 [89.9–93.3] | 0.6 [0.2–1.1] | 0.6 [0.2–1.1] | 0.1% | 0.5% | 0.0% | — |
| ESR | rated | 96.8 [96.0–97.5] | 95.8 [94.8–96.7] | 99.9 [99.9–100.0] | 89.9 | 94.0 | 96.6 [95.8–97.3] | 96.2 [95.2–97.0] | 88.8 [86.9–90.6] | 88.3 [86.3–90.2] | 1.0 [0.4–1.7] | 1.1 [0.5–1.8] | 0.1% | 0.7% | 0.0% | Smith//Score (WME\*), STAR (WME\*), BTR-score (WME\*) +3 more |
| MARS-T | hybrid | 96.7 [95.9–97.5] | 96.6 [95.7–97.4] | 100.0 [99.9–100.0] | 89.8 | 94.0 | 97.0 [96.2–97.7] | 97.6 [96.8–98.1] | 91.0 [89.1–92.7] | 89.5 [87.5–91.4] | 0.7 [0.2–1.2] | 0.8 [0.3–1.4] | 0.0% | 0.5% | 0.0% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax (WME\*) +1 more |
| STAR | hybrid | 96.6 [95.8–97.3] | 97.2 [96.4–97.9] | 99.9 [99.9–100.0] | 90.0 | 94.0 | 96.8 [96.0–97.5] | 97.5 [96.8–98.1] | 91.2 [89.5–92.8] | 91.0 [89.2–92.7] | 0.5 [0.1–1.0] | 0.4 [0.1–0.8] | 0.1% | 0.8% | 0.0% | Soft Minimax (WME\*) |
| DMS | rated | 96.5 [95.7–97.2] | 93.1 [91.8–94.5] | 100.0 [99.9–100.0] | 89.5 | 94.0 | 96.8 [96.1–97.6] | 92.2 [90.8–93.5] | 84.0 [81.7–86.1] | 85.2 [82.8–87.3] | 0.9 [0.4–1.6] | 0.4 [0.1–0.9] | 0.0% | 0.6% | 0.0% | Smith//Score (WME\*), ESR (WME\*), STAR (WME\*) +7 more |
| Score | rated | 96.5 [95.6–97.2] | 93.7 [92.4–94.8] | 100.0 [100.0–100.0] | 89.3 | 94.0 | 96.7 [95.8–97.4] | 94.8 [93.7–95.7] | 86.5 [84.4–88.4] | 85.4 [83.2–87.5] | 1.1 [0.5–1.7] | 1.0 [0.4–1.6] | 0.0% | 0.6% | 0.0% | Smith//Score (WME\*), ESR (WME\*), STAR (WME\*) +7 more |
| Soft Minimax | hybrid | 96.4 [95.6–97.1] | 97.9 [97.2–98.4] | 100.0 [99.9–100.0] | 90.3 | 94.0 | 96.5 [95.7–97.3] | 98.1 [97.6–98.6] | 92.4 [90.6–93.8] | 91.5 [89.7–93.0] | 0.6 [0.2–1.1] | 0.3 [0.0–0.7] | 0.1% | 0.5% | 0.0% | BTR-score (VSE\*), Soft Minimax-D (VSE\*) |
| Smith//Score | condorcet-based | 96.3 [95.4–97.1] | 97.5 [96.7–98.1] | 99.9 [99.9–100.0] | 90.1 | 94.0 | 96.9 [96.1–97.6] | 97.9 [97.3–98.4] | 91.9 [90.2–93.3] | 91.7 [89.9–93.4] | 0.6 [0.2–1.1] | 1.1 [0.5–1.7] | 0.1% | 0.7% | 0.0% | BTR-score (VSE\*), Soft Minimax-D (VSE\*) |
| ARA | hybrid | 96.3 [95.4–97.0] | 96.6 [95.9–97.3] | 100.0 [99.9–100.0] | 89.9 | 94.0 | 96.1 [95.3–96.9] | 96.8 [96.1–97.4] | 89.4 [87.5–91.2] | 89.6 [87.7–91.4] | 0.9 [0.4–1.5] | 0.6 [0.2–1.1] | 0.0% | 0.6% | 0.0% | Soft Minimax-D (VSE\* WME\*), Smith//Score (WME\*), BTR-score (WME\*) +3 more |
| Minimax | minimax-family | 95.9 [94.9–96.7] | 99.8 [99.6–99.9] | 98.0 [98.0–98.1] | 79.6 | 82.8 | 95.8 [94.9–96.7] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 99.1 [98.4–99.6] | 0.8 [0.3–1.4] | 0.9 [0.4–1.5] | 5.7% | 9.9% | 0.8% | — |
| Schulze | condorcet | 95.9 [94.9–96.7] | 99.7 [99.6–99.9] | 98.0 [98.0–98.1] | 79.6 | 82.8 | 95.8 [94.9–96.7] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 99.0 [98.4–99.5] | 0.8 [0.3–1.4] | 0.9 [0.4–1.5] | 5.7% | 9.9% | 0.7% | — |
| Kemeny-Young | condorcet | 95.7 [94.8–96.6] | 99.7 [99.5–99.8] | 98.1 [98.0–98.1] | 79.6 | 82.8 | 95.7 [94.7–96.6] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 98.9 [98.1–99.5] | 0.9 [0.4–1.5] | 0.8 [0.3–1.4] | 5.6% | 9.9% | 0.7% | — |
| MJ | median-based | 95.7 [94.7–96.6] | 91.8 [90.2–93.4] | 99.9 [99.9–100.0] | 89.6 | 94.0 | 95.4 [94.4–96.4] | 89.7 [87.9–91.3] | 81.1 [78.6–83.5] | 83.5 [81.1–85.7] | 1.2 [0.6–1.9] | 1.1 [0.5–1.7] | 0.0% | 0.8% | 0.0% | ESR (VSE\* WME\*), Geometric median (VSE\* WME\*), STAR (VSE\* WME\*) +12 more |
| Ranked pairs | condorcet | 95.6 [94.7–96.5] | 99.7 [99.5–99.9] | 98.1 [98.0–98.1] | 79.7 | 82.8 | 95.9 [94.9–96.7] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.8 [98.1–99.4] | 0.8 [0.3–1.4] | 1.1 [0.5–1.8] | 5.6% | 9.8% | 0.7% | Minimax (VSE\*) |
| 321 | hybrid | 95.6 [94.6–96.5] | 91.3 [89.5–92.8] | 99.9 [99.9–99.9] | 81.9 | 85.0 | 95.1 [94.1–96.0] | 89.8 [88.0–91.6] | 80.9 [78.4–83.1] | 83.3 [80.8–85.5] | 0.9 [0.4–1.5] | 0.6 [0.2–1.1] | 0.2% | 1.3% | 0.0% | Score (VSE\* WME\*), DMS (VSE\* WME\*), ESR (VSE\* WME\*) +13 more |
| BTR-IRV | condorcet-based | 95.5 [94.5–96.4] | 98.9 [98.5–99.3] | 97.9 [97.9–98.0] | 79.4 | 82.8 | 95.6 [94.7–96.4] | 99.8 [99.7–99.9] | 100.0 [100.0–100.0] | 97.5 [96.5–98.4] | 1.0 [0.4–1.7] | 1.0 [0.4–1.7] | 5.8% | 10.3% | 0.8% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +4 more |
| MMWV | minimax-family | 95.5 [94.6–96.3] | 99.5 [99.2–99.8] | 98.4 [98.3–98.4] | 79.8 | 82.8 | 95.8 [94.9–96.7] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.5 [97.7–99.2] | 0.8 [0.3–1.4] | 0.8 [0.3–1.4] | 0.1% | 14.5% | 0.0% | Minimax (VSE\* WME\*), Schulze (VSE\*) |
| Score runoff | two-round | 95.5 [94.5–96.3] | 99.0 [98.5–99.4] | 99.9 [99.9–99.9] | 89.8 | 94.0 | 95.6 [94.7–96.4] | 99.1 [98.8–99.5] | 98.3 [97.4–99.0] | 97.7 [96.7–98.6] | 0.9 [0.4–1.5] | 0.5 [0.1–1.0] | 0.2% | 1.0% | 0.0% | — |
| MAV | median-based | 95.5 [94.5–96.4] | 90.9 [89.4–92.4] | 100.0 [99.9–100.0] | 89.8 | 94.0 | 94.4 [93.2–95.5] | 87.0 [85.0–88.9] | 77.5 [74.8–80.1] | 81.6 [79.1–83.8] | 1.6 [0.9–2.5] | 1.6 [0.9–2.5] | 0.0% | 0.7% | 0.0% | Score (VSE\* WME\*), DMS (VSE\* WME\*), ESR (VSE\* WME\*) +13 more |
| MMPO | minimax-family | 95.4 [94.4–96.3] | 99.6 [99.3–99.8] | 98.4 [98.3–98.4] | 79.8 | 82.8 | 95.8 [94.9–96.7] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.4 [97.6–99.1] | 0.8 [0.3–1.4] | 0.8 [0.3–1.4] | 0.1% | 14.6% | 0.0% | Schulze (VSE\*), Minimax (VSE\*) |
| Borda | positional | 95.0 [93.7–96.1] | 95.4 [94.4–96.3] | 97.8 [97.7–97.9] | 78.7 | 82.8 | 93.8 [92.3–95.0] | 95.3 [94.2–96.2] | 86.6 [84.4–88.7] | 87.3 [85.2–89.2] | 0.8 [0.3–1.4] | 0.8 [0.3–1.4] | 6.5% | 10.2% | 0.8% | Smith//Score (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity), BTR-score (VSE\* WME\* sincerity) +17 more |
| Approval runoff | two-round | 94.9 [93.8–96.0] | 96.4 [95.3–97.4] | 99.7 [99.7–99.8] | 63.3 | 64.8 | 95.3 [94.3–96.3] | 95.7 [94.6–96.7] | 93.8 [92.4–95.3] | 94.1 [92.7–95.5] | 1.1 [0.5–1.8] | 1.3 [0.6–2.0] | 0.2% | 2.0% | 0.0% | Soft Minimax (VSE\* WME\*), Smith//Score (VSE\*), ESR (VSE\*) +8 more |
| Copeland | condorcet | 94.8 [93.8–95.8] | 98.9 [98.5–99.3] | 98.0 [97.9–98.0] | 79.5 | 82.8 | 95.4 [94.4–96.2] | 99.6 [99.4–99.8] | 100.0 [100.0–100.0] | 98.0 [97.1–98.7] | 0.8 [0.3–1.4] | 1.1 [0.5–1.8] | 5.5% | 10.2% | 0.7% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +5 more |
| Approval cond. runoff | two-round | 94.5 [93.4–95.7] | 93.8 [92.4–95.2] | 99.8 [99.8–99.8] | 63.5 | 64.8 | 95.5 [94.5–96.3] | 93.3 [91.9–94.6] | 88.2 [86.0–90.1] | 89.0 [86.9–90.9] | 1.1 [0.5–1.8] | 1.3 [0.6–2.0] | 0.1% | 1.5% | 0.0% | Smith//Score (VSE\* WME\*), ESR (VSE\* WME\*), STAR (VSE\* WME\*) +12 more |
| Approval | approval-based | 94.5 [93.4–95.6] | 90.0 [88.3–91.7] | 99.9 [99.9–99.9] | 63.8 | 64.8 | 92.8 [91.3–94.3] | 87.8 [86.0–89.7] | 78.7 [76.0–81.3] | 81.3 [78.6–83.7] | 3.0 [2.0–4.2] | 1.8 [1.0–2.7] | 0.0% | 1.0% | 0.0% | Smith//Score (VSE\* WME\*), Score (VSE\* WME\*), DMS (VSE\* WME\*) +15 more |
| IRV | plurality-based | 93.2 [91.8–94.5] | 92.0 [90.3–93.4] | 97.8 [97.8–97.9] | 79.4 | 82.8 | 91.8 [90.3–93.1] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 88.9 [86.8–90.7] | 2.5 [1.5–3.5] | 2.0 [1.2–2.9] | 5.6% | 10.7% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +26 more |
| IRV-3 | plurality-based | 92.9 [91.5–94.2] | 91.0 [89.3–92.6] | 99.0 [98.9–99.0] | 72.2 | 74.1 | 90.8 [89.1–92.3] | 87.1 [85.0–89.1] | 82.2 [79.7–84.3] | 85.8 [83.7–87.8] | 3.0 [2.0–4.0] | 2.0 [1.3–2.9] | 6.0% | 5.5% | 1.3% | Score (VSE\* WME\* sincerity), Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*) +24 more |
| ER-IRV | plurality-based | 92.8 [91.4–94.1] | 91.4 [89.7–92.9] | 98.2 [98.2–98.3] | 79.4 | 82.8 | 91.8 [90.3–93.1] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 87.6 [85.5–89.5] | 2.5 [1.5–3.5] | 1.8 [1.0–2.7] | 1.4% | 14.7% | 0.2% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +26 more |
| TTR | two-round | 91.5 [89.6–93.1] | 89.9 [88.0–91.8] | 97.4 [97.2–97.7] | 56.1 | 58.5 | 89.7 [87.9–91.5] | 84.3 [82.1–86.5] | 80.8 [78.3–83.1] | 86.9 [84.8–88.9] | 3.5 [2.4–4.7] | 3.0 [2.0–4.1] | 6.7% | 0.0% | 2.8% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +28 more |
| Supplementary vote | plurality-based | 89.5 [87.8–91.3] | 85.5 [83.3–87.7] | 97.8 [97.7–97.9] | 79.3 | 82.8 | 83.3 [80.8–86.0] | 79.4 [76.9–81.9] | 71.9 [69.0–74.7] | 77.6 [75.0–80.2] | 6.9 [5.3–8.5] | 4.6 [3.4–5.9] | 6.4% | 12.1% | 1.1% | Smith//Score (VSE\* WME\* sincerity), IRV-3 (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity) +30 more |
| Plurality | plurality-based | 84.7 [82.2–87.0] | 77.8 [75.2–80.3] | 98.6 [98.5–98.7] | 56.8 | 58.5 | 70.8 [66.9–74.5] | 69.6 [66.6–72.7] | 63.7 [60.8–66.9] | 70.4 [67.5–73.1] | 12.8 [10.9–14.9] | 7.1 [5.5–8.7] | 7.8% | 0.0% | 1.4% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +30 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 46.3% |
| Supplementary vote | 43.4% |
| IRV-3 | 39.9% |
| TTR | 39.1% |
| Approval | 38.9% |
| IRV | 38.3% |
| ER-IRV | 38.0% |
| Geometric median | 37.5% |
| Approval runoff | 37.3% |
| BTR-IRV | 36.9% |
| MAV | 36.9% |
| Approval cond. runoff | 36.8% |
| Score | 36.8% |
| BTR-score | 36.7% |
| ESR | 36.6% |
| DMS | 36.6% |
| Smith//Score | 36.5% |
| MJ | 36.4% |
| MARS | 36.4% |
| Soft Minimax-40 | 36.4% |
| STAR | 36.4% |
| MARS-T | 36.3% |
| Soft Minimax-D | 36.3% |
| Kemeny-Young | 36.2% |
| Minimax | 36.1% |
| Schulze | 36.1% |
| Ranked pairs | 36.1% |
| ARA | 35.9% |
| Score runoff | 35.8% |
| 321 | 35.7% |
| Soft Minimax | 35.4% |
| Copeland | 35.3% |
| MMPO | 35.2% |
| MMWV | 35.1% |
| Borda | 34.8% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 977 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 23 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 89.4% (91.0%) | 6.8% (6.7%) | 3.9% (2.4%) | 69.6% (56.5%) | 90.2% (88.5%) | 84.6% |
| Soft Minimax-D | 90.2% (91.0%) | 6.4% (6.7%) | 3.4% (2.4%) | 56.5% (56.5%) | 99.2% (99.2%) | 91.8% |
| MARS | 89.6% (91.0%) | 6.8% (6.7%) | 3.7% (2.4%) | 65.2% (56.5%) | 96.8% (97.4%) | 90.0% |
| Soft Minimax-40 | 90.1% (91.0%) | 7.0% (6.7%) | 3.0% (2.4%) | 65.2% (56.5%) | 94.3% (93.6%) | 89.0% |
| BTR-score | 90.8% (91.0%) | 6.6% (6.7%) | 2.7% (2.4%) | 47.8% (56.5%) | 100.0% (100.0%) | 91.7% |
| ESR | 90.1% (91.0%) | 7.6% (6.7%) | 2.4% (2.4%) | 56.5% (56.5%) | 95.4% (96.1%) | 88.3% |
| MARS-T | 90.0% (91.0%) | 6.8% (6.7%) | 3.3% (2.4%) | 47.8% (56.5%) | 95.8% (96.9%) | 89.5% |
| STAR | 89.4% (91.0%) | 6.8% (6.7%) | 3.9% (2.4%) | 65.2% (56.5%) | 99.2% (99.5%) | 91.0% |
| DMS | 90.2% (91.0%) | 6.8% (6.7%) | 3.1% (2.4%) | 60.9% (56.5%) | 88.7% (87.4%) | 85.2% |
| Score | 89.7% (91.0%) | 6.8% (6.7%) | 3.6% (2.4%) | 69.6% (56.5%) | 89.3% (89.6%) | 85.4% |
| Soft Minimax | 90.0% (91.0%) | 7.3% (6.7%) | 2.8% (2.4%) | 52.2% (56.5%) | 100.0% (100.0%) | 91.5% |
| Smith//Score | 90.2% (91.0%) | 6.8% (6.7%) | 3.1% (2.4%) | 69.6% (56.5%) | 100.0% (100.0%) | 91.7% |
| ARA | 90.4% (91.0%) | 6.6% (6.7%) | 3.1% (2.4%) | 73.9% (56.5%) | 93.3% (92.2%) | 89.6% |
| Minimax | 98.6% (100.0%) | 0.5% (0.0%) | 0.9% (0.0%) | 34.8% (0.0%) | 100.0% (100.0%) | 99.1% |
| Schulze | 98.6% (100.0%) | 0.5% (0.0%) | 0.9% (0.0%) | 34.8% (0.0%) | 100.0% (100.0%) | 99.0% |
| Kemeny-Young | 98.5% (100.0%) | 0.6% (0.0%) | 0.9% (0.0%) | 30.4% (0.0%) | 100.0% (100.0%) | 98.9% |
| MJ | 89.0% (91.0%) | 7.3% (6.7%) | 3.7% (2.4%) | 60.9% (56.5%) | 87.7% (84.6%) | 83.5% |
| Ranked pairs | 98.5% (100.0%) | 0.6% (0.0%) | 0.9% (0.0%) | 47.8% (0.0%) | 100.0% (100.0%) | 98.8% |
| 321 | 83.2% (82.2%) | 13.7% (13.5%) | 3.1% (4.3%) | 91.3% (87.0%) | 95.9% (94.9%) | 83.3% |
| BTR-IRV | 97.2% (100.0%) | 1.1% (0.0%) | 1.6% (0.0%) | 17.4% (0.0%) | 100.0% (100.0%) | 97.5% |
| MMWV | 97.9% (100.0%) | 1.1% (0.0%) | 1.0% (0.0%) | 8.7% (0.0%) | 100.0% (100.0%) | 98.5% |
| MAV | 90.3% (91.0%) | 6.7% (6.7%) | 3.1% (2.4%) | 56.5% (56.5%) | 86.4% (82.4%) | 81.6% |
| MMPO | 97.3% (100.0%) | 1.1% (0.0%) | 1.5% (0.0%) | 8.7% (0.0%) | 99.8% (100.0%) | 98.4% |
| Borda | 97.5% (100.0%) | 1.3% (0.0%) | 1.1% (0.0%) | 52.2% (0.0%) | 87.4% (86.6%) | 87.3% |
| Copeland | 97.5% (100.0%) | 0.9% (0.0%) | 1.5% (0.0%) | 43.5% (0.0%) | 100.0% (100.0%) | 98.0% |
| IRV | 97.0% (100.0%) | 1.9% (0.0%) | 1.0% (0.0%) | 52.2% (0.0%) | 88.6% (84.8%) | 88.9% |
| IRV-3 | 88.5% (89.9%) | 6.9% (5.6%) | 4.6% (4.5%) | 52.2% (34.8%) | 94.0% (90.5%) | 85.8% |
| ER-IRV | 96.3% (100.0%) | 2.3% (0.0%) | 1.4% (0.0%) | 43.5% (0.0%) | 87.9% (84.8%) | 87.6% |
| Supplementary vote | 95.5% (100.0%) | 3.5% (0.0%) | 1.0% (0.0%) | 60.9% (0.0%) | 79.0% (71.9%) | 77.6% |

![strategy arrows](arrows-c3.png)

![strategy shifts](shift-c3.png)

![sincerity](sincerity-c3.png)

![fidelity](fidelity-c3.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c3.png)

## c4: utility linear

1000 elections (seeds 1), 977 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, linear utility; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 97.7%; it also has the highest total utility in 87.3% of those; electing it loses 2.6% of VSE; favourite ÷ least liked: — (utility not positive). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 97.1 to 97.2; with the best VSE (100) WME 95.5 (nothing lower is possible there either); the most balanced point VSE 99.0 / WME 99.2. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 98.1 (at WME 98.5); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -71.1 / WME -53.2.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Soft Minimax-40, MARS-T, MARS, Soft Minimax-D, BTR-score, MMWV, Ranked pairs, Score runoff.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Soft Minimax-D, BTR-score, MMWV, Ranked pairs, Minimax, Schulze, Score runoff.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Soft Minimax-40 | hybrid | 97.9 [97.2–98.4] | 95.8 [94.8–96.7] | 100.0 [100.0–100.0] | 90.1 | 94.0 | 98.3 [97.7–98.7] | 96.0 [95.0–96.8] | 88.2 [86.3–90.1] | 87.9 [85.9–89.8] | 0.3 [0.0–0.7] | 0.5 [0.1–1.0] | 0.0% | 0.6% | 0.0% | Smith//Score (WME\*), STAR (WME\*), BTR-score (WME\*) +4 more |
| MARS-T | hybrid | 97.9 [97.2–98.4] | 96.9 [96.0–97.6] | 100.0 [100.0–100.0] | 89.7 | 94.0 | 98.2 [97.7–98.7] | 97.2 [96.3–97.8] | 90.1 [88.1–91.9] | 89.9 [87.9–91.8] | 0.3 [0.0–0.7] | 0.5 [0.1–1.0] | 0.0% | 0.6% | 0.0% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax (WME\*) +1 more |
| MARS | hybrid | 97.8 [97.2–98.3] | 96.9 [96.1–97.6] | 100.0 [99.9–100.0] | 89.9 | 94.0 | 98.2 [97.6–98.7] | 97.2 [96.3–97.8] | 90.2 [88.2–92.0] | 90.0 [88.0–91.8] | 0.3 [0.0–0.7] | 0.4 [0.1–0.9] | 0.0% | 0.6% | 0.0% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax (WME\*) +1 more |
| Soft Minimax-D | hybrid | 97.7 [97.1–98.4] | 97.6 [96.8–98.2] | 100.0 [100.0–100.0] | 90.1 | 94.0 | 98.1 [97.5–98.6] | 97.5 [96.7–98.1] | 91.0 [89.1–92.7] | 91.9 [90.0–93.6] | 0.2 [0.0–0.5] | 0.3 [0.0–0.7] | 0.0% | 0.5% | 0.0% | — |
| BTR-score | hybrid | 97.5 [96.8–98.1] | 98.1 [97.5–98.6] | 100.0 [99.9–100.0] | 90.2 | 94.0 | 97.6 [97.0–98.2] | 97.9 [97.3–98.5] | 92.2 [90.5–93.8] | 93.3 [91.7–94.8] | 0.3 [0.0–0.7] | 0.4 [0.1–0.8] | 0.1% | 0.6% | 0.0% | — |
| ESR | rated | 97.5 [96.8–98.1] | 96.7 [95.8–97.4] | 99.9 [99.9–99.9] | 89.9 | 94.0 | 97.7 [97.1–98.2] | 96.7 [95.9–97.5] | 89.8 [87.9–91.6] | 90.4 [88.6–92.1] | 0.5 [0.1–0.9] | 0.7 [0.2–1.3] | 0.1% | 0.8% | 0.0% | Smith//Score (WME\*), STAR (WME\*), Score runoff (WME\*) +3 more |
| STAR | hybrid | 97.4 [96.7–98.1] | 97.5 [96.7–98.2] | 99.9 [99.9–100.0] | 90.1 | 94.0 | 97.5 [96.9–98.1] | 97.2 [96.4–97.8] | 91.0 [89.2–92.7] | 92.2 [90.4–93.7] | 0.3 [0.0–0.7] | 0.4 [0.1–0.8] | 0.1% | 0.7% | 0.0% | Score runoff (WME\*) |
| Smith//Score | condorcet-based | 97.4 [96.7–98.1] | 97.9 [97.2–98.4] | 100.0 [99.9–100.0] | 90.2 | 94.0 | 97.6 [97.0–98.2] | 97.8 [97.2–98.4] | 92.0 [90.3–93.6] | 92.4 [90.7–94.0] | 0.3 [0.0–0.7] | 0.7 [0.3–1.3] | 0.0% | 0.5% | 0.0% | Score runoff (WME\*) |
| ARA | hybrid | 97.4 [96.6–98.0] | 95.9 [95.0–96.7] | 100.0 [99.9–100.0] | 89.9 | 94.0 | 97.3 [96.6–97.9] | 95.3 [94.3–96.2] | 88.1 [86.2–90.1] | 88.5 [86.5–90.5] | 0.6 [0.2–1.1] | 0.6 [0.2–1.1] | 0.1% | 0.5% | 0.0% | Smith//Score (WME\*), STAR (WME\*), Score runoff (WME\*) +5 more |
| Soft Minimax | hybrid | 97.3 [96.6–98.0] | 97.9 [97.3–98.5] | 100.0 [99.9–100.0] | 90.3 | 94.0 | 97.5 [96.7–98.1] | 98.0 [97.3–98.5] | 92.3 [90.6–93.9] | 92.4 [90.7–94.0] | 0.3 [0.0–0.7] | 0.3 [0.0–0.7] | 0.1% | 0.5% | 0.0% | Score runoff (WME\*), Soft Minimax-D (VSE\*) |
| Geometric median | rated | 97.3 [96.6–97.9] | 94.8 [93.7–95.8] | 99.9 [99.9–99.9] | 90.0 | 94.0 | 97.1 [96.4–97.8] | 93.6 [92.3–94.7] | 84.3 [82.2–86.5] | 86.5 [84.5–88.5] | 0.4 [0.1–0.8] | 0.5 [0.1–1.0] | 0.1% | 1.0% | 0.0% | Smith//Score (WME\*), ESR (WME\*), STAR (WME\*) +6 more |
| MMWV | minimax-family | 97.1 [96.4–97.8] | 99.7 [99.6–99.9] | 98.4 [98.4–98.5] | 79.8 | 82.6 | 97.2 [96.4–97.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.9 [98.2–99.5] | 0.3 [0.0–0.7] | 0.3 [0.0–0.7] | 0.1% | 13.6% | 0.0% | — |
| Ranked pairs | condorcet | 97.1 [96.3–97.8] | 99.9 [99.8–100.0] | 98.2 [98.1–98.2] | 79.6 | 82.6 | 97.2 [96.5–97.9] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 99.4 [98.9–99.8] | 0.3 [0.0–0.7] | 0.5 [0.1–1.0] | 5.5% | 8.8% | 0.7% | — |
| Minimax | minimax-family | 97.1 [96.3–97.7] | 99.8 [99.7–99.9] | 98.1 [98.1–98.2] | 79.6 | 82.6 | 97.2 [96.4–97.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 99.2 [98.7–99.7] | 0.3 [0.0–0.7] | 0.4 [0.1–0.8] | 5.5% | 8.8% | 0.7% | — |
| DMS | rated | 97.0 [96.4–97.6] | 92.5 [91.0–93.8] | 100.0 [100.0–100.0] | 89.5 | 94.0 | 96.9 [96.2–97.6] | 92.8 [91.4–94.0] | 84.2 [81.9–86.3] | 84.1 [81.8–86.3] | 1.1 [0.5–1.8] | 0.5 [0.1–1.0] | 0.0% | 0.5% | 0.0% | MARS (VSE\* WME\*), MARS-T (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +10 more |
| Schulze | condorcet | 97.0 [96.2–97.7] | 99.8 [99.7–99.9] | 98.1 [98.1–98.2] | 79.6 | 82.6 | 97.2 [96.4–97.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 99.2 [98.7–99.7] | 0.3 [0.0–0.7] | 0.4 [0.1–0.8] | 5.5% | 8.8% | 0.7% | — |
| Score runoff | two-round | 97.0 [96.2–97.7] | 98.9 [98.5–99.3] | 99.9 [99.9–99.9] | 89.9 | 94.0 | 97.0 [96.2–97.7] | 99.0 [98.6–99.3] | 97.9 [97.0–98.7] | 97.9 [97.0–98.7] | 0.4 [0.1–0.8] | 0.5 [0.1–1.0] | 0.1% | 0.9% | 0.0% | — |
| Score | rated | 97.0 [96.3–97.6] | 92.7 [91.3–94.0] | 100.0 [100.0–100.0] | 89.4 | 94.0 | 97.2 [96.6–97.8] | 93.8 [92.5–94.9] | 84.7 [82.6–86.8] | 83.1 [80.6–85.4] | 0.6 [0.2–1.1] | 0.7 [0.2–1.3] | 0.0% | 0.5% | 0.0% | MARS (VSE\* WME\*), MARS-T (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +10 more |
| Borda | positional | 97.0 [96.2–97.7] | 95.8 [94.8–96.7] | 97.9 [97.9–98.0] | 78.7 | 82.6 | 95.6 [94.5–96.6] | 95.3 [94.2–96.2] | 86.6 [84.4–88.7] | 87.8 [85.6–89.8] | 0.4 [0.1–0.9] | 0.1 [0.0–0.3] | 6.4% | 9.1% | 0.7% | MARS (VSE\* WME\* sincerity), MARS-T (VSE\* WME\* sincerity), Soft Minimax-D (VSE\* WME\* sincerity) +18 more |
| MMPO | minimax-family | 96.9 [96.2–97.7] | 99.6 [99.3–99.8] | 98.4 [98.4–98.5] | 79.8 | 82.6 | 97.2 [96.4–97.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.6 [97.8–99.3] | 0.3 [0.0–0.7] | 0.3 [0.0–0.7] | 0.2% | 13.7% | 0.0% | MMWV (VSE\* WME\*), Ranked pairs (WME\*), Schulze (WME\*) +1 more |
| Kemeny-Young | condorcet | 96.9 [96.1–97.6] | 99.6 [99.3–99.8] | 98.2 [98.1–98.2] | 79.6 | 82.6 | 97.1 [96.3–97.8] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 98.8 [98.1–99.4] | 0.4 [0.1–0.8] | 0.4 [0.1–0.8] | 5.5% | 8.9% | 0.7% | Ranked pairs (WME\*), Schulze (WME\*), Minimax (WME\*) |
| Copeland | condorcet | 96.6 [95.7–97.4] | 99.3 [99.0–99.6] | 98.1 [98.1–98.2] | 79.5 | 82.6 | 96.8 [96.1–97.5] | 99.6 [99.4–99.8] | 100.0 [100.0–100.0] | 98.6 [97.8–99.2] | 0.3 [0.0–0.7] | 0.5 [0.1–1.0] | 5.5% | 8.8% | 0.7% | Ranked pairs (VSE\* WME\*), MMWV (VSE\* WME\*), Schulze (WME\*) +2 more |
| Approval runoff | two-round | 96.5 [95.6–97.2] | 97.2 [96.2–98.0] | 99.8 [99.7–99.8] | 61.7 | 63.1 | 96.3 [95.5–97.1] | 96.5 [95.6–97.5] | 94.7 [93.4–96.1] | 95.6 [94.5–97.0] | 0.8 [0.3–1.4] | 0.7 [0.2–1.2] | 0.3% | 1.7% | 0.0% | Smith//Score (VSE\*), ESR (VSE\*), STAR (VSE\*) +6 more |
| BTR-IRV | condorcet-based | 96.4 [95.5–97.2] | 98.7 [98.2–99.2] | 98.0 [98.0–98.1] | 79.4 | 82.6 | 97.0 [96.3–97.7] | 99.8 [99.7–99.9] | 100.0 [100.0–100.0] | 97.2 [96.2–98.2] | 0.4 [0.1–0.8] | 0.7 [0.2–1.3] | 5.7% | 9.1% | 0.9% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Minimax (VSE\* WME\*) +6 more |
| MJ | median-based | 95.9 [94.9–96.8] | 92.2 [90.7–93.5] | 99.9 [99.9–100.0] | 89.7 | 94.0 | 95.8 [94.8–96.6] | 90.6 [89.0–92.2] | 81.0 [78.3–83.3] | 83.4 [81.0–85.6] | 0.8 [0.3–1.4] | 1.0 [0.4–1.7] | 0.0% | 0.7% | 0.0% | Smith//Score (VSE\* WME\*), ESR (VSE\* WME\*), Geometric median (VSE\* WME\*) +13 more |
| 321 | hybrid | 95.7 [94.8–96.5] | 89.9 [88.1–91.5] | 99.9 [99.9–99.9] | 81.8 | 85.0 | 94.6 [93.6–95.6] | 87.6 [85.4–89.5] | 79.0 [76.4–81.4] | 81.4 [78.9–83.8] | 1.4 [0.7–2.2] | 0.7 [0.2–1.3] | 0.2% | 1.3% | 0.0% | Smith//Score (VSE\* WME\*), Score (VSE\* WME\*), DMS (VSE\* WME\*) +14 more |
| Approval cond. runoff | two-round | 95.6 [94.4–96.6] | 93.7 [92.4–95.0] | 99.8 [99.8–99.8] | 61.8 | 63.1 | 96.1 [95.2–96.9] | 93.4 [92.0–94.6] | 87.9 [85.8–89.9] | 88.1 [85.9–90.1] | 0.8 [0.3–1.4] | 0.9 [0.4–1.5] | 0.2% | 1.6% | 0.0% | Smith//Score (VSE\* WME\*), Approval runoff (VSE\* WME\*), ESR (VSE\* WME\*) +12 more |
| MAV | median-based | 94.9 [93.8–95.9] | 91.2 [89.7–92.7] | 99.9 [99.9–100.0] | 89.8 | 94.0 | 94.0 [92.8–95.1] | 87.1 [85.1–89.1] | 77.0 [74.1–79.6] | 81.5 [79.1–83.9] | 1.2 [0.6–1.9] | 1.2 [0.6–1.9] | 0.0% | 0.8% | 0.0% | Smith//Score (VSE\* WME\*), Approval runoff (VSE\* WME\*), ESR (VSE\* WME\*) +14 more |
| Approval | approval-based | 94.5 [93.2–95.6] | 91.0 [89.2–92.5] | 99.9 [99.9–99.9] | 62.0 | 63.1 | 93.4 [91.9–94.7] | 89.2 [87.4–90.8] | 79.7 [76.9–82.1] | 82.4 [79.7–84.8] | 2.7 [1.7–3.7] | 2.5 [1.6–3.5] | 0.0% | 1.1% | 0.0% | Smith//Score (VSE\* WME\*), Approval runoff (VSE\* WME\*), Score (VSE\* WME\*) +15 more |
| ER-IRV | plurality-based | 94.4 [93.3–95.4] | 93.0 [91.5–94.4] | 98.3 [98.2–98.4] | 79.4 | 82.6 | 91.5 [90.0–93.0] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 89.1 [87.1–90.9] | 2.3 [1.4–3.2] | 1.4 [0.7–2.2] | 1.5% | 13.6% | 0.2% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), ESR (VSE\* WME\* sincerity) +24 more |
| IRV-3 | plurality-based | 93.7 [92.5–94.8] | 90.9 [89.2–92.5] | 99.1 [99.0–99.1] | 70.0 | 71.7 | 90.4 [88.7–92.0] | 87.1 [85.0–89.1] | 82.2 [79.7–84.3] | 85.8 [83.7–87.8] | 2.9 [1.9–3.9] | 1.4 [0.7–2.1] | 6.1% | 4.6% | 1.2% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +24 more |
| IRV | plurality-based | 93.7 [92.4–94.8] | 92.0 [90.3–93.4] | 97.9 [97.8–98.0] | 79.2 | 82.6 | 91.5 [90.0–93.0] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 87.9 [85.8–89.9] | 2.3 [1.4–3.2] | 1.7 [0.9–2.6] | 5.8% | 9.5% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +26 more |
| TTR | two-round | 91.9 [90.2–93.5] | 89.7 [87.7–91.6] | 97.8 [97.6–98.0] | 53.6 | 55.7 | 89.2 [87.5–91.1] | 84.3 [82.1–86.5] | 80.8 [78.3–83.1] | 86.0 [83.7–88.1] | 3.4 [2.4–4.5] | 3.0 [2.0–4.1] | 6.5% | 0.0% | 2.5% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +29 more |
| Supplementary vote | plurality-based | 88.9 [87.1–90.7] | 85.5 [83.5–87.7] | 97.9 [97.9–98.0] | 79.3 | 82.6 | 82.4 [79.9–85.2] | 79.4 [76.9–81.9] | 71.9 [69.0–74.7] | 77.1 [74.5–79.7] | 6.9 [5.3–8.3] | 4.3 [3.0–5.6] | 6.4% | 10.8% | 1.1% | Smith//Score (VSE\* WME\* sincerity), IRV-3 (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity) +30 more |
| Plurality | plurality-based | 83.8 [81.3–86.1] | 78.0 [75.5–80.5] | 98.7 [98.6–98.8] | 54.1 | 55.7 | 70.2 [66.5–73.9] | 69.6 [66.6–72.7] | 63.7 [60.8–66.9] | 70.2 [67.6–72.9] | 12.8 [10.9–15.0] | 7.1 [5.7–8.6] | 8.4% | 0.0% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +31 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 44.9% |
| Supplementary vote | 40.5% |
| IRV-3 | 37.7% |
| TTR | 36.3% |
| ER-IRV | 36.1% |
| IRV | 35.9% |
| Approval | 35.6% |
| Geometric median | 34.9% |
| ESR | 34.8% |
| MAV | 34.7% |
| Approval cond. runoff | 34.4% |
| MARS-T | 34.4% |
| Soft Minimax-D | 34.2% |
| Approval runoff | 34.0% |
| Smith//Score | 33.9% |
| MARS | 33.9% |
| BTR-score | 33.9% |
| BTR-IRV | 33.9% |
| MJ | 33.9% |
| DMS | 33.7% |
| Soft Minimax | 33.6% |
| MMPO | 33.6% |
| MMWV | 33.5% |
| Kemeny-Young | 33.5% |
| Soft Minimax-40 | 33.4% |
| ARA | 33.4% |
| STAR | 33.3% |
| Score runoff | 33.2% |
| Ranked pairs | 33.2% |
| Schulze | 33.1% |
| Minimax | 33.1% |
| 321 | 32.9% |
| Score | 32.9% |
| Copeland | 32.5% |
| Borda | 31.6% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 977 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 23 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Soft Minimax-40 | 91.2% (91.0%) | 5.8% (6.2%) | 3.0% (2.8%) | 47.8% (47.8%) | 94.0% (93.2%) | 87.9% |
| MARS-T | 91.3% (91.0%) | 6.2% (6.2%) | 2.5% (2.8%) | 65.2% (47.8%) | 94.1% (96.6%) | 89.9% |
| MARS | 91.0% (91.0%) | 5.7% (6.2%) | 3.3% (2.8%) | 52.2% (47.8%) | 96.4% (97.1%) | 90.0% |
| Soft Minimax-D | 91.7% (91.0%) | 5.6% (6.2%) | 2.7% (2.8%) | 60.9% (47.8%) | 98.2% (98.8%) | 91.9% |
| BTR-score | 91.9% (91.0%) | 5.4% (6.2%) | 2.7% (2.8%) | 69.6% (47.8%) | 100.0% (100.0%) | 93.3% |
| ESR | 91.3% (91.0%) | 5.5% (6.2%) | 3.2% (2.8%) | 65.2% (47.8%) | 96.5% (96.9%) | 90.4% |
| STAR | 91.4% (91.0%) | 6.1% (6.2%) | 2.5% (2.8%) | 73.9% (47.8%) | 99.2% (98.6%) | 92.2% |
| Smith//Score | 91.2% (91.0%) | 5.6% (6.2%) | 3.2% (2.8%) | 56.5% (47.8%) | 100.0% (100.0%) | 92.4% |
| ARA | 91.4% (91.0%) | 5.8% (6.2%) | 2.8% (2.8%) | 56.5% (47.8%) | 93.0% (91.7%) | 88.5% |
| Soft Minimax | 91.1% (91.0%) | 6.3% (6.2%) | 2.6% (2.8%) | 52.2% (47.8%) | 100.0% (100.0%) | 92.4% |
| Geometric median | 91.2% (91.0%) | 6.3% (6.2%) | 2.5% (2.8%) | 65.2% (47.8%) | 89.8% (87.4%) | 86.5% |
| MMWV | 98.4% (100.0%) | 0.6% (0.0%) | 1.0% (0.0%) | 13.0% (0.0%) | 100.0% (100.0%) | 98.9% |
| Ranked pairs | 98.8% (100.0%) | 0.4% (0.0%) | 0.8% (0.0%) | 30.4% (0.0%) | 100.0% (100.0%) | 99.4% |
| Minimax | 98.6% (100.0%) | 0.4% (0.0%) | 1.0% (0.0%) | 34.8% (0.0%) | 100.0% (100.0%) | 99.2% |
| DMS | 90.8% (91.0%) | 7.2% (6.2%) | 2.0% (2.8%) | 56.5% (47.8%) | 87.6% (87.5%) | 84.1% |
| Schulze | 98.4% (100.0%) | 0.4% (0.0%) | 1.2% (0.0%) | 30.4% (0.0%) | 100.0% (100.0%) | 99.2% |
| Score | 90.2% (91.0%) | 7.1% (6.2%) | 2.8% (2.8%) | 56.5% (47.8%) | 88.4% (88.8%) | 83.1% |
| Borda | 97.2% (100.0%) | 1.4% (0.0%) | 1.3% (0.0%) | 43.5% (0.0%) | 88.0% (86.6%) | 87.8% |
| MMPO | 98.1% (100.0%) | 0.7% (0.0%) | 1.2% (0.0%) | 8.7% (0.0%) | 99.9% (100.0%) | 98.6% |
| Kemeny-Young | 98.3% (100.0%) | 0.8% (0.0%) | 0.9% (0.0%) | 30.4% (0.0%) | 100.0% (100.0%) | 98.8% |
| Copeland | 98.2% (100.0%) | 0.8% (0.0%) | 1.0% (0.0%) | 43.5% (0.0%) | 100.0% (100.0%) | 98.6% |
| BTR-IRV | 96.8% (100.0%) | 1.5% (0.0%) | 1.6% (0.0%) | 30.4% (0.0%) | 100.0% (100.0%) | 97.2% |
| MJ | 91.3% (91.0%) | 5.5% (6.2%) | 3.2% (2.8%) | 56.5% (47.8%) | 87.5% (83.6%) | 83.4% |
| 321 | 83.0% (80.7%) | 13.6% (14.8%) | 3.4% (4.5%) | 91.3% (87.0%) | 95.4% (94.4%) | 81.4% |
| MAV | 91.0% (91.0%) | 6.7% (6.2%) | 2.4% (2.8%) | 52.2% (47.8%) | 86.1% (80.2%) | 81.5% |
| ER-IRV | 96.0% (100.0%) | 2.6% (0.0%) | 1.4% (0.0%) | 39.1% (0.0%) | 89.4% (84.8%) | 89.1% |
| IRV-3 | 89.2% (89.9%) | 6.3% (5.6%) | 4.5% (4.5%) | 43.5% (34.8%) | 93.8% (90.5%) | 85.8% |
| IRV | 96.5% (100.0%) | 2.4% (0.0%) | 1.1% (0.0%) | 26.1% (0.0%) | 89.0% (84.8%) | 87.9% |
| Supplementary vote | 95.6% (100.0%) | 3.5% (0.0%) | 0.9% (0.0%) | 52.2% (0.0%) | 78.2% (71.9%) | 77.1% |

![strategy arrows](arrows-c4.png)

![strategy shifts](shift-c4.png)

![sincerity](sincerity-c4.png)

![fidelity](fidelity-c4.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c4.png)

## c5: utility quadratic

1000 elections (seeds 1), 977 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, quadratic utility; 3 poll round(s), 10 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 97.7%; it also has the highest total utility in 83.1% of those; electing it loses 4.1% of VSE; favourite ÷ least liked: — (utility not positive). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 95.5 to 95.6; with the best VSE (100) WME 91.7 (nothing lower is possible there either); the most balanced point VSE 98.3 / WME 98.9. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 98.1 (at WME 95.4); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -74.2 / WME -53.2.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): MARS-T, Soft Minimax-D, Soft Minimax, Borda, Score runoff, MMPO, Ranked pairs, Approval runoff.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Soft Minimax-D, Soft Minimax, BTR-score, Score runoff, MMPO, Ranked pairs, Minimax, Kemeny-Young, MMWV, Schulze.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| MARS-T | hybrid | 97.3 [96.7–98.0] | 92.6 [90.9–94.1] | 100.0 [100.0–100.0] | 90.3 | 94.0 | 97.4 [96.7–98.0] | 92.7 [91.0–94.1] | 84.8 [82.5–87.0] | 85.0 [82.8–87.0] | 0.3 [0.0–0.7] | 0.3 [0.0–0.7] | 0.0% | 0.4% | 0.0% | Soft Minimax-D (WME\*) |
| Soft Minimax-40 | hybrid | 97.3 [96.6–97.9] | 91.8 [90.2–93.4] | 100.0 [100.0–100.0] | 90.5 | 94.0 | 97.7 [97.0–98.3] | 91.1 [89.3–92.7] | 82.9 [80.6–85.2] | 83.8 [81.5–85.9] | 0.2 [0.0–0.5] | 0.3 [0.0–0.7] | 0.0% | 0.4% | 0.0% | Smith//Score (WME\*), STAR (WME\*), MARS (WME\*) +1 more |
| Soft Minimax-D | hybrid | 97.1 [96.4–97.8] | 94.1 [92.7–95.4] | 100.0 [100.0–100.0] | 90.4 | 94.0 | 97.3 [96.7–98.0] | 93.6 [92.1–94.9] | 86.6 [84.5–88.7] | 87.1 [84.9–89.1] | 0.2 [0.0–0.5] | 0.3 [0.0–0.7] | 0.0% | 0.4% | 0.0% | — |
| ARA | hybrid | 97.1 [96.2–97.8] | 90.8 [89.0–92.3] | 100.0 [100.0–100.0] | 90.3 | 94.0 | 97.3 [96.6–97.9] | 90.5 [88.7–92.2] | 82.2 [79.9–84.5] | 81.7 [79.2–84.1] | 0.4 [0.1–0.8] | 0.4 [0.1–0.8] | 0.1% | 0.5% | 0.0% | Smith//Score (WME\*), ESR (WME\*), STAR (WME\*) +6 more |
| MARS | hybrid | 97.0 [96.3–97.7] | 92.9 [91.3–94.2] | 100.0 [100.0–100.0] | 90.3 | 94.0 | 97.3 [96.6–98.0] | 93.0 [91.4–94.4] | 85.3 [83.0–87.4] | 85.3 [83.0–87.4] | 0.3 [0.0–0.7] | 0.4 [0.1–0.8] | 0.1% | 0.4% | 0.0% | Smith//Score (WME\*), ESR (WME\*), STAR (WME\*) +3 more |
| DMS | rated | 96.8 [96.1–97.5] | 88.8 [86.8–90.6] | 100.0 [100.0–100.0] | 89.9 | 94.0 | 96.3 [95.5–97.1] | 88.0 [85.9–90.0] | 78.8 [76.3–81.2] | 79.0 [76.4–81.6] | 0.5 [0.1–1.0] | 0.6 [0.2–1.1] | 0.0% | 0.4% | 0.0% | Smith//Score (WME\*), ESR (WME\*), STAR (WME\*) +8 more |
| Smith//Score | condorcet-based | 96.8 [95.9–97.5] | 94.1 [92.6–95.4] | 100.0 [100.0–100.0] | 90.6 | 94.0 | 96.9 [96.1–97.6] | 93.8 [92.2–95.1] | 87.4 [85.3–89.3] | 87.8 [85.9–89.7] | 0.3 [0.0–0.7] | 0.6 [0.2–1.1] | 0.1% | 0.4% | 0.0% | Soft Minimax (WME\*) |
| Soft Minimax | hybrid | 96.8 [96.0–97.5] | 94.6 [93.2–95.8] | 100.0 [100.0–100.0] | 90.7 | 94.0 | 96.6 [95.9–97.4] | 94.3 [92.8–95.6] | 88.3 [86.3–90.1] | 88.5 [86.6–90.4] | 0.2 [0.0–0.5] | 0.3 [0.0–0.7] | 0.0% | 0.4% | 0.0% | — |
| STAR | hybrid | 96.8 [96.0–97.5] | 93.7 [92.3–95.1] | 100.0 [100.0–100.0] | 90.4 | 94.0 | 97.0 [96.2–97.7] | 93.3 [91.7–94.6] | 86.4 [84.3–88.4] | 86.9 [84.8–89.0] | 0.2 [0.0–0.5] | 0.3 [0.0–0.7] | 0.1% | 0.6% | 0.0% | BTR-score (WME\*), Soft Minimax (WME\*) |
| Score | rated | 96.7 [96.0–97.4] | 88.6 [86.6–90.5] | 100.0 [100.0–100.0] | 89.8 | 94.0 | 97.0 [96.2–97.7] | 87.6 [85.5–89.5] | 78.3 [75.9–80.7] | 79.9 [77.3–82.4] | 0.3 [0.0–0.7] | 0.5 [0.1–1.0] | 0.0% | 0.4% | 0.0% | MARS-T (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*), Smith//Score (WME\*) +8 more |
| ESR | rated | 96.7 [96.0–97.4] | 93.8 [92.4–95.1] | 99.9 [99.9–100.0] | 90.4 | 94.0 | 96.7 [95.9–97.4] | 93.6 [91.9–94.9] | 86.8 [84.8–88.8] | 87.5 [85.4–89.5] | 0.2 [0.0–0.5] | 0.3 [0.0–0.7] | 0.1% | 0.7% | 0.0% | Soft Minimax (WME\*), Soft Minimax-D (VSE\*) |
| BTR-score | hybrid | 96.7 [95.9–97.4] | 94.4 [93.0–95.6] | 100.0 [99.9–100.0] | 90.5 | 94.0 | 96.7 [95.9–97.4] | 94.1 [92.6–95.3] | 87.9 [85.9–89.8] | 88.2 [86.1–90.1] | 0.2 [0.0–0.5] | 0.5 [0.1–1.0] | 0.1% | 0.5% | 0.0% | — |
| Borda | positional | 96.1 [95.1–96.9] | 95.8 [94.9–96.7] | 98.2 [98.1–98.3] | 77.4 | 80.6 | 95.0 [94.0–95.9] | 95.3 [94.2–96.2] | 86.6 [84.4–88.7] | 87.7 [85.6–89.6] | 0.2 [0.0–0.5] | 0.1 [0.0–0.3] | 6.4% | 7.2% | 0.6% | Score runoff (WME\* sincerity), Ranked pairs (WME\*), Schulze (WME\*) +8 more |
| Score runoff | two-round | 96.1 [95.2–96.9] | 98.1 [97.5–98.7] | 99.9 [99.9–100.0] | 90.3 | 94.0 | 96.3 [95.4–97.1] | 98.2 [97.5–98.7] | 96.1 [94.8–97.1] | 95.9 [94.6–97.0] | 0.2 [0.0–0.5] | 0.2 [0.0–0.5] | 0.1% | 0.8% | 0.0% | — |
| 321 | hybrid | 95.6 [94.7–96.4] | 84.2 [81.6–86.7] | 99.9 [99.9–99.9] | 82.6 | 85.6 | 94.1 [92.9–95.2] | 79.7 [76.5–82.5] | 72.9 [70.0–75.6] | 75.9 [73.2–78.6] | 0.7 [0.2–1.3] | 0.5 [0.1–1.0] | 0.2% | 1.0% | 0.0% | Smith//Score (VSE\* WME\*), Score (VSE\* WME\*), DMS (VSE\* WME\*) +14 more |
| MMPO | minimax-family | 95.5 [94.5–96.3] | 99.7 [99.4–99.9] | 98.7 [98.6–98.7] | 78.3 | 80.6 | 95.5 [94.6–96.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.9 [98.2–99.5] | 0.5 [0.1–1.0] | 0.5 [0.1–1.0] | 0.1% | 11.5% | 0.0% | — |
| Ranked pairs | condorcet | 95.4 [94.5–96.3] | 99.7 [99.5–99.9] | 98.4 [98.3–98.4] | 78.1 | 80.6 | 95.6 [94.6–96.5] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.9 [98.2–99.5] | 0.4 [0.1–0.8] | 0.6 [0.2–1.1] | 5.4% | 7.0% | 0.6% | — |
| Minimax | minimax-family | 95.4 [94.4–96.2] | 99.4 [99.1–99.8] | 98.4 [98.4–98.5] | 78.1 | 80.6 | 95.5 [94.6–96.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.8 [98.1–99.4] | 0.5 [0.1–1.0] | 0.7 [0.3–1.2] | 5.5% | 7.0% | 0.6% | — |
| Kemeny-Young | condorcet | 95.3 [94.4–96.2] | 99.5 [99.2–99.8] | 98.4 [98.3–98.4] | 78.1 | 80.6 | 95.6 [94.6–96.4] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 98.8 [98.1–99.4] | 0.5 [0.1–1.0] | 0.5 [0.1–1.0] | 5.4% | 7.0% | 0.6% | — |
| MMWV | minimax-family | 95.3 [94.4–96.2] | 99.7 [99.4–99.9] | 98.7 [98.6–98.7] | 78.3 | 80.6 | 95.5 [94.6–96.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 99.0 [98.3–99.5] | 0.5 [0.1–1.0] | 0.6 [0.2–1.2] | 0.1% | 11.5% | 0.0% | — |
| Schulze | condorcet | 95.3 [94.4–96.2] | 99.4 [99.1–99.8] | 98.4 [98.3–98.5] | 78.1 | 80.6 | 95.5 [94.6–96.4] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 98.8 [98.1–99.4] | 0.5 [0.1–1.0] | 0.7 [0.3–1.2] | 5.5% | 7.1% | 0.6% | — |
| Geometric median | rated | 95.3 [94.3–96.2] | 93.3 [91.7–94.7] | 99.9 [99.9–99.9] | 90.3 | 94.0 | 95.1 [94.1–96.0] | 94.2 [92.8–95.4] | 86.8 [84.7–89.0] | 86.0 [83.8–88.1] | 0.6 [0.2–1.1] | 0.7 [0.2–1.2] | 0.1% | 1.0% | 0.0% | Smith//Score (VSE\*), Approval runoff (WME\*), ESR (VSE\*) +8 more |
| Approval | approval-based | 95.2 [94.2–96.1] | 92.8 [91.4–94.0] | 99.9 [99.9–99.9] | 60.6 | 61.7 | 92.3 [91.0–93.7] | 90.3 [88.6–91.8] | 81.6 [79.0–83.8] | 84.8 [82.6–86.8] | 2.8 [1.8–3.8] | 1.0 [0.5–1.7] | 0.0% | 0.9% | 0.0% | BTR-score (VSE\* WME\*), Soft Minimax (VSE\* WME\*), Smith//Score (VSE\*) +8 more |
| Copeland | condorcet | 95.2 [94.1–96.1] | 99.0 [98.6–99.4] | 98.4 [98.3–98.4] | 78.0 | 80.6 | 95.4 [94.4–96.3] | 99.6 [99.4–99.8] | 100.0 [100.0–100.0] | 98.4 [97.6–99.1] | 0.5 [0.1–1.0] | 0.5 [0.1–1.0] | 5.5% | 7.1% | 0.6% | Ranked pairs (WME\*), Kemeny-Young (WME\*), MMWV (WME\*) +1 more |
| BTR-IRV | condorcet-based | 95.1 [94.1–96.0] | 99.2 [98.8–99.6] | 98.3 [98.2–98.4] | 77.9 | 80.6 | 95.5 [94.5–96.4] | 99.8 [99.7–99.9] | 100.0 [100.0–100.0] | 98.0 [97.0–98.8] | 0.5 [0.1–1.0] | 0.4 [0.1–0.8] | 5.7% | 7.2% | 0.7% | Ranked pairs (WME\*), MMWV (WME\*), MMPO (WME\*) |
| Approval runoff | two-round | 94.7 [93.7–95.7] | 98.1 [97.5–98.7] | 99.8 [99.8–99.8] | 60.4 | 61.7 | 94.2 [93.1–95.3] | 97.4 [96.5–98.1] | 95.4 [94.1–96.6] | 96.6 [95.4–97.7] | 0.7 [0.2–1.3] | 0.9 [0.4–1.5] | 0.2% | 1.4% | 0.0% | Score runoff (VSE\*) |
| Approval cond. runoff | two-round | 94.7 [93.4–95.8] | 93.3 [91.9–94.5] | 99.9 [99.8–99.9] | 60.4 | 61.7 | 93.7 [92.6–94.8] | 92.6 [91.1–93.7] | 85.9 [83.7–87.9] | 86.7 [84.6–88.8] | 1.1 [0.5–1.7] | 1.1 [0.5–1.7] | 0.1% | 1.2% | 0.0% | Score runoff (VSE\* WME\*), Smith//Score (VSE\*), Approval runoff (WME\*) +7 more |
| MJ | median-based | 93.7 [92.5–94.8] | 90.1 [88.1–91.8] | 99.9 [99.9–100.0] | 90.1 | 94.0 | 92.9 [91.7–94.0] | 88.1 [86.1–89.9] | 79.7 [77.0–82.2] | 81.6 [79.0–83.8] | 1.3 [0.7–2.0] | 0.9 [0.4–1.5] | 0.0% | 0.7% | 0.0% | Smith//Score (VSE\* WME\*), Approval (VSE\* WME\*), ESR (VSE\* WME\*) +14 more |
| MAV | median-based | 92.5 [91.3–93.8] | 89.1 [87.2–90.9] | 100.0 [99.9–100.0] | 90.1 | 94.0 | 90.3 [88.7–91.7] | 85.7 [83.6–87.6] | 77.0 [74.2–79.6] | 80.7 [77.9–83.0] | 2.0 [1.2–2.9] | 1.4 [0.7–2.1] | 0.0% | 0.7% | 0.0% | Smith//Score (VSE\* WME\*), Approval (VSE\* WME\*), Approval runoff (VSE\* WME\*) +15 more |
| ER-IRV | plurality-based | 91.2 [89.8–92.6] | 92.8 [91.2–94.2] | 98.5 [98.4–98.6] | 77.9 | 80.6 | 87.5 [85.6–89.3] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 88.7 [86.6–90.6] | 3.0 [2.0–4.1] | 2.0 [1.1–2.9] | 1.5% | 11.7% | 0.2% | Approval runoff (VSE\* WME\* sincerity), Score runoff (VSE\* WME\* sincerity), Ranked pairs (VSE\* WME\*) +21 more |
| IRV | plurality-based | 91.2 [89.7–92.6] | 93.0 [91.5–94.3] | 98.1 [98.1–98.2] | 77.7 | 80.6 | 87.5 [85.6–89.3] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 89.4 [87.3–91.2] | 3.0 [2.0–4.1] | 2.2 [1.3–3.1] | 5.9% | 7.8% | 1.2% | Approval runoff (VSE\* WME\* sincerity), Score runoff (VSE\* WME\* sincerity), Ranked pairs (VSE\* WME\*) +21 more |
| TTR | two-round | 90.8 [89.4–92.3] | 91.0 [89.2–92.8] | 98.1 [98.0–98.3] | 49.1 | 50.9 | 85.1 [83.0–87.1] | 84.3 [82.1–86.5] | 80.8 [78.3–83.1] | 87.6 [85.5–89.7] | 4.4 [3.1–5.7] | 2.7 [1.7–3.7] | 7.1% | 0.0% | 2.2% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +26 more |
| IRV-3 | plurality-based | 90.3 [88.7–91.8] | 92.2 [90.7–93.6] | 99.2 [99.2–99.3] | 65.4 | 66.8 | 86.3 [84.2–88.3] | 87.1 [85.0–89.1] | 82.2 [79.7–84.3] | 86.8 [84.6–88.7] | 3.7 [2.7–4.9] | 2.6 [1.7–3.6] | 6.2% | 3.6% | 1.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +20 more |
| Supplementary vote | plurality-based | 85.9 [84.0–87.9] | 85.2 [83.1–87.3] | 98.2 [98.1–98.2] | 77.7 | 80.6 | 77.9 [74.8–81.0] | 79.4 [76.9–81.9] | 71.9 [69.0–74.7] | 76.8 [74.2–79.4] | 7.9 [6.2–9.5] | 4.2 [3.0–5.4] | 6.8% | 8.9% | 1.0% | Smith//Score (VSE\* WME\* sincerity), IRV-3 (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity) +30 more |
| Plurality | plurality-based | 80.5 [77.8–83.0] | 79.1 [76.4–81.7] | 99.0 [98.9–99.0] | 49.6 | 50.9 | 66.7 [62.7–70.6] | 69.6 [66.6–72.7] | 63.7 [60.8–66.9] | 71.8 [68.9–74.5] | 13.7 [11.6–15.8] | 7.4 [5.8–9.1] | 8.6% | 0.0% | 1.1% | Smith//Score (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity), MAV (VSE\* WME\* sincerity) +31 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 40.6% |
| Supplementary vote | 36.8% |
| IRV-3 | 34.2% |
| TTR | 33.1% |
| IRV | 32.2% |
| ER-IRV | 32.1% |
| Approval | 30.3% |
| MAV | 30.2% |
| Geometric median | 30.1% |
| BTR-IRV | 30.0% |
| MJ | 29.9% |
| Approval cond. runoff | 29.9% |
| Soft Minimax-D | 29.7% |
| Approval runoff | 29.6% |
| BTR-score | 29.4% |
| Smith//Score | 29.2% |
| ESR | 29.2% |
| MMWV | 29.1% |
| Kemeny-Young | 29.0% |
| Copeland | 28.9% |
| MMPO | 28.9% |
| Schulze | 28.8% |
| MARS | 28.8% |
| Minimax | 28.8% |
| Ranked pairs | 28.8% |
| ARA | 28.6% |
| Soft Minimax | 28.5% |
| Soft Minimax-40 | 28.5% |
| STAR | 28.4% |
| Score | 28.4% |
| MARS-T | 28.4% |
| DMS | 28.3% |
| Score runoff | 28.1% |
| 321 | 27.6% |
| Borda | 27.2% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 977 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 23 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| MARS-T | 86.5% (86.6%) | 9.9% (10.6%) | 3.6% (2.8%) | 73.9% (52.2%) | 95.8% (96.5%) | 85.0% |
| Soft Minimax-40 | 87.2% (86.6%) | 10.5% (10.6%) | 2.3% (2.8%) | 65.2% (52.2%) | 94.6% (93.5%) | 83.8% |
| Soft Minimax-D | 87.0% (86.6%) | 10.1% (10.6%) | 2.9% (2.8%) | 52.2% (52.2%) | 98.9% (98.9%) | 87.1% |
| ARA | 86.6% (86.6%) | 9.9% (10.6%) | 3.5% (2.8%) | 65.2% (52.2%) | 92.1% (91.8%) | 81.7% |
| MARS | 86.6% (86.6%) | 10.7% (10.6%) | 2.7% (2.8%) | 60.9% (52.2%) | 97.2% (97.3%) | 85.3% |
| DMS | 87.0% (86.6%) | 10.7% (10.6%) | 2.3% (2.8%) | 56.5% (52.2%) | 88.1% (88.1%) | 79.0% |
| Smith//Score | 87.0% (86.6%) | 10.6% (10.6%) | 2.4% (2.8%) | 60.9% (52.2%) | 100.0% (100.0%) | 87.8% |
| Soft Minimax | 87.1% (86.6%) | 10.3% (10.6%) | 2.6% (2.8%) | 65.2% (52.2%) | 100.0% (100.0%) | 88.5% |
| STAR | 87.2% (86.6%) | 10.5% (10.6%) | 2.3% (2.8%) | 69.6% (52.2%) | 98.3% (98.9%) | 86.9% |
| Score | 86.1% (86.6%) | 10.5% (10.6%) | 3.4% (2.8%) | 65.2% (52.2%) | 89.0% (88.3%) | 79.9% |
| ESR | 87.5% (86.6%) | 9.9% (10.6%) | 2.6% (2.8%) | 52.2% (52.2%) | 98.0% (97.8%) | 87.5% |
| BTR-score | 87.1% (86.6%) | 10.3% (10.6%) | 2.6% (2.8%) | 69.6% (52.2%) | 100.0% (100.0%) | 88.2% |
| Borda | 98.0% (100.0%) | 1.1% (0.0%) | 0.9% (0.0%) | 56.5% (0.0%) | 88.3% (86.6%) | 87.7% |
| 321 | 76.8% (74.3%) | 19.9% (20.8%) | 3.4% (4.9%) | 100.0% (91.3%) | 96.6% (95.3%) | 75.9% |
| MMPO | 98.1% (100.0%) | 0.7% (0.0%) | 1.2% (0.0%) | 17.4% (0.0%) | 100.0% (100.0%) | 98.9% |
| Ranked pairs | 98.4% (100.0%) | 0.9% (0.0%) | 0.7% (0.0%) | 26.1% (0.0%) | 100.0% (100.0%) | 98.9% |
| Minimax | 98.2% (100.0%) | 0.9% (0.0%) | 0.9% (0.0%) | 26.1% (0.0%) | 100.0% (100.0%) | 98.8% |
| Kemeny-Young | 98.4% (100.0%) | 0.7% (0.0%) | 0.9% (0.0%) | 26.1% (0.0%) | 100.0% (100.0%) | 98.8% |
| MMWV | 98.1% (100.0%) | 0.7% (0.0%) | 1.2% (0.0%) | 17.4% (0.0%) | 100.0% (100.0%) | 99.0% |
| Schulze | 98.2% (100.0%) | 1.0% (0.0%) | 0.8% (0.0%) | 21.7% (0.0%) | 100.0% (100.0%) | 98.8% |
| Geometric median | 87.0% (86.6%) | 10.2% (10.6%) | 2.8% (2.8%) | 73.9% (52.2%) | 90.3% (87.8%) | 86.0% |
| Copeland | 98.0% (100.0%) | 0.7% (0.0%) | 1.3% (0.0%) | 21.7% (0.0%) | 100.0% (100.0%) | 98.4% |
| BTR-IRV | 97.5% (100.0%) | 1.5% (0.0%) | 0.9% (0.0%) | 30.4% (0.0%) | 100.0% (100.0%) | 98.0% |
| MJ | 86.3% (86.6%) | 10.7% (10.6%) | 3.0% (2.8%) | 69.6% (52.2%) | 85.8% (81.7%) | 81.6% |
| MAV | 86.8% (86.6%) | 10.0% (10.6%) | 3.2% (2.8%) | 60.9% (52.2%) | 84.0% (77.6%) | 80.7% |
| ER-IRV | 96.5% (100.0%) | 2.4% (0.0%) | 1.1% (0.0%) | 43.5% (0.0%) | 89.1% (84.8%) | 88.7% |
| IRV | 96.7% (100.0%) | 2.5% (0.0%) | 0.8% (0.0%) | 39.1% (0.0%) | 89.9% (84.8%) | 89.4% |
| IRV-3 | 89.8% (89.9%) | 5.6% (5.6%) | 4.6% (4.5%) | 39.1% (34.8%) | 94.7% (90.5%) | 86.8% |
| Supplementary vote | 94.6% (100.0%) | 4.1% (0.0%) | 1.3% (0.0%) | 39.1% (0.0%) | 79.5% (71.9%) | 76.8% |

![strategy arrows](arrows-c5.png)

![strategy shifts](shift-c5.png)

![sincerity](sincerity-c5.png)

![fidelity](fidelity-c5.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c5.png)

Scatter and chain plots leave out Random pair runoff, Anti-plurality, TRASH, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
