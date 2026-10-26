# Ballot Front Finder Lab — analysis

Input: base, hard (simulator version 1.0.0; analysis version 1.0.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

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

![sweep over candidate dispersion + laziness + exits + clones + poll size](chain-cand_dispersion-lazy-exits-clones-poll_size.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis). A condition near or above that row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 0.99 | 1.00 | 0.97 | 0.99 | 0.99 |
| c2: candidate dispersion 0.5, laziness 0.1, exits 1, clones 2, poll size 9 | 0.90 | 0.96 | 0.76 | 0.95 | 0.89 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 97.6% | 68.6% | 16.2% | 8.0 |
| c2: candidate dispersion 0.5, laziness 0.1, exits 1, clones 2, poll size 9 | 92.7% | 54.8% | 17.5% | 3.2 |

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

## c2: candidate dispersion 0.5, laziness 0.1, exits 1, clones 2, poll size 9

1000 elections (seeds 1), 927 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7, candidates: dispersion 0.5; 3 poll round(s), 10 polls of 9 voters, poll error 2.0x, search 10, aggressive share 0.5, laziness 0.1, strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 92.7%; it also has the highest total utility in 54.8% of those; electing it loses 17.5% of VSE; favourite ÷ least liked 3.2 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 80.5 to 81.8; with the best VSE (100) WME 68.9 (nothing lower is possible there either); the most balanced point VSE 92.1 / WME 95.8. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 89.9 (at WME 86.8); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -26.8 / WME -19.1.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, ESR, STAR, MARS, Soft Minimax-D, Soft Minimax, Minimax, Schulze, MMPO, MMWV.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Geometric median, Soft Minimax-D, Score runoff, Soft Minimax, BTR-score, MMPO, Ranked pairs, Kemeny-Young, MMWV.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 87.0 [85.2–88.8] | 82.8 [80.7–85.0] | 99.9 [99.9–100.0] | 89.8 | 94.6 | 88.3 [86.6–90.0] | 87.1 [85.2–88.9] | 76.1 [73.4–78.7] | 70.4 [67.6–73.2] | 6.4 [4.9–7.9] | 8.2 [6.6–9.8] | 0.0% | 0.8% | 0.0% | — |
| DMS | rated | 85.1 [83.3–87.1] | 82.6 [80.5–84.8] | 99.8 [99.7–99.8] | 89.1 | 94.5 | 86.9 [85.2–88.6] | 89.0 [87.3–90.6] | 77.0 [74.2–79.7] | 70.2 [67.3–73.2] | 6.3 [4.8–7.8] | 6.9 [5.3–8.4] | 0.0% | 2.7% | 0.0% | Score (WME\*), ESR (WME\*), Geometric median (VSE\*) +9 more |
| ESR | rated | 85.1 [83.1–86.9] | 88.2 [86.6–90.0] | 99.8 [99.7–99.8] | 89.4 | 94.4 | 85.8 [84.1–87.7] | 92.8 [91.4–94.1] | 84.1 [81.8–86.5] | 77.3 [74.7–80.0] | 6.4 [4.9–7.8] | 6.7 [5.2–8.3] | 0.2% | 2.7% | 0.0% | STAR (WME\*), Score runoff (WME\*), BTR-score (WME\*) +4 more |
| STAR | hybrid | 85.0 [83.1–87.0] | 90.6 [89.1–92.2] | 99.8 [99.7–99.8] | 89.6 | 94.3 | 85.0 [83.2–86.9] | 92.9 [91.6–94.1] | 83.7 [81.3–86.0] | 80.5 [78.1–83.0] | 6.4 [4.9–7.8] | 6.8 [5.2–8.3] | 0.2% | 2.9% | 0.0% | Soft Minimax (WME\*) |
| MAV | median-based | 85.0 [83.2–86.9] | 79.4 [76.9–81.8] | 99.8 [99.7–99.8] | 89.5 | 94.7 | 86.7 [85.0–88.5] | 84.6 [82.6–86.6] | 72.6 [69.7–75.2] | 67.8 [64.9–70.7] | 6.4 [4.8–7.9] | 7.6 [5.9–9.2] | 0.1% | 2.7% | 0.0% | Geometric median (VSE\* WME\*), Approval runoff (WME\*), Score (WME\*) +12 more |
| MARS | hybrid | 84.9 [82.8–86.9] | 91.1 [89.6–92.7] | 99.8 [99.7–99.8] | 89.4 | 94.3 | 85.0 [83.3–86.8] | 95.3 [94.1–96.3] | 88.1 [86.1–90.1] | 81.1 [78.7–83.6] | 5.9 [4.4–7.3] | 6.4 [4.9–8.0] | 0.1% | 2.7% | 0.0% | Soft Minimax (WME\*) |
| Soft Minimax-D | hybrid | 84.8 [83.0–86.7] | 92.1 [90.6–93.4] | 99.8 [99.8–99.9] | 89.9 | 94.3 | 84.0 [82.1–86.0] | 95.6 [94.4–96.5] | 88.9 [87.0–90.9] | 81.6 [79.1–84.0] | 6.4 [4.9–8.0] | 6.6 [5.1–8.1] | 0.1% | 2.2% | 0.0% | — |
| MARS-T | hybrid | 84.8 [82.9–86.7] | 89.7 [88.1–91.4] | 99.8 [99.7–99.8] | 89.2 | 94.3 | 85.4 [83.7–87.2] | 94.7 [93.4–95.7] | 87.2 [85.0–89.2] | 79.5 [77.2–82.2] | 5.9 [4.4–7.3] | 6.7 [5.2–8.3] | 0.1% | 2.6% | 0.0% | Smith//Score (WME\*), Score runoff (WME\*), BTR-score (WME\*) +2 more |
| Score runoff | two-round | 84.6 [82.7–86.7] | 91.7 [90.3–93.2] | 99.7 [99.7–99.8] | 88.9 | 94.2 | 85.6 [83.9–87.4] | 92.9 [91.6–94.2] | 84.4 [82.0–86.8] | 82.6 [80.2–84.8] | 6.4 [5.0–7.9] | 7.1 [5.5–8.6] | 0.3% | 2.9% | 0.0% | — |
| MJ | median-based | 84.4 [82.3–86.5] | 81.4 [79.0–83.5] | 99.8 [99.7–99.8] | 89.4 | 94.5 | 85.0 [83.0–86.8] | 86.3 [84.5–88.2] | 74.8 [71.9–77.5] | 68.4 [65.4–71.2] | 6.9 [5.3–8.4] | 7.3 [5.8–8.8] | 0.1% | 2.8% | 0.0% | Smith//Score (WME\*), Score (WME\*), ESR (WME\*) +10 more |
| Score | rated | 84.2 [82.4–86.1] | 85.2 [83.2–87.2] | 99.8 [99.7–99.8] | 88.5 | 94.3 | 85.0 [83.2–86.8] | 91.4 [90.0–92.7] | 80.6 [78.2–83.2] | 73.1 [70.4–75.9] | 6.9 [5.3–8.4] | 7.5 [5.9–9.2] | 0.0% | 3.2% | 0.0% | Smith//Score (WME\*), ESR (WME\*), Geometric median (VSE\*) +9 more |
| Soft Minimax | hybrid | 84.2 [82.3–85.9] | 92.9 [91.5–94.2] | 99.7 [99.7–99.7] | 89.9 | 94.2 | 83.2 [81.2–85.2] | 96.2 [95.1–97.0] | 90.5 [88.7–92.4] | 83.5 [81.3–85.7] | 6.3 [4.9–7.8] | 6.7 [5.2–8.2] | 0.3% | 2.8% | 0.0% | — |
| ARA | hybrid | 83.9 [82.0–85.8] | 90.5 [88.9–92.0] | 99.7 [99.7–99.8] | 89.5 | 94.4 | 84.3 [82.5–86.2] | 93.4 [92.1–94.6] | 85.1 [82.9–87.5] | 80.3 [78.0–82.8] | 6.2 [4.7–7.6] | 7.1 [5.5–8.7] | 0.2% | 3.0% | 0.0% | BTR-score (WME\*), Soft Minimax (WME\*) |
| 321 | hybrid | 83.8 [81.7–85.7] | 83.7 [81.6–85.9] | 99.6 [99.6–99.7] | 82.4 | 86.7 | 84.8 [83.0–86.7] | 86.9 [85.0–88.7] | 76.8 [73.9–79.4] | 73.4 [70.6–76.1] | 6.5 [5.0–8.0] | 7.6 [6.0–9.2] | 0.2% | 4.3% | 0.0% | MMPO (WME\*), Smith//Score (WME\*), ESR (WME\*) +10 more |
| Approval runoff | two-round | 83.7 [81.5–85.8] | 83.6 [81.5–85.6] | 99.2 [99.1–99.3] | 67.4 | 71.2 | 84.5 [82.5–86.6] | 83.7 [81.5–85.8] | 74.3 [71.6–77.1] | 75.3 [72.6–77.9] | 8.2 [6.4–9.8] | 8.6 [6.8–10.4] | 0.5% | 5.3% | 0.1% | Ranked pairs (WME\*), MMPO (WME\*), Smith//Score (WME\*) +11 more |
| BTR-score | hybrid | 83.6 [81.7–85.6] | 92.3 [90.9–93.6] | 99.7 [99.7–99.7] | 89.6 | 94.3 | 83.7 [81.7–85.7] | 95.9 [94.9–96.8] | 90.4 [88.5–92.3] | 84.5 [82.4–86.8] | 6.2 [4.7–7.7] | 6.4 [5.0–7.9] | 0.2% | 3.0% | 0.0% | — |
| Soft Minimax-40 | hybrid | 83.4 [81.3–85.5] | 90.8 [89.2–92.3] | 99.7 [99.7–99.7] | 89.5 | 94.3 | 84.7 [82.8–86.6] | 94.2 [93.0–95.3] | 86.9 [84.7–89.0] | 82.2 [79.9–84.6] | 5.9 [4.4–7.3] | 7.1 [5.5–8.7] | 0.2% | 3.4% | 0.0% | STAR (VSE\*), Soft Minimax (WME\*), Soft Minimax-D (VSE\*) |
| Smith//Score | condorcet-based | 83.2 [81.1–85.3] | 91.8 [90.4–93.2] | 99.8 [99.8–99.8] | 89.8 | 94.3 | 83.4 [81.4–85.4] | 95.6 [94.4–96.6] | 89.8 [87.8–91.6] | 82.7 [80.3–85.2] | 6.4 [4.9–7.8] | 6.8 [5.3–8.3] | 0.1% | 2.2% | 0.0% | STAR (VSE\*), Soft Minimax-D (VSE\*) |
| Approval cond. runoff | two-round | 82.9 [80.7–85.0] | 79.7 [77.3–82.1] | 99.4 [99.3–99.4] | 67.8 | 71.5 | 84.2 [82.2–86.3] | 83.6 [81.4–85.7] | 74.3 [71.4–77.1] | 70.9 [68.1–74.0] | 8.1 [6.4–9.7] | 8.4 [6.8–10.1] | 0.2% | 5.6% | 0.0% | DMS (VSE\* WME\*), ESR (VSE\* WME\*), Geometric median (VSE\* WME\*) +15 more |
| ER-IRV | plurality-based | 82.6 [80.3–84.7] | 82.9 [80.8–85.2] | 98.3 [98.2–98.4] | 78.4 | 82.5 | 82.6 [80.2–84.8] | 85.5 [83.5–87.5] | 78.1 [75.7–80.5] | 73.3 [70.5–76.1] | 8.1 [6.4–9.8] | 8.0 [6.3–9.6] | 1.8% | 13.3% | 0.6% | ESR (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity), Score runoff (VSE\* WME\* sincerity) +20 more |
| Minimax | minimax-family | 82.6 [80.5–84.5] | 94.1 [92.9–95.2] | 98.2 [98.1–98.3] | 78.5 | 81.8 | 82.2 [80.1–84.2] | 98.4 [97.8–98.8] | 95.1 [93.6–96.4] | 85.8 [83.5–87.8] | 6.6 [5.1–8.1] | 7.2 [5.6–8.8] | 6.5% | 9.2% | 2.0% | Soft Minimax (VSE\* sincerity), MMWV (WME\*) |
| Schulze | condorcet | 82.1 [79.9–84.1] | 94.2 [93.0–95.3] | 98.2 [98.1–98.3] | 78.6 | 81.8 | 82.2 [80.2–84.3] | 98.3 [97.8–98.8] | 95.1 [93.6–96.4] | 86.1 [83.8–88.2] | 6.5 [5.0–8.1] | 7.0 [5.4–8.6] | 6.6% | 9.1% | 2.0% | Soft Minimax (VSE\* sincerity), MMWV (WME\*) |
| MMPO | minimax-family | 82.0 [80.0–83.9] | 95.5 [94.5–96.4] | 98.7 [98.6–98.7] | 78.3 | 81.7 | 82.0 [79.9–84.1] | 97.9 [97.3–98.4] | 94.6 [93.3–95.9] | 89.5 [87.5–91.4] | 7.0 [5.4–8.6] | 6.9 [5.5–8.5] | 0.5% | 14.4% | 0.2% | — |
| Ranked pairs | condorcet | 81.9 [79.9–83.9] | 94.7 [93.6–95.7] | 98.2 [98.2–98.3] | 78.6 | 81.7 | 82.1 [80.1–84.2] | 98.3 [97.7–98.8] | 95.1 [93.6–96.4] | 85.9 [83.5–88.0] | 6.6 [5.1–8.2] | 7.2 [5.6–8.8] | 6.7% | 8.9% | 1.9% | — |
| Kemeny-Young | condorcet | 81.8 [79.7–83.8] | 94.9 [93.8–95.8] | 98.2 [98.1–98.3] | 78.6 | 81.7 | 82.0 [79.9–84.1] | 98.2 [97.6–98.7] | 95.1 [93.6–96.4] | 86.8 [84.4–88.9] | 6.9 [5.3–8.5] | 7.6 [5.9–9.4] | 6.5% | 9.3% | 1.9% | — |
| Copeland | condorcet | 81.4 [79.2–83.5] | 92.9 [91.5–94.2] | 98.1 [98.1–98.2] | 77.9 | 81.4 | 81.7 [79.7–83.8] | 97.7 [97.0–98.2] | 94.7 [93.2–96.0] | 84.3 [82.1–86.6] | 6.6 [5.1–8.1] | 6.7 [5.1–8.2] | 7.0% | 9.2% | 2.0% | Smith//Score (VSE\* sincerity), Score runoff (VSE\* sincerity), BTR-score (VSE\* sincerity) +6 more |
| MMWV | minimax-family | 81.1 [79.0–83.2] | 95.8 [94.8–96.7] | 98.8 [98.7–98.8] | 78.6 | 81.7 | 82.0 [79.9–84.0] | 97.9 [97.2–98.5] | 95.0 [93.6–96.3] | 88.7 [86.7–90.7] | 7.0 [5.4–8.5] | 7.7 [6.1–9.4] | 0.5% | 13.5% | 0.2% | — |
| IRV | plurality-based | 80.7 [78.2–83.1] | 81.2 [78.9–83.7] | 97.5 [97.4–97.6] | 78.5 | 82.5 | 82.6 [80.2–84.8] | 85.5 [83.5–87.5] | 78.1 [75.7–80.5] | 71.3 [68.6–74.1] | 8.1 [6.4–9.8] | 8.9 [7.0–10.6] | 6.4% | 9.5% | 2.9% | Smith//Score (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity), ESR (VSE\* WME\* sincerity) +25 more |
| BTR-IRV | condorcet-based | 80.4 [78.1–82.4] | 91.9 [90.5–93.1] | 98.0 [97.9–98.1] | 78.1 | 81.7 | 81.7 [79.6–83.8] | 97.5 [96.8–98.1] | 94.6 [93.1–96.0] | 84.3 [82.1–86.5] | 6.9 [5.4–8.4] | 7.8 [6.1–9.4] | 6.6% | 9.5% | 2.3% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +12 more |
| Approval | approval-based | 79.7 [77.3–82.3] | 73.5 [70.7–75.9] | 99.6 [99.5–99.6] | 69.7 | 72.9 | 83.4 [81.0–85.7] | 78.9 [76.4–81.3] | 66.9 [64.0–69.9] | 62.7 [59.7–65.8] | 10.2 [8.3–12.0] | 12.4 [10.3–14.3] | 0.0% | 4.4% | 0.0% | MMPO (VSE\* WME\*), Smith//Score (VSE\* WME\*), Approval runoff (VSE\* WME\*) +18 more |
| Supplementary vote | plurality-based | 78.8 [76.3–81.3] | 68.5 [65.5–71.6] | 97.8 [97.7–97.9] | 78.5 | 82.5 | 81.6 [79.1–84.0] | 72.6 [69.8–75.3] | 61.9 [58.8–65.0] | 58.7 [55.7–61.8] | 10.8 [8.8–12.6] | 11.8 [9.9–13.6] | 6.5% | 11.4% | 2.4% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +29 more |
| IRV-3 | plurality-based | 78.6 [75.9–81.2] | 76.6 [74.0–79.2] | 98.0 [97.9–98.1] | 75.1 | 78.5 | 82.9 [80.6–85.0] | 82.6 [80.4–84.9] | 73.9 [71.3–76.7] | 67.3 [64.3–70.2] | 8.2 [6.5–9.9] | 10.6 [8.6–12.6] | 6.1% | 5.4% | 2.9% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +28 more |
| TTR | two-round | 78.0 [75.7–80.6] | 77.5 [74.6–80.3] | 95.8 [95.5–96.1] | 58.0 | 61.8 | 80.9 [78.5–83.1] | 82.5 [80.1–84.8] | 76.0 [73.4–78.7] | 72.3 [69.5–75.4] | 9.2 [7.5–10.8] | 11.0 [9.1–12.8] | 9.0% | 0.0% | 5.0% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), Kemeny-Young (VSE\* WME\* sincerity) +28 more |
| Borda | positional | 77.6 [74.8–80.0] | 88.7 [87.1–90.3] | 98.2 [98.2–98.3] | 77.3 | 81.1 | 74.8 [72.0–77.5] | 92.1 [90.7–93.5] | 80.8 [78.3–83.3] | 76.4 [73.6–79.0] | 8.3 [6.6–10.1] | 7.8 [6.1–9.5] | 6.9% | 8.7% | 1.7% | Smith//Score (VSE\* WME\* sincerity), Score runoff (VSE\* WME\* sincerity), BTR-score (VSE\* WME\* sincerity) +16 more |
| Plurality | plurality-based | 62.3 [57.8–66.3] | 50.7 [47.1–54.3] | 97.0 [96.8–97.2] | 58.9 | 61.9 | 66.4 [62.3–70.3] | 54.9 [51.4–58.5] | 45.8 [42.6–49.2] | 43.8 [40.7–47.0] | 19.5 [17.1–22.1] | 20.6 [18.2–23.1] | 11.1% | 0.0% | 3.6% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), Kemeny-Young (VSE\* WME\* sincerity) +30 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Supplementary vote | 60.2% |
| Plurality | 58.5% |
| IRV-3 | 54.9% |
| Approval | 53.5% |
| MAV | 52.9% |
| TTR | 52.7% |
| MJ | 51.3% |
| DMS | 51.2% |
| Approval runoff | 51.0% |
| Approval cond. runoff | 51.0% |
| IRV | 50.9% |
| ESR | 50.7% |
| ER-IRV | 50.4% |
| Soft Minimax-40 | 50.0% |
| Geometric median | 49.3% |
| BTR-score | 48.9% |
| Smith//Score | 48.8% |
| Soft Minimax | 48.6% |
| Score | 48.6% |
| Soft Minimax-D | 48.5% |
| MARS-T | 48.4% |
| Minimax | 48.3% |
| Schulze | 48.2% |
| MARS | 48.2% |
| BTR-IRV | 48.1% |
| 321 | 48.0% |
| ARA | 47.9% |
| Ranked pairs | 47.6% |
| Kemeny-Young | 47.3% |
| Score runoff | 46.5% |
| MMWV | 46.4% |
| STAR | 46.4% |
| MMPO | 46.3% |
| Copeland | 46.2% |
| Borda | 41.7% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Plurality | 45.1% | 1.35 | 1.13 | 92% |
| TTR | 42.7% | 0.94 | 0.82 | 98% |
| Supplementary vote | 40.3% | 1.53 | 0.82 | 98% |
| IRV | 34.2% | 0.58 | 0.16 | 99% |
| ER-IRV | 34.2% | 0.58 | 0.16 | 99% |
| IRV-3 | 34.2% | 1.08 | 0.52 | 99% |
| Approval | 34.2% | 1.03 | 0.46 | 100% |
| Approval runoff | 33.9% | 0.77 | 0.66 | 100% |
| Approval cond. runoff | 32.0% | 0.83 | 0.67 | 100% |
| MAV | 29.6% | 0.70 | 0.18 | 100% |
| 321 | 28.2% | 0.66 | 0.59 | 100% |
| DMS | 27.1% | 0.55 | 0.15 | 100% |
| MJ | 24.1% | 0.58 | 0.17 | 99% |
| Geometric median | 20.0% | 0.56 | 0.21 | 100% |
| Score runoff | 19.9% | 0.32 | 0.38 | 100% |
| Score | 19.6% | 0.41 | 0.18 | 100% |
| ARA | 19.4% | 0.44 | 0.22 | 99% |
| STAR | 18.9% | 0.42 | 0.34 | 100% |
| Soft Minimax-40 | 18.9% | 0.39 | 0.19 | 100% |
| ESR | 17.3% | 0.44 | 0.17 | 100% |
| BTR-score | 16.0% | 0.37 | 0.23 | 100% |
| MARS | 14.8% | 0.34 | 0.17 | 100% |
| Soft Minimax | 14.8% | 0.32 | 0.21 | 100% |
| MARS-T | 13.9% | 0.32 | 0.13 | 100% |
| Smith//Score | 13.5% | 0.32 | 0.15 | 100% |
| Copeland | 13.2% | 0.18 | 0.25 | 100% |
| Soft Minimax-D | 12.7% | 0.31 | 0.16 | 100% |
| Borda | 12.4% | 0.16 | 0.46 | 100% |
| BTR-IRV | 11.6% | 0.23 | 0.18 | 100% |
| MMPO | 10.7% | 0.19 | 0.16 | 100% |
| MMWV | 9.3% | 0.17 | 0.14 | 100% |
| Kemeny-Young | 8.9% | 0.17 | 0.15 | 99% |
| Ranked pairs | 8.8% | 0.18 | 0.14 | 100% |
| Schulze | 8.3% | 0.17 | 0.12 | 100% |
| Minimax | 8.2% | 0.17 | 0.12 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 927 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 73 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 78.2% (82.2%) | 15.0% (13.2%) | 6.8% (4.6%) | 69.9% (54.8%) | 84.5% (87.4%) | 70.4% |
| DMS | 78.2% (82.5%) | 14.9% (11.7%) | 6.9% (5.8%) | 64.4% (53.4%) | 82.9% (85.4%) | 70.2% |
| ESR | 79.6% (84.0%) | 15.1% (11.3%) | 5.3% (4.6%) | 76.7% (61.6%) | 93.1% (95.6%) | 77.3% |
| STAR | 81.6% (84.3%) | 12.7% (10.2%) | 5.7% (5.5%) | 82.2% (72.6%) | 95.7% (92.0%) | 80.5% |
| MAV | 76.5% (81.7%) | 16.4% (13.1%) | 7.1% (5.3%) | 69.9% (49.3%) | 82.5% (83.1%) | 67.8% |
| MARS | 79.8% (86.0%) | 14.2% (9.2%) | 5.9% (4.9%) | 63.0% (63.0%) | 95.0% (96.7%) | 81.1% |
| Soft Minimax-D | 79.7% (87.7%) | 13.5% (8.8%) | 6.8% (3.5%) | 75.3% (64.4%) | 98.5% (98.9%) | 81.6% |
| MARS-T | 80.0% (85.2%) | 13.3% (9.8%) | 6.7% (5.0%) | 75.3% (63.0%) | 93.3% (95.9%) | 79.5% |
| MJ | 77.5% (81.7%) | 17.2% (12.9%) | 5.4% (5.4%) | 74.0% (60.3%) | 79.5% (83.0%) | 68.4% |
| Score | 77.3% (83.1%) | 16.0% (11.3%) | 6.7% (5.6%) | 69.9% (57.5%) | 87.1% (88.5%) | 73.1% |
| Soft Minimax | 80.3% (88.1%) | 14.1% (8.3%) | 5.6% (3.6%) | 64.4% (64.4%) | 100.0% (100.0%) | 83.5% |
| ARA | 80.6% (84.8%) | 13.8% (9.8%) | 5.6% (5.4%) | 67.1% (58.9%) | 91.1% (91.7%) | 80.3% |
| 321 | 73.1% (72.9%) | 20.7% (17.2%) | 6.1% (9.9%) | 84.9% (82.2%) | 96.6% (95.5%) | 73.4% |
| BTR-score | 81.9% (88.2%) | 12.5% (8.4%) | 5.6% (3.3%) | 75.3% (63.0%) | 100.0% (100.0%) | 84.5% |
| Soft Minimax-40 | 81.8% (85.9%) | 12.4% (10.2%) | 5.8% (3.9%) | 71.2% (64.4%) | 93.8% (94.6%) | 82.2% |
| Smith//Score | 80.7% (87.1%) | 13.2% (8.6%) | 6.1% (4.3%) | 76.7% (67.1%) | 100.0% (100.0%) | 82.7% |
| ER-IRV | 80.8% (91.6%) | 11.3% (5.0%) | 7.9% (3.5%) | 57.5% (42.5%) | 82.8% (84.0%) | 73.3% |
| Minimax | 82.5% (92.6%) | 11.8% (3.7%) | 5.7% (3.8%) | 57.5% (37.0%) | 100.0% (100.0%) | 85.8% |
| Schulze | 83.3% (92.6%) | 11.3% (3.7%) | 5.4% (3.8%) | 60.3% (35.6%) | 100.0% (100.0%) | 86.1% |
| MMPO | 85.3% (92.1%) | 7.4% (3.8%) | 7.2% (4.1%) | 50.7% (38.4%) | 99.2% (99.2%) | 89.5% |
| Ranked pairs | 83.3% (92.4%) | 11.0% (3.7%) | 5.7% (3.9%) | 57.5% (34.2%) | 100.0% (100.0%) | 85.9% |
| Kemeny-Young | 83.6% (92.4%) | 10.0% (3.7%) | 6.4% (3.9%) | 58.9% (32.9%) | 100.0% (100.0%) | 86.8% |
| Copeland | 80.7% (91.4%) | 11.0% (3.8%) | 8.3% (4.9%) | 64.4% (50.7%) | 100.0% (100.0%) | 84.3% |
| MMWV | 84.5% (91.9%) | 8.6% (3.8%) | 6.9% (4.3%) | 53.4% (39.7%) | 100.0% (100.0%) | 88.7% |
| IRV | 77.7% (91.6%) | 15.3% (5.0%) | 7.0% (3.5%) | 60.3% (42.5%) | 81.6% (84.0%) | 71.3% |
| BTR-IRV | 81.0% (92.8%) | 11.7% (3.8%) | 7.3% (3.5%) | 75.3% (64.4%) | 100.0% (100.0%) | 84.3% |
| Supplementary vote | 70.4% (78.4%) | 23.4% (18.3%) | 6.1% (3.2%) | 69.9% (56.2%) | 77.1% (79.2%) | 58.7% |
| IRV-3 | 68.9% (74.4%) | 22.7% (19.3%) | 8.4% (6.3%) | 69.9% (71.2%) | 93.0% (95.5%) | 67.3% |
| Borda | 81.2% (93.1%) | 10.2% (2.7%) | 8.5% (4.2%) | 50.7% (23.3%) | 83.1% (80.8%) | 76.4% |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

Scatter and chain plots leave out Random pair runoff, Anti-plurality, TRASH, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
