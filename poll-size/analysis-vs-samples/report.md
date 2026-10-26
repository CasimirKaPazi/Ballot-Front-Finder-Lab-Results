# Ballot Front Finder Lab — analysis

Input: samples, poll-size (simulator version 1.0.0; analysis version 1.0.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

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

![sweep over poll size](chain-poll_size.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis). A condition near or above that row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 0.97 | 1.00 | 0.94 | 0.98 | 0.97 |
| c2: poll size 9 | 1.00 | 1.00 | 0.93 | 0.97 | 0.89 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 97.7% | 69.5% | 15.1% | 7.9 |
| c2: poll size 9 | 97.7% | 69.5% | 15.1% | 7.9 |

## c1: base

1000 elections (seeds 1), 977 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 97.7%; it also has the highest total utility in 69.5% of those; electing it loses 15.1% of VSE; favourite ÷ least liked 7.9 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 84.4 to 84.7; with the best VSE (100) WME 76.9 (nothing lower is possible there either); the most balanced point VSE 93.9 / WME 96.1. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 90.2 (at WME 92.5); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -58.7 / WME -53.2.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, ESR, ARA, Soft Minimax-40, MARS-T, Soft Minimax-D, BTR-score, Soft Minimax, Score runoff, MMPO, MMWV, Kemeny-Young.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): ESR, Soft Minimax, Score runoff, MMPO, MMWV, Ranked pairs, Minimax, Kemeny-Young, Schulze.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 89.8 [88.0–91.7] | 90.7 [89.1–92.4] | 100.0 [99.9–100.0] | 90.4 | 94.1 | 90.7 [88.9–92.3] | 90.4 [88.7–91.9] | 81.0 [78.4–83.5] | 81.5 [78.9–84.0] | 2.7 [1.7–3.8] | 3.2 [2.1–4.3] | 0.0% | 0.7% | 0.0% | ESR (WME\*), ARA (WME\*), Soft Minimax-40 (WME\*) |
| ESR | rated | 89.0 [87.0–90.8] | 93.9 [92.5–95.1] | 99.9 [99.9–100.0] | 90.3 | 94.1 | 89.2 [87.5–90.9] | 93.8 [92.3–95.0] | 87.0 [84.9–89.1] | 86.4 [84.2–88.7] | 3.0 [2.0–4.1] | 3.0 [2.0–4.1] | 0.1% | 0.7% | 0.0% | — |
| DMS | rated | 89.0 [86.9–90.8] | 90.4 [88.7–92.0] | 100.0 [99.9–100.0] | 90.1 | 94.1 | 90.1 [88.2–91.8] | 89.6 [87.8–91.3] | 80.7 [78.1–83.1] | 81.1 [78.7–83.5] | 3.4 [2.3–4.6] | 3.9 [2.7–5.2] | 0.0% | 0.7% | 0.0% | Score (WME\*), ESR (WME\*), ARA (WME\*) +2 more |
| ARA | hybrid | 88.7 [86.8–90.7] | 94.4 [93.1–95.5] | 100.0 [99.9–100.0] | 90.3 | 94.1 | 89.3 [87.4–91.0] | 94.7 [93.5–95.9] | 87.5 [85.4–89.6] | 86.8 [84.8–88.9] | 2.5 [1.6–3.5] | 2.7 [1.7–3.8] | 0.0% | 0.7% | 0.0% | MARS (WME\*), Soft Minimax-D (WME\*) |
| Soft Minimax-40 | hybrid | 88.5 [86.5–90.3] | 94.5 [93.3–95.5] | 100.0 [99.9–100.0] | 90.4 | 94.1 | 88.8 [86.8–90.6] | 94.5 [93.3–95.6] | 86.7 [84.5–88.7] | 86.5 [84.3–88.6] | 3.1 [2.0–4.3] | 3.3 [2.3–4.5] | 0.1% | 0.6% | 0.0% | Soft Minimax-D (WME\*) |
| Score | rated | 88.3 [86.4–90.1] | 91.6 [90.1–93.0] | 100.0 [99.9–100.0] | 90.0 | 94.1 | 89.9 [88.2–91.6] | 92.8 [91.3–94.1] | 84.7 [82.4–86.9] | 82.6 [80.4–85.0] | 3.0 [2.0–4.2] | 3.1 [2.1–4.2] | 0.0% | 0.6% | 0.0% | ESR (WME\*), STAR (WME\*), BTR-score (WME\*) +5 more |
| 321 | hybrid | 88.2 [86.2–90.2] | 90.4 [88.7–92.0] | 99.9 [99.9–99.9] | 83.1 | 85.8 | 88.2 [86.2–90.1] | 89.6 [87.9–91.3] | 80.9 [78.2–83.2] | 82.1 [79.6–84.4] | 2.7 [1.7–3.7] | 3.0 [1.9–4.1] | 0.1% | 1.1% | 0.0% | Smith//Score (WME\*), ESR (WME\*), STAR (WME\*) +6 more |
| MAV | median-based | 88.1 [86.0–90.1] | 90.1 [88.4–91.8] | 100.0 [99.9–100.0] | 90.1 | 94.1 | 88.6 [86.6–90.5] | 87.7 [85.8–89.5] | 77.9 [75.4–80.5] | 81.1 [78.5–83.4] | 2.7 [1.7–3.8] | 3.0 [2.0–4.1] | 0.0% | 0.7% | 0.0% | Smith//Score (WME\*), ESR (WME\*), Geometric median (VSE\*) +8 more |
| MARS-T | hybrid | 87.4 [85.3–89.3] | 94.9 [93.8–96.0] | 100.0 [99.9–100.0] | 90.3 | 94.1 | 88.0 [86.0–89.9] | 95.8 [94.7–96.8] | 89.3 [87.1–91.2] | 87.8 [85.8–89.8] | 3.1 [2.0–4.2] | 3.2 [2.1–4.4] | 0.0% | 0.6% | 0.0% | BTR-score (WME\*), Soft Minimax (WME\*) |
| Soft Minimax-D | hybrid | 87.4 [85.3–89.4] | 95.8 [94.7–96.8] | 100.0 [100.0–100.0] | 90.5 | 94.1 | 87.4 [85.3–89.4] | 96.2 [95.0–97.1] | 89.8 [87.8–91.8] | 89.5 [87.5–91.5] | 3.4 [2.3–4.6] | 3.5 [2.4–4.7] | 0.0% | 0.5% | 0.0% | Soft Minimax (WME\*) |
| MARS | hybrid | 87.2 [85.0–89.2] | 95.5 [94.4–96.6] | 100.0 [99.9–100.0] | 90.3 | 94.1 | 88.0 [86.0–89.8] | 95.9 [94.8–96.8] | 89.6 [87.6–91.6] | 88.5 [86.5–90.6] | 3.1 [2.0–4.2] | 3.5 [2.3–4.7] | 0.0% | 0.5% | 0.0% | Soft Minimax (WME\*) |
| MJ | median-based | 87.0 [84.8–88.9] | 90.5 [88.8–92.1] | 100.0 [99.9–100.0] | 90.1 | 94.1 | 88.0 [86.0–90.0] | 89.9 [88.2–91.6] | 81.0 [78.5–83.2] | 81.4 [78.8–83.7] | 2.2 [1.3–3.0] | 2.7 [1.8–3.7] | 0.0% | 0.7% | 0.0% | ESR (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*), Smith//Score (WME\*) +12 more |
| BTR-score | hybrid | 86.7 [84.7–88.5] | 95.9 [94.9–96.8] | 100.0 [99.9–100.0] | 90.4 | 94.1 | 87.1 [85.0–89.0] | 96.4 [95.5–97.3] | 90.6 [88.7–92.4] | 89.8 [87.8–91.6] | 3.4 [2.3–4.6] | 3.6 [2.4–4.7] | 0.1% | 0.6% | 0.0% | Soft Minimax (WME\*) |
| Soft Minimax | hybrid | 86.6 [84.5–88.8] | 96.7 [95.8–97.6] | 100.0 [99.9–100.0] | 90.5 | 94.1 | 87.1 [85.0–89.0] | 96.6 [95.6–97.4] | 90.7 [88.9–92.6] | 91.0 [89.2–92.9] | 3.3 [2.2–4.5] | 3.4 [2.3–4.6] | 0.1% | 0.5% | 0.0% | — |
| Smith//Score | condorcet-based | 86.6 [84.3–88.6] | 95.8 [94.6–96.8] | 100.0 [99.9–100.0] | 90.4 | 94.1 | 87.3 [85.2–89.2] | 96.4 [95.4–97.3] | 90.5 [88.6–92.3] | 89.6 [87.6–91.5] | 3.3 [2.2–4.5] | 3.9 [2.7–5.2] | 0.1% | 0.6% | 0.0% | Score runoff (WME\*), Soft Minimax (WME\*) |
| STAR | hybrid | 86.5 [84.0–88.5] | 95.3 [94.1–96.3] | 99.9 [99.9–100.0] | 90.3 | 94.1 | 88.2 [86.3–90.0] | 95.8 [94.7–96.8] | 89.4 [87.4–91.4] | 88.9 [86.9–91.0] | 2.7 [1.7–3.7] | 3.2 [2.2–4.4] | 0.1% | 0.7% | 0.0% | Score runoff (WME\*), BTR-score (WME\*), Soft Minimax (WME\*) |
| Approval | approval-based | 86.5 [84.1–88.7] | 86.5 [84.4–88.4] | 99.9 [99.9–99.9] | 69.7 | 70.8 | 85.1 [82.6–87.4] | 84.5 [82.2–86.8] | 75.5 [72.6–78.3] | 76.7 [74.0–79.4] | 5.4 [4.0–7.0] | 5.0 [3.7–6.4] | 0.0% | 1.0% | 0.0% | DMS (VSE\* WME\*), ESR (VSE\* WME\*), Geometric median (VSE\* WME\*) +16 more |
| Approval runoff | two-round | 86.1 [84.0–88.2] | 94.8 [93.5–96.1] | 99.8 [99.8–99.8] | 69.4 | 70.8 | 86.6 [84.5–88.7] | 93.0 [91.5–94.5] | 91.0 [89.1–92.7] | 92.4 [90.7–94.0] | 3.4 [2.3–4.6] | 3.2 [2.1–4.3] | 0.1% | 1.6% | 0.0% | ESR (VSE\*), Score runoff (WME\*), ARA (VSE\*) +2 more |
| IRV | plurality-based | 85.6 [83.2–87.8] | 90.8 [89.1–92.3] | 97.6 [97.5–97.7] | 76.2 | 79.7 | 85.5 [83.2–87.6] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 86.2 [84.0–88.2] | 4.4 [3.1–5.7] | 4.1 [2.9–5.4] | 5.4% | 14.0% | 1.6% | ESR (VSE\* WME\* sincerity), ARA (VSE\* WME\* sincerity), Soft Minimax-40 (VSE\* WME\* sincerity) +19 more |
| Approval cond. runoff | two-round | 85.4 [82.9–87.8] | 93.2 [91.7–94.6] | 99.8 [99.8–99.9] | 69.5 | 70.8 | 86.9 [84.9–88.9] | 91.5 [89.8–93.1] | 87.5 [85.3–89.5] | 88.6 [86.6–90.5] | 3.4 [2.3–4.6] | 3.2 [2.1–4.3] | 0.1% | 1.5% | 0.0% | Soft Minimax-D (VSE\* WME\*), Smith//Score (WME\*), Approval runoff (WME\*) +9 more |
| Score runoff | two-round | 85.3 [83.0–87.4] | 98.0 [97.2–98.7] | 99.9 [99.9–99.9] | 90.3 | 94.1 | 86.0 [83.8–87.9] | 98.3 [97.6–98.9] | 97.0 [95.9–98.0] | 96.9 [95.8–97.9] | 3.2 [2.1–4.4] | 3.2 [2.1–4.4] | 0.1% | 0.8% | 0.0% | — |
| ER-IRV | plurality-based | 85.2 [82.9–87.3] | 90.2 [88.3–91.8] | 98.1 [98.0–98.2] | 76.2 | 79.7 | 85.5 [83.2–87.6] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 85.5 [83.2–87.6] | 4.4 [3.1–5.7] | 3.7 [2.6–4.9] | 0.8% | 18.2% | 0.2% | ESR (VSE\* WME\* sincerity), MARS (VSE\* WME\* sincerity), MARS-T (VSE\* WME\* sincerity) +20 more |
| IRV-3 | plurality-based | 84.5 [82.1–86.8] | 89.4 [87.6–91.2] | 98.8 [98.7–98.9] | 76.0 | 78.2 | 84.6 [82.0–86.8] | 87.1 [85.0–89.1] | 82.2 [79.7–84.3] | 83.8 [81.6–86.0] | 5.2 [3.8–6.7] | 4.7 [3.3–6.1] | 5.4% | 7.5% | 1.4% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity) +18 more |
| Borda | positional | 84.4 [82.1–86.6] | 93.2 [91.8–94.5] | 97.6 [97.5–97.7] | 75.9 | 79.7 | 81.4 [78.5–83.8] | 95.3 [94.2–96.2] | 86.6 [84.4–88.7] | 84.2 [81.8–86.5] | 4.0 [2.8–5.2] | 3.7 [2.6–4.9] | 6.1% | 13.9% | 1.0% | Smith//Score (VSE\* WME\* sincerity), BTR-score (VSE\* WME\* sincerity), MARS (VSE\* WME\* sincerity) +19 more |
| MMPO | minimax-family | 83.9 [81.7–86.1] | 99.1 [98.7–99.4] | 98.3 [98.2–98.3] | 76.5 | 79.7 | 84.6 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.8 [95.7–97.8] | 3.8 [2.6–5.1] | 4.1 [2.9–5.4] | 0.1% | 18.2% | 0.0% | — |
| BTR-IRV | condorcet-based | 83.7 [81.4–85.9] | 98.2 [97.6–98.8] | 97.7 [97.6–97.8] | 76.3 | 79.7 | 84.1 [81.9–86.4] | 99.8 [99.7–99.9] | 100.0 [100.0–100.0] | 96.6 [95.5–97.6] | 4.0 [2.8–5.4] | 4.6 [3.3–5.9] | 5.7% | 13.6% | 1.1% | Score runoff (VSE\* sincerity), Ranked pairs (WME\*), Schulze (WME\*) +4 more |
| MMWV | minimax-family | 83.6 [81.5–85.9] | 99.2 [98.8–99.5] | 98.2 [98.2–98.3] | 76.5 | 79.7 | 84.6 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 96.8 [95.7–97.8] | 3.8 [2.6–5.1] | 4.3 [3.0–5.7] | 0.1% | 18.2% | 0.0% | — |
| Copeland | condorcet | 83.3 [81.1–85.5] | 98.4 [97.8–98.9] | 97.7 [97.7–97.8] | 76.2 | 79.7 | 83.8 [81.6–86.0] | 99.6 [99.4–99.8] | 100.0 [100.0–100.0] | 96.0 [94.8–97.2] | 4.0 [2.8–5.4] | 4.1 [2.9–5.4] | 5.6% | 13.2% | 1.0% | Score runoff (VSE\* sincerity), Ranked pairs (WME\*), Schulze (WME\*) +4 more |
| Ranked pairs | condorcet | 83.3 [80.9–85.6] | 99.1 [98.7–99.4] | 97.7 [97.7–97.8] | 76.3 | 79.7 | 84.5 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.1 [96.0–98.1] | 3.9 [2.7–5.2] | 4.2 [3.0–5.6] | 5.7% | 13.3% | 1.0% | — |
| Minimax | minimax-family | 83.1 [80.6–85.4] | 99.1 [98.7–99.5] | 97.7 [97.7–97.8] | 76.3 | 79.7 | 84.6 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.4 [96.4–98.4] | 3.8 [2.6–5.1] | 4.2 [3.0–5.5] | 5.6% | 13.3% | 1.0% | — |
| Kemeny-Young | condorcet | 83.0 [80.7–85.3] | 99.3 [98.9–99.6] | 97.7 [97.7–97.8] | 76.3 | 79.7 | 84.3 [82.0–86.6] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 97.5 [96.4–98.5] | 4.0 [2.8–5.3] | 4.3 [3.1–5.6] | 5.6% | 13.3% | 1.0% | — |
| Schulze | condorcet | 83.0 [80.5–85.3] | 99.1 [98.7–99.4] | 97.7 [97.6–97.8] | 76.3 | 79.7 | 84.6 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 97.2 [96.1–98.3] | 3.8 [2.6–5.1] | 4.3 [3.1–5.7] | 5.6% | 13.3% | 1.1% | — |
| TTR | two-round | 82.9 [80.3–85.6] | 89.6 [87.7–91.5] | 97.2 [96.9–97.4] | 63.5 | 66.3 | 83.9 [81.6–86.5] | 84.3 [82.1–86.5] | 80.8 [78.3–83.1] | 87.2 [85.2–89.2] | 5.2 [3.8–6.6] | 5.1 [3.8–6.5] | 5.9% | 0.0% | 3.0% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +28 more |
| Supplementary vote | plurality-based | 82.9 [80.2–85.4] | 83.7 [81.5–86.0] | 97.7 [97.6–97.8] | 76.3 | 79.7 | 79.1 [75.9–82.5] | 79.4 [76.9–81.9] | 71.9 [69.0–74.7] | 75.7 [72.9–78.3] | 8.2 [6.6–9.9] | 6.5 [4.9–8.2] | 5.7% | 14.8% | 1.3% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +30 more |
| Plurality | plurality-based | 77.5 [74.2–80.7] | 74.9 [72.2–77.7] | 98.4 [98.2–98.5] | 64.3 | 66.3 | 67.4 [63.2–71.6] | 69.6 [66.6–72.7] | 63.7 [60.8–66.9] | 67.2 [64.2–70.1] | 12.7 [10.8–14.8] | 8.6 [7.0–10.4] | 6.9% | 0.0% | 1.5% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +30 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 51.8% |
| Supplementary vote | 48.2% |
| Approval | 46.5% |
| IRV-3 | 46.1% |
| TTR | 46.1% |
| IRV | 45.8% |
| DMS | 45.8% |
| Geometric median | 45.7% |
| ESR | 45.1% |
| Score | 45.0% |
| ER-IRV | 44.7% |
| Approval cond. runoff | 44.4% |
| BTR-score | 44.4% |
| ARA | 44.2% |
| BTR-IRV | 44.2% |
| MARS | 43.9% |
| Smith//Score | 43.9% |
| MARS-T | 43.8% |
| STAR | 43.8% |
| Approval runoff | 43.8% |
| Soft Minimax-D | 43.6% |
| 321 | 43.6% |
| MAV | 43.4% |
| Ranked pairs | 43.0% |
| Soft Minimax-40 | 42.9% |
| Kemeny-Young | 42.8% |
| Schulze | 42.8% |
| Minimax | 42.8% |
| MJ | 42.7% |
| Soft Minimax | 42.5% |
| Score runoff | 42.4% |
| MMPO | 42.1% |
| MMWV | 42.1% |
| Copeland | 41.7% |
| Borda | 40.9% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 977 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 23 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 88.9% (89.4%) | 8.5% (7.9%) | 2.6% (2.8%) | 73.9% (56.5%) | 88.4% (88.5%) | 81.5% |
| ESR | 89.3% (89.4%) | 8.1% (7.9%) | 2.7% (2.8%) | 73.9% (56.5%) | 94.5% (95.0%) | 86.4% |
| DMS | 89.4% (89.4%) | 8.4% (7.9%) | 2.3% (2.8%) | 65.2% (56.5%) | 87.3% (87.2%) | 81.1% |
| ARA | 88.4% (89.4%) | 9.1% (7.9%) | 2.5% (2.8%) | 69.6% (56.5%) | 94.5% (94.9%) | 86.8% |
| Soft Minimax-40 | 88.4% (89.4%) | 8.3% (7.9%) | 3.3% (2.8%) | 60.9% (56.5%) | 95.8% (95.0%) | 86.5% |
| Score | 87.8% (89.4%) | 8.9% (7.9%) | 3.3% (2.8%) | 69.6% (56.5%) | 90.1% (91.1%) | 82.6% |
| 321 | 81.3% (81.3%) | 14.5% (15.5%) | 4.2% (3.3%) | 82.6% (82.6%) | 97.1% (95.4%) | 82.1% |
| MAV | 89.4% (89.4%) | 8.1% (7.9%) | 2.6% (2.8%) | 65.2% (56.5%) | 86.0% (83.8%) | 81.1% |
| MARS-T | 87.9% (89.4%) | 8.9% (7.9%) | 3.2% (2.8%) | 65.2% (56.5%) | 95.5% (96.9%) | 87.8% |
| Soft Minimax-D | 88.9% (89.4%) | 7.8% (7.9%) | 3.3% (2.8%) | 65.2% (56.5%) | 99.3% (99.2%) | 89.5% |
| MARS | 88.2% (89.4%) | 8.6% (7.9%) | 3.2% (2.8%) | 65.2% (56.5%) | 97.6% (97.5%) | 88.5% |
| MJ | 89.6% (89.4%) | 7.2% (7.9%) | 3.3% (2.8%) | 69.6% (56.5%) | 87.2% (85.7%) | 81.4% |
| BTR-score | 88.9% (89.4%) | 8.4% (7.9%) | 2.7% (2.8%) | 52.2% (56.5%) | 100.0% (100.0%) | 89.8% |
| Soft Minimax | 89.7% (89.4%) | 7.8% (7.9%) | 2.6% (2.8%) | 60.9% (56.5%) | 100.0% (100.0%) | 91.0% |
| Smith//Score | 88.7% (89.4%) | 8.5% (7.9%) | 2.8% (2.8%) | 60.9% (56.5%) | 100.0% (100.0%) | 89.6% |
| STAR | 88.5% (89.4%) | 8.5% (7.9%) | 3.0% (2.8%) | 56.5% (56.5%) | 98.8% (98.9%) | 88.9% |
| IRV | 94.6% (100.0%) | 3.7% (0.0%) | 1.7% (0.0%) | 34.8% (0.0%) | 87.1% (84.8%) | 86.2% |
| ER-IRV | 93.9% (100.0%) | 3.0% (0.0%) | 3.2% (0.0%) | 30.4% (0.0%) | 86.9% (84.8%) | 85.5% |
| IRV-3 | 87.6% (89.9%) | 7.0% (5.6%) | 5.4% (4.5%) | 52.2% (34.8%) | 93.3% (90.5%) | 83.8% |
| Borda | 93.6% (100.0%) | 4.2% (0.0%) | 2.3% (0.0%) | 39.1% (0.0%) | 87.7% (86.6%) | 84.2% |
| MMPO | 95.6% (100.0%) | 2.0% (0.0%) | 2.4% (0.0%) | 30.4% (0.0%) | 99.9% (100.0%) | 96.8% |
| BTR-IRV | 95.7% (100.0%) | 2.1% (0.0%) | 2.1% (0.0%) | 34.8% (0.0%) | 100.0% (100.0%) | 96.6% |
| MMWV | 95.6% (100.0%) | 1.9% (0.0%) | 2.5% (0.0%) | 26.1% (0.0%) | 100.0% (100.0%) | 96.8% |
| Copeland | 95.4% (100.0%) | 2.6% (0.0%) | 2.0% (0.0%) | 26.1% (0.0%) | 100.0% (100.0%) | 96.0% |
| Ranked pairs | 95.8% (100.0%) | 2.4% (0.0%) | 1.8% (0.0%) | 47.8% (0.0%) | 100.0% (100.0%) | 97.1% |
| Minimax | 96.2% (100.0%) | 2.0% (0.0%) | 1.7% (0.0%) | 43.5% (0.0%) | 100.0% (100.0%) | 97.4% |
| Kemeny-Young | 96.2% (100.0%) | 2.1% (0.0%) | 1.6% (0.0%) | 43.5% (0.0%) | 100.0% (100.0%) | 97.5% |
| Schulze | 95.9% (100.0%) | 2.1% (0.0%) | 1.9% (0.0%) | 47.8% (0.0%) | 100.0% (100.0%) | 97.2% |
| Supplementary vote | 94.9% (100.0%) | 4.4% (0.0%) | 0.7% (0.0%) | 39.1% (0.0%) | 76.6% (71.9%) | 75.7% |

![strategy arrows](arrows-c1.png)

![strategy shifts](shift-c1.png)

![sincerity](sincerity-c1.png)

![fidelity](fidelity-c1.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c1.png)

## c2: poll size 9

1000 elections (seeds 1), 977 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7; 3 poll round(s), 5 polls of 9 voters, poll error 2.0x, search 10, aggressive share 0.5, strategic share 0.0, adaptive share 1.0, scores 0..5. Electorates: Condorcet winner in 97.7%; it also has the highest total utility in 69.5% of those; electing it loses 15.1% of VSE; favourite ÷ least liked 7.9 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 84.4 to 84.7; with the best VSE (100) WME 76.9 (nothing lower is possible there either); the most balanced point VSE 93.9 / WME 96.1. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 90.2 (at WME 92.5); the dotted line in the plots. A random candidate (the zero of classical VSE) gives VSE -58.7 / WME -53.2.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Geometric median, ESR, ARA, MARS, STAR, Soft Minimax-D, Soft Minimax, Score runoff, MMPO.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): ARA, MARS, STAR, Score runoff.

| Method | Group | VSE\* | WME\* | Sincerity | Fidelity | (honest) | VSE | WME | CE | CE\* | Low | Low\* | Compromise | Burial | Push-over | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Geometric median | rated | 89.5 [87.6–91.3] | 89.5 [87.8–91.2] | 100.0 [100.0–100.0] | 90.2 | 94.1 | 90.7 [88.9–92.3] | 90.4 [88.7–91.9] | 81.0 [78.4–83.5] | 80.2 [77.7–82.6] | 2.7 [1.7–3.8] | 2.8 [1.8–3.8] | 0.0% | 0.3% | 0.0% | ESR (WME\*), MARS (WME\*), ARA (WME\*) +1 more |
| DMS | rated | 89.3 [87.5–91.0] | 88.4 [86.5–90.3] | 99.9 [99.9–100.0] | 90.0 | 94.1 | 90.1 [88.2–91.8] | 89.6 [87.8–91.3] | 80.7 [78.1–83.1] | 79.5 [76.8–82.1] | 3.4 [2.3–4.6] | 3.4 [2.3–4.7] | 0.0% | 1.0% | 0.0% | Score (WME\*), ESR (WME\*), MARS (WME\*) +2 more |
| ESR | rated | 88.6 [86.7–90.4] | 92.6 [91.0–94.0] | 99.9 [99.9–99.9] | 90.2 | 94.1 | 89.2 [87.5–90.9] | 93.8 [92.3–95.0] | 87.0 [84.9–89.1] | 85.0 [82.6–87.2] | 3.0 [2.0–4.1] | 3.2 [2.1–4.4] | 0.1% | 1.0% | 0.0% | STAR (WME\*), MARS (WME\*), MARS-T (WME\*) +1 more |
| Score | rated | 88.6 [86.8–90.4] | 90.7 [89.0–92.2] | 99.9 [99.9–99.9] | 89.7 | 94.1 | 89.9 [88.2–91.6] | 92.8 [91.3–94.1] | 84.7 [82.4–86.9] | 81.2 [78.6–83.6] | 3.0 [2.0–4.2] | 3.0 [2.0–4.2] | 0.0% | 1.3% | 0.0% | ESR (WME\*), STAR (WME\*), MARS (WME\*) +4 more |
| ARA | hybrid | 88.1 [86.0–90.1] | 94.4 [93.3–95.7] | 99.9 [99.9–99.9] | 90.0 | 94.1 | 89.3 [87.4–91.0] | 94.7 [93.5–95.9] | 87.5 [85.4–89.6] | 87.0 [84.8–89.1] | 2.5 [1.6–3.5] | 2.4 [1.5–3.4] | 0.1% | 1.4% | 0.0% | — |
| MARS | hybrid | 88.1 [86.1–89.9] | 95.0 [93.9–96.1] | 99.9 [99.9–99.9] | 90.1 | 94.1 | 88.0 [86.0–89.8] | 95.9 [94.8–96.8] | 89.6 [87.6–91.6] | 88.1 [86.1–90.1] | 3.1 [2.0–4.2] | 3.2 [2.1–4.4] | 0.1% | 1.0% | 0.0% | — |
| Soft Minimax-40 | hybrid | 88.0 [86.0–90.1] | 93.6 [92.2–94.8] | 99.9 [99.9–99.9] | 90.1 | 94.1 | 88.8 [86.8–90.6] | 94.5 [93.3–95.6] | 86.7 [84.5–88.7] | 85.3 [83.0–87.6] | 3.1 [2.0–4.3] | 3.3 [2.2–4.4] | 0.1% | 1.4% | 0.0% | STAR (WME\*), MARS (WME\*), Soft Minimax-D (WME\*) |
| MAV | median-based | 87.9 [85.8–89.8] | 86.9 [84.8–88.9] | 99.9 [99.9–99.9] | 90.0 | 94.1 | 88.6 [86.6–90.5] | 87.7 [85.8–89.5] | 77.9 [75.4–80.5] | 78.3 [75.3–80.7] | 2.7 [1.7–3.8] | 2.7 [1.7–3.8] | 0.1% | 0.9% | 0.0% | Smith//Score (WME\*), Score (WME\*), MJ (WME\*) +11 more |
| STAR | hybrid | 87.5 [85.5–89.3] | 95.0 [94.0–96.1] | 99.9 [99.9–99.9] | 90.1 | 94.1 | 88.2 [86.3–90.0] | 95.8 [94.7–96.8] | 89.4 [87.4–91.4] | 87.9 [85.9–90.1] | 2.7 [1.7–3.7] | 3.1 [2.1–4.2] | 0.1% | 1.1% | 0.0% | — |
| MJ | median-based | 87.5 [85.4–89.5] | 89.1 [87.3–90.9] | 99.9 [99.9–99.9] | 90.0 | 94.1 | 88.0 [86.0–90.0] | 89.9 [88.2–91.6] | 81.0 [78.5–83.2] | 81.5 [79.0–83.8] | 2.2 [1.3–3.0] | 2.5 [1.6–3.5] | 0.0% | 1.0% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), DMS (VSE\*) +11 more |
| MARS-T | hybrid | 87.5 [85.5–89.4] | 94.3 [93.2–95.5] | 99.9 [99.9–99.9] | 90.0 | 94.1 | 88.0 [86.0–89.9] | 95.8 [94.7–96.8] | 89.3 [87.1–91.2] | 86.9 [84.8–89.2] | 3.1 [2.0–4.2] | 3.2 [2.1–4.4] | 0.1% | 1.0% | 0.0% | Smith//Score (WME\*), BTR-score (WME\*), MARS (WME\*) +1 more |
| 321 | hybrid | 87.3 [85.2–89.2] | 89.1 [87.2–90.9] | 99.9 [99.9–99.9] | 82.9 | 85.8 | 88.2 [86.2–90.1] | 89.6 [87.9–91.3] | 80.9 [78.2–83.2] | 79.8 [77.3–82.3] | 2.7 [1.7–3.7] | 3.3 [2.2–4.5] | 0.1% | 1.5% | 0.0% | Smith//Score (WME\*), Approval runoff (WME\*), DMS (VSE\*) +11 more |
| Soft Minimax-D | hybrid | 87.1 [85.0–89.1] | 95.1 [94.0–96.1] | 99.9 [99.9–99.9] | 90.3 | 94.1 | 87.4 [85.3–89.4] | 96.2 [95.0–97.1] | 89.8 [87.8–91.8] | 88.4 [86.2–90.5] | 3.4 [2.3–4.6] | 3.1 [2.1–4.2] | 0.1% | 1.1% | 0.0% | Score runoff (WME\*) |
| Soft Minimax | hybrid | 86.8 [84.7–88.8] | 95.7 [94.5–96.7] | 99.9 [99.9–99.9] | 90.3 | 94.1 | 87.1 [85.0–89.0] | 96.6 [95.6–97.4] | 90.7 [88.9–92.6] | 89.9 [87.9–91.8] | 3.3 [2.2–4.5] | 3.2 [2.1–4.3] | 0.1% | 1.0% | 0.0% | Score runoff (WME\*), MARS (VSE\*) |
| Smith//Score | condorcet-based | 86.5 [84.3–88.6] | 95.7 [94.6–96.7] | 99.9 [99.9–99.9] | 90.2 | 94.1 | 87.3 [85.2–89.2] | 96.4 [95.4–97.3] | 90.5 [88.6–92.3] | 90.2 [88.3–92.1] | 3.3 [2.2–4.5] | 3.3 [2.2–4.5] | 0.1% | 0.9% | 0.0% | Score runoff (WME\*), MARS (VSE\*) |
| BTR-score | hybrid | 86.1 [83.9–88.2] | 95.7 [94.6–96.6] | 99.9 [99.9–99.9] | 90.2 | 94.1 | 87.1 [85.0–89.0] | 96.4 [95.5–97.3] | 90.6 [88.7–92.4] | 89.5 [87.5–91.3] | 3.4 [2.3–4.6] | 3.3 [2.2–4.5] | 0.1% | 1.0% | 0.0% | STAR (VSE\*), Score runoff (WME\*), MARS (VSE\*) |
| Score runoff | two-round | 85.8 [83.8–87.8] | 98.2 [97.5–98.8] | 99.9 [99.9–99.9] | 90.0 | 94.1 | 86.0 [83.8–87.9] | 98.3 [97.6–98.9] | 97.0 [95.9–98.0] | 96.8 [95.6–97.9] | 3.2 [2.1–4.4] | 3.3 [2.2–4.4] | 0.1% | 1.2% | 0.0% | — |
| Approval cond. runoff | two-round | 85.6 [83.4–87.8] | 90.0 [88.1–91.9] | 99.8 [99.8–99.8] | 69.2 | 70.8 | 86.9 [84.9–88.9] | 91.5 [89.8–93.1] | 87.5 [85.3–89.5] | 86.1 [83.8–88.2] | 3.4 [2.3–4.6] | 3.9 [2.7–5.2] | 0.1% | 2.2% | 0.0% | ESR (VSE\* WME\*), STAR (VSE\* WME\*), MARS (VSE\* WME\*) +13 more |
| Approval runoff | two-round | 85.5 [83.3–87.7] | 92.6 [90.9–94.1] | 99.7 [99.7–99.8] | 69.1 | 70.8 | 86.6 [84.5–88.7] | 93.0 [91.5–94.5] | 91.0 [89.1–92.7] | 90.7 [88.7–92.4] | 3.4 [2.3–4.6] | 3.7 [2.5–5.0] | 0.1% | 2.3% | 0.0% | STAR (VSE\* WME\*), MARS (VSE\* WME\*), MARS-T (VSE\* WME\*) +8 more |
| Approval | approval-based | 85.2 [82.7–87.5] | 82.8 [80.4–85.0] | 99.9 [99.8–99.9] | 69.3 | 70.8 | 85.1 [82.6–87.4] | 84.5 [82.2–86.8] | 75.5 [72.6–78.3] | 74.2 [71.3–77.0] | 5.4 [4.0–7.0] | 4.3 [3.0–5.6] | 0.0% | 1.9% | 0.0% | Score (VSE\* WME\*), MJ (VSE\* WME\*), MAV (VSE\* WME\*) +16 more |
| ER-IRV | plurality-based | 84.8 [82.3–87.0] | 89.3 [87.5–91.1] | 98.3 [98.2–98.4] | 76.2 | 79.7 | 85.5 [83.2–87.6] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 83.5 [81.2–85.8] | 4.4 [3.1–5.7] | 4.4 [3.1–5.7] | 1.0% | 17.3% | 0.3% | ESR (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity), MARS (VSE\* WME\* sincerity) +19 more |
| MMPO | minimax-family | 83.3 [80.9–85.8] | 98.5 [97.9–99.0] | 98.4 [98.3–98.5] | 76.4 | 79.7 | 84.6 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 95.1 [93.7–96.3] | 3.8 [2.6–5.1] | 4.1 [2.9–5.4] | 0.2% | 17.8% | 0.1% | Score runoff (VSE\* sincerity) |
| Ranked pairs | condorcet | 83.2 [80.8–85.6] | 97.9 [97.2–98.5] | 97.9 [97.8–98.0] | 76.5 | 79.7 | 84.5 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 93.5 [91.9–95.0] | 3.9 [2.7–5.2] | 4.2 [3.0–5.6] | 6.0% | 12.2% | 1.3% | Score runoff (VSE\* sincerity) |
| MMWV | minimax-family | 83.1 [80.8–85.7] | 98.3 [97.6–98.9] | 98.4 [98.3–98.5] | 76.5 | 79.7 | 84.6 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 94.6 [93.1–95.9] | 3.8 [2.6–5.1] | 4.4 [3.1–5.7] | 0.2% | 17.5% | 0.1% | Score runoff (VSE\* sincerity) |
| IRV-3 | plurality-based | 83.0 [80.5–85.4] | 85.8 [83.7–87.8] | 98.5 [98.4–98.6] | 75.6 | 78.2 | 84.6 [82.0–86.8] | 87.1 [85.0–89.1] | 82.2 [79.7–84.3] | 79.7 [77.3–82.2] | 5.2 [3.8–6.7] | 5.9 [4.5–7.4] | 6.4% | 6.2% | 2.1% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +26 more |
| Minimax | minimax-family | 83.0 [80.6–85.4] | 97.6 [96.8–98.3] | 97.9 [97.9–98.0] | 76.5 | 79.7 | 84.6 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 93.2 [91.6–94.7] | 3.8 [2.6–5.1] | 4.4 [3.2–5.8] | 6.0% | 12.0% | 1.4% | Score runoff (VSE\* sincerity), MMPO (WME\*) |
| BTR-IRV | condorcet-based | 82.7 [80.2–85.2] | 96.5 [95.5–97.5] | 97.8 [97.7–97.9] | 76.4 | 79.7 | 84.1 [81.9–86.4] | 99.8 [99.7–99.9] | 100.0 [100.0–100.0] | 93.2 [91.6–94.8] | 4.0 [2.8–5.4] | 4.1 [2.9–5.3] | 6.1% | 12.4% | 1.7% | Score runoff (VSE\* WME\* sincerity), Smith//Score (VSE\* sincerity), BTR-score (VSE\* sincerity) +4 more |
| Schulze | condorcet | 82.6 [80.4–85.0] | 97.5 [96.7–98.3] | 97.9 [97.9–98.0] | 76.6 | 79.7 | 84.6 [82.3–86.8] | 100.0 [100.0–100.0] | 100.0 [100.0–100.0] | 92.8 [91.1–94.4] | 3.8 [2.6–5.1] | 4.3 [3.1–5.8] | 6.0% | 12.1% | 1.3% | Score runoff (VSE\* sincerity), MMPO (WME\*) |
| Kemeny-Young | condorcet | 81.9 [79.4–84.4] | 97.5 [96.6–98.3] | 97.9 [97.8–98.0] | 76.5 | 79.7 | 84.3 [82.0–86.6] | 100.0 [99.9–100.0] | 100.0 [100.0–100.0] | 93.1 [91.6–94.7] | 4.0 [2.8–5.3] | 4.8 [3.5–6.3] | 6.0% | 12.2% | 1.4% | Score runoff (VSE\* sincerity), Ranked pairs (VSE\*), MMPO (WME\*) |
| Copeland | condorcet | 81.4 [79.0–84.0] | 96.9 [96.1–97.6] | 97.9 [97.8–97.9] | 76.4 | 79.7 | 83.8 [81.6–86.0] | 99.6 [99.4–99.8] | 100.0 [100.0–100.0] | 92.6 [91.0–94.1] | 4.0 [2.8–5.4] | 4.4 [3.2–5.8] | 6.1% | 12.4% | 1.4% | Score runoff (VSE\* WME\* sincerity), Ranked pairs (VSE\* WME\*), MMPO (VSE\* WME\*) +4 more |
| TTR | two-round | 81.1 [78.5–83.7] | 83.9 [81.6–86.2] | 97.1 [96.8–97.3] | 63.2 | 66.3 | 83.9 [81.6–86.5] | 84.3 [82.1–86.5] | 80.8 [78.3–83.1] | 81.1 [78.7–83.4] | 5.2 [3.8–6.6] | 6.2 [4.7–7.8] | 7.1% | 0.0% | 3.1% | Smith//Score (VSE\* WME\* sincerity), ER-IRV (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +28 more |
| Borda | positional | 80.9 [78.2–83.7] | 91.9 [90.4–93.2] | 97.9 [97.8–97.9] | 76.2 | 79.7 | 81.4 [78.5–83.8] | 95.3 [94.2–96.2] | 86.6 [84.4–88.7] | 80.9 [78.6–83.2] | 4.0 [2.8–5.2] | 4.0 [2.8–5.2] | 6.4% | 12.1% | 1.4% | Smith//Score (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity), Score runoff (VSE\* WME\* sincerity) +19 more |
| IRV | plurality-based | 80.8 [78.1–83.6] | 84.6 [82.4–86.9] | 97.6 [97.5–97.7] | 76.2 | 79.7 | 85.5 [83.2–87.6] | 88.2 [86.2–90.0] | 84.8 [82.6–86.9] | 79.2 [76.6–81.7] | 4.4 [3.1–5.7] | 5.6 [4.3–7.0] | 6.3% | 12.5% | 2.2% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +27 more |
| Supplementary vote | plurality-based | 78.1 [75.0–81.3] | 78.1 [75.4–80.8] | 97.7 [97.7–97.8] | 76.3 | 79.7 | 79.1 [75.9–82.5] | 79.4 [76.9–81.9] | 71.9 [69.0–74.7] | 70.3 [67.6–73.2] | 8.2 [6.6–9.9] | 7.5 [5.9–9.1] | 6.3% | 13.5% | 2.0% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +30 more |
| Plurality | plurality-based | 67.3 [63.3–71.2] | 67.1 [63.9–70.5] | 97.7 [97.5–97.8] | 63.6 | 66.3 | 67.4 [63.2–71.6] | 69.6 [66.6–72.7] | 63.7 [60.8–66.9] | 63.7 [60.7–66.4] | 12.7 [10.8–14.8] | 12.0 [10.1–14.0] | 7.4% | 0.0% | 2.4% | Smith//Score (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity) +31 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when the race between the two leaders matters enough to them): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Plurality | 49.1% |
| Supplementary vote | 48.9% |
| MJ | 48.1% |
| IRV | 48.0% |
| IRV-3 | 47.8% |
| Approval | 47.7% |
| DMS | 47.7% |
| ER-IRV | 47.6% |
| Geometric median | 47.4% |
| MARS-T | 47.3% |
| Approval cond. runoff | 47.3% |
| ESR | 47.3% |
| TTR | 47.2% |
| MAV | 47.2% |
| MARS | 47.1% |
| ARA | 47.1% |
| STAR | 47.1% |
| Soft Minimax | 47.0% |
| Smith//Score | 46.9% |
| Soft Minimax-40 | 46.8% |
| BTR-score | 46.7% |
| 321 | 46.6% |
| Score | 46.6% |
| Soft Minimax-D | 46.5% |
| BTR-IRV | 46.4% |
| Approval runoff | 46.4% |
| Score runoff | 45.9% |
| Kemeny-Young | 45.5% |
| Copeland | 45.4% |
| Ranked pairs | 45.3% |
| Schulze | 45.3% |
| MMWV | 45.2% |
| MMPO | 45.1% |
| Minimax | 45.0% |
| Borda | 44.2% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 977 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 23 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Geometric median | 88.6% (89.4%) | 8.4% (7.9%) | 3.0% (2.8%) | 60.9% (56.5%) | 87.2% (88.5%) | 80.2% |
| DMS | 87.8% (89.4%) | 8.9% (7.9%) | 3.3% (2.8%) | 56.5% (56.5%) | 87.3% (87.2%) | 79.5% |
| ESR | 88.8% (89.4%) | 8.9% (7.9%) | 2.3% (2.8%) | 78.3% (56.5%) | 93.2% (95.0%) | 85.0% |
| Score | 87.3% (89.4%) | 8.5% (7.9%) | 4.2% (2.8%) | 69.6% (56.5%) | 89.4% (91.1%) | 81.2% |
| ARA | 88.8% (89.4%) | 8.7% (7.9%) | 2.5% (2.8%) | 56.5% (56.5%) | 95.0% (94.9%) | 87.0% |
| MARS | 88.0% (89.4%) | 8.6% (7.9%) | 3.4% (2.8%) | 73.9% (56.5%) | 97.3% (97.5%) | 88.1% |
| Soft Minimax-40 | 87.1% (89.4%) | 9.9% (7.9%) | 3.0% (2.8%) | 65.2% (56.5%) | 95.6% (95.0%) | 85.3% |
| MAV | 89.2% (89.4%) | 8.1% (7.9%) | 2.8% (2.8%) | 52.2% (56.5%) | 84.7% (83.8%) | 78.3% |
| STAR | 88.3% (89.4%) | 8.4% (7.9%) | 3.3% (2.8%) | 60.9% (56.5%) | 98.9% (98.9%) | 87.9% |
| MJ | 88.7% (89.4%) | 8.1% (7.9%) | 3.2% (2.8%) | 60.9% (56.5%) | 87.8% (85.7%) | 81.5% |
| MARS-T | 87.8% (89.4%) | 9.3% (7.9%) | 2.9% (2.8%) | 65.2% (56.5%) | 96.1% (96.9%) | 86.9% |
| 321 | 79.6% (81.3%) | 16.1% (15.5%) | 4.3% (3.3%) | 73.9% (82.6%) | 96.3% (95.4%) | 79.8% |
| Soft Minimax-D | 88.1% (89.4%) | 9.0% (7.9%) | 2.9% (2.8%) | 60.9% (56.5%) | 99.5% (99.2%) | 88.4% |
| Soft Minimax | 88.8% (89.4%) | 8.5% (7.9%) | 2.7% (2.8%) | 60.9% (56.5%) | 100.0% (100.0%) | 89.9% |
| Smith//Score | 89.6% (89.4%) | 7.8% (7.9%) | 2.7% (2.8%) | 65.2% (56.5%) | 100.0% (100.0%) | 90.2% |
| BTR-score | 88.5% (89.4%) | 8.7% (7.9%) | 2.8% (2.8%) | 56.5% (56.5%) | 100.0% (100.0%) | 89.5% |
| ER-IRV | 92.9% (100.0%) | 4.5% (0.0%) | 2.6% (0.0%) | 47.8% (0.0%) | 86.8% (84.8%) | 83.5% |
| MMPO | 93.2% (100.0%) | 3.4% (0.0%) | 3.4% (0.0%) | 26.1% (0.0%) | 99.8% (100.0%) | 95.1% |
| Ranked pairs | 92.2% (100.0%) | 5.5% (0.0%) | 2.3% (0.0%) | 52.2% (0.0%) | 100.0% (100.0%) | 93.5% |
| MMWV | 92.9% (100.0%) | 4.0% (0.0%) | 3.1% (0.0%) | 26.1% (0.0%) | 100.0% (100.0%) | 94.6% |
| IRV-3 | 85.8% (89.9%) | 9.1% (5.6%) | 5.1% (4.5%) | 87.0% (34.8%) | 91.0% (90.5%) | 79.7% |
| Minimax | 92.1% (100.0%) | 6.0% (0.0%) | 1.8% (0.0%) | 52.2% (0.0%) | 100.0% (100.0%) | 93.2% |
| BTR-IRV | 92.5% (100.0%) | 5.0% (0.0%) | 2.5% (0.0%) | 43.5% (0.0%) | 100.0% (100.0%) | 93.2% |
| Schulze | 91.6% (100.0%) | 6.4% (0.0%) | 1.9% (0.0%) | 47.8% (0.0%) | 100.0% (100.0%) | 92.8% |
| Kemeny-Young | 92.2% (100.0%) | 6.2% (0.0%) | 1.5% (0.0%) | 52.2% (0.0%) | 100.0% (100.0%) | 93.1% |
| Copeland | 91.7% (100.0%) | 5.4% (0.0%) | 2.9% (0.0%) | 39.1% (0.0%) | 100.0% (100.0%) | 92.6% |
| Borda | 92.3% (100.0%) | 5.9% (0.0%) | 1.7% (0.0%) | 60.9% (0.0%) | 84.6% (86.6%) | 80.9% |
| IRV | 89.4% (100.0%) | 8.2% (0.0%) | 2.5% (0.0%) | 52.2% (0.0%) | 85.6% (84.8%) | 79.2% |
| Supplementary vote | 90.2% (100.0%) | 8.3% (0.0%) | 1.5% (0.0%) | 52.2% (0.0%) | 73.8% (71.9%) | 70.3% |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

Scatter and chain plots leave out Random pair runoff, Anti-plurality, TRASH, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
