# Ballot Front Finder Lab — analysis

Input: base, width, utility/01-utility=linear, utility/02-utility=quadratic (simulator version 2.1.0; analysis version 2.1.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

How to read this:

- The values are the result. The **front** is judged on VSE\*, WME\* and sincerity: the result with strategic voters, and how honest they can afford to be. The **Pareto front** uses the point estimates as they are: a method is on it if no other is at least as good on all three and better on one, however small the difference (sincerity: below 1 point counts as a tie). **Undominated** asks for clear differences instead (see Dominated by). The two need not agree: a noise-sized advantage on one axis keeps a method on the Pareto front, but does not protect it from a method clearly better on another axis; a method beaten by a hair on every axis leaves the Pareto front but can stay undominated. Honest VSE and WME depend more on modelling assumptions and are shown for context. **Dominated by** lists the methods that are clearly better on at least one of the three front axes and clearly worse on none, with the axes on which they are better in brackets. Clearly means the paired 95% interval of the difference excludes 0; smaller differences count as ties. Sincerity differences are measured very precisely, but below 1 point no voter is noticeably more honest, so they count as ties too, here and on the Pareto front. It is a pointer for interpretation, relative to the methods included, not a verdict.
- **Sincerity**: how far the learned ballots go against the voters' own preferences, within each method's ballot format: the utility-weighted pairs a ballot puts in the wrong order, relative to the most dishonest ballot the format allows (100 = no reversed preference, 0 = as far against one's preferences as the format permits). Ties (exaggeration, levelling) are not reversals. **Fidelity**: how well the ballots reproduce the voters' utility differences (100 = exactly, a tie loses half a pair, a mirrored pair all of it); it includes what the format cannot express. **(honest)** is the fidelity of the sincere ballots: the format's own loss; the drop from there to Fidelity is what strategy adds (docs/analysis.md, Sincerity and fidelity). Both are undefined (—) for TRASH and the Inverse-\* methods, whose honest ballot works against the voter's own interest; on the front they count as ties on sincerity.
- The intervals in the tables are for each value on its own. They are wide because elections differ a lot from each other. Comparisons between methods are much more precise, because all methods face the same elections: use the paired differences for those, not overlapping intervals.
- Two methods whose paired difference interval includes 0 are tied at this sample size; their order in the table is not a finding.
- **VSE** and **WME** are efficiencies with 0 = random ballot and 100 = the best candidate of each election (values above 100 cannot be reached). VSE measures total utility; WME the winner's worst head-to-head margin by sincere preferences (a refined Condorcet measure; every election counts; Minimax reaches 100 with honest ballots by construction). CE (Condorcet efficiency, elections with a Condorcet winner only, elected or not) is shown for comparison and not used for dominance; **NUCW** likewise, for the elections where the Condorcet winner also has the highest total of normalized utilities (each voter's utilities stretched between their least and most liked candidate, as ballots see them): there the two measures agree as far as ballots can tell, and a good method should elect it nearly always. **Span**: the chance of electing from the trade-off span, the candidates no other beats on both the normalized total and the worst margin (where the two agree, only that candidate; runs since 2026-11-04). **Low** and **Low\***: the share of elections whose winner (the expected total utility of the winner lottery) lies below the median candidate's total utility, with honest and with learned ballots: how often a method elects a poor candidate, which the averages of VSE hide. Not used for dominance.
- **Lead** and **Gap** (with honest ballots; **Lead\*** and **Gap\*** with learned ones), a natural baseline: the trivial rules are the random ballot; the Condorcet winner, else random ballot; its mirror on the utility side (the normalized utility winner, weakened as much as CW//random ballot is against the best worst margin); a random pair decided by majority. **Lead**: against every lottery of them, the method is ahead by at least this many points on VSE or on WME; negative: some lottery of them is better by that much on both, so the method is no better than a coin among trivial rules. **Gap**: how far the method stays below the frontier of normalized ballots (the dotted line in the plots) in the same sense, by that many points on both axes; negative beyond it (the frontier is not a strict bound). No weights between VSE and WME are involved. The intervals leave out the noise of the frontier, which is the full sample's. Not used for dominance.
- **Jury** and **Jury\*** (equivalent jury size, honest and learned ballots; runs since 2026-11-05): a random jury of k voters decides with its honest normalized ballots, ideally for each trade-off between the normalized total and the worst margin among the jurors, and is judged on the whole electorate. A jury of one is the random ballot; the larger the jury, the closer it comes to the frontier of normalized ballots. The equivalent jury size is the smallest jury whose lotteries reach the method (Lead against them at most 0), interpolated on a log scale between the sizes run; > the largest size where no jury run reaches it. How many voters' honest judgment a method turns into its result. Not used for dominance.
- The strategic axes (VSE\*, WME\*) give the result after the poll rounds: voters see polls of the previous round (the method's counts only) and answer them all at once (docs/model.md, Strategy engine).
- **Compromise, burial, push-over** are shares of voters whose learned ballot reverses a real preference, by kind (see strategy.strategy_types): compromise raises a liked second choice, or the lesser evil of the two poll leaders, above the favourite; push-over raises a disliked candidate who is no poll leader to the top (to steer a runoff or an elimination); burial keeps the favourite on top and reverses a preference below it. **Exaggeration** counts the voters whose ballot differs from the sincere one without reversing any preference: candidates tied, left unranked or given the lowest or highest score together with others (a candidate buried or raised as far as the format allows without a reversal).

## Across conditions

![axes](axes-heatmap.png)

**Sweeps:** for each parameter varied around the base, the methods' learned values as a chain of arrows in the order of the parameter's values (light to dark). Parameters that only affect learning start each chain at the honest point; others show the honest values as a dashed chain. Parameters changed together (e.g. exits and clones) form one chain.

![sweep over width](chain-width.png)

![sweep over utility](chain-utility.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis); for conditions with fewer elections than the base, a further row gives the agreement expected between the base and a run of that size. A condition near or above its row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 0.97 | 0.99 | 0.95 | 0.99 | 0.99 |
| noise reference: c1 vs a run of 2000 elections | 0.95 | 0.99 | 0.92 | 0.99 | 0.99 |
| c2: width 1.4 | 0.80 | 0.99 | 0.77 | 0.99 | 0.98 |
| c3: utility linear | 0.40 | 0.91 | 0.54 | 0.96 | 0.98 |
| c4: utility quadratic | 0.66 | 0.65 | 0.66 | 0.79 | 0.95 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 97.5% | 69.9% | 14.9% | 8.7 |
| c2: width 1.4 | 97.9% | 80.0% | 6.5% | 2.9 |
| c3: utility linear | 97.9% | 86.6% | 2.5% | — |
| c4: utility quadratic | 97.9% | 82.6% | 3.7% | — |

## c1: base

4000 elections (seeds 1), 3900 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 97.5%; it also has the highest total utility in 69.9% of those; electing it loses 14.9% of VSE; favourite ÷ least liked 8.7 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 83.9 to 84.5; with the best VSE (100) WME 79.5 (nothing lower is possible there either); the most balanced point VSE 95.1 / WME 96.2. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 91.0 (at WME 92.5); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 90.9 (at WME 92.4); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -53.4 / WME -48.1. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 83.9 / WME 99.8. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 87.6 / 96.3; normalized utility winner 91.0 / 92.5; best worst margin 84.2 / 100.0; Condorcet winner, else random ballot 83.0 / 98.8; Condorcet winner, else utility winner 85.4 / 99.6; random pair, majority 15.4 / 2.6; nearest the median voter 88.7 / 96.9; nearest the mean voter 75.2 / 92.3.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): ESR-MARS, Score runoff, ARA, Soft Minimax-40, MARS, Soft Minimax-D, BTR-score, Soft Minimax, MMWV, Ranked pairs, Schulze.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): ARA, Soft Minimax-40, Soft Minimax-D, BTR-score, Soft Minimax, MMWV, Ranked pairs, Schulze, Minimax.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 83.0 / 98.8; its mirror on the utility side 89.9 / 91.3; random pair 15.4 / 2.6. No better than a lottery of them (Lead ≤ 0): with learned ballots MAV, 321, Delegated 321, DMS, MJ, Score, Approval runoff, Approval cond. runoff, Approval, ER-IRV, Supplementary vote, TTR, IRV-3, IRV, Asset, Borda, Plurality; with honest ballots MAV, Approval runoff, Approval cond. runoff, Approval, ER-IRV, Supplementary vote, TTR, IRV-3, IRV, Asset, Borda, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ESR-MARS | hybrid | 87.9 [87.0–88.8] | 94.8 [94.2–95.4] | 0.6 [0.2–1.1] | 1.9 [1.2–2.6] | 64 [60–69] | 100.0 [100.0–100.0] | 93.0 | 94.3 | 87.9 [86.9–88.9] | 96.5 [96.0–96.9] | 1.4 [1.0–1.8] | 1.2 [0.6–1.8] | 73 [69–79] | 90.2 [89.3–91.1] | 88.0 [86.9–89.1] | 98.5 [98.1–98.9] | 96.2 [95.6–96.9] | 97.7 [97.2–98.2] | 95.7 [95.0–96.3] | 3.4 [2.8–3.9] | 3.4 [2.9–4.0] | 0.0% | 0.2% | 0.0% | 5.5% | STAR (WME\*), Score runoff (WME\*), ARA (WME\*) +1 more |
| Score runoff | two-round | 87.7 [86.7–88.7] | 95.9 [95.4–96.4] | 1.1 [0.7–1.5] | 1.5 [0.9–2.1] | 69 [64–74] | 99.9 [99.9–99.9] | 92.6 | 94.2 | 88.2 [87.1–89.1] | 96.4 [96.0–96.8] | 1.5 [1.1–1.9] | 1.1 [0.5–1.7] | 75 [70–80] | 90.2 [89.3–91.1] | 89.5 [88.6–90.5] | 99.4 [99.1–99.6] | 98.2 [97.7–98.6] | 98.5 [98.2–98.9] | 97.3 [96.8–97.8] | 3.3 [2.7–3.8] | 3.6 [3.0–4.1] | 0.0% | 1.3% | 0.0% | 3.3% | MARS (WME\*), ARA (WME\*), Soft Minimax-40 (WME\*) |
| ARA | hybrid | 87.6 [86.6–88.6] | 96.4 [96.0–96.8] | 1.2 [0.8–1.7] | 1.4 [0.8–2.0] | 71 [66–76] | 100.0 [100.0–100.0] | 93.4 | 94.3 | 87.8 [86.8–88.7] | 96.9 [96.5–97.3] | 1.6 [1.2–2.0] | 1.1 [0.5–1.7] | 76 [72–81] | 90.7 [89.8–91.7] | 89.9 [88.9–90.8] | 98.0 [97.6–98.5] | 97.4 [96.9–98.0] | 97.5 [97.0–98.0] | 96.8 [96.3–97.3] | 3.2 [2.7–3.8] | 3.3 [2.8–3.9] | 0.0% | 0.1% | 0.0% | 3.7% | — |
| MAV | median-based | 87.5 [86.6–88.5] | 93.1 [92.4–93.8] | -0.4 [-0.9–0.2] | 2.8 [2.0–3.5] | 56 [52–60] | 100.0 [100.0–100.0] | 93.4 | 94.5 | 87.6 [86.7–88.6] | 93.5 [92.8–94.1] | -0.1 [-0.7–0.4] | 2.6 [1.8–3.3] | 58 [54–62] | 85.8 [84.7–86.9] | 85.3 [84.2–86.4] | 94.6 [93.8–95.4] | 93.8 [93.0–94.7] | 94.3 [93.6–95.0] | 93.8 [93.0–94.5] | 4.0 [3.4–4.7] | 3.9 [3.3–4.5] | 0.0% | 0.1% | 0.0% | 5.0% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +6 more |
| 321 | hybrid | 87.4 [86.5–88.4] | 93.3 [92.6–93.9] | -0.4 [-1.0–0.2] | 2.8 [2.0–3.5] | 56 [52–61] | 100.0 [100.0–100.0] | 85.6 | 86.7 | 88.0 [87.0–89.0] | 94.5 [94.0–95.1] | 0.6 [0.1–1.1] | 2.0 [1.3–2.6] | 63 [59–69] | 87.4 [86.4–88.4] | 86.0 [84.9–87.0] | 96.4 [95.8–97.0] | 94.7 [93.9–95.5] | 95.5 [94.8–96.1] | 94.0 [93.3–94.7] | 3.3 [2.8–3.9] | 3.4 [2.9–4.0] | 0.0% | 0.5% | 0.0% | 4.0% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +6 more |
| Delegated 321 | delegated | 87.4 [86.5–88.4] | 93.3 [92.6–93.9] | -0.4 [-1.0–0.2] | 2.8 [2.0–3.5] | 56 [52–61] | — | — | — | 88.0 [87.0–89.0] | 94.5 [94.0–95.1] | 0.6 [0.1–1.1] | 2.0 [1.3–2.6] | 63 [59–69] | 87.4 [86.4–88.4] | 86.0 [84.9–87.0] | 96.4 [95.8–97.0] | 94.7 [93.9–95.5] | 95.5 [94.8–96.1] | 94.0 [93.3–94.7] | 3.3 [2.8–3.9] | 3.4 [2.9–4.0] | 0.0% | 0.5% | 0.0% | 4.0% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +6 more |
| Soft Minimax-40 | hybrid | 87.4 [86.4–88.4] | 96.5 [96.1–97.0] | 1.2 [0.8–1.6] | 1.5 [0.8–2.1] | 70 [66–75] | 100.0 [100.0–100.0] | 93.6 | 94.3 | 87.3 [86.3–88.3] | 96.9 [96.5–97.3] | 1.3 [1.0–1.7] | 1.4 [0.7–1.9] | 72 [69–77] | 91.2 [90.3–92.1] | 90.6 [89.7–91.5] | 98.5 [98.1–98.9] | 97.9 [97.5–98.4] | 97.5 [97.0–97.9] | 97.0 [96.5–97.5] | 3.1 [2.5–3.6] | 3.0 [2.5–3.5] | 0.0% | 0.2% | 0.0% | 2.6% | — |
| STAR | hybrid | 87.4 [86.4–88.4] | 95.6 [95.1–96.2] | 0.8 [0.3–1.2] | 1.9 [1.2–2.5] | 64 [61–69] | 99.9 [99.9–100.0] | 93.2 | 94.3 | 87.9 [87.0–88.9] | 96.3 [95.9–96.8] | 1.4 [1.0–1.8] | 1.3 [0.7–1.9] | 72 [68–77] | 90.0 [89.1–90.9] | 88.9 [88.0–89.9] | 98.9 [98.5–99.3] | 97.7 [97.2–98.2] | 98.0 [97.6–98.5] | 97.0 [96.4–97.5] | 3.4 [2.9–4.0] | 3.6 [3.0–4.2] | 0.0% | 0.6% | 0.0% | 3.0% | MARS (WME\*), MARS-T (WME\*), ARA (WME\*) +2 more |
| MARS-T | hybrid | 87.4 [86.3–88.4] | 96.4 [95.9–96.8] | 1.1 [0.7–1.5] | 1.6 [0.9–2.2] | 69 [65–73] | 100.0 [100.0–100.0] | 93.2 | 94.2 | 87.5 [86.5–88.5] | 97.4 [97.0–97.7] | 1.7 [1.3–2.1] | 1.1 [0.4–1.6] | 78 [74–82] | 91.9 [91.1–92.7] | 90.2 [89.3–91.2] | 98.9 [98.5–99.2] | 97.4 [96.9–98.0] | 98.0 [97.5–98.4] | 96.7 [96.1–97.3] | 3.5 [2.9–4.0] | 3.5 [2.9–4.1] | 0.0% | 0.2% | 0.0% | 4.7% | MARS (WME\*) |
| DMS | rated | 87.3 [86.4–88.2] | 92.3 [91.6–93.0] | -0.9 [-1.4–-0.3] | 3.1 [2.3–3.8] | 53 [49–57] | 100.0 [100.0–100.0] | 92.7 | 94.4 | 88.4 [87.5–89.3] | 94.9 [94.4–95.4] | 0.9 [0.5–1.4] | 1.6 [0.9–2.2] | 68 [63–74] | 86.7 [85.7–87.8] | 84.0 [82.9–85.2] | 97.1 [96.5–97.6] | 93.1 [92.3–94.0] | 96.6 [96.0–97.2] | 93.1 [92.3–93.9] | 3.2 [2.7–3.8] | 3.6 [3.0–4.2] | 0.0% | 0.1% | 0.0% | 7.3% | Approval runoff (WME\*), MJ (WME\*), MAV (WME\*) +11 more |
| MARS | hybrid | 87.2 [86.2–88.3] | 96.8 [96.3–97.2] | 1.2 [0.8–1.7] | 1.5 [0.8–2.0] | 71 [67–75] | 100.0 [100.0–100.0] | 93.2 | 94.3 | 87.3 [86.3–88.4] | 97.5 [97.1–97.8] | 1.6 [1.3–2.1] | 1.1 [0.5–1.6] | 78 [74–82] | 92.1 [91.2–92.9] | 90.7 [89.8–91.6] | 98.9 [98.5–99.2] | 97.8 [97.2–98.3] | 98.1 [97.6–98.5] | 97.1 [96.5–97.6] | 3.6 [3.0–4.1] | 3.6 [3.0–4.2] | 0.0% | 0.2% | 0.0% | 4.7% | Soft Minimax-D (WME\*) |
| MJ | median-based | 87.1 [86.2–88.2] | 93.6 [92.9–94.2] | -0.3 [-0.8–0.2] | 2.9 [2.1–3.5] | 55 [52–60] | 100.0 [100.0–100.0] | 93.2 | 94.4 | 87.3 [86.3–88.4] | 94.3 [93.7–94.8] | 0.1 [-0.4–0.6] | 2.5 [1.8–3.1] | 59 [55–63] | 86.7 [85.7–87.8] | 86.0 [85.0–87.1] | 95.0 [94.3–95.8] | 94.1 [93.3–94.9] | 94.8 [94.2–95.6] | 93.9 [93.2–94.7] | 4.2 [3.5–4.8] | 4.1 [3.5–4.7] | 0.0% | 0.1% | 0.0% | 5.3% | ESR-MARS (VSE\* WME\*), ESR (WME\*), STAR (WME\*) +7 more |
| Score | rated | 87.1 [86.1–88.1] | 92.8 [92.1–93.4] | -0.8 [-1.3–-0.2] | 3.2 [2.3–3.9] | 52 [48–57] | 100.0 [100.0–100.0] | 92.4 | 94.3 | 88.2 [87.1–89.1] | 95.7 [95.2–96.1] | 1.2 [0.8–1.6] | 1.4 [0.8–2.0] | 70 [66–76] | 87.8 [86.8–88.8] | 83.7 [82.5–84.8] | 97.1 [96.6–97.7] | 91.9 [91.1–92.9] | 96.7 [96.2–97.3] | 92.3 [91.5–93.1] | 3.1 [2.6–3.7] | 3.6 [3.1–4.3] | 0.0% | 0.1% | 0.0% | 8.5% | ESR-MARS (VSE\* WME\*), Smith//Score (WME\*), MJ (WME\*) +9 more |
| Approval runoff | two-round | 86.9 [85.8–87.8] | 93.5 [92.9–94.2] | -0.5 [-1.1–-0.0] | 3.1 [2.3–3.8] | 54 [50–58] | 99.9 [99.9–99.9] | 72.6 | 73.2 | 87.1 [86.1–88.1] | 93.2 [92.6–93.9] | -0.5 [-1.0–0.0] | 3.0 [2.2–3.7] | 54 [50–59] | 86.8 [85.8–87.7] | 87.2 [86.2–88.2] | 95.4 [94.8–96.1] | 95.6 [94.9–96.2] | 94.7 [94.0–95.4] | 94.8 [94.1–95.4] | 4.1 [3.5–4.8] | 4.2 [3.5–4.8] | 0.0% | 1.0% | 0.0% | 1.5% | Score runoff (VSE\* WME\*), ESR-MARS (VSE\* WME\*), Smith//Score (WME\*) +9 more |
| ESR | rated | 86.8 [85.8–87.8] | 94.7 [94.1–95.3] | 0.0 [-0.4–0.5] | 2.6 [1.9–3.3] | 58 [55–62] | 100.0 [100.0–100.0] | 92.5 | 94.3 | 87.2 [86.2–88.2] | 96.5 [96.1–97.0] | 1.1 [0.7–1.5] | 1.6 [0.9–2.2] | 69 [65–74] | 91.1 [90.1–92.0] | 88.4 [87.4–89.3] | 98.6 [98.2–99.0] | 95.5 [94.7–96.2] | 97.6 [97.1–98.1] | 94.8 [94.1–95.5] | 3.5 [3.0–4.2] | 3.9 [3.3–4.5] | 0.0% | 0.1% | 0.0% | 8.4% | Score runoff (VSE\* WME\*), ARA (VSE\* WME\*), Smith//Score (WME\*) +8 more |
| Soft Minimax-D | hybrid | 86.8 [85.8–87.9] | 97.3 [96.9–97.7] | 1.3 [0.9–1.7] | 1.4 [0.8–1.9] | 73 [69–77] | 100.0 [100.0–100.0] | 93.8 | 94.2 | 86.9 [85.9–87.9] | 97.7 [97.3–98.0] | 1.5 [1.2–1.9] | 1.2 [0.6–1.6] | 77 [73–81] | 93.0 [92.2–93.8] | 92.4 [91.6–93.3] | 98.8 [98.4–99.1] | 98.3 [97.9–98.8] | 97.9 [97.4–98.3] | 97.4 [96.9–97.9] | 3.3 [2.7–3.9] | 3.4 [2.8–4.0] | 0.0% | 0.1% | 0.0% | 1.4% | — |
| Approval cond. runoff | two-round | 86.5 [85.3–87.5] | 92.8 [92.2–93.5] | -1.1 [-1.6–-0.4] | 3.6 [2.7–4.4] | 50 [46–54] | 99.9 [99.9–99.9] | 73.0 | 73.5 | 87.1 [86.0–88.0] | 93.1 [92.4–93.8] | -0.6 [-1.2–-0.1] | 3.1 [2.3–3.8] | 53 [49–58] | 86.4 [85.3–87.4] | 86.4 [85.4–87.4] | 95.1 [94.4–95.8] | 94.6 [93.9–95.4] | 94.4 [93.7–95.1] | 94.0 [93.3–94.7] | 4.2 [3.5–4.8] | 4.2 [3.6–4.9] | 0.0% | 0.7% | 0.0% | 2.0% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), MARS (VSE\* WME\*) +15 more |
| BTR-score | hybrid | 86.4 [85.3–87.5] | 97.6 [97.2–98.0] | 1.2 [0.9–1.7] | 1.4 [0.9–1.9] | 74 [70–78] | 100.0 [100.0–100.0] | 93.7 | 94.2 | 86.4 [85.3–87.5] | 97.9 [97.6–98.2] | 1.3 [1.0–1.7] | 1.3 [0.7–1.7] | 76 [73–80] | 93.6 [92.9–94.4] | 93.1 [92.2–93.9] | 98.7 [98.3–99.2] | 98.4 [97.9–98.8] | 97.8 [97.4–98.3] | 97.5 [97.0–98.0] | 3.6 [3.0–4.2] | 3.6 [3.1–4.3] | 0.0% | 0.2% | 0.0% | 2.0% | — |
| Approval | approval-based | 86.3 [85.2–87.4] | 90.3 [89.5–91.1] | -2.4 [-3.0–-1.7] | 4.4 [3.4–5.3] | 44 [41–48] | 100.0 [100.0–100.0] | 74.3 | 74.5 | 86.3 [85.2–87.4] | 90.4 [89.6–91.2] | -2.3 [-3.0–-1.6] | 4.3 [3.4–5.3] | 44 [41–48] | 81.5 [80.2–82.7] | 81.7 [80.5–82.8] | 91.8 [90.9–92.8] | 91.7 [90.8–92.6] | 91.8 [90.8–92.6] | 91.7 [90.8–92.5] | 4.7 [4.0–5.3] | 4.5 [3.9–5.2] | 0.0% | 0.1% | 0.0% | 2.4% | MAV (VSE\* WME\*), DMS (VSE\* WME\*), STAR (VSE\* WME\*) +19 more |
| Smith//Score | condorcet-based | 86.2 [85.1–87.4] | 97.5 [97.2–97.9] | 1.1 [0.7–1.5] | 1.5 [1.0–2.0] | 72 [69–77] | 100.0 [100.0–100.0] | 93.7 | 94.2 | 86.4 [85.3–87.5] | 97.9 [97.6–98.2] | 1.3 [0.9–1.7] | 1.3 [0.8–1.7] | 76 [72–80] | 93.6 [92.8–94.4] | 92.9 [92.2–93.8] | 98.7 [98.3–99.2] | 98.3 [97.8–98.8] | 97.8 [97.4–98.3] | 97.4 [96.9–97.9] | 3.6 [3.0–4.2] | 3.7 [3.1–4.3] | 0.0% | 0.2% | 0.0% | 2.2% | Soft Minimax-D (VSE\*) |
| Soft Minimax | hybrid | 86.2 [85.1–87.3] | 97.7 [97.4–98.1] | 1.1 [0.8–1.5] | 1.4 [0.9–1.9] | 74 [71–77] | 100.0 [100.0–100.0] | 93.9 | 94.3 | 86.1 [85.0–87.2] | 98.0 [97.7–98.3] | 1.2 [0.9–1.6] | 1.3 [0.8–1.7] | 76 [73–80] | 93.9 [93.2–94.6] | 93.3 [92.5–94.1] | 98.5 [98.1–99.0] | 98.1 [97.7–98.6] | 97.7 [97.2–98.1] | 97.3 [96.8–97.8] | 3.5 [2.9–4.0] | 3.5 [2.9–4.0] | 0.0% | 0.1% | 0.0% | 1.4% | — |
| IMDS | rated | 85.9 [84.7–86.9] | 97.3 [96.9–97.7] | 0.8 [0.4–1.1] | 1.8 [1.3–2.3] | 68 [65–72] | 100.0 [100.0–100.0] | 92.4 | 94.3 | 85.9 [84.8–87.0] | 97.9 [97.6–98.2] | 1.1 [0.7–1.5] | 1.4 [0.9–1.8] | 74 [71–78] | 93.9 [93.1–94.6] | 92.7 [91.9–93.5] | 98.5 [98.1–98.9] | 97.7 [97.2–98.2] | 97.5 [97.0–98.0] | 96.7 [96.1–97.2] | 3.5 [2.9–4.1] | 3.6 [3.0–4.2] | 0.0% | 0.1% | 0.0% | 8.3% | BTR-score (VSE\*), Soft Minimax (WME\*), Soft Minimax-D (VSE\*) |
| ER-IRV | plurality-based | 85.5 [84.4–86.6] | 94.7 [94.0–95.3] | -0.6 [-1.1–-0.1] | 3.3 [2.6–4.1] | 53 [49–57] | 99.7 [99.7–99.7] | 78.5 | 79.0 | 86.0 [84.9–87.0] | 95.4 [94.8–96.0] | -0.1 [-0.5–0.3] | 2.8 [2.1–3.4] | 58 [54–61] | 92.5 [91.7–93.3] | 91.5 [90.7–92.3] | 97.0 [96.4–97.6] | 96.0 [95.3–96.6] | 95.6 [94.9–96.2] | 94.8 [94.0–95.5] | 3.8 [3.2–4.3] | 4.2 [3.5–4.8] | 0.6% | 2.0% | 0.2% | 0.4% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), BTR-score (VSE\* WME\*) +10 more |
| Supplementary vote | plurality-based | 84.9 [83.8–86.0] | 87.8 [86.7–88.8] | -4.2 [-5.0–-3.4] | 5.9 [5.0–6.9] | 36 [33–39] | 99.6 [99.6–99.6] | 78.1 | 78.7 | 85.6 [84.4–86.6] | 88.6 [87.6–89.6] | -3.5 [-4.3–-2.8] | 5.3 [4.4–6.2] | 39 [36–42] | 82.0 [80.8–83.1] | 81.5 [80.3–82.7] | 89.8 [88.8–90.9] | 89.2 [88.1–90.3] | 89.4 [88.5–90.4] | 88.9 [87.9–89.9] | 4.5 [3.9–5.2] | 4.9 [4.2–5.5] | 1.9% | 1.2% | 0.2% | 0.0% | Smith//Score (VSE\* WME\*), Approval (VSE\* WME\*), Approval runoff (VSE\* WME\*) +29 more |
| TTR | two-round | 84.5 [83.4–85.6] | 93.9 [93.2–94.7] | -1.5 [-2.1–-1.0] | 4.2 [3.5–5.0] | 47 [44–51] | 98.8 [98.7–98.9] | 64.5 | 65.5 | 85.1 [84.0–86.2] | 94.1 [93.4–94.8] | -1.2 [-1.7–-0.6] | 3.9 [3.1–4.6] | 49 [46–53] | 91.0 [90.1–91.9] | 90.5 [89.6–91.4] | 95.5 [94.7–96.2] | 95.0 [94.2–95.7] | 93.9 [93.1–94.6] | 93.6 [92.8–94.3] | 4.4 [3.8–5.1] | 4.6 [3.9–5.2] | 2.0% | 0.0% | 1.0% | 0.0% | Smith//Score (VSE\* WME\* sincerity), IMDS (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity) +24 more |
| MMWV | minimax-family | 84.5 [83.4–85.6] | 99.3 [99.1–99.5] | 1.0 [0.7–1.4] | 0.7 [0.4–0.9] | 87 [84–90] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.8 [97.3–98.2] | 99.5 [99.2–99.7] | 99.0 [98.7–99.3] | 98.7 [98.4–99.1] | 98.2 [97.8–98.5] | 4.3 [3.7–5.0] | 4.2 [3.6–4.9] | 0.1% | 1.2% | 0.0% | 0.1% | — |
| Ranked pairs | condorcet | 84.5 [83.4–85.6] | 99.3 [99.1–99.5] | 1.0 [0.7–1.4] | 0.6 [0.4–0.8] | 88 [85–91] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.1–85.5] | 99.6 [99.5–99.7] | 1.1 [0.8–1.4] | 0.4 [0.2–0.5] | 92 [90–94] | 98.6 [98.2–99.0] | 97.9 [97.4–98.3] | 99.5 [99.2–99.7] | 99.1 [98.7–99.4] | 98.6 [98.2–98.9] | 98.1 [97.6–98.5] | 4.3 [3.7–5.0] | 4.3 [3.7–5.0] | 0.4% | 0.7% | 0.0% | 0.1% | — |
| Schulze | condorcet | 84.4 [83.3–85.6] | 99.4 [99.2–99.5] | 1.0 [0.7–1.4] | 0.6 [0.4–0.8] | 89 [86–91] | 99.9 [99.8–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.9 [97.4–98.3] | 99.5 [99.2–99.7] | 99.0 [98.7–99.3] | 98.7 [98.4–99.1] | 98.2 [97.8–98.5] | 4.3 [3.7–5.0] | 4.2 [3.6–4.9] | 0.4% | 0.8% | 0.0% | 0.1% | — |
| MMPO | minimax-family | 84.4 [83.3–85.6] | 99.3 [99.1–99.4] | 1.0 [0.6–1.4] | 0.7 [0.5–0.9] | 87 [84–90] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.7 [97.2–98.2] | 99.5 [99.2–99.7] | 99.0 [98.6–99.3] | 98.7 [98.4–99.1] | 98.1 [97.7–98.5] | 4.3 [3.7–5.0] | 4.2 [3.6–4.9] | 0.1% | 1.2% | 0.0% | 0.1% | Schulze (WME\*), Minimax (WME\*) |
| Minimax | minimax-family | 84.4 [83.3–85.5] | 99.3 [99.2–99.5] | 1.0 [0.7–1.4] | 0.6 [0.4–0.8] | 88 [85–91] | 99.9 [99.8–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.9 [97.4–98.3] | 99.5 [99.2–99.7] | 99.1 [98.8–99.4] | 98.7 [98.4–99.1] | 98.2 [97.8–98.6] | 4.3 [3.7–5.0] | 4.2 [3.6–4.9] | 0.4% | 0.8% | 0.0% | 0.1% | — |
| BTR-IRV | condorcet-based | 84.3 [83.2–85.5] | 98.5 [98.2–98.8] | 0.5 [0.2–0.9] | 1.4 [1.0–1.7] | 77 [74–81] | 99.7 [99.7–99.8] | 78.2 | 78.6 | 84.2 [83.0–85.3] | 99.4 [99.2–99.5] | 0.9 [0.6–1.2] | 0.6 [0.4–0.8] | 88 [85–91] | 98.6 [98.3–99.0] | 96.6 [95.9–97.1] | 99.5 [99.2–99.7] | 98.5 [98.1–98.9] | 98.3 [97.8–98.7] | 97.2 [96.6–97.7] | 4.5 [3.9–5.2] | 4.5 [3.8–5.1] | 0.7% | 1.1% | 0.1% | 0.3% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +4 more |
| Kemeny-Young | condorcet | 84.2 [83.0–85.3] | 99.3 [99.2–99.5] | 0.9 [0.5–1.2] | 0.6 [0.4–0.8] | 88 [85–91] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.1–85.4] | 99.6 [99.5–99.7] | 1.1 [0.8–1.4] | 0.4 [0.2–0.5] | 93 [90–94] | 98.6 [98.2–98.9] | 97.8 [97.3–98.2] | 99.5 [99.2–99.7] | 98.9 [98.5–99.3] | 98.7 [98.3–99.0] | 98.0 [97.6–98.4] | 4.4 [3.8–5.1] | 4.5 [3.9–5.1] | 0.4% | 0.7% | 0.0% | 0.1% | Ranked pairs (VSE\*) |
| Copeland | condorcet | 84.0 [82.8–85.2] | 99.1 [98.9–99.3] | 0.7 [0.4–1.0] | 0.9 [0.6–1.1] | 84 [81–88] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.0 [82.8–85.2] | 99.4 [99.3–99.6] | 0.8 [0.6–1.1] | 0.6 [0.4–0.7] | 89 [87–92] | 98.5 [98.2–98.9] | 97.9 [97.4–98.3] | 99.4 [99.1–99.6] | 99.1 [98.7–99.3] | 98.1 [97.6–98.5] | 97.7 [97.3–98.1] | 4.3 [3.7–5.0] | 4.3 [3.7–5.0] | 0.4% | 0.7% | 0.0% | 0.1% | Schulze (VSE\* WME\*), Ranked pairs (VSE\*), MMWV (VSE\*) |
| IRV-3 | plurality-based | 83.8 [82.5–85.0] | 91.4 [90.4–92.3] | -3.1 [-4.0–-2.4] | 5.8 [4.8–6.8] | 38 [35–42] | 99.2 [99.1–99.2] | 76.7 | 77.9 | 86.3 [85.3–87.3] | 94.3 [93.6–94.9] | -0.4 [-0.9–0.0] | 3.1 [2.4–3.8] | 54 [51–58] | 90.2 [89.3–91.2] | 87.4 [86.4–88.4] | 95.7 [95.0–96.4] | 92.6 [91.8–93.5] | 94.4 [93.7–95.2] | 91.7 [90.9–92.5] | 3.9 [3.4–4.5] | 5.1 [4.5–5.8] | 2.6% | 1.0% | 1.3% | 0.1% | Smith//Score (VSE\* WME\*), ER-IRV (VSE\* WME\*), Approval runoff (VSE\* WME\*) +29 more |
| IRV | plurality-based | 83.6 [82.3–84.8] | 92.5 [91.7–93.3] | -2.7 [-3.4–-2.0] | 5.4 [4.5–6.3] | 41 [38–44] | 99.0 [99.0–99.1] | 77.7 | 79.0 | 86.0 [84.9–87.0] | 95.4 [94.8–96.0] | -0.1 [-0.5–0.3] | 2.8 [2.1–3.4] | 58 [54–61] | 92.5 [91.7–93.3] | 89.1 [88.0–90.0] | 97.0 [96.4–97.6] | 93.6 [92.8–94.4] | 95.6 [94.9–96.2] | 92.5 [91.7–93.3] | 3.8 [3.2–4.3] | 4.8 [4.2–5.5] | 2.6% | 1.8% | 1.4% | 0.1% | Ranked pairs (VSE\* WME\*), MMWV (VSE\* WME\*), Smith//Score (VSE\* WME\*) +28 more |
| Asset | delegated | 82.0 [80.6–83.3] | 85.4 [84.3–86.6] | -7.0 [-7.9–-6.0] | 8.8 [7.6–9.9] | 28 [26–30] | 99.2 [99.2–99.3] | 66.8 | 67.5 | 82.8 [81.6–84.0] | 86.3 [85.2–87.3] | -6.1 [-7.0–-5.2] | 8.0 [6.9–9.0] | 30 [28–32] | 76.8 [75.4–78.1] | 76.0 [74.7–77.4] | 81.8 [80.4–83.1] | 81.0 [79.6–82.3] | 82.0 [80.8–83.2] | 81.4 [80.1–82.6] | 4.0 [3.4–4.6] | 4.5 [3.8–5.1] | 1.7% | 0.0% | 0.5% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +33 more |
| Borda | positional | 81.7 [80.5–83.1] | 91.1 [90.4–91.9] | -4.3 [-5.1–-3.6] | 7.0 [6.1–7.9] | 34 [32–37] | 99.1 [99.1–99.2] | 75.9 | 77.8 | 82.7 [81.5–83.9] | 94.2 [93.6–94.8] | -2.4 [-3.1–-1.7] | 5.0 [4.3–5.6] | 45 [43–48] | 84.5 [83.4–85.6] | 81.0 [79.8–82.2] | 88.3 [87.3–89.5] | 86.6 [85.5–87.8] | 88.6 [87.7–89.6] | 87.2 [86.1–88.2] | 3.3 [2.8–3.9] | 4.4 [3.8–5.0] | 3.3% | 5.1% | 0.0% | 4.4% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +31 more |
| Plurality | plurality-based | 76.7 [75.1–78.2] | 75.8 [74.4–77.3] | -13.2 [-14.7–-11.7] | 14.4 [12.8–15.9] | 17 [16–19] | 99.5 [99.5–99.5] | 64.3 | 64.9 | 74.3 [72.6–76.1] | 74.9 [73.5–76.2] | -15.6 [-17.2–-13.8] | 16.7 [15.0–18.4] | 15 [14–16] | 66.7 [65.2–68.2] | 67.1 [65.6–68.6] | 75.3 [73.8–76.8] | 75.9 [74.4–77.5] | 75.8 [74.4–77.1] | 76.7 [75.4–77.9] | 8.8 [8.0–9.7] | 7.8 [7.0–8.7] | 4.4% | 0.0% | 0.1% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +34 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Borda | 13.1% |
| Score | 9.2% |
| ESR | 9.1% |
| IMDS | 9.0% |
| Plurality | 8.6% |
| DMS | 8.0% |
| IRV | 6.3% |
| ESR-MARS | 6.2% |
| IRV-3 | 6.0% |
| MJ | 5.8% |
| MAV | 5.5% |
| 321 | 5.2% |
| Delegated 321 | 5.2% |
| MARS-T | 5.2% |
| MARS | 5.1% |
| Approval | 4.9% |
| Score runoff | 4.8% |
| TTR | 4.6% |
| Approval cond. runoff | 4.5% |
| Asset | 4.2% |
| ARA | 4.1% |
| STAR | 3.8% |
| Supplementary vote | 3.6% |
| Approval runoff | 3.5% |
| ER-IRV | 3.5% |
| Soft Minimax-40 | 2.9% |
| Smith//Score | 2.5% |
| BTR-IRV | 2.4% |
| BTR-score | 2.4% |
| Soft Minimax-D | 1.6% |
| Soft Minimax | 1.6% |
| Minimax | 1.4% |
| MMPO | 1.4% |
| MMWV | 1.4% |
| Schulze | 1.4% |
| Copeland | 1.3% |
| Ranked pairs | 1.3% |
| Kemeny-Young | 1.3% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Plurality | 34.6% | 1.12 | 0.92 | 94% |
| Supplementary vote | 23.8% | 1.07 | 0.59 | 99% |
| TTR | 20.8% | 0.53 | 0.58 | 99% |
| Approval | 20.3% | 0.71 | 0.29 | 100% |
| Asset | 18.5% | 0.27 | 0.08 | 100% |
| IRV-3 | 17.4% | 0.51 | 0.25 | 100% |
| Approval runoff | 17.4% | 0.44 | 0.37 | 100% |
| Approval cond. runoff | 16.9% | 0.51 | 0.38 | 100% |
| MAV | 16.3% | 0.47 | 0.07 | 100% |
| IRV | 15.5% | 0.22 | 0.05 | 100% |
| ER-IRV | 15.5% | 0.22 | 0.05 | 100% |
| MJ | 15.4% | 0.40 | 0.06 | 100% |
| 321 | 14.8% | 0.38 | 0.42 | 100% |
| Delegated 321 | 14.8% | 0.38 | 0.42 | 100% |
| DMS | 14.5% | 0.34 | 0.06 | 100% |
| ESR | 11.7% | 0.32 | 0.08 | 100% |
| Score | 11.7% | 0.30 | 0.08 | 100% |
| Score runoff | 11.2% | 0.19 | 0.22 | 100% |
| Borda | 10.8% | 0.05 | 0.56 | 100% |
| Soft Minimax-40 | 10.7% | 0.29 | 0.08 | 100% |
| ARA | 10.5% | 0.29 | 0.09 | 100% |
| ESR-MARS | 10.4% | 0.28 | 0.07 | 100% |
| STAR | 9.9% | 0.27 | 0.18 | 100% |
| IMDS | 9.4% | 0.25 | 0.10 | 100% |
| Soft Minimax | 8.7% | 0.24 | 0.09 | 100% |
| MARS-T | 8.5% | 0.22 | 0.06 | 100% |
| MARS | 8.5% | 0.23 | 0.07 | 100% |
| Soft Minimax-D | 7.9% | 0.21 | 0.06 | 100% |
| Smith//Score | 7.6% | 0.21 | 0.06 | 100% |
| BTR-score | 7.5% | 0.21 | 0.06 | 100% |
| Copeland | 4.0% | 0.01 | 0.07 | 100% |
| BTR-IRV | 3.0% | 0.03 | 0.04 | 100% |
| Ranked pairs | 3.0% | 0.02 | 0.05 | 100% |
| Schulze | 2.8% | 0.01 | 0.04 | 100% |
| Minimax | 2.8% | 0.01 | 0.04 | 100% |
| MMWV | 2.8% | 0.01 | 0.04 | 100% |
| MMPO | 2.8% | 0.01 | 0.04 | 100% |
| Kemeny-Young | 2.6% | 0.02 | 0.05 | 100% |

**Manipulation** (from honest ballots, see manipulation.py; after John Huang's votesim): for each candidate X other than the honest winner T, the voters who prefer X to T try the naive exaggerations of their ballot type for X against T, everyone else votes sincerely. One-sided: the share of elections in which some such coalition gains, and the VSE when the worst successful manipulation for the electorate is recorded (the honest result where none succeeds). Two-sided: T's supporters answer with whichever exaggeration for T counters it best; the share in which a coalition still gains, and the VSE recorded the same way. Compare VSE in the main table (honest).

| Method | Manipulable (one-sided) | VSE (one-sided) | Manipulable (two-sided) | VSE (two-sided) |
|---|---|---|---|---|
| TTR | 17.2 [16.1–18.3]% | 77.1 [75.7–78.5] | 9.0 [8.2–9.9]% | 81.4 [80.1–82.7] |
| Approval runoff | 32.4 [31.1–33.9]% | 73.3 [71.9–74.6] | 12.2 [11.2–13.2]% | 81.3 [80.0–82.6] |
| IRV-3 | 28.5 [27.2–29.9]% | 72.8 [71.2–74.4] | 9.9 [9.0–10.8]% | 80.9 [79.6–82.2] |
| Soft Minimax | 42.4 [40.9–43.9]% | 70.4 [69.0–71.8] | 7.3 [6.5–8.1]% | 82.8 [81.5–84.0] |
| IMDS | 45.3 [43.8–46.9]% | 68.3 [66.8–69.8] | 7.3 [6.5–8.2]% | 82.5 [81.2–83.7] |
| Score runoff | 55.3 [53.8–56.8]% | 63.1 [61.5–64.7] | 10.3 [9.3–11.2]% | 82.3 [81.0–83.6] |
| BTR-score | 50.7 [49.2–52.2]% | 62.1 [60.5–63.8] | 7.3 [6.6–8.1]% | 82.6 [81.4–83.9] |
| Supplementary vote | 37.1 [35.7–38.7]% | 62.0 [60.1–63.9] | 15.2 [14.1–16.3]% | 78.8 [77.4–80.1] |
| Smith//Score | 51.6 [50.2–53.1]% | 61.0 [59.3–62.7] | 7.4 [6.6–8.2]% | 82.6 [81.4–83.9] |
| Soft Minimax-40 | 59.0 [57.4–60.5]% | 59.8 [58.2–61.3] | 9.4 [8.6–10.3]% | 82.7 [81.5–84.0] |
| Soft Minimax-D | 59.1 [57.5–60.6]% | 59.8 [58.2–61.4] | 7.9 [7.0–8.8]% | 82.8 [81.6–84.0] |
| Asset | 42.0 [40.3–43.6]% | 59.5 [57.5–61.5] | 22.9 [21.5–24.1]% | 75.3 [73.9–76.8] |
| Approval cond. runoff | 57.6 [56.3–59.1]% | 57.9 [56.2–59.5] | 13.1 [12.1–14.1]% | 80.9 [79.7–82.3] |
| Schulze | 59.9 [58.2–61.4]% | 56.4 [54.8–58.1] | 2.0 [1.6–2.5]% | 83.3 [82.1–84.5] |
| Minimax | 60.1 [58.5–61.7]% | 56.1 [54.4–57.8] | 2.0 [1.6–2.5]% | 83.3 [82.1–84.5] |
| Ranked pairs | 61.1 [59.5–62.6]% | 55.9 [54.2–57.6] | 1.8 [1.5–2.3]% | 83.4 [82.2–84.6] |
| MMWV | 60.2 [58.6–61.8]% | 55.8 [54.2–57.6] | 2.0 [1.6–2.5]% | 83.3 [82.0–84.5] |
| STAR | 60.1 [58.6–61.7]% | 55.7 [54.0–57.3] | 11.1 [10.1–12.1]% | 82.0 [80.8–83.3] |
| MMPO | 60.3 [58.7–61.9]% | 55.6 [54.0–57.4] | 2.0 [1.6–2.5]% | 83.1 [81.9–84.4] |
| IRV | 50.0 [48.4–51.5]% | 54.9 [53.0–56.8] | 8.3 [7.5–9.1]% | 78.5 [76.9–80.1] |
| Kemeny-Young | 63.0 [61.4–64.5]% | 53.8 [52.2–55.6] | 1.8 [1.4–2.3]% | 83.4 [82.1–84.6] |
| ER-IRV | 51.3 [49.8–52.8]% | 53.1 [51.2–55.1] | 8.2 [7.4–9.0]% | 78.2 [76.6–79.8] |
| BTR-IRV | 62.0 [60.4–63.4]% | 52.4 [50.6–54.2] | 1.1 [0.9–1.5]% | 83.3 [82.0–84.5] |
| ARA | 68.2 [66.7–69.7]% | 50.6 [48.9–52.3] | 10.1 [9.2–11.1]% | 82.2 [80.9–83.4] |
| 321 | 71.4 [70.0–72.9]% | 45.8 [43.9–47.5] | 13.4 [12.4–14.5]% | 81.5 [80.2–82.8] |
| Delegated 321 | 71.4 [70.0–72.9]% | 45.8 [43.9–47.5] | 13.4 [12.4–14.5]% | 81.5 [80.2–82.8] |
| MARS | 70.6 [69.2–72.1]% | 43.9 [42.0–45.7] | 9.0 [8.2–9.9]% | 82.5 [81.2–83.7] |
| MARS-T | 72.0 [70.6–73.5]% | 42.2 [40.4–44.0] | 9.2 [8.3–10.0]% | 82.5 [81.2–83.7] |
| ESR | 69.2 [67.9–70.8]% | 41.8 [39.9–43.7] | 9.4 [8.5–10.3]% | 82.3 [81.1–83.5] |
| Copeland | 78.1 [76.8–79.5]% | 40.0 [38.2–41.7] | 2.1 [1.6–2.5]% | 83.3 [82.1–84.5] |
| MAV | 75.8 [74.5–77.1]% | 38.6 [36.7–40.3] | 14.5 [13.5–15.5]% | 81.4 [80.1–82.6] |
| Approval | 72.1 [70.6–73.4]% | 37.6 [35.7–39.7] | 16.8 [15.7–17.9]% | 78.8 [77.4–80.2] |
| ESR-MARS | 75.3 [73.9–76.7]% | 36.0 [34.1–37.9] | 10.6 [9.7–11.6]% | 82.1 [80.9–83.3] |
| MJ | 78.4 [77.1–79.7]% | 35.7 [33.9–37.5] | 14.2 [13.1–15.2]% | 81.1 [79.8–82.4] |
| DMS | 88.1 [87.2–89.2]% | 16.2 [14.4–18.0] | 14.1 [13.1–15.2]% | 81.3 [80.0–82.5] |
| Score | 89.6 [88.6–90.5]% | 10.7 [8.7–12.7] | 13.2 [12.2–14.3]% | 81.4 [80.2–82.7] |
| Plurality | 69.9 [68.5–71.4]% | 5.3 [2.1–8.2] | 29.6 [28.1–31.2]% | 64.3 [62.2–66.4] |
| Borda | 94.8 [94.0–95.5]% | -10.2 [-12.5–-8.1] | 17.1 [15.9–18.3]% | 75.8 [74.2–77.2] |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 3900 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 100 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| ESR-MARS | 90.1% (90.9%) | 7.2% (6.9%) | 2.7% (2.2%) | 81.0% (76.0%) | 95.2% (96.6%) | 88.0% |
| ARA | 90.2% (90.6%) | 7.1% (6.7%) | 2.7% (2.7%) | 73.0% (65.0%) | 96.0% (96.4%) | 89.9% |
| MAV | 89.3% (89.5%) | 8.3% (8.1%) | 2.4% (2.4%) | 76.0% (68.0%) | 91.4% (92.1%) | 85.3% |
| 321 | 82.6% (81.6%) | 10.0% (8.8%) | 7.4% (9.5%) | 81.0% (79.0%) | 97.7% (98.8%) | 86.0% |
| Delegated 321 | 82.6% (81.6%) | 10.0% (8.8%) | 7.4% (9.5%) | 81.0% (79.0%) | 97.7% (98.8%) | 86.0% |
| Soft Minimax-40 | 90.5% (91.0%) | 7.0% (7.0%) | 2.5% (2.0%) | 75.0% (73.0%) | 97.5% (97.9%) | 90.6% |
| STAR | 90.3% (91.5%) | 7.0% (6.1%) | 2.7% (2.4%) | 78.0% (75.0%) | 95.4% (94.7%) | 88.9% |
| MARS-T | 91.1% (91.6%) | 6.6% (6.1%) | 2.3% (2.3%) | 76.0% (75.0%) | 96.6% (97.8%) | 90.2% |
| DMS | 89.3% (89.6%) | 7.8% (7.7%) | 2.9% (2.7%) | 72.0% (68.0%) | 90.0% (92.7%) | 84.0% |
| MARS | 91.2% (91.7%) | 6.7% (6.2%) | 2.2% (2.1%) | 76.0% (73.0%) | 97.0% (97.9%) | 90.7% |
| MJ | 89.5% (89.7%) | 8.1% (7.8%) | 2.5% (2.4%) | 69.0% (64.0%) | 91.0% (91.6%) | 86.0% |
| Score | 89.7% (90.3%) | 7.3% (7.0%) | 3.1% (2.7%) | 74.0% (73.0%) | 89.2% (93.0%) | 83.7% |
| ESR | 90.9% (91.8%) | 6.8% (6.5%) | 2.2% (1.6%) | 84.0% (79.0%) | 95.7% (97.8%) | 88.4% |
| Soft Minimax-D | 91.7% (92.2%) | 6.4% (6.2%) | 1.8% (1.7%) | 76.0% (69.0%) | 99.6% (99.7%) | 92.4% |
| BTR-score | 91.9% (92.4%) | 6.1% (5.8%) | 1.9% (1.8%) | 79.0% (76.0%) | 100.0% (100.0%) | 93.1% |
| Smith//Score | 91.8% (92.4%) | 6.2% (5.8%) | 2.0% (1.8%) | 77.0% (77.0%) | 100.0% (100.0%) | 92.9% |
| Soft Minimax | 92.2% (92.8%) | 5.8% (5.4%) | 2.0% (1.7%) | 71.0% (67.0%) | 100.0% (100.0%) | 93.3% |
| IMDS | 91.6% (92.8%) | 6.1% (5.4%) | 2.3% (1.7%) | 77.0% (76.0%) | 99.7% (99.8%) | 92.7% |
| ER-IRV | 96.9% (98.9%) | 2.2% (0.8%) | 0.9% (0.3%) | 54.0% (44.0%) | 93.3% (93.5%) | 91.5% |
| Supplementary vote | 90.3% (91.4%) | 9.1% (8.3%) | 0.6% (0.2%) | 65.0% (57.0%) | 89.2% (89.3%) | 81.5% |
| MMWV | 97.4% (98.4%) | 1.5% (1.2%) | 1.1% (0.4%) | 40.0% (37.0%) | 100.0% (100.0%) | 97.8% |
| Ranked pairs | 97.6% (98.4%) | 1.5% (1.2%) | 0.9% (0.4%) | 45.0% (40.0%) | 100.0% (100.0%) | 97.9% |
| Schulze | 97.5% (98.4%) | 1.5% (1.2%) | 1.0% (0.4%) | 41.0% (37.0%) | 100.0% (100.0%) | 97.9% |
| MMPO | 97.3% (98.4%) | 1.6% (1.2%) | 1.1% (0.4%) | 41.0% (37.0%) | 100.0% (100.0%) | 97.7% |
| Minimax | 97.4% (98.4%) | 1.5% (1.2%) | 1.0% (0.4%) | 40.0% (37.0%) | 100.0% (100.0%) | 97.9% |
| BTR-IRV | 96.2% (98.6%) | 2.2% (1.2%) | 1.6% (0.3%) | 70.0% (64.0%) | 100.0% (100.0%) | 96.6% |
| Kemeny-Young | 97.4% (98.4%) | 1.7% (1.2%) | 1.0% (0.4%) | 48.0% (42.0%) | 100.0% (100.0%) | 97.8% |
| Copeland | 97.5% (98.3%) | 1.4% (1.2%) | 1.1% (0.6%) | 41.0% (40.0%) | 100.0% (100.0%) | 97.9% |
| IRV-3 | 88.4% (91.1%) | 9.1% (6.8%) | 2.5% (2.1%) | 83.0% (81.0%) | 96.8% (97.5%) | 87.4% |
| IRV | 94.5% (98.9%) | 4.2% (0.8%) | 1.3% (0.3%) | 61.0% (44.0%) | 92.7% (93.5%) | 89.1% |
| Borda | 93.7% (98.8%) | 2.5% (1.0%) | 3.8% (0.2%) | 37.0% (11.0%) | 83.6% (85.1%) | 81.0% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| ESR-MARS | 0.1% (0.0%) | 11.1% (10.7%) | 0.9% (0.9%) |
| Score runoff | 0.3% (0.2%) | — (—) | — (—) |
| ARA | 0.9% (0.4%) | 11.3% (11.6%) | 1.0% (1.0%) |
| MAV | 0.1% (0.0%) | 10.2% (10.0%) | 0.9% (0.8%) |
| 321 | 0.1% (0.2%) | 24.4% (26.3%) | 2.0% (2.2%) |
| Delegated 321 | 0.1% (0.2%) | 24.4% (26.3%) | 2.0% (2.2%) |
| Soft Minimax-40 | 0.9% (0.5%) | 11.2% (11.0%) | 1.0% (0.9%) |
| STAR | 0.2% (0.1%) | 13.0% (13.3%) | 1.1% (1.1%) |
| MARS-T | 0.0% (0.0%) | 10.8% (10.6%) | 0.9% (0.9%) |
| DMS | 0.0% (0.0%) | 11.0% (10.8%) | 1.0% (0.9%) |
| MARS | 0.1% (0.0%) | 10.9% (10.8%) | 0.9% (0.9%) |
| MJ | 0.0% (0.0%) | 10.1% (10.4%) | 0.9% (0.9%) |
| Score | 0.4% (0.1%) | 11.8% (11.1%) | 1.0% (0.9%) |
| Approval runoff | 0.8% (0.4%) | — (—) | — (—) |
| ESR | 0.4% (0.2%) | 11.3% (10.6%) | 1.0% (0.9%) |
| Soft Minimax-D | 0.1% (0.1%) | 10.9% (11.1%) | 0.9% (0.9%) |
| Approval cond. runoff | 0.7% (0.4%) | — (—) | — (—) |
| BTR-score | 0.0% (0.0%) | 10.7% (10.7%) | 0.9% (0.9%) |
| Approval | 1.1% (0.6%) | — (—) | — (—) |
| Smith//Score | 0.0% (0.0%) | 10.7% (10.6%) | 0.9% (0.9%) |
| Soft Minimax | 1.1% (0.9%) | 11.2% (11.2%) | 1.0% (1.0%) |
| IMDS | 1.4% (0.8%) | 11.7% (11.3%) | 1.0% (1.0%) |
| ER-IRV | 1.2% (0.8%) | 0.7% (0.0%) | 0.0% (0.0%) |
| Supplementary vote | 0.8% (0.8%) | 0.0% (0.0%) | 0.0% (0.0%) |
| TTR | 1.1% (0.7%) | — (—) | — (—) |
| MMWV | 0.9% (0.9%) | 0.5% (0.0%) | 0.0% (0.0%) |
| Ranked pairs | 0.8% (0.8%) | 0.1% (0.0%) | 0.0% (0.0%) |
| Schulze | 0.8% (0.9%) | 0.2% (0.0%) | 0.0% (0.0%) |
| MMPO | 0.9% (0.9%) | 0.5% (0.0%) | 0.0% (0.0%) |
| Minimax | 0.9% (0.9%) | 0.2% (0.0%) | 0.0% (0.0%) |
| BTR-IRV | 0.1% (0.1%) | 0.3% (0.0%) | 0.0% (0.0%) |
| Kemeny-Young | 0.5% (0.5%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Copeland | 1.4% (1.1%) | 0.6% (0.0%) | 0.1% (0.0%) |
| IRV-3 | 0.8% (0.8%) | 10.5% (10.5%) | 0.8% (0.7%) |
| IRV | 0.8% (0.8%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Asset | 1.2% (1.1%) | — (—) | — (—) |
| Borda | 0.5% (0.4%) | 3.7% (0.0%) | 0.2% (0.0%) |
| Plurality | 1.4% (1.1%) | — (—) | — (—) |

![strategy arrows](arrows-c1.png)

![strategy shifts](shift-c1.png)

![sincerity](sincerity-c1.png)

![fidelity](fidelity-c1.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c1.png)

## c2: width 1.4

2000 elections (seeds 1), 1957 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 1.4, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 97.9%; it also has the highest total utility in 80.0% of those; electing it loses 6.5% of VSE; favourite ÷ least liked 2.9 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 92.6 to 93.0; with the best VSE (100) WME 89.3 (nothing lower is possible there either); the most balanced point VSE 97.4 / WME 98.4. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 95.3 (at WME 94.7); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 95.2 (at WME 95.3); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -59.3 / WME -47.7. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 92.7 / WME 99.9. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 94.1 / 97.3; normalized utility winner 95.3 / 94.7; best worst margin 92.8 / 100.0; Condorcet winner, else random ballot 91.8 / 99.0; Condorcet winner, else utility winner 93.6 / 99.9; random pair, majority 21.3 / 2.9; nearest the median voter 95.9 / 96.7; nearest the mean voter 87.8 / 92.4.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Soft Minimax-40, MARS, Soft Minimax-D, Soft Minimax, Minimax, Schulze.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Soft Minimax-40, Soft Minimax-D, Smith//Score, BTR-score, Minimax, Schulze, Ranked pairs, Copeland, Kemeny-Young.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 91.8 / 99.0; its mirror on the utility side 94.4 / 93.7; random pair 21.3 / 2.9. No better than a lottery of them (Lead ≤ 0): with learned ballots 321, Delegated 321, MJ, Approval runoff, Approval cond. runoff, MAV, DMS, Score, Approval, ER-IRV, TTR, IRV, Borda, IRV-3, Supplementary vote, Asset, Plurality; with honest ballots Approval runoff, Approval cond. runoff, MAV, DMS, Score, Approval, ER-IRV, TTR, IRV, Borda, IRV-3, Supplementary vote, Asset, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Soft Minimax-40 | hybrid | 94.1 [93.2–94.9] | 97.6 [97.2–98.1] | 1.1 [0.6–1.6] | 0.7 [-0.0–1.4] | 80 [74–91] | 100.0 [100.0–100.0] | 93.3 | 94.2 | 94.2 [93.3–95.0] | 97.9 [97.4–98.3] | 1.2 [0.8–1.7] | 0.6 [-0.1–1.2] | 84 [77–95] | 92.4 [91.3–93.6] | 92.2 [90.9–93.3] | 98.6 [98.1–99.1] | 97.9 [97.2–98.5] | 97.9 [97.2–98.5] | 97.2 [96.4–97.9] | 1.4 [0.9–1.9] | 1.5 [0.9–2.0] | 0.0% | 0.1% | 0.0% | 2.8% | — |
| Score runoff | two-round | 94.1 [93.3–94.9] | 96.6 [96.0–97.2] | 0.7 [0.3–1.2] | 0.9 [0.2–1.6] | 74 [67–82] | 99.9 [99.9–99.9] | 92.3 | 94.1 | 94.4 [93.6–95.2] | 96.6 [96.0–97.1] | 1.0 [0.5–1.4] | 0.6 [-0.0–1.3] | 79 [72–90] | 89.2 [87.7–90.6] | 90.0 [88.6–91.3] | 98.9 [98.4–99.4] | 97.8 [97.2–98.5] | 98.2 [97.6–98.8] | 97.1 [96.4–97.8] | 1.2 [0.7–1.8] | 1.4 [0.9–1.9] | 0.0% | 1.4% | 0.0% | 3.7% | Smith//Score (WME\*), BTR-score (WME\*), MARS (WME\*) +4 more |
| MARS-T | hybrid | 93.9 [93.1–94.7] | 97.6 [97.1–98.0] | 0.9 [0.5–1.4] | 0.9 [0.2–1.5] | 78 [72–87] | 100.0 [100.0–100.0] | 92.9 | 94.1 | 94.0 [93.1–94.8] | 98.2 [97.8–98.5] | 1.2 [0.7–1.6] | 0.7 [0.0–1.2] | 83 [78–93] | 92.5 [91.3–93.6] | 90.9 [89.6–92.2] | 98.6 [98.0–99.2] | 97.1 [96.4–97.9] | 98.0 [97.4–98.6] | 96.6 [95.7–97.4] | 1.4 [0.9–1.9] | 1.6 [1.1–2.1] | 0.0% | 0.1% | 0.0% | 4.5% | Smith//Score (WME\*), BTR-score (WME\*), MARS (WME\*) +1 more |
| MARS | hybrid | 93.9 [93.1–94.7] | 97.8 [97.4–98.2] | 1.0 [0.6–1.5] | 0.9 [0.2–1.4] | 79 [74–88] | 100.0 [100.0–100.0] | 92.9 | 94.1 | 93.9 [93.1–94.8] | 98.2 [97.9–98.6] | 1.2 [0.7–1.6] | 0.7 [0.0–1.2] | 84 [79–93] | 92.7 [91.5–93.8] | 91.6 [90.4–92.9] | 98.5 [97.9–99.0] | 97.5 [96.7–98.3] | 97.9 [97.2–98.5] | 97.0 [96.2–97.7] | 1.4 [0.9–1.8] | 1.5 [0.9–2.0] | 0.0% | 0.1% | 0.0% | 4.5% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax-D (WME\*) |
| STAR | hybrid | 93.9 [93.0–94.8] | 95.8 [95.1–96.4] | 0.4 [-0.2–0.9] | 1.2 [0.4–2.0] | 68 [61–77] | 99.9 [99.9–100.0] | 92.9 | 94.1 | 94.5 [93.7–95.2] | 96.7 [96.1–97.2] | 1.0 [0.6–1.5] | 0.6 [-0.1–1.3] | 80 [73–94] | 89.0 [87.6–90.4] | 87.9 [86.5–89.3] | 98.6 [98.0–99.1] | 96.7 [95.9–97.5] | 98.0 [97.4–98.6] | 96.2 [95.4–97.0] | 1.1 [0.7–1.6] | 1.4 [0.9–1.9] | 0.0% | 0.6% | 0.0% | 3.3% | Smith//Score (WME\*), Score runoff (WME\*), BTR-score (WME\*) +7 more |
| ESR-MARS | hybrid | 93.9 [93.1–94.7] | 96.6 [96.0–97.2] | 0.6 [0.1–1.1] | 1.1 [0.4–1.8] | 72 [65–81] | 100.0 [100.0–100.0] | 92.7 | 94.1 | 94.1 [93.3–95.0] | 97.8 [97.4–98.2] | 1.2 [0.7–1.7] | 0.7 [-0.1–1.2] | 83 [77–93] | 91.7 [90.5–92.9] | 89.7 [88.4–91.0] | 98.4 [97.7–99.0] | 95.8 [94.9–96.6] | 97.8 [97.1–98.4] | 95.5 [94.6–96.4] | 1.3 [0.8–1.8] | 1.6 [1.1–2.1] | 0.0% | 0.2% | 0.0% | 5.3% | Smith//Score (WME\*), BTR-score (WME\*), MARS (WME\*) +3 more |
| Soft Minimax-D | hybrid | 93.7 [92.8–94.6] | 98.3 [97.9–98.7] | 1.1 [0.6–1.5] | 0.7 [0.2–1.3] | 82 [77–90] | 100.0 [100.0–100.0] | 93.7 | 94.1 | 93.8 [92.9–94.6] | 98.5 [98.2–98.9] | 1.1 [0.7–1.6] | 0.6 [0.1–1.1] | 85 [80–92] | 94.3 [93.3–95.3] | 93.9 [92.8–95.0] | 98.7 [98.1–99.2] | 98.3 [97.7–98.9] | 97.9 [97.2–98.5] | 97.5 [96.8–98.2] | 1.5 [0.9–2.0] | 1.5 [1.0–2.1] | 0.0% | 0.1% | 0.0% | 1.4% | — |
| ARA | hybrid | 93.7 [92.8–94.5] | 97.3 [96.7–97.7] | 0.7 [0.2–1.2] | 1.1 [0.4–1.8] | 73 [67–80] | 100.0 [100.0–100.0] | 93.0 | 94.1 | 93.8 [92.9–94.6] | 97.7 [97.3–98.1] | 0.9 [0.4–1.4] | 1.0 [0.3–1.5] | 77 [72–84] | 91.4 [90.2–92.6] | 90.5 [89.3–91.8] | 97.6 [96.8–98.2] | 96.8 [96.0–97.6] | 97.0 [96.2–97.8] | 96.4 [95.6–97.2] | 1.5 [0.9–2.1] | 1.6 [1.1–2.1] | 0.0% | 0.2% | 0.0% | 4.1% | Smith//Score (WME\*), BTR-score (WME\*), MARS (WME\*) +2 more |
| Smith//Score | condorcet-based | 93.6 [92.7–94.5] | 98.2 [97.8–98.6] | 1.0 [0.5–1.5] | 0.8 [0.3–1.4] | 80 [75–88] | 100.0 [100.0–100.0] | 93.5 | 94.1 | 93.6 [92.7–94.5] | 98.6 [98.3–98.9] | 1.1 [0.7–1.5] | 0.7 [0.1–1.1] | 84 [80–91] | 94.4 [93.4–95.5] | 93.5 [92.4–94.5] | 98.7 [98.2–99.2] | 97.8 [97.1–98.5] | 97.7 [97.0–98.4] | 97.0 [96.2–97.8] | 1.5 [1.0–2.1] | 1.6 [1.0–2.1] | 0.0% | 0.2% | 0.0% | 2.1% | — |
| ESR | rated | 93.6 [92.7–94.5] | 96.3 [95.6–96.9] | 0.3 [-0.3–0.9] | 1.4 [0.6–2.1] | 67 [61–76] | 100.0 [100.0–100.0] | 92.0 | 94.2 | 93.6 [92.7–94.6] | 97.9 [97.4–98.3] | 0.8 [0.4–1.4] | 1.0 [0.3–1.6] | 77 [71–86] | 92.8 [91.7–93.9] | 89.9 [88.6–91.3] | 98.3 [97.7–98.9] | 95.2 [94.2–96.1] | 97.4 [96.6–98.1] | 94.6 [93.6–95.5] | 1.5 [0.9–2.1] | 1.8 [1.1–2.4] | 0.0% | 0.1% | 0.0% | 8.4% | Smith//Score (WME\*), BTR-score (WME\*), MARS (WME\*) +5 more |
| BTR-score | hybrid | 93.6 [92.7–94.4] | 98.3 [97.9–98.7] | 1.0 [0.5–1.4] | 0.8 [0.3–1.4] | 81 [76–88] | 100.0 [100.0–100.0] | 93.5 | 94.1 | 93.7 [92.7–94.5] | 98.6 [98.3–98.9] | 1.1 [0.7–1.6] | 0.6 [0.1–1.1] | 85 [80–92] | 94.5 [93.5–95.6] | 93.6 [92.5–94.7] | 98.7 [98.2–99.2] | 97.8 [97.1–98.4] | 97.8 [97.1–98.5] | 97.1 [96.3–97.8] | 1.5 [0.9–2.0] | 1.6 [1.1–2.2] | 0.0% | 0.2% | 0.0% | 2.0% | — |
| 321 | hybrid | 93.4 [92.5–94.3] | 93.8 [92.8–94.8] | -0.6 [-1.3–0.1] | 1.9 [1.0–2.8] | 59 [52–69] | 100.0 [100.0–100.0] | 84.6 | 85.8 | 93.9 [93.0–94.7] | 95.0 [94.1–95.8] | 0.1 [-0.6–0.7] | 1.3 [0.5–2.1] | 66 [59–77] | 87.9 [86.4–89.3] | 86.9 [85.2–88.4] | 95.3 [94.2–96.2] | 93.9 [92.7–94.9] | 94.5 [93.4–95.4] | 93.6 [92.4–94.6] | 1.5 [0.9–2.0] | 1.6 [1.1–2.1] | 0.0% | 0.4% | 0.0% | 4.7% | Soft Minimax-40 (VSE\* WME\*), Ranked pairs (WME\*), Schulze (WME\*) +17 more |
| Delegated 321 | delegated | 93.4 [92.5–94.3] | 93.8 [92.8–94.8] | -0.6 [-1.3–0.1] | 1.9 [1.0–2.8] | 59 [52–69] | — | — | — | 93.9 [93.0–94.7] | 95.0 [94.1–95.8] | 0.1 [-0.6–0.7] | 1.3 [0.5–2.1] | 66 [59–77] | 87.9 [86.4–89.3] | 86.9 [85.2–88.4] | 95.3 [94.2–96.2] | 93.9 [92.7–94.9] | 94.5 [93.4–95.4] | 93.6 [92.4–94.6] | 1.5 [0.9–2.0] | 1.6 [1.1–2.1] | 0.0% | 0.4% | 0.0% | 4.7% | Soft Minimax-40 (VSE\* WME\*), Ranked pairs (WME\*), Schulze (WME\*) +17 more |
| MJ | median-based | 93.4 [92.5–94.3] | 94.7 [93.9–95.5] | -0.3 [-1.0–0.3] | 1.8 [1.0–2.6] | 60 [54–68] | 100.0 [100.0–100.0] | 93.1 | 94.4 | 93.7 [92.8–94.5] | 95.3 [94.5–96.0] | 0.1 [-0.5–0.7] | 1.5 [0.7–2.2] | 64 [58–74] | 88.1 [86.6–89.6] | 87.6 [86.1–89.1] | 93.9 [92.7–95.1] | 93.0 [91.8–94.1] | 93.5 [92.3–94.5] | 92.6 [91.5–93.7] | 1.7 [1.1–2.2] | 1.7 [1.1–2.2] | 0.0% | 0.1% | 0.0% | 4.7% | Score runoff (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*), Ranked pairs (WME\*) +17 more |
| Soft Minimax | hybrid | 93.2 [92.3–94.1] | 98.4 [98.0–98.8] | 0.8 [0.3–1.2] | 1.0 [0.4–1.5] | 79 [75–86] | 100.0 [100.0–100.0] | 93.7 | 94.1 | 93.4 [92.5–94.3] | 98.6 [98.3–99.0] | 0.9 [0.5–1.4] | 0.8 [0.2–1.3] | 83 [78–89] | 94.8 [93.8–95.7] | 94.4 [93.4–95.4] | 98.5 [97.9–99.0] | 98.2 [97.6–98.8] | 97.8 [97.1–98.4] | 97.5 [96.8–98.1] | 1.5 [0.9–2.0] | 1.5 [0.9–2.0] | 0.0% | 0.1% | 0.0% | 1.4% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +6 more |
| Approval runoff | two-round | 93.2 [92.3–94.1] | 94.1 [93.2–95.0] | -0.7 [-1.4–0.0] | 2.1 [1.1–2.9] | 57 [51–65] | 99.9 [99.8–99.9] | 68.6 | 69.1 | 93.5 [92.6–94.4] | 93.9 [93.0–94.9] | -0.5 [-1.2–0.2] | 1.8 [0.9–2.7] | 60 [53–70] | 88.3 [86.9–89.7] | 88.5 [87.1–89.9] | 93.7 [92.5–94.9] | 93.9 [92.7–95.0] | 93.0 [91.8–94.1] | 93.1 [92.0–94.2] | 2.2 [1.6–2.9] | 2.2 [1.6–2.9] | 0.0% | 0.9% | 0.0% | 1.4% | Score runoff (VSE\* WME\*), MARS-T (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +17 more |
| IMDS | rated | 92.9 [91.9–93.8] | 98.0 [97.5–98.4] | 0.4 [-0.1–0.9] | 1.3 [0.7–1.9] | 73 [68–79] | 100.0 [100.0–100.0] | 91.9 | 94.1 | 93.1 [92.1–94.0] | 98.5 [98.1–98.9] | 0.7 [0.2–1.1] | 1.0 [0.4–1.5] | 80 [75–86] | 94.8 [93.8–95.8] | 93.7 [92.6–94.7] | 98.4 [97.9–99.0] | 97.4 [96.6–98.1] | 97.5 [96.8–98.2] | 96.3 [95.4–97.1] | 1.8 [1.2–2.4] | 2.0 [1.4–2.6] | 0.0% | 0.1% | 0.0% | 8.6% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +12 more |
| Minimax | minimax-family | 92.9 [91.9–93.7] | 99.4 [99.1–99.6] | 0.8 [0.4–1.3] | 0.6 [0.2–0.8] | 89 [85–93] | 99.9 [99.8–99.9] | 81.3 | 81.5 | 92.8 [91.8–93.7] | 99.7 [99.5–99.8] | 0.9 [0.5–1.3] | 0.3 [0.0–0.5] | 93 [90–96] | 98.5 [97.9–99.0] | 97.6 [97.0–98.3] | 99.3 [98.9–99.7] | 99.0 [98.5–99.4] | 98.6 [98.0–99.1] | 98.3 [97.7–98.8] | 1.9 [1.4–2.5] | 1.8 [1.3–2.5] | 0.5% | 0.7% | 0.0% | 0.1% | — |
| Schulze | condorcet | 92.8 [91.9–93.7] | 99.4 [99.1–99.6] | 0.8 [0.3–1.3] | 0.6 [0.2–0.8] | 89 [85–93] | 99.9 [99.8–99.9] | 81.3 | 81.5 | 92.8 [91.8–93.7] | 99.7 [99.5–99.8] | 0.9 [0.5–1.3] | 0.3 [0.0–0.5] | 93 [90–96] | 98.5 [97.9–99.0] | 97.6 [97.0–98.3] | 99.3 [98.9–99.7] | 98.9 [98.4–99.4] | 98.6 [98.0–99.1] | 98.2 [97.5–98.7] | 1.9 [1.4–2.5] | 1.8 [1.3–2.5] | 0.5% | 0.7% | 0.0% | 0.1% | — |
| Ranked pairs | condorcet | 92.8 [91.9–93.7] | 99.3 [99.0–99.5] | 0.8 [0.3–1.2] | 0.6 [0.2–0.9] | 88 [84–92] | 99.9 [99.9–99.9] | 81.3 | 81.5 | 92.9 [91.9–93.8] | 99.6 [99.4–99.8] | 0.9 [0.5–1.3] | 0.4 [0.0–0.6] | 92 [89–95] | 98.5 [98.0–99.0] | 97.7 [97.1–98.4] | 99.3 [98.9–99.7] | 98.9 [98.4–99.4] | 98.4 [97.8–98.9] | 97.9 [97.3–98.5] | 2.0 [1.5–2.6] | 2.0 [1.4–2.6] | 0.4% | 0.6% | 0.0% | 0.1% | — |
| Approval cond. runoff | two-round | 92.8 [91.8–93.8] | 93.3 [92.3–94.3] | -1.2 [-2.1–-0.4] | 2.5 [1.5–3.5] | 53 [45–61] | 99.9 [99.9–100.0] | 69.1 | 69.5 | 93.5 [92.6–94.4] | 93.8 [92.9–94.8] | -0.5 [-1.3–0.2] | 1.7 [0.9–2.6] | 61 [53–71] | 87.6 [86.1–89.1] | 87.6 [86.1–89.0] | 93.3 [92.1–94.5] | 92.9 [91.7–94.2] | 92.7 [91.5–93.8] | 92.3 [91.1–93.5] | 2.2 [1.6–2.9] | 2.1 [1.5–2.8] | 0.0% | 0.5% | 0.0% | 2.2% | Smith//Score (VSE\* WME\*), STAR (VSE\* WME\*), Score runoff (VSE\* WME\*) +21 more |
| Copeland | condorcet | 92.8 [91.9–93.7] | 99.3 [99.1–99.5] | 0.7 [0.3–1.2] | 0.6 [0.3–0.9] | 88 [84–91] | 99.9 [99.9–99.9] | 81.3 | 81.5 | 92.8 [91.9–93.7] | 99.6 [99.4–99.7] | 0.9 [0.5–1.3] | 0.4 [0.1–0.6] | 92 [89–94] | 98.4 [97.9–99.0] | 97.8 [97.2–98.4] | 99.2 [98.8–99.6] | 98.9 [98.4–99.3] | 98.1 [97.5–98.7] | 97.8 [97.2–98.4] | 1.9 [1.4–2.5] | 1.8 [1.2–2.4] | 0.4% | 0.6% | 0.0% | 0.1% | — |
| MAV | median-based | 92.8 [91.9–93.7] | 93.5 [92.5–94.4] | -1.2 [-1.8–-0.4] | 2.5 [1.6–3.3] | 53 [46–60] | 100.0 [100.0–100.0] | 93.3 | 94.4 | 92.9 [92.0–93.8] | 93.7 [92.8–94.6] | -0.9 [-1.6–-0.2] | 2.3 [1.5–3.2] | 54 [48–62] | 86.1 [84.6–87.6] | 85.8 [84.3–87.3] | 91.8 [90.5–93.0] | 91.4 [90.0–92.7] | 91.6 [90.3–92.8] | 91.2 [89.9–92.4] | 1.9 [1.3–2.5] | 1.9 [1.4–2.5] | 0.0% | 0.1% | 0.0% | 4.5% | Smith//Score (VSE\* WME\*), ESR (VSE\* WME\*), STAR (VSE\* WME\*) +20 more |
| DMS | rated | 92.8 [91.8–93.7] | 92.0 [90.9–93.1] | -1.6 [-2.3–-0.8] | 2.6 [1.7–3.5] | 52 [45–59] | 100.0 [100.0–100.0] | 92.2 | 94.2 | 93.8 [92.9–94.6] | 94.8 [94.1–95.5] | -0.0 [-0.5–0.5] | 1.5 [0.7–2.2] | 64 [58–74] | 85.5 [83.9–87.1] | 81.7 [79.9–83.5] | 95.6 [94.7–96.6] | 90.3 [88.9–91.7] | 95.3 [94.4–96.3] | 90.6 [89.3–91.8] | 1.7 [1.1–2.3] | 1.8 [1.3–2.5] | 0.0% | 0.1% | 0.0% | 7.8% | Smith//Score (VSE\* WME\*), ESR (VSE\* WME\*), STAR (VSE\* WME\*) +24 more |
| Score | rated | 92.7 [91.8–93.7] | 92.6 [91.6–93.5] | -1.5 [-2.2–-0.7] | 2.6 [1.7–3.4] | 52 [45–59] | 100.0 [100.0–100.0] | 91.9 | 94.2 | 93.4 [92.5–94.3] | 95.3 [94.6–96.0] | -0.1 [-0.7–0.4] | 1.7 [0.9–2.5] | 61 [56–69] | 86.1 [84.6–87.6] | 81.9 [80.2–83.5] | 96.1 [95.2–97.0] | 89.6 [88.2–91.0] | 95.7 [94.8–96.6] | 89.7 [88.4–91.0] | 1.4 [0.8–1.9] | 1.6 [1.0–2.1] | 0.0% | 0.1% | 0.0% | 8.9% | Smith//Score (VSE\* WME\*), ESR (VSE\* WME\*), STAR (VSE\* WME\*) +23 more |
| MMPO | minimax-family | 92.7 [91.7–93.6] | 99.4 [99.1–99.6] | 0.7 [0.2–1.2] | 0.6 [0.2–0.8] | 89 [85–92] | 99.9 [99.9–99.9] | 81.3 | 81.5 | 92.8 [91.8–93.7] | 99.7 [99.5–99.8] | 0.9 [0.5–1.3] | 0.3 [0.0–0.5] | 93 [90–96] | 98.5 [97.9–99.0] | 97.6 [97.0–98.3] | 99.3 [98.9–99.7] | 98.8 [98.3–99.3] | 98.6 [98.0–99.1] | 98.1 [97.5–98.7] | 1.9 [1.4–2.5] | 1.9 [1.4–2.5] | 0.1% | 1.1% | 0.0% | 0.1% | Minimax (VSE\*) |
| MMWV | minimax-family | 92.7 [91.7–93.6] | 99.4 [99.1–99.6] | 0.7 [0.2–1.2] | 0.6 [0.2–0.8] | 89 [85–92] | 99.9 [99.9–99.9] | 81.3 | 81.5 | 92.8 [91.8–93.7] | 99.7 [99.5–99.8] | 0.9 [0.5–1.3] | 0.3 [0.0–0.5] | 93 [90–96] | 98.5 [97.9–99.0] | 97.6 [97.0–98.3] | 99.3 [98.9–99.7] | 98.9 [98.4–99.4] | 98.6 [98.0–99.1] | 98.1 [97.6–98.7] | 1.9 [1.4–2.5] | 1.9 [1.4–2.5] | 0.1% | 1.1% | 0.0% | 0.1% | Schulze (VSE\*), Minimax (VSE\*) |
| Kemeny-Young | condorcet | 92.7 [91.7–93.6] | 99.4 [99.1–99.6] | 0.7 [0.2–1.1] | 0.6 [0.2–0.9] | 88 [84–92] | 99.9 [99.9–99.9] | 81.3 | 81.5 | 92.8 [91.9–93.7] | 99.6 [99.4–99.8] | 0.9 [0.5–1.3] | 0.4 [0.1–0.6] | 93 [89–96] | 98.5 [97.9–99.0] | 97.6 [97.0–98.3] | 99.3 [98.9–99.7] | 98.7 [98.2–99.2] | 98.5 [98.0–99.0] | 98.0 [97.3–98.5] | 2.1 [1.5–2.7] | 2.1 [1.6–2.8] | 0.4% | 0.5% | 0.0% | 0.1% | — |
| Approval | approval-based | 92.6 [91.5–93.6] | 92.2 [91.2–93.2] | -1.7 [-2.5–-0.8] | 2.7 [1.7–3.8] | 50 [44–59] | 100.0 [100.0–100.0] | 70.1 | 70.3 | 92.7 [91.6–93.7] | 92.4 [91.3–93.4] | -1.6 [-2.4–-0.6] | 2.6 [1.6–3.7] | 51 [44–60] | 84.0 [82.4–85.5] | 84.0 [82.4–85.6] | 90.7 [89.3–92.1] | 90.5 [89.1–91.9] | 90.5 [89.1–91.8] | 90.4 [89.2–91.7] | 2.6 [1.9–3.4] | 2.6 [1.9–3.3] | 0.0% | 0.2% | 0.0% | 2.7% | Smith//Score (VSE\* WME\*), ESR (VSE\* WME\*), STAR (VSE\* WME\*) +26 more |
| ER-IRV | plurality-based | 92.4 [91.5–93.4] | 95.1 [94.1–95.9] | -0.9 [-1.5–-0.2] | 2.6 [1.7–3.4] | 54 [49–60] | 99.7 [99.7–99.7] | 81.2 | 81.8 | 92.8 [91.9–93.7] | 95.9 [95.1–96.6] | -0.4 [-1.0–0.2] | 2.1 [1.4–2.9] | 59 [54–64] | 92.9 [91.8–94.0] | 91.8 [90.5–92.9] | 95.6 [94.6–96.5] | 94.5 [93.4–95.5] | 94.5 [93.6–95.5] | 93.6 [92.5–94.6] | 2.2 [1.6–2.9] | 2.5 [1.8–3.3] | 0.8% | 1.7% | 0.2% | 0.5% | Smith//Score (VSE\* WME\*), ESR (VSE\* WME\*), Score runoff (VSE\* WME\*) +20 more |
| BTR-IRV | condorcet-based | 92.4 [91.4–93.3] | 98.8 [98.4–99.1] | 0.3 [-0.2–0.8] | 1.1 [0.6–1.5] | 80 [76–85] | 99.7 [99.7–99.8] | 81.2 | 81.6 | 92.9 [91.9–93.8] | 99.6 [99.3–99.7] | 0.9 [0.5–1.4] | 0.4 [0.1–0.6] | 92 [88–95] | 98.6 [98.0–99.1] | 96.5 [95.7–97.3] | 99.3 [98.9–99.6] | 98.1 [97.5–98.7] | 98.2 [97.7–98.8] | 97.0 [96.2–97.7] | 2.1 [1.5–2.8] | 2.4 [1.8–3.0] | 0.7% | 1.0% | 0.1% | 0.3% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +7 more |
| TTR | two-round | 91.6 [90.5–92.6] | 94.5 [93.5–95.4] | -1.6 [-2.4–-0.8] | 3.4 [2.4–4.3] | 48 [43–54] | 99.0 [98.9–99.1] | 59.9 | 60.7 | 92.0 [91.1–93.1] | 94.5 [93.5–95.4] | -1.3 [-2.0–-0.6] | 3.0 [2.1–3.8] | 50 [45–56] | 90.9 [89.6–92.1] | 90.9 [89.6–92.2] | 94.1 [92.9–95.1] | 94.0 [92.8–95.1] | 93.0 [91.9–94.0] | 93.1 [91.8–94.2] | 2.6 [1.9–3.3] | 2.8 [2.1–3.5] | 2.2% | 0.0% | 0.8% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +25 more |
| IRV | plurality-based | 90.7 [89.5–91.9] | 92.8 [91.6–94.0] | -2.7 [-3.8–-1.7] | 4.3 [3.2–5.5] | 41 [36–46] | 99.1 [99.0–99.2] | 80.5 | 81.8 | 92.8 [91.9–93.7] | 95.9 [95.1–96.6] | -0.4 [-1.0–0.2] | 2.1 [1.4–2.9] | 59 [54–64] | 92.9 [91.8–94.0] | 89.3 [88.0–90.7] | 95.6 [94.6–96.5] | 92.4 [91.1–93.6] | 94.5 [93.6–95.5] | 91.6 [90.3–92.7] | 2.2 [1.6–2.9] | 2.9 [2.1–3.7] | 2.8% | 1.7% | 1.1% | 0.1% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +29 more |
| Borda | positional | 90.4 [89.2–91.5] | 92.2 [91.2–93.2] | -3.1 [-4.1–-2.1] | 4.7 [3.7–5.8] | 38 [35–43] | 99.2 [99.1–99.2] | 78.6 | 80.6 | 91.0 [89.9–92.0] | 94.1 [93.3–94.9] | -2.1 [-3.1–-1.3] | 3.9 [3.0–4.9] | 45 [41–50] | 84.5 [82.9–86.0] | 81.9 [80.2–83.6] | 89.4 [87.8–90.7] | 87.3 [85.7–88.9] | 89.6 [88.2–90.9] | 87.8 [86.3–89.2] | 1.3 [0.8–1.8] | 1.7 [1.1–2.2] | 3.2% | 4.3% | 0.0% | 3.7% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +29 more |
| IRV-3 | plurality-based | 90.3 [89.0–91.6] | 91.5 [90.3–92.6] | -3.5 [-4.6–-2.4] | 5.0 [3.7–6.1] | 37 [32–42] | 99.3 [99.2–99.4] | 75.3 | 76.3 | 92.6 [91.7–93.5] | 94.4 [93.5–95.3] | -0.9 [-1.6–-0.2] | 2.5 [1.6–3.3] | 54 [48–60] | 90.3 [89.0–91.6] | 87.5 [86.1–88.9] | 93.7 [92.5–94.8] | 90.7 [89.2–92.0] | 92.9 [91.8–94.0] | 90.1 [88.8–91.3] | 2.3 [1.7–3.0] | 3.2 [2.5–4.0] | 2.5% | 0.9% | 1.1% | 0.1% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +30 more |
| Supplementary vote | plurality-based | 89.5 [88.2–90.8] | 87.6 [86.1–89.0] | -4.8 [-6.1–-3.6] | 5.8 [4.6–7.2] | 32 [28–37] | 99.6 [99.6–99.6] | 80.2 | 80.7 | 90.2 [89.0–91.3] | 88.3 [86.9–89.7] | -4.2 [-5.4–-3.1] | 5.2 [4.0–6.3] | 34 [31–39] | 81.6 [79.9–83.3] | 81.2 [79.5–83.0] | 86.2 [84.5–87.8] | 85.6 [83.9–87.3] | 86.4 [84.9–87.8] | 85.9 [84.4–87.4] | 3.3 [2.5–4.1] | 3.6 [2.9–4.5] | 2.1% | 1.0% | 0.2% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +32 more |
| Asset | delegated | 88.4 [87.1–89.7] | 85.9 [84.3–87.3] | -6.0 [-7.3–-4.6] | 6.9 [5.7–8.2] | 28 [25–32] | 99.3 [99.3–99.4] | 61.7 | 62.4 | 88.8 [87.5–90.0] | 86.7 [85.3–88.1] | -5.6 [-6.9–-4.3] | 6.5 [5.3–7.8] | 30 [27–33] | 76.9 [75.1–78.8] | 76.5 [74.7–78.3] | 80.9 [79.0–82.7] | 80.3 [78.5–82.1] | 81.6 [79.8–83.4] | 81.2 [79.5–82.8] | 2.5 [1.8–3.2] | 2.9 [2.1–3.6] | 1.8% | 0.0% | 0.4% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +32 more |
| Plurality | plurality-based | 80.9 [78.9–82.8] | 75.6 [73.6–77.6] | -13.5 [-15.5–-11.6] | 14.4 [12.5–16.5] | 15 [13–16] | 99.5 [99.4–99.5] | 60.3 | 60.9 | 79.0 [76.9–81.0] | 74.4 [72.2–76.4] | -15.4 [-17.4–-13.5] | 16.3 [14.4–18.4] | 13 [12–15] | 66.5 [64.5–68.5] | 67.2 [65.2–69.2] | 71.6 [69.5–73.6] | 72.7 [70.6–74.8] | 72.1 [70.2–74.0] | 73.4 [71.6–75.3] | 7.7 [6.6–8.9] | 6.8 [5.7–8.0] | 4.6% | 0.0% | 0.1% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +34 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Borda | 11.3% |
| Score | 9.3% |
| IMDS | 9.0% |
| ESR | 8.8% |
| Plurality | 8.5% |
| DMS | 8.3% |
| IRV | 6.1% |
| IRV-3 | 5.7% |
| ESR-MARS | 5.6% |
| 321 | 5.6% |
| Delegated 321 | 5.6% |
| Score runoff | 5.2% |
| MJ | 5.0% |
| Approval | 4.9% |
| MAV | 4.9% |
| MARS-T | 4.8% |
| MARS | 4.7% |
| TTR | 4.4% |
| ARA | 4.4% |
| Approval cond. runoff | 4.3% |
| STAR | 4.0% |
| Asset | 3.9% |
| Supplementary vote | 3.8% |
| ER-IRV | 3.3% |
| Approval runoff | 3.3% |
| Soft Minimax-40 | 3.0% |
| Smith//Score | 2.4% |
| BTR-score | 2.3% |
| BTR-IRV | 2.2% |
| Soft Minimax | 1.5% |
| Soft Minimax-D | 1.5% |
| Minimax | 1.3% |
| MMPO | 1.3% |
| Schulze | 1.3% |
| MMWV | 1.3% |
| Copeland | 1.2% |
| Ranked pairs | 1.1% |
| Kemeny-Young | 1.1% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Plurality | 35.2% | 1.09 | 0.90 | 94% |
| Supplementary vote | 23.6% | 1.06 | 0.59 | 99% |
| TTR | 20.7% | 0.52 | 0.59 | 99% |
| Asset | 18.1% | 0.28 | 0.07 | 100% |
| Approval | 17.9% | 0.64 | 0.30 | 100% |
| IRV-3 | 17.2% | 0.51 | 0.24 | 100% |
| Approval cond. runoff | 16.1% | 0.50 | 0.38 | 100% |
| IRV | 16.0% | 0.21 | 0.05 | 100% |
| ER-IRV | 16.0% | 0.21 | 0.05 | 100% |
| Approval runoff | 15.8% | 0.41 | 0.36 | 100% |
| MAV | 15.8% | 0.45 | 0.07 | 100% |
| 321 | 14.4% | 0.33 | 0.45 | 100% |
| Delegated 321 | 14.4% | 0.33 | 0.45 | 100% |
| MJ | 14.3% | 0.40 | 0.07 | 100% |
| DMS | 13.1% | 0.26 | 0.06 | 100% |
| Score runoff | 11.0% | 0.14 | 0.20 | 100% |
| Borda | 10.7% | 0.06 | 0.56 | 100% |
| Score | 9.7% | 0.22 | 0.08 | 100% |
| STAR | 9.7% | 0.20 | 0.18 | 100% |
| ESR | 9.7% | 0.24 | 0.07 | 100% |
| Soft Minimax-40 | 8.5% | 0.21 | 0.08 | 100% |
| ARA | 8.4% | 0.21 | 0.09 | 100% |
| ESR-MARS | 7.8% | 0.19 | 0.06 | 100% |
| IMDS | 7.6% | 0.19 | 0.09 | 100% |
| Soft Minimax | 7.4% | 0.18 | 0.08 | 100% |
| Soft Minimax-D | 6.7% | 0.16 | 0.05 | 100% |
| MARS | 6.2% | 0.16 | 0.06 | 100% |
| MARS-T | 6.2% | 0.16 | 0.05 | 100% |
| Smith//Score | 5.8% | 0.14 | 0.05 | 100% |
| BTR-score | 5.8% | 0.14 | 0.05 | 100% |
| Copeland | 3.9% | 0.01 | 0.07 | 100% |
| Ranked pairs | 3.0% | 0.02 | 0.04 | 100% |
| Schulze | 3.0% | 0.01 | 0.04 | 100% |
| Minimax | 3.0% | 0.01 | 0.04 | 100% |
| MMWV | 3.0% | 0.01 | 0.04 | 100% |
| MMPO | 3.0% | 0.01 | 0.04 | 100% |
| BTR-IRV | 2.9% | 0.02 | 0.04 | 100% |
| Kemeny-Young | 2.8% | 0.01 | 0.05 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 1957 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 43 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Soft Minimax-40 | 91.6% (92.2%) | 6.1% (6.0%) | 2.4% (1.7%) | 74.4% (67.4%) | 96.9% (97.0%) | 92.2% |
| MARS-T | 92.1% (92.3%) | 6.2% (5.9%) | 1.7% (1.8%) | 76.7% (72.1%) | 96.2% (97.1%) | 90.9% |
| MARS | 92.1% (92.4%) | 6.0% (5.5%) | 1.9% (2.1%) | 79.1% (74.4%) | 96.7% (97.7%) | 91.6% |
| STAR | 91.1% (92.4%) | 5.9% (5.3%) | 3.0% (2.3%) | 67.4% (69.8%) | 93.7% (92.7%) | 87.9% |
| ESR-MARS | 91.6% (92.0%) | 6.0% (5.8%) | 2.4% (2.1%) | 76.7% (74.4%) | 95.1% (96.8%) | 89.7% |
| Soft Minimax-D | 93.2% (93.5%) | 5.1% (4.9%) | 1.7% (1.6%) | 79.1% (72.1%) | 99.4% (99.5%) | 93.9% |
| ARA | 91.0% (91.3%) | 6.6% (6.3%) | 2.4% (2.4%) | 62.8% (53.5%) | 94.6% (94.6%) | 90.5% |
| Smith//Score | 92.3% (93.1%) | 5.3% (4.9%) | 2.4% (2.0%) | 81.4% (81.4%) | 100.0% (100.0%) | 93.5% |
| ESR | 92.0% (92.8%) | 6.2% (6.0%) | 1.8% (1.2%) | 72.1% (65.1%) | 95.8% (98.7%) | 89.9% |
| BTR-score | 92.3% (93.1%) | 5.4% (4.9%) | 2.3% (2.1%) | 81.4% (79.1%) | 100.0% (100.0%) | 93.6% |
| 321 | 81.2% (79.8%) | 9.2% (8.8%) | 9.6% (11.4%) | 79.1% (79.1%) | 97.1% (98.3%) | 86.9% |
| Delegated 321 | 81.2% (79.8%) | 9.2% (8.8%) | 9.6% (11.4%) | 79.1% (79.1%) | 97.1% (98.3%) | 86.9% |
| MJ | 91.2% (91.6%) | 6.4% (6.3%) | 2.4% (2.1%) | 69.8% (60.5%) | 90.8% (91.9%) | 87.6% |
| Soft Minimax | 93.6% (93.9%) | 4.9% (4.6%) | 1.5% (1.5%) | 72.1% (69.8%) | 100.0% (100.0%) | 94.4% |
| IMDS | 92.5% (93.7%) | 5.2% (4.6%) | 2.3% (1.7%) | 81.4% (86.0%) | 99.9% (99.8%) | 93.7% |
| Minimax | 97.0% (98.2%) | 1.6% (1.2%) | 1.3% (0.7%) | 46.5% (44.2%) | 100.0% (100.0%) | 97.6% |
| Schulze | 97.1% (98.2%) | 1.6% (1.2%) | 1.3% (0.7%) | 46.5% (44.2%) | 100.0% (100.0%) | 97.6% |
| Ranked pairs | 97.1% (98.2%) | 1.6% (1.2%) | 1.3% (0.6%) | 48.8% (48.8%) | 100.0% (100.0%) | 97.7% |
| Copeland | 97.3% (98.1%) | 1.5% (1.2%) | 1.1% (0.7%) | 46.5% (39.5%) | 100.0% (100.0%) | 97.8% |
| MAV | 91.4% (91.6%) | 6.5% (6.5%) | 2.1% (1.8%) | 69.8% (60.5%) | 89.7% (90.4%) | 85.8% |
| DMS | 90.0% (90.6%) | 7.5% (7.1%) | 2.5% (2.2%) | 67.4% (69.8%) | 86.9% (89.6%) | 81.7% |
| Score | 90.7% (91.1%) | 7.0% (6.4%) | 2.3% (2.5%) | 74.4% (67.4%) | 86.4% (89.8%) | 81.9% |
| MMPO | 97.1% (98.2%) | 1.7% (1.2%) | 1.2% (0.7%) | 48.8% (44.2%) | 99.9% (100.0%) | 97.6% |
| MMWV | 97.1% (98.2%) | 1.7% (1.2%) | 1.2% (0.7%) | 46.5% (44.2%) | 100.0% (100.0%) | 97.6% |
| Kemeny-Young | 96.9% (98.2%) | 1.8% (1.2%) | 1.3% (0.6%) | 48.8% (48.8%) | 100.0% (100.0%) | 97.6% |
| ER-IRV | 96.5% (98.7%) | 2.8% (0.9%) | 0.7% (0.5%) | 53.5% (39.5%) | 93.9% (94.1%) | 91.8% |
| BTR-IRV | 96.2% (98.5%) | 2.4% (1.1%) | 1.4% (0.4%) | 65.1% (62.8%) | 100.0% (100.0%) | 96.5% |
| IRV | 94.3% (98.7%) | 4.3% (0.9%) | 1.4% (0.5%) | 62.8% (39.5%) | 92.9% (94.1%) | 89.3% |
| Borda | 94.5% (98.8%) | 2.7% (0.9%) | 2.8% (0.3%) | 37.2% (11.6%) | 84.2% (85.1%) | 81.9% |
| IRV-3 | 88.2% (91.0%) | 9.1% (7.0%) | 2.7% (2.0%) | 88.4% (83.7%) | 96.8% (97.6%) | 87.5% |
| Supplementary vote | 90.1% (91.2%) | 9.6% (8.6%) | 0.3% (0.2%) | 58.1% (51.2%) | 89.5% (89.4%) | 81.2% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| Soft Minimax-40 | 0.9% (0.4%) | 11.3% (11.3%) | 0.9% (0.9%) |
| Score runoff | 0.4% (0.2%) | — (—) | — (—) |
| MARS-T | 0.0% (0.1%) | 11.1% (11.0%) | 0.9% (0.9%) |
| MARS | 0.2% (0.1%) | 11.2% (11.2%) | 0.9% (0.9%) |
| STAR | 0.6% (0.3%) | 13.8% (13.8%) | 1.1% (1.1%) |
| ESR-MARS | 0.1% (0.1%) | 11.0% (11.2%) | 0.9% (0.9%) |
| Soft Minimax-D | 0.1% (0.1%) | 11.1% (10.9%) | 0.9% (0.9%) |
| ARA | 1.1% (0.5%) | 12.8% (12.2%) | 1.0% (1.0%) |
| Smith//Score | 0.1% (0.0%) | 10.9% (11.2%) | 0.8% (0.9%) |
| ESR | 0.7% (0.4%) | 10.5% (11.2%) | 0.9% (0.9%) |
| BTR-score | 0.1% (0.0%) | 10.8% (11.3%) | 0.8% (0.9%) |
| 321 | 0.2% (0.4%) | 27.0% (27.8%) | 2.1% (2.1%) |
| Delegated 321 | 0.2% (0.4%) | 27.0% (27.8%) | 2.1% (2.1%) |
| MJ | 0.0% (0.0%) | 11.0% (10.7%) | 1.0% (0.9%) |
| Soft Minimax | 0.9% (0.9%) | 11.5% (11.4%) | 0.9% (0.9%) |
| Approval runoff | 1.1% (0.9%) | — (—) | — (—) |
| IMDS | 1.3% (0.7%) | 10.8% (11.4%) | 0.9% (1.0%) |
| Minimax | 0.8% (0.9%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Schulze | 0.8% (0.9%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Ranked pairs | 0.8% (0.6%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Approval cond. runoff | 0.8% (0.8%) | — (—) | — (—) |
| Copeland | 1.6% (1.3%) | 0.5% (0.0%) | 0.0% (0.0%) |
| MAV | 0.2% (0.1%) | 11.3% (10.5%) | 1.0% (0.9%) |
| DMS | 0.0% (0.0%) | 10.1% (10.3%) | 0.8% (0.8%) |
| Score | 0.4% (0.3%) | 11.3% (11.7%) | 0.9% (0.9%) |
| MMPO | 0.9% (0.9%) | 0.5% (0.0%) | 0.0% (0.0%) |
| MMWV | 0.8% (0.9%) | 0.4% (0.0%) | 0.0% (0.0%) |
| Kemeny-Young | 0.6% (0.4%) | 0.3% (0.0%) | 0.0% (0.0%) |
| Approval | 1.1% (0.5%) | — (—) | — (—) |
| ER-IRV | 0.9% (0.9%) | 0.7% (0.0%) | 0.0% (0.0%) |
| BTR-IRV | 0.1% (0.0%) | 0.2% (0.0%) | 0.0% (0.0%) |
| TTR | 1.0% (0.9%) | — (—) | — (—) |
| IRV | 0.5% (0.9%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Borda | 0.4% (0.4%) | 4.0% (0.0%) | 0.2% (0.0%) |
| IRV-3 | 0.7% (0.7%) | 11.0% (10.3%) | 0.8% (0.7%) |
| Supplementary vote | 0.8% (0.7%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Asset | 0.9% (1.0%) | — (—) | — (—) |
| Plurality | 1.5% (1.2%) | — (—) | — (—) |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

## c3: utility linear

2000 elections (seeds 1), 1957 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, linear utility, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 97.9%; it also has the highest total utility in 86.6% of those; electing it loses 2.5% of VSE; favourite ÷ least liked: — (utility not positive). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 97.1 to 97.2; with the best VSE (100) WME 95.3 (nothing lower is possible there either); the most balanced point VSE 98.9 / WME 99.3. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 98.3 (at WME 98.2); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 98.2 (at WME 98.4); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -65.2 / WME -47.7. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 97.0 / WME 99.9. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 97.4 / 96.9; normalized utility winner 97.7 / 93.9; best worst margin 97.1 / 100.0; Condorcet winner, else random ballot 96.0 / 99.0; Condorcet winner, else utility winner 97.6 / 99.9; random pair, majority 27.4 / 2.9; nearest the median voter 99.4 / 96.7; nearest the mean voter 97.1 / 92.4.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Soft Minimax-40, MARS, Soft Minimax-D, BTR-score, Soft Minimax, Minimax, Ranked pairs, MMWV, Kemeny-Young.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): BTR-score, Minimax, Ranked pairs, Schulze, MMWV, Kemeny-Young, Copeland.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 96.0 / 99.0; its mirror on the utility side 96.7 / 92.8; random pair 27.4 / 2.9. No better than a lottery of them (Lead ≤ 0): with learned ballots 321, Delegated 321, MJ, Approval runoff, MAV, DMS, Approval, Score, Approval cond. runoff, ER-IRV, TTR, Borda, IRV, IRV-3, Supplementary vote, Asset, Plurality; with honest ballots 321, Delegated 321, Approval runoff, MAV, Approval, Approval cond. runoff, ER-IRV, TTR, Borda, IRV, IRV-3, Supplementary vote, Asset, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Soft Minimax-40 | hybrid | 97.8 [97.4–98.2] | 96.4 [95.7–97.0] | 1.3 [0.9–1.8] | 0.5 [0.1–0.9] | 82 [74–92] | 100.0 [100.0–100.0] | 93.2 | 94.1 | 97.8 [97.4–98.2] | 96.3 [95.6–97.0] | 1.3 [0.9–1.8] | 0.5 [0.1–0.9] | 82 [73–92] | 89.1 [87.7–90.6] | 88.7 [87.2–90.2] | 98.1 [97.4–98.7] | 97.2 [96.3–97.9] | 97.8 [97.2–98.4] | 97.0 [96.2–97.7] | 0.2 [0.1–0.5] | 0.2 [0.1–0.5] | 0.0% | 0.1% | 0.0% | 2.7% | BTR-score (WME\*), Soft Minimax-D (WME\*) |
| ESR-MARS | hybrid | 97.7 [97.4–98.1] | 95.9 [95.2–96.6] | 1.2 [0.8–1.8] | 0.5 [0.2–0.9] | 80 [73–90] | 100.0 [100.0–100.0] | 92.6 | 94.1 | 97.9 [97.6–98.3] | 96.8 [96.2–97.4] | 1.5 [1.1–2.0] | 0.3 [-0.0–0.7] | 87 [79–97] | 89.8 [88.4–91.1] | 88.3 [86.9–89.7] | 98.0 [97.3–98.6] | 96.0 [95.1–96.9] | 97.9 [97.2–98.5] | 96.0 [95.2–96.9] | 0.2 [0.1–0.5] | 0.4 [0.1–0.7] | 0.0% | 0.1% | 0.0% | 5.0% | Smith//Score (WME\*), BTR-score (WME\*), MARS (WME\*) +1 more |
| MARS | hybrid | 97.7 [97.3–98.1] | 96.7 [96.1–97.3] | 1.3 [0.8–1.8] | 0.6 [0.1–1.0] | 80 [71–90] | 100.0 [100.0–100.0] | 92.8 | 94.1 | 97.9 [97.5–98.3] | 97.0 [96.3–97.5] | 1.5 [1.1–2.0] | 0.3 [-0.0–0.7] | 87 [79–96] | 90.1 [88.7–91.3] | 89.6 [88.2–90.8] | 97.8 [97.1–98.5] | 97.2 [96.4–98.0] | 97.8 [97.2–98.4] | 97.2 [96.5–97.9] | 0.3 [0.1–0.5] | 0.4 [0.1–0.8] | 0.0% | 0.1% | 0.0% | 4.3% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax-D (WME\*) |
| Soft Minimax-D | hybrid | 97.7 [97.3–98.1] | 97.4 [96.8–97.9] | 1.3 [0.9–1.8] | 0.6 [0.2–1.0] | 80 [73–89] | 100.0 [100.0–100.0] | 93.6 | 94.1 | 97.7 [97.3–98.1] | 97.5 [96.9–98.1] | 1.3 [0.9–1.8] | 0.6 [0.2–1.0] | 81 [74–90] | 92.2 [90.9–93.3] | 92.1 [90.8–93.2] | 98.1 [97.4–98.8] | 97.8 [97.0–98.4] | 97.8 [97.2–98.5] | 97.4 [96.7–98.0] | 0.4 [0.1–0.6] | 0.4 [0.1–0.7] | 0.0% | 0.1% | 0.0% | 1.3% | Smith//Score (WME\*), BTR-score (WME\*) |
| Score runoff | two-round | 97.6 [97.1–97.9] | 95.3 [94.5–96.1] | 1.0 [0.5–1.4] | 0.7 [0.3–1.2] | 75 [67–84] | 99.9 [99.9–99.9] | 92.2 | 94.0 | 97.5 [97.1–97.9] | 94.6 [93.7–95.4] | 0.9 [0.5–1.3] | 0.8 [0.4–1.2] | 74 [66–81] | 86.5 [84.9–87.9] | 88.1 [86.7–89.6] | 98.5 [97.9–99.1] | 97.6 [96.8–98.3] | 98.5 [98.0–99.0] | 97.5 [96.8–98.2] | 0.7 [0.3–1.0] | 0.5 [0.2–0.9] | 0.0% | 1.2% | 0.0% | 3.5% | Ranked pairs (WME\*), Minimax (WME\*), Smith//Score (WME\*) +8 more |
| MARS-T | hybrid | 97.5 [97.1–97.9] | 96.2 [95.5–96.8] | 1.0 [0.5–1.6] | 0.8 [0.3–1.2] | 74 [66–83] | 100.0 [100.0–100.0] | 92.8 | 94.0 | 97.9 [97.5–98.3] | 96.7 [96.0–97.3] | 1.5 [1.0–1.9] | 0.4 [-0.0–0.8] | 86 [78–95] | 89.5 [88.0–90.8] | 88.6 [87.0–89.9] | 97.8 [97.1–98.5] | 96.8 [96.0–97.6] | 97.8 [97.2–98.5] | 96.8 [96.0–97.5] | 0.3 [0.1–0.5] | 0.4 [0.1–0.8] | 0.0% | 0.1% | 0.0% | 4.3% | MARS (VSE\* WME\*), Ranked pairs (WME\*), Schulze (WME\*) +7 more |
| BTR-score | hybrid | 97.5 [97.0–97.9] | 97.9 [97.4–98.4] | 1.2 [0.7–1.7] | 0.7 [0.3–1.1] | 78 [72–85] | 100.0 [100.0–100.0] | 93.5 | 94.1 | 97.5 [97.1–98.0] | 97.9 [97.3–98.4] | 1.2 [0.8–1.7] | 0.7 [0.3–1.1] | 80 [74–87] | 93.2 [92.0–94.3] | 93.0 [91.9–94.2] | 97.8 [97.1–98.6] | 97.4 [96.7–98.2] | 97.5 [96.7–98.2] | 97.2 [96.4–97.9] | 0.3 [0.1–0.5] | 0.3 [0.1–0.5] | 0.0% | 0.1% | 0.0% | 1.8% | — |
| Smith//Score | condorcet-based | 97.5 [97.0–97.9] | 97.8 [97.2–98.3] | 1.2 [0.7–1.7] | 0.8 [0.4–1.1] | 78 [71–84] | 100.0 [100.0–100.0] | 93.4 | 94.1 | 97.5 [97.1–97.9] | 97.9 [97.3–98.4] | 1.2 [0.8–1.7] | 0.7 [0.3–1.1] | 80 [73–87] | 93.2 [92.0–94.3] | 92.8 [91.6–94.0] | 97.8 [97.1–98.6] | 97.4 [96.6–98.2] | 97.5 [96.8–98.2] | 97.1 [96.4–97.9] | 0.4 [0.1–0.6] | 0.3 [0.1–0.5] | 0.0% | 0.2% | 0.0% | 2.0% | Minimax (WME\*) |
| ARA | hybrid | 97.4 [97.0–97.8] | 95.9 [95.2–96.6] | 0.9 [0.4–1.4] | 0.9 [0.5–1.3] | 71 [63–79] | 100.0 [100.0–100.0] | 92.8 | 94.1 | 97.3 [96.9–97.7] | 95.5 [94.7–96.2] | 0.8 [0.4–1.3] | 0.9 [0.5–1.4] | 69 [62–77] | 87.6 [86.0–89.0] | 88.0 [86.4–89.5] | 96.6 [95.7–97.4] | 96.1 [95.1–97.0] | 96.6 [95.8–97.3] | 96.2 [95.3–97.0] | 0.4 [0.2–0.8] | 0.4 [0.2–0.8] | 0.0% | 0.1% | 0.0% | 3.9% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +11 more |
| ESR | rated | 97.3 [96.8–97.8] | 96.6 [95.9–97.3] | 0.9 [0.4–1.4] | 1.0 [0.5–1.4] | 70 [63–79] | 100.0 [100.0–100.0] | 91.7 | 94.1 | 97.6 [97.2–98.0] | 97.8 [97.2–98.3] | 1.3 [0.8–1.8] | 0.6 [0.2–1.0] | 80 [73–90] | 92.9 [91.7–94.0] | 90.1 [88.8–91.4] | 97.9 [97.2–98.6] | 95.2 [94.2–96.2] | 97.6 [96.8–98.2] | 95.0 [94.0–96.0] | 0.4 [0.1–0.8] | 0.5 [0.2–0.8] | 0.0% | 0.1% | 0.0% | 8.5% | Soft Minimax-D (VSE\* WME\*), Ranked pairs (WME\*), Schulze (WME\*) +11 more |
| STAR | hybrid | 97.3 [96.8–97.7] | 94.5 [93.7–95.3] | 0.6 [0.2–1.1] | 1.0 [0.6–1.5] | 68 [61–76] | 100.0 [100.0–100.0] | 92.7 | 94.0 | 97.4 [97.0–97.8] | 94.3 [93.4–95.1] | 0.8 [0.3–1.2] | 0.8 [0.4–1.3] | 72 [64–79] | 85.3 [83.6–86.9] | 85.8 [84.1–87.3] | 97.7 [96.9–98.4] | 96.2 [95.3–97.1] | 97.9 [97.2–98.5] | 96.0 [95.1–96.8] | 0.6 [0.3–0.9] | 0.6 [0.3–0.9] | 0.0% | 0.4% | 0.0% | 3.4% | Soft Minimax-40 (VSE\* WME\*), ESR-MARS (VSE\* WME\*), Ranked pairs (WME\*) +15 more |
| IMDS | rated | 97.3 [96.8–97.7] | 97.8 [97.3–98.3] | 1.0 [0.5–1.4] | 0.9 [0.5–1.3] | 75 [68–81] | 100.0 [100.0–100.0] | 91.6 | 94.1 | 97.3 [96.8–97.8] | 98.0 [97.5–98.5] | 1.1 [0.6–1.5] | 0.8 [0.4–1.2] | 77 [70–83] | 93.5 [92.5–94.6] | 92.8 [91.7–93.9] | 97.7 [96.9–98.4] | 97.1 [96.3–97.9] | 97.2 [96.5–97.9] | 96.4 [95.6–97.2] | 0.5 [0.2–0.9] | 0.7 [0.3–1.0] | 0.0% | 0.1% | 0.0% | 8.6% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +4 more |
| Soft Minimax | hybrid | 97.2 [96.7–97.7] | 98.0 [97.4–98.5] | 1.0 [0.6–1.4] | 0.9 [0.5–1.3] | 76 [69–81] | 100.0 [100.0–100.0] | 93.7 | 94.1 | 97.3 [96.8–97.7] | 98.1 [97.5–98.6] | 1.0 [0.6–1.5] | 0.8 [0.5–1.2] | 77 [71–83] | 93.6 [92.5–94.6] | 93.6 [92.5–94.7] | 97.7 [96.9–98.4] | 97.5 [96.7–98.2] | 97.3 [96.6–98.0] | 97.1 [96.3–97.8] | 0.5 [0.2–0.9] | 0.5 [0.2–0.9] | 0.0% | 0.1% | 0.0% | 1.3% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +5 more |
| Minimax | minimax-family | 97.1 [96.6–97.5] | 99.4 [99.1–99.6] | 1.0 [0.6–1.4] | 0.5 [0.3–0.8] | 89 [85–93] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 97.0 [96.5–97.5] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 93 [89–95] | 98.5 [97.9–99.0] | 97.7 [97.1–98.4] | 99.2 [98.8–99.6] | 98.9 [98.4–99.4] | 98.5 [97.9–99.0] | 98.2 [97.5–98.8] | 0.7 [0.3–1.0] | 0.7 [0.3–1.0] | 0.4% | 0.6% | 0.0% | 0.1% | — |
| Ranked pairs | condorcet | 97.0 [96.5–97.5] | 99.4 [99.2–99.6] | 0.9 [0.5–1.4] | 0.5 [0.3–0.8] | 90 [86–93] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 97.1 [96.6–97.5] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 92 [89–95] | 98.5 [98.0–99.0] | 97.9 [97.3–98.5] | 99.2 [98.8–99.6] | 98.9 [98.4–99.4] | 98.4 [97.8–99.0] | 98.1 [97.5–98.7] | 0.7 [0.4–1.1] | 0.7 [0.4–1.0] | 0.4% | 0.5% | 0.0% | 0.1% | — |
| Schulze | condorcet | 97.0 [96.5–97.5] | 99.4 [99.1–99.6] | 0.9 [0.5–1.4] | 0.5 [0.3–0.8] | 90 [85–93] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 97.0 [96.5–97.5] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 93 [89–95] | 98.5 [97.9–99.0] | 97.8 [97.2–98.4] | 99.2 [98.8–99.6] | 98.9 [98.4–99.4] | 98.5 [97.9–99.0] | 98.2 [97.6–98.8] | 0.7 [0.3–1.0] | 0.7 [0.3–1.0] | 0.4% | 0.5% | 0.0% | 0.1% | — |
| MMWV | minimax-family | 97.0 [96.5–97.5] | 99.4 [99.2–99.6] | 0.9 [0.5–1.4] | 0.5 [0.3–0.7] | 90 [86–93] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 97.0 [96.5–97.5] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 93 [89–95] | 98.5 [97.9–99.0] | 97.9 [97.3–98.5] | 99.2 [98.8–99.6] | 98.9 [98.4–99.4] | 98.5 [97.9–99.0] | 98.2 [97.5–98.7] | 0.7 [0.3–1.0] | 0.6 [0.3–0.9] | 0.1% | 0.9% | 0.0% | 0.1% | — |
| MMPO | minimax-family | 97.0 [96.4–97.4] | 99.4 [99.2–99.6] | 0.9 [0.5–1.3] | 0.5 [0.3–0.8] | 90 [86–93] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 97.0 [96.5–97.5] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 93 [89–95] | 98.5 [97.9–99.0] | 97.9 [97.3–98.5] | 99.2 [98.8–99.6] | 98.9 [98.4–99.4] | 98.5 [97.9–99.0] | 98.2 [97.5–98.7] | 0.7 [0.3–1.0] | 0.7 [0.3–1.0] | 0.1% | 0.9% | 0.0% | 0.1% | Minimax (VSE\*) |
| Kemeny-Young | condorcet | 96.9 [96.4–97.4] | 99.5 [99.2–99.7] | 0.9 [0.5–1.3] | 0.5 [0.3–0.7] | 90 [86–93] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 97.0 [96.5–97.5] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 93 [89–96] | 98.5 [97.9–99.0] | 97.8 [97.2–98.5] | 99.2 [98.8–99.6] | 98.8 [98.2–99.3] | 98.5 [97.9–99.0] | 98.1 [97.5–98.7] | 0.8 [0.4–1.1] | 0.8 [0.4–1.1] | 0.4% | 0.4% | 0.0% | 0.1% | — |
| Copeland | condorcet | 96.9 [96.4–97.4] | 99.4 [99.2–99.6] | 0.9 [0.5–1.3] | 0.6 [0.3–0.8] | 89 [86–92] | 99.9 [99.9–99.9] | 82.0 | 82.2 | 97.1 [96.6–97.5] | 99.6 [99.4–99.7] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 92 [89–94] | 98.4 [97.9–99.0] | 98.0 [97.4–98.6] | 99.2 [98.7–99.6] | 98.8 [98.3–99.3] | 98.3 [97.7–98.8] | 98.0 [97.4–98.6] | 0.6 [0.3–0.9] | 0.7 [0.4–1.0] | 0.4% | 0.5% | 0.0% | 0.1% | — |
| 321 | hybrid | 96.6 [96.1–97.1] | 91.7 [90.4–92.9] | -0.2 [-0.8–0.5] | 1.7 [1.2–2.2] | 56 [49–63] | 100.0 [99.9–100.0] | 83.9 | 85.3 | 96.6 [96.1–97.1] | 91.4 [90.3–92.6] | -0.1 [-0.7–0.5] | 1.6 [1.1–2.1] | 57 [50–64] | 83.2 [81.5–84.8] | 83.6 [81.9–85.3] | 93.0 [91.9–94.1] | 92.0 [90.7–93.2] | 92.8 [91.7–93.9] | 92.0 [90.9–93.1] | 0.7 [0.4–1.1] | 0.8 [0.4–1.1] | 0.0% | 0.4% | 0.0% | 4.6% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), ESR (VSE\* WME\*) +21 more |
| Delegated 321 | delegated | 96.6 [96.1–97.1] | 91.7 [90.4–92.9] | -0.2 [-0.8–0.5] | 1.7 [1.2–2.2] | 56 [49–63] | — | — | — | 96.6 [96.1–97.1] | 91.4 [90.3–92.6] | -0.1 [-0.7–0.5] | 1.6 [1.1–2.1] | 57 [50–64] | 83.2 [81.5–84.8] | 83.6 [81.9–85.3] | 93.0 [91.9–94.1] | 92.0 [90.7–93.2] | 92.8 [91.7–93.9] | 92.0 [90.9–93.1] | 0.7 [0.4–1.1] | 0.8 [0.4–1.1] | 0.0% | 0.4% | 0.0% | 4.6% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), ESR (VSE\* WME\*) +21 more |
| BTR-IRV | condorcet-based | 96.6 [96.0–97.1] | 98.8 [98.5–99.2] | 0.5 [-0.0–1.0] | 1.1 [0.7–1.4] | 80 [76–85] | 99.8 [99.7–99.8] | 81.9 | 82.3 | 97.1 [96.5–97.5] | 99.6 [99.3–99.7] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 92 [88–95] | 98.6 [98.0–99.1] | 96.7 [95.9–97.4] | 99.1 [98.6–99.5] | 97.9 [97.2–98.5] | 98.2 [97.6–98.8] | 97.0 [96.3–97.8] | 0.7 [0.4–1.1] | 1.1 [0.7–1.7] | 0.7% | 0.8% | 0.1% | 0.2% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +4 more |
| MJ | median-based | 96.4 [95.9–96.9] | 94.6 [93.8–95.4] | -0.1 [-0.7–0.5] | 1.8 [1.3–2.3] | 54 [48–62] | 100.0 [100.0–100.0] | 93.1 | 94.3 | 96.6 [96.1–97.1] | 94.9 [94.0–95.6] | 0.1 [-0.5–0.7] | 1.6 [1.2–2.1] | 57 [51–64] | 87.2 [85.8–88.6] | 86.7 [85.2–88.2] | 92.0 [90.6–93.3] | 91.3 [90.0–92.7] | 92.1 [91.0–93.2] | 91.5 [90.4–92.8] | 0.7 [0.4–1.0] | 0.9 [0.5–1.2] | 0.0% | 0.1% | 0.0% | 4.2% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Minimax (VSE\* WME\*) +18 more |
| Approval runoff | two-round | 96.2 [95.7–96.8] | 95.3 [94.5–96.0] | -0.2 [-0.8–0.4] | 2.0 [1.4–2.6] | 53 [47–60] | 99.9 [99.8–99.9] | 64.4 | 64.8 | 96.4 [95.8–96.9] | 95.1 [94.4–95.9] | -0.1 [-0.7–0.5] | 1.9 [1.3–2.4] | 54 [48–62] | 89.4 [88.1–90.8] | 89.7 [88.3–91.0] | 93.5 [92.2–94.7] | 93.8 [92.6–94.9] | 92.7 [91.5–93.9] | 93.1 [91.9–94.2] | 1.2 [0.8–1.8] | 1.2 [0.8–1.8] | 0.0% | 0.8% | 0.0% | 1.4% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +18 more |
| MAV | median-based | 95.9 [95.3–96.5] | 94.0 [93.2–94.9] | -0.6 [-1.2–0.1] | 2.3 [1.8–3.0] | 48 [43–54] | 100.0 [100.0–100.0] | 93.3 | 94.4 | 95.8 [95.2–96.4] | 93.8 [92.9–94.7] | -0.7 [-1.4–-0.0] | 2.4 [1.9–3.0] | 46 [42–53] | 85.9 [84.3–87.4] | 85.9 [84.4–87.4] | 89.7 [88.2–91.2] | 89.8 [88.2–91.3] | 90.1 [88.8–91.5] | 90.1 [88.7–91.4] | 1.0 [0.6–1.5] | 1.1 [0.7–1.5] | 0.0% | 0.1% | 0.0% | 3.8% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +21 more |
| DMS | rated | 95.9 [95.4–96.5] | 91.0 [89.8–92.1] | -0.8 [-1.4–-0.2] | 2.3 [1.8–2.9] | 47 [43–53] | 100.0 [100.0–100.0] | 91.8 | 94.1 | 97.2 [96.7–97.6] | 93.3 [92.3–94.2] | 0.5 [0.1–0.9] | 1.1 [0.7–1.5] | 66 [60–74] | 82.5 [80.8–84.2] | 80.3 [78.5–82.1] | 95.4 [94.4–96.4] | 89.6 [88.1–91.0] | 96.0 [95.0–96.8] | 91.1 [89.8–92.3] | 0.5 [0.2–0.9] | 0.5 [0.2–0.8] | 0.0% | 0.1% | 0.0% | 8.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +26 more |
| Approval | approval-based | 95.9 [95.2–96.4] | 94.0 [93.2–94.8] | -0.7 [-1.3–0.0] | 2.4 [1.8–3.0] | 47 [43–54] | 100.0 [100.0–100.0] | 66.0 | 66.3 | 95.9 [95.3–96.5] | 94.4 [93.6–95.2] | -0.6 [-1.2–0.1] | 2.3 [1.8–3.0] | 48 [44–55] | 85.8 [84.3–87.3] | 85.2 [83.8–86.8] | 91.5 [90.2–92.9] | 91.1 [89.7–92.5] | 91.4 [90.1–92.7] | 90.9 [89.7–92.1] | 1.1 [0.6–1.5] | 1.1 [0.7–1.6] | 0.0% | 0.2% | 0.0% | 2.9% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +21 more |
| Score | rated | 95.8 [95.3–96.4] | 91.1 [89.8–92.2] | -0.9 [-1.5–-0.2] | 2.4 [1.9–3.0] | 46 [42–52] | 100.0 [100.0–100.0] | 91.5 | 94.1 | 96.9 [96.4–97.4] | 93.0 [92.0–93.9] | 0.2 [-0.2–0.6] | 1.4 [0.9–1.9] | 61 [55–67] | 82.7 [81.0–84.4] | 80.4 [78.6–82.2] | 95.7 [94.8–96.7] | 89.0 [87.4–90.3] | 96.2 [95.4–97.0] | 90.1 [88.7–91.4] | 0.8 [0.4–1.2] | 0.7 [0.4–1.1] | 0.0% | 0.1% | 0.0% | 8.9% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +26 more |
| Approval cond. runoff | two-round | 95.8 [95.1–96.5] | 94.7 [93.9–95.5] | -0.6 [-1.3–0.0] | 2.4 [1.8–3.1] | 48 [44–55] | 99.9 [99.9–100.0] | 65.1 | 65.5 | 96.1 [95.5–96.7] | 95.1 [94.3–95.9] | -0.3 [-1.0–0.3] | 2.2 [1.6–2.7] | 52 [46–58] | 88.2 [86.8–89.6] | 88.1 [86.7–89.5] | 93.0 [91.8–94.3] | 92.9 [91.6–94.1] | 92.5 [91.3–93.7] | 92.2 [91.1–93.4] | 1.3 [0.8–1.8] | 1.4 [0.9–1.9] | 0.0% | 0.4% | 0.0% | 2.5% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +20 more |
| ER-IRV | plurality-based | 95.5 [94.8–96.1] | 95.3 [94.4–96.1] | -0.9 [-1.5–-0.3] | 2.8 [2.2–3.4] | 49 [44–55] | 99.7 [99.7–99.7] | 81.8 | 82.4 | 95.6 [95.0–96.2] | 95.9 [95.1–96.6] | -0.7 [-1.3–-0.2] | 2.6 [2.1–3.2] | 53 [47–59] | 92.9 [91.8–94.0] | 91.9 [90.7–93.1] | 94.5 [93.4–95.6] | 93.4 [92.2–94.6] | 93.8 [92.7–94.9] | 92.9 [91.7–94.0] | 1.2 [0.8–1.7] | 1.1 [0.7–1.6] | 0.9% | 1.6% | 0.1% | 0.5% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +20 more |
| TTR | two-round | 94.8 [94.1–95.5] | 94.2 [93.1–95.1] | -1.6 [-2.3–-0.9] | 3.5 [2.8–4.2] | 43 [39–48] | 99.1 [99.0–99.2] | 54.7 | 55.5 | 95.0 [94.3–95.6] | 94.6 [93.6–95.4] | -1.4 [-2.1–-0.8] | 3.3 [2.7–4.0] | 45 [41–50] | 91.0 [89.7–92.2] | 90.4 [89.0–91.6] | 93.0 [91.7–94.2] | 92.3 [90.9–93.5] | 92.3 [91.0–93.4] | 91.6 [90.3–92.8] | 1.4 [0.9–1.8] | 1.3 [0.8–1.8] | 2.2% | 0.0% | 0.7% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +24 more |
| Borda | positional | 94.4 [93.6–95.2] | 92.6 [91.6–93.6] | -2.1 [-3.0–-1.3] | 3.9 [3.1–4.6] | 38 [34–42] | 99.2 [99.2–99.3] | 79.1 | 81.1 | 95.3 [94.6–95.9] | 94.1 [93.3–94.9] | -1.2 [-1.9–-0.4] | 3.0 [2.3–3.7] | 44 [41–48] | 84.5 [82.9–86.0] | 82.5 [80.8–84.1] | 89.9 [88.5–91.3] | 88.2 [86.6–89.6] | 90.5 [89.2–91.7] | 88.7 [87.3–90.0] | 0.7 [0.3–1.1] | 0.7 [0.3–1.0] | 3.1% | 3.6% | 0.0% | 2.9% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +27 more |
| IRV | plurality-based | 93.6 [92.7–94.6] | 92.9 [91.6–94.0] | -2.8 [-3.8–-1.9] | 4.6 [3.7–5.5] | 38 [33–42] | 99.2 [99.1–99.2] | 81.2 | 82.4 | 95.6 [95.0–96.2] | 95.9 [95.1–96.6] | -0.7 [-1.3–-0.2] | 2.6 [2.1–3.2] | 53 [47–59] | 92.9 [91.8–94.0] | 89.7 [88.3–91.0] | 94.5 [93.4–95.6] | 91.4 [90.0–92.8] | 93.8 [92.7–94.9] | 91.0 [89.7–92.2] | 1.2 [0.8–1.7] | 1.8 [1.2–2.4] | 2.9% | 1.5% | 0.9% | 0.1% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +27 more |
| IRV-3 | plurality-based | 93.4 [92.4–94.3] | 91.9 [90.7–93.0] | -3.1 [-4.1–-2.2] | 4.9 [4.0–5.8] | 34 [31–39] | 99.4 [99.3–99.4] | 71.8 | 72.7 | 95.2 [94.5–95.8] | 94.4 [93.5–95.2] | -1.2 [-2.0–-0.6] | 3.1 [2.5–3.8] | 44 [41–50] | 90.4 [89.1–91.7] | 88.2 [86.8–89.5] | 92.1 [90.8–93.4] | 90.0 [88.5–91.3] | 92.0 [90.8–93.2] | 89.9 [88.6–91.1] | 1.1 [0.7–1.6] | 1.8 [1.2–2.4] | 2.8% | 0.8% | 0.9% | 0.1% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +30 more |
| Supplementary vote | plurality-based | 91.4 [90.4–92.5] | 87.3 [85.8–88.9] | -5.3 [-6.5–-4.2] | 6.8 [5.8–7.9] | 26 [23–28] | 99.6 [99.5–99.6] | 80.6 | 81.2 | 91.9 [90.9–92.9] | 88.4 [87.0–89.8] | -4.8 [-5.8–-3.7] | 6.3 [5.3–7.4] | 27 [25–30] | 81.7 [80.0–83.4] | 81.0 [79.2–82.7] | 84.0 [82.2–85.9] | 83.2 [81.5–85.1] | 84.6 [82.9–86.1] | 83.7 [81.9–85.4] | 2.5 [1.8–3.2] | 2.8 [2.1–3.5] | 2.2% | 1.0% | 0.2% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +32 more |
| Asset | delegated | 91.2 [90.1–92.2] | 86.6 [85.0–88.1] | -5.5 [-6.6–-4.5] | 7.0 [6.1–8.1] | 25 [22–27] | 99.4 [99.3–99.5] | 56.1 | 56.7 | 91.1 [90.0–92.0] | 86.7 [85.3–88.1] | -5.6 [-6.8–-4.6] | 7.2 [6.2–8.3] | 25 [22–27] | 76.9 [75.1–78.8] | 77.0 [75.2–78.8] | 80.4 [78.5–82.2] | 80.6 [78.7–82.4] | 81.2 [79.5–83.0] | 81.5 [79.7–83.1] | 1.8 [1.2–2.4] | 1.8 [1.3–2.5] | 2.0% | 0.0% | 0.3% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +32 more |
| Plurality | plurality-based | 81.8 [79.9–83.5] | 75.0 [72.9–77.2] | -15.0 [-16.8–-13.1] | 16.5 [14.7–18.3] | 12 [10–13] | 99.6 [99.5–99.6] | 55.7 | 56.3 | 80.3 [78.4–82.2] | 74.6 [72.4–76.6] | -16.4 [-18.5–-14.4] | 18.0 [16.1–19.9] | 11 [10.0–12] | 66.6 [64.8–68.7] | 66.9 [64.9–69.0] | 69.7 [67.5–71.8] | 70.2 [68.2–72.5] | 70.7 [68.8–72.8] | 71.5 [69.7–73.6] | 7.0 [5.9–8.1] | 5.8 [4.8–6.8] | 4.6% | 0.0% | 0.1% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +34 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Borda | 9.8% |
| Score | 9.1% |
| IMDS | 8.9% |
| ESR | 8.8% |
| DMS | 8.4% |
| Plurality | 8.0% |
| IRV | 5.8% |
| IRV-3 | 5.6% |
| 321 | 5.4% |
| Delegated 321 | 5.4% |
| ESR-MARS | 5.3% |
| Approval | 4.8% |
| Score runoff | 4.8% |
| MJ | 4.5% |
| MARS | 4.5% |
| MARS-T | 4.5% |
| Approval cond. runoff | 4.3% |
| MAV | 4.2% |
| ARA | 4.1% |
| TTR | 4.1% |
| STAR | 3.9% |
| Asset | 3.8% |
| Supplementary vote | 3.8% |
| ER-IRV | 3.4% |
| Soft Minimax-40 | 2.9% |
| Approval runoff | 2.8% |
| Smith//Score | 2.2% |
| BTR-score | 2.0% |
| BTR-IRV | 1.9% |
| Soft Minimax | 1.4% |
| Soft Minimax-D | 1.4% |
| Minimax | 1.1% |
| Schulze | 1.1% |
| MMPO | 1.1% |
| MMWV | 1.1% |
| Copeland | 1.0% |
| Ranked pairs | 1.0% |
| Kemeny-Young | 0.9% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Plurality | 35.3% | 1.09 | 0.90 | 94% |
| Supplementary vote | 23.8% | 1.06 | 0.59 | 99% |
| TTR | 20.7% | 0.52 | 0.59 | 99% |
| Approval | 19.4% | 0.57 | 0.36 | 100% |
| Asset | 18.1% | 0.28 | 0.07 | 100% |
| IRV-3 | 17.3% | 0.51 | 0.24 | 100% |
| MAV | 16.7% | 0.45 | 0.08 | 100% |
| IRV | 16.0% | 0.21 | 0.05 | 100% |
| ER-IRV | 16.0% | 0.21 | 0.05 | 100% |
| Approval cond. runoff | 15.9% | 0.44 | 0.38 | 100% |
| MJ | 14.1% | 0.37 | 0.06 | 100% |
| Score runoff | 13.5% | 0.10 | 0.22 | 100% |
| Approval runoff | 13.4% | 0.34 | 0.36 | 100% |
| 321 | 12.7% | 0.25 | 0.38 | 99% |
| Delegated 321 | 12.7% | 0.25 | 0.38 | 99% |
| STAR | 11.3% | 0.14 | 0.18 | 100% |
| Borda | 10.7% | 0.06 | 0.56 | 100% |
| DMS | 10.2% | 0.18 | 0.05 | 100% |
| ESR | 8.2% | 0.21 | 0.10 | 100% |
| Score | 7.4% | 0.14 | 0.08 | 100% |
| IMDS | 7.1% | 0.17 | 0.11 | 100% |
| Soft Minimax | 7.1% | 0.17 | 0.10 | 100% |
| Soft Minimax-40 | 6.6% | 0.14 | 0.07 | 100% |
| ESR-MARS | 6.6% | 0.14 | 0.06 | 100% |
| ARA | 6.4% | 0.14 | 0.08 | 100% |
| MARS | 5.8% | 0.12 | 0.06 | 100% |
| MARS-T | 5.8% | 0.11 | 0.06 | 100% |
| BTR-score | 5.6% | 0.13 | 0.05 | 100% |
| Smith//Score | 5.5% | 0.12 | 0.05 | 100% |
| Soft Minimax-D | 5.3% | 0.12 | 0.06 | 100% |
| Copeland | 3.9% | 0.01 | 0.07 | 100% |
| Schulze | 3.0% | 0.01 | 0.04 | 100% |
| Minimax | 3.0% | 0.01 | 0.04 | 100% |
| MMWV | 3.0% | 0.01 | 0.04 | 100% |
| MMPO | 3.0% | 0.01 | 0.04 | 100% |
| Ranked pairs | 3.0% | 0.02 | 0.04 | 100% |
| BTR-IRV | 2.9% | 0.02 | 0.04 | 100% |
| Kemeny-Young | 2.8% | 0.01 | 0.05 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 1957 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 43 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Soft Minimax-40 | 90.7% (90.9%) | 6.7% (6.6%) | 2.6% (2.5%) | 58.1% (60.5%) | 94.4% (94.6%) | 88.7% |
| ESR-MARS | 90.8% (91.1%) | 6.4% (6.2%) | 2.8% (2.7%) | 62.8% (60.5%) | 93.9% (95.9%) | 88.3% |
| MARS | 90.6% (91.2%) | 6.4% (6.2%) | 3.0% (2.6%) | 65.1% (58.1%) | 95.9% (96.5%) | 89.6% |
| Soft Minimax-D | 91.9% (92.1%) | 6.2% (6.0%) | 1.9% (1.9%) | 74.4% (74.4%) | 98.7% (98.6%) | 92.1% |
| MARS-T | 90.4% (91.2%) | 6.5% (6.3%) | 3.1% (2.6%) | 67.4% (60.5%) | 94.4% (95.7%) | 88.6% |
| BTR-score | 91.4% (91.9%) | 5.5% (5.6%) | 3.2% (2.5%) | 74.4% (74.4%) | 100.0% (100.0%) | 93.0% |
| Smith//Score | 91.3% (92.0%) | 5.6% (5.5%) | 3.1% (2.6%) | 67.4% (69.8%) | 100.0% (100.0%) | 92.8% |
| ARA | 90.4% (90.6%) | 6.9% (6.9%) | 2.6% (2.4%) | 65.1% (67.4%) | 92.2% (91.6%) | 88.0% |
| ESR | 91.1% (91.8%) | 6.1% (6.0%) | 2.8% (2.1%) | 69.8% (65.1%) | 96.5% (98.9%) | 90.1% |
| STAR | 89.2% (91.0%) | 7.3% (6.2%) | 3.5% (2.8%) | 72.1% (58.1%) | 92.6% (89.8%) | 85.8% |
| IMDS | 91.1% (92.3%) | 5.9% (5.6%) | 3.0% (2.0%) | 88.4% (74.4%) | 100.0% (100.0%) | 92.8% |
| Soft Minimax | 92.5% (92.5%) | 5.6% (5.7%) | 1.8% (1.8%) | 67.4% (67.4%) | 100.0% (100.0%) | 93.6% |
| Minimax | 97.3% (98.2%) | 1.6% (1.2%) | 1.1% (0.7%) | 48.8% (46.5%) | 100.0% (100.0%) | 97.7% |
| Ranked pairs | 97.5% (98.2%) | 1.5% (1.2%) | 1.0% (0.6%) | 48.8% (48.8%) | 100.0% (100.0%) | 97.9% |
| Schulze | 97.4% (98.2%) | 1.5% (1.2%) | 1.1% (0.7%) | 48.8% (46.5%) | 100.0% (100.0%) | 97.8% |
| MMWV | 97.4% (98.2%) | 1.5% (1.2%) | 1.1% (0.7%) | 48.8% (46.5%) | 100.0% (100.0%) | 97.9% |
| MMPO | 97.4% (98.2%) | 1.5% (1.2%) | 1.1% (0.7%) | 48.8% (46.5%) | 100.0% (100.0%) | 97.9% |
| Kemeny-Young | 97.3% (98.2%) | 1.6% (1.2%) | 1.1% (0.6%) | 48.8% (48.8%) | 100.0% (100.0%) | 97.8% |
| Copeland | 97.6% (98.1%) | 1.4% (1.2%) | 1.0% (0.7%) | 48.8% (41.9%) | 100.0% (100.0%) | 98.0% |
| 321 | 78.9% (77.0%) | 13.2% (13.6%) | 7.8% (9.4%) | 79.1% (76.7%) | 96.7% (97.5%) | 83.6% |
| Delegated 321 | 78.9% (77.0%) | 13.2% (13.6%) | 7.8% (9.4%) | 79.1% (76.7%) | 96.7% (97.5%) | 83.6% |
| BTR-IRV | 96.4% (98.5%) | 2.4% (1.1%) | 1.2% (0.4%) | 62.8% (62.8%) | 100.0% (100.0%) | 96.7% |
| MJ | 90.4% (90.8%) | 6.9% (6.7%) | 2.7% (2.5%) | 58.1% (55.8%) | 89.4% (90.1%) | 86.7% |
| MAV | 89.9% (90.3%) | 7.5% (7.1%) | 2.6% (2.6%) | 65.1% (60.5%) | 89.4% (89.1%) | 85.9% |
| DMS | 90.0% (90.5%) | 7.2% (6.9%) | 2.8% (2.5%) | 62.8% (55.8%) | 84.5% (87.8%) | 80.3% |
| Score | 89.9% (90.4%) | 7.1% (6.7%) | 3.1% (2.9%) | 67.4% (58.1%) | 84.1% (87.6%) | 80.4% |
| ER-IRV | 96.6% (98.7%) | 2.8% (0.9%) | 0.6% (0.5%) | 55.8% (39.5%) | 93.9% (94.1%) | 91.9% |
| Borda | 95.0% (98.8%) | 2.5% (0.9%) | 2.5% (0.3%) | 39.5% (11.6%) | 84.1% (85.1%) | 82.5% |
| IRV | 95.0% (98.7%) | 3.8% (0.9%) | 1.2% (0.5%) | 65.1% (39.5%) | 93.2% (94.1%) | 89.7% |
| IRV-3 | 89.2% (91.1%) | 8.4% (6.9%) | 2.4% (2.0%) | 86.0% (81.4%) | 96.9% (97.6%) | 88.2% |
| Supplementary vote | 89.7% (91.2%) | 9.8% (8.6%) | 0.5% (0.2%) | 55.8% (51.2%) | 89.4% (89.6%) | 81.0% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| Soft Minimax-40 | 1.1% (0.7%) | 12.0% (11.6%) | 1.0% (0.9%) |
| ESR-MARS | 0.1% (0.1%) | 11.6% (11.1%) | 0.9% (0.9%) |
| MARS | 0.2% (0.2%) | 11.6% (11.3%) | 0.9% (0.9%) |
| Soft Minimax-D | 0.1% (0.1%) | 11.1% (11.0%) | 0.9% (0.9%) |
| Score runoff | 0.3% (0.2%) | — (—) | — (—) |
| MARS-T | 0.1% (0.1%) | 11.9% (11.5%) | 0.9% (0.9%) |
| BTR-score | 0.1% (0.1%) | 11.9% (11.5%) | 1.0% (0.9%) |
| Smith//Score | 0.0% (0.1%) | 11.9% (11.4%) | 1.0% (0.9%) |
| ARA | 1.1% (0.8%) | 11.6% (11.5%) | 0.9% (0.9%) |
| ESR | 1.4% (0.8%) | 12.4% (11.2%) | 1.1% (1.0%) |
| STAR | 0.4% (0.4%) | 15.0% (14.5%) | 1.1% (1.1%) |
| IMDS | 1.8% (1.0%) | 11.9% (11.6%) | 1.1% (1.1%) |
| Soft Minimax | 1.2% (1.2%) | 11.3% (11.5%) | 1.0% (1.1%) |
| Minimax | 0.7% (0.8%) | 0.4% (0.0%) | 0.0% (0.0%) |
| Ranked pairs | 0.8% (0.6%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Schulze | 0.7% (0.8%) | 0.4% (0.0%) | 0.0% (0.0%) |
| MMWV | 0.9% (0.8%) | 0.5% (0.0%) | 0.0% (0.0%) |
| MMPO | 0.9% (0.8%) | 0.5% (0.0%) | 0.0% (0.0%) |
| Kemeny-Young | 0.6% (0.4%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Copeland | 1.4% (1.2%) | 0.6% (0.0%) | 0.0% (0.0%) |
| 321 | 0.5% (0.9%) | 23.4% (24.6%) | 1.7% (1.8%) |
| Delegated 321 | 0.5% (0.9%) | 23.4% (24.6%) | 1.7% (1.8%) |
| BTR-IRV | 0.1% (0.0%) | 0.5% (0.0%) | 0.0% (0.0%) |
| MJ | 0.0% (0.0%) | 10.1% (10.1%) | 0.8% (0.8%) |
| Approval runoff | 0.9% (0.4%) | — (—) | — (—) |
| MAV | 0.3% (0.2%) | 11.1% (10.3%) | 1.0% (0.9%) |
| DMS | 0.0% (0.0%) | 11.0% (10.5%) | 0.8% (0.8%) |
| Approval | 1.7% (0.9%) | — (—) | — (—) |
| Score | 0.3% (0.4%) | 11.9% (11.6%) | 0.9% (0.9%) |
| Approval cond. runoff | 1.2% (0.7%) | — (—) | — (—) |
| ER-IRV | 1.1% (0.9%) | 0.8% (0.0%) | 0.1% (0.0%) |
| TTR | 0.9% (0.9%) | — (—) | — (—) |
| Borda | 0.4% (0.4%) | 3.1% (0.0%) | 0.2% (0.0%) |
| IRV | 0.9% (0.9%) | 0.3% (0.0%) | 0.0% (0.0%) |
| IRV-3 | 1.2% (0.7%) | 10.4% (10.4%) | 0.7% (0.7%) |
| Supplementary vote | 1.1% (0.7%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Asset | 1.2% (1.0%) | — (—) | — (—) |
| Plurality | 1.2% (1.0%) | — (—) | — (—) |

![strategy arrows](arrows-c3.png)

![strategy shifts](shift-c3.png)

![sincerity](sincerity-c3.png)

![fidelity](fidelity-c3.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c3.png)

## c4: utility quadratic

2000 elections (seeds 1), 1957 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, quadratic utility, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 97.9%; it also has the highest total utility in 82.6% of those; electing it loses 3.7% of VSE; favourite ÷ least liked: — (utility not positive). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 95.7 to 95.9; with the best VSE (100) WME 92.4 (nothing lower is possible there either); the most balanced point VSE 98.4 / WME 99.0. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 98.4 (at WME 95.7); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 98.5 (at WME 94.3); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -68.5 / WME -47.7. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 95.8 / WME 99.9. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 96.9 / 94.6; normalized utility winner 98.0 / 89.3; best worst margin 95.8 / 100.0; Condorcet winner, else random ballot 94.7 / 99.0; Condorcet winner, else utility winner 96.3 / 99.8; random pair, majority 31.6 / 2.9; nearest the median voter 97.3 / 96.7; nearest the mean voter 100.0 / 92.4.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): MARS, Soft Minimax-D, BTR-score, IMDS, Approval runoff, Minimax, Ranked pairs, Kemeny-Young, Copeland.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): MARS, Soft Minimax-D, ESR-MARS, BTR-score, Soft Minimax, Minimax, Schulze, Ranked pairs, MMWV, MMPO, Kemeny-Young, Copeland.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 94.7 / 99.0; its mirror on the utility side 97.0 / 88.2; random pair 31.6 / 2.9. No better than a lottery of them (Lead ≤ 0): with learned ballots 321, Delegated 321, Approval, MJ, DMS, Score, MAV, Borda, ER-IRV, TTR, IRV, IRV-3, Supplementary vote, Asset, Plurality; with honest ballots 321, Delegated 321, Approval, MJ, DMS, MAV, Borda, ER-IRV, TTR, IRV, IRV-3, Supplementary vote, Asset, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| MARS | hybrid | 97.8 [97.4–98.2] | 94.2 [93.2–95.1] | 1.7 [1.2–2.3] | 0.6 [0.3–1.0] | 73 [65–85] | 100.0 [100.0–100.0] | 92.8 | 94.1 | 98.0 [97.7–98.3] | 93.0 [91.8–94.1] | 1.7 [1.2–2.2] | 0.4 [0.1–0.8] | 80 [72–92] | 84.4 [82.6–86.0] | 85.9 [84.3–87.4] | 97.6 [96.8–98.4] | 97.8 [97.1–98.5] | 97.4 [96.7–98.1] | 97.3 [96.6–98.0] | 0.1 [0.0–0.2] | 0.2 [0.1–0.4] | 0.0% | 0.1% | 0.0% | 4.0% | — |
| Soft Minimax-D | hybrid | 97.8 [97.4–98.2] | 94.5 [93.5–95.4] | 1.8 [1.3–2.3] | 0.6 [0.3–1.0] | 73 [65–85] | 100.0 [100.0–100.0] | 93.6 | 94.1 | 97.8 [97.5–98.2] | 94.1 [93.1–95.1] | 1.7 [1.3–2.3] | 0.6 [0.3–1.0] | 74 [65–86] | 87.6 [86.1–89.1] | 88.1 [86.6–89.5] | 98.0 [97.3–98.7] | 98.0 [97.3–98.7] | 97.9 [97.2–98.5] | 97.9 [97.3–98.5] | 0.3 [0.1–0.5] | 0.4 [0.1–0.7] | 0.0% | 0.0% | 0.0% | 1.4% | — |
| ESR-MARS | hybrid | 97.7 [97.4–98.1] | 93.9 [92.9–94.8] | 1.6 [1.1–2.2] | 0.7 [0.3–1.1] | 71 [62–81] | 100.0 [100.0–100.0] | 92.7 | 94.1 | 98.1 [97.7–98.4] | 92.9 [91.8–94.0] | 1.7 [1.2–2.2] | 0.4 [0.1–0.7] | 81 [72–94] | 84.3 [82.6–85.9] | 85.2 [83.6–86.7] | 97.8 [97.0–98.5] | 97.1 [96.3–97.9] | 97.5 [96.7–98.2] | 96.6 [95.8–97.4] | 0.1 [0.0–0.2] | 0.1 [0.0–0.2] | 0.0% | 0.2% | 0.0% | 4.2% | — |
| MARS-T | hybrid | 97.7 [97.4–98.1] | 93.6 [92.6–94.6] | 1.6 [1.1–2.1] | 0.7 [0.4–1.1] | 71 [63–80] | 100.0 [100.0–100.0] | 92.8 | 94.1 | 98.0 [97.7–98.3] | 93.0 [91.8–94.1] | 1.7 [1.2–2.2] | 0.4 [0.1–0.8] | 80 [71–93] | 84.4 [82.6–85.9] | 84.7 [83.0–86.2] | 97.8 [97.0–98.5] | 97.2 [96.4–98.0] | 97.6 [96.9–98.2] | 96.8 [96.0–97.5] | 0.1 [0.0–0.2] | 0.1 [0.0–0.4] | 0.0% | 0.1% | 0.0% | 4.0% | MARS (WME\*), Soft Minimax-D (WME\*) |
| Score runoff | two-round | 97.7 [97.3–98.1] | 93.0 [91.9–93.9] | 1.4 [1.0–1.9] | 0.7 [0.4–1.1] | 69 [62–79] | 99.9 [99.9–99.9] | 92.2 | 94.0 | 97.8 [97.4–98.2] | 89.8 [88.6–91.1] | 1.0 [0.6–1.4] | 0.6 [0.2–1.0] | 73 [66–82] | 80.3 [78.6–82.1] | 84.7 [83.1–86.3] | 98.4 [97.8–99.0] | 97.7 [97.0–98.4] | 98.4 [97.8–98.9] | 97.5 [96.9–98.2] | 0.2 [0.1–0.4] | 0.2 [0.1–0.5] | 0.0% | 0.9% | 0.0% | 3.8% | MARS (WME\*), Soft Minimax-D (WME\*), ESR-MARS (WME\*) |
| Soft Minimax-40 | hybrid | 97.6 [97.2–98.0] | 92.2 [91.1–93.2] | 1.2 [0.8–1.8] | 0.8 [0.4–1.2] | 68 [60–77] | 100.0 [100.0–100.0] | 93.1 | 94.1 | 97.7 [97.3–98.1] | 91.3 [90.1–92.5] | 1.1 [0.7–1.6] | 0.7 [0.4–1.1] | 70 [62–80] | 83.2 [81.4–84.8] | 83.6 [81.8–85.2] | 97.6 [96.8–98.4] | 97.3 [96.5–98.1] | 97.5 [96.8–98.1] | 97.3 [96.6–97.9] | 0.2 [0.1–0.5] | 0.3 [0.1–0.5] | 0.0% | 0.1% | 0.0% | 2.7% | MARS (WME\*), MARS-T (WME\*), Soft Minimax-D (WME\*) +1 more |
| ARA | hybrid | 97.6 [97.2–98.0] | 91.3 [90.2–92.4] | 1.1 [0.6–1.6] | 0.8 [0.5–1.2] | 67 [60–77] | 100.0 [100.0–100.0] | 92.8 | 94.1 | 97.7 [97.3–98.0] | 90.6 [89.3–91.9] | 1.0 [0.5–1.5] | 0.8 [0.4–1.2] | 68 [60–78] | 81.2 [79.4–82.9] | 81.8 [80.0–83.5] | 96.1 [95.2–97.0] | 95.7 [94.7–96.6] | 96.4 [95.6–97.2] | 95.8 [95.0–96.7] | 0.2 [0.0–0.4] | 0.2 [0.0–0.4] | 0.0% | 0.1% | 0.0% | 3.7% | Score runoff (WME\*), MARS (WME\*), MARS-T (WME\*) +3 more |
| STAR | hybrid | 97.6 [97.2–98.0] | 91.3 [90.2–92.5] | 1.1 [0.6–1.5] | 0.8 [0.5–1.2] | 67 [59–76] | 100.0 [100.0–100.0] | 92.6 | 94.0 | 97.8 [97.4–98.2] | 89.5 [88.2–90.8] | 0.9 [0.5–1.3] | 0.7 [0.3–1.0] | 72 [65–80] | 79.6 [77.8–81.3] | 81.7 [80.0–83.4] | 97.5 [96.8–98.3] | 97.1 [96.2–97.9] | 97.7 [97.0–98.3] | 97.0 [96.2–97.7] | 0.1 [0.0–0.4] | 0.4 [0.1–0.7] | 0.0% | 0.3% | 0.0% | 3.7% | Score runoff (WME\*), BTR-score (WME\*), MARS (WME\*) +4 more |
| BTR-score | hybrid | 97.1 [96.6–97.6] | 95.0 [94.1–96.0] | 1.3 [0.8–1.9] | 1.3 [0.8–1.7] | 61 [55–68] | 100.0 [100.0–100.0] | 93.4 | 94.1 | 97.1 [96.7–97.6] | 94.6 [93.5–95.6] | 1.3 [0.7–1.8] | 1.3 [0.9–1.8] | 60 [54–67] | 88.9 [87.4–90.2] | 89.0 [87.6–90.4] | 97.8 [97.0–98.5] | 97.7 [97.0–98.5] | 97.3 [96.5–98.0] | 97.2 [96.5–97.9] | 0.3 [0.1–0.5] | 0.4 [0.1–0.7] | 0.0% | 0.1% | 0.0% | 1.8% | — |
| Smith//Score | condorcet-based | 97.1 [96.6–97.5] | 95.0 [94.0–95.9] | 1.2 [0.8–1.8] | 1.4 [0.9–1.8] | 60 [54–66] | 100.0 [100.0–100.0] | 93.4 | 94.1 | 97.1 [96.7–97.6] | 94.6 [93.5–95.6] | 1.3 [0.7–1.8] | 1.3 [0.9–1.8] | 60 [54–67] | 88.9 [87.4–90.2] | 89.0 [87.6–90.4] | 97.8 [97.0–98.5] | 97.6 [96.8–98.3] | 97.4 [96.6–98.0] | 97.0 [96.4–97.8] | 0.2 [0.1–0.5] | 0.4 [0.1–0.6] | 0.0% | 0.1% | 0.0% | 1.9% | BTR-score (VSE\*) |
| Soft Minimax | hybrid | 97.0 [96.5–97.4] | 95.0 [94.0–95.9] | 1.2 [0.7–1.7] | 1.4 [1.0–1.9] | 59 [53–65] | 100.0 [100.0–100.0] | 93.6 | 94.1 | 97.0 [96.6–97.5] | 94.8 [93.8–95.8] | 1.2 [0.7–1.7] | 1.4 [0.9–1.8] | 59 [53–66] | 89.7 [88.3–91.0] | 89.7 [88.4–91.1] | 97.7 [96.9–98.4] | 97.5 [96.7–98.2] | 97.2 [96.5–97.9] | 97.1 [96.4–97.8] | 0.4 [0.1–0.7] | 0.4 [0.2–0.8] | 0.0% | 0.1% | 0.0% | 1.4% | — |
| IMDS | rated | 96.9 [96.3–97.3] | 95.4 [94.5–96.3] | 1.2 [0.7–1.7] | 1.5 [1.1–1.9] | 59 [54–64] | 100.0 [100.0–100.0] | 91.6 | 94.1 | 97.1 [96.6–97.5] | 94.8 [93.7–95.8] | 1.2 [0.7–1.8] | 1.4 [0.9–1.8] | 59 [54–66] | 89.6 [88.2–90.9] | 89.6 [88.3–90.8] | 97.6 [96.8–98.4] | 97.2 [96.4–98.0] | 97.3 [96.5–97.9] | 97.0 [96.2–97.6] | 0.3 [0.1–0.5] | 0.4 [0.1–0.7] | 0.0% | 0.1% | 0.0% | 8.2% | BTR-score (VSE\*) |
| ESR | rated | 96.5 [95.9–97.0] | 94.3 [93.3–95.3] | 0.7 [0.1–1.3] | 1.9 [1.4–2.4] | 51 [46–58] | 100.0 [100.0–100.0] | 91.6 | 94.1 | 97.0 [96.5–97.5] | 94.7 [93.7–95.7] | 1.2 [0.6–1.7] | 1.4 [0.9–1.9] | 59 [52–66] | 88.8 [87.3–90.2] | 87.5 [86.0–89.0] | 97.6 [96.8–98.3] | 95.6 [94.4–96.5] | 97.0 [96.2–97.6] | 95.1 [94.2–96.0] | 0.3 [0.1–0.5] | 0.6 [0.2–1.0] | 0.0% | 0.1% | 0.0% | 8.1% | IMDS (VSE\* WME\*), Smith//Score (VSE\*), Approval runoff (WME\*) +6 more |
| 321 | hybrid | 96.0 [95.5–96.5] | 87.3 [85.8–88.9] | -1.0 [-1.5–-0.2] | 2.4 [1.9–3.0] | 40 [35–45] | 100.0 [100.0–100.0] | 84.2 | 85.7 | 96.1 [95.5–96.6] | 85.7 [84.0–87.3] | -0.9 [-1.5–-0.3] | 2.4 [1.8–2.9] | 41 [35–47] | 77.4 [75.4–79.1] | 78.4 [76.5–80.2] | 91.9 [90.5–93.3] | 91.3 [89.9–92.7] | 92.2 [91.1–93.4] | 92.3 [91.1–93.4] | 0.9 [0.4–1.2] | 0.5 [0.2–0.9] | 0.0% | 0.3% | 0.0% | 4.6% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), STAR (VSE\* WME\*) +22 more |
| Delegated 321 | delegated | 96.0 [95.5–96.5] | 87.3 [85.8–88.9] | -1.0 [-1.5–-0.2] | 2.4 [1.9–3.0] | 40 [35–45] | — | — | — | 96.1 [95.5–96.6] | 85.7 [84.0–87.3] | -0.9 [-1.5–-0.3] | 2.4 [1.8–2.9] | 41 [35–47] | 77.4 [75.4–79.1] | 78.4 [76.5–80.2] | 91.9 [90.5–93.3] | 91.3 [89.9–92.7] | 92.2 [91.1–93.4] | 92.3 [91.1–93.4] | 0.9 [0.4–1.2] | 0.5 [0.2–0.9] | 0.0% | 0.3% | 0.0% | 4.6% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), STAR (VSE\* WME\*) +22 more |
| Approval runoff | two-round | 96.0 [95.4–96.5] | 96.5 [95.8–97.0] | 0.6 [0.1–1.2] | 2.0 [1.5–2.5] | 59 [54–63] | 99.9 [99.9–99.9] | 62.3 | 62.6 | 95.8 [95.1–96.4] | 96.0 [95.2–96.6] | 0.4 [-0.2–1.0] | 2.2 [1.7–2.8] | 55 [50–60] | 88.9 [87.7–90.3] | 89.8 [88.5–91.1] | 94.6 [93.5–95.7] | 95.2 [94.2–96.3] | 94.2 [93.2–95.2] | 94.7 [93.8–95.7] | 1.1 [0.7–1.6] | 0.8 [0.4–1.1] | 0.0% | 0.7% | 0.0% | 1.4% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +4 more |
| Approval cond. runoff | two-round | 95.8 [95.2–96.3] | 94.1 [93.3–94.9] | 0.1 [-0.5–0.6] | 2.6 [2.0–3.1] | 45 [43–50] | 100.0 [99.9–100.0] | 63.6 | 64.0 | 95.9 [95.2–96.4] | 94.4 [93.6–95.2] | 0.2 [-0.4–0.8] | 2.5 [1.9–3.1] | 46 [44–52] | 85.5 [83.9–87.1] | 85.6 [84.1–87.1] | 93.8 [92.6–95.0] | 93.3 [92.1–94.5] | 93.5 [92.3–94.5] | 93.2 [92.2–94.3] | 1.0 [0.6–1.5] | 0.9 [0.5–1.4] | 0.0% | 0.3% | 0.0% | 2.8% | IMDS (VSE\* WME\*), Ranked pairs (WME\*), Schulze (WME\*) +15 more |
| Minimax | minimax-family | 95.7 [95.2–96.3] | 99.4 [99.1–99.6] | 0.9 [0.6–1.4] | 0.6 [0.3–0.8] | 89 [84–93] | 99.9 [99.9–99.9] | 80.3 | 80.5 | 95.7 [95.1–96.3] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 93 [89–95] | 98.5 [97.9–99.0] | 97.8 [97.1–98.4] | 99.2 [98.7–99.6] | 98.7 [98.1–99.2] | 98.3 [97.7–98.9] | 97.9 [97.2–98.5] | 0.5 [0.2–0.9] | 0.6 [0.3–1.0] | 0.4% | 0.4% | 0.0% | 0.1% | — |
| Schulze | condorcet | 95.7 [95.2–96.3] | 99.4 [99.1–99.6] | 0.9 [0.5–1.3] | 0.6 [0.3–0.8] | 89 [84–93] | 99.9 [99.9–99.9] | 80.3 | 80.5 | 95.7 [95.1–96.3] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 93 [89–95] | 98.5 [97.9–99.0] | 97.7 [97.1–98.4] | 99.2 [98.7–99.6] | 98.6 [98.0–99.2] | 98.3 [97.7–98.9] | 97.8 [97.2–98.5] | 0.5 [0.2–0.9] | 0.6 [0.3–1.0] | 0.4% | 0.4% | 0.0% | 0.1% | — |
| Ranked pairs | condorcet | 95.7 [95.1–96.2] | 99.4 [99.1–99.6] | 0.9 [0.5–1.3] | 0.5 [0.3–0.8] | 89 [85–93] | 99.9 [99.9–99.9] | 80.3 | 80.5 | 95.7 [95.1–96.3] | 99.6 [99.4–99.8] | 0.9 [0.6–1.3] | 0.4 [0.2–0.6] | 92 [88–95] | 98.5 [98.0–99.0] | 98.0 [97.4–98.6] | 99.2 [98.7–99.6] | 98.8 [98.2–99.3] | 98.2 [97.6–98.8] | 97.9 [97.2–98.5] | 0.6 [0.3–0.9] | 0.7 [0.3–1.1] | 0.3% | 0.4% | 0.0% | 0.0% | — |
| MMWV | minimax-family | 95.7 [95.1–96.2] | 99.4 [99.1–99.6] | 0.9 [0.5–1.3] | 0.5 [0.3–0.8] | 89 [85–93] | 99.9 [99.9–99.9] | 80.3 | 80.5 | 95.7 [95.1–96.3] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 93 [89–95] | 98.5 [97.9–99.0] | 97.8 [97.2–98.4] | 99.2 [98.7–99.6] | 98.7 [98.2–99.3] | 98.3 [97.7–98.9] | 97.9 [97.2–98.5] | 0.5 [0.2–0.9] | 0.5 [0.2–0.9] | 0.1% | 0.7% | 0.0% | 0.1% | — |
| MMPO | minimax-family | 95.7 [95.1–96.2] | 99.4 [99.1–99.6] | 0.9 [0.5–1.3] | 0.6 [0.3–0.8] | 89 [84–92] | 99.9 [99.9–99.9] | 80.3 | 80.5 | 95.7 [95.1–96.3] | 99.6 [99.4–99.8] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 93 [89–95] | 98.5 [97.9–99.0] | 97.7 [97.1–98.4] | 99.2 [98.7–99.6] | 98.7 [98.1–99.2] | 98.3 [97.7–98.9] | 97.8 [97.2–98.5] | 0.5 [0.2–0.9] | 0.5 [0.2–0.9] | 0.1% | 0.7% | 0.0% | 0.1% | — |
| Approval | approval-based | 95.6 [95.0–96.2] | 93.1 [92.1–94.0] | -0.3 [-0.9–0.4] | 2.8 [2.2–3.4] | 42 [38–46] | 100.0 [100.0–100.0] | 64.0 | 64.3 | 95.7 [95.1–96.3] | 93.5 [92.6–94.4] | -0.1 [-0.7–0.5] | 2.7 [2.1–3.3] | 44 [40–48] | 84.0 [82.3–85.7] | 83.6 [82.0–85.3] | 93.0 [91.8–94.3] | 92.1 [90.8–93.4] | 92.8 [91.6–94.0] | 92.2 [91.1–93.3] | 1.1 [0.7–1.6] | 1.0 [0.6–1.5] | 0.0% | 0.2% | 0.0% | 2.8% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), ESR (VSE\* WME\*) +18 more |
| Kemeny-Young | condorcet | 95.6 [95.1–96.2] | 99.4 [99.2–99.6] | 0.8 [0.5–1.3] | 0.5 [0.3–0.8] | 89 [85–93] | 99.9 [99.9–99.9] | 80.3 | 80.5 | 95.7 [95.1–96.2] | 99.6 [99.4–99.8] | 0.9 [0.6–1.3] | 0.4 [0.2–0.6] | 93 [89–96] | 98.5 [97.9–99.0] | 97.8 [97.2–98.5] | 99.2 [98.7–99.6] | 98.6 [98.0–99.2] | 98.3 [97.7–98.9] | 97.9 [97.2–98.5] | 0.5 [0.2–0.9] | 0.6 [0.3–1.0] | 0.4% | 0.3% | 0.0% | 0.1% | — |
| Copeland | condorcet | 95.6 [95.0–96.2] | 99.4 [99.2–99.6] | 0.8 [0.5–1.2] | 0.5 [0.3–0.7] | 90 [86–93] | 99.9 [99.9–99.9] | 80.2 | 80.4 | 95.7 [95.1–96.3] | 99.6 [99.4–99.7] | 1.0 [0.6–1.4] | 0.4 [0.2–0.6] | 92 [89–94] | 98.4 [97.9–99.0] | 98.0 [97.4–98.6] | 99.1 [98.7–99.6] | 98.9 [98.4–99.4] | 98.0 [97.4–98.6] | 97.9 [97.2–98.5] | 0.4 [0.1–0.6] | 0.4 [0.1–0.6] | 0.4% | 0.4% | 0.0% | 0.1% | — |
| MJ | median-based | 95.4 [94.8–96.0] | 93.4 [92.3–94.5] | -0.3 [-1.0–0.3] | 3.0 [2.4–3.5] | 42 [38–47] | 100.0 [100.0–100.0] | 93.2 | 94.4 | 95.4 [94.8–96.0] | 93.4 [92.3–94.5] | -0.4 [-1.0–0.3] | 3.0 [2.4–3.6] | 42 [38–46] | 86.1 [84.6–87.7] | 86.5 [85.0–88.0] | 92.3 [91.0–93.6] | 92.5 [91.3–93.8] | 92.2 [91.0–93.4] | 92.3 [91.2–93.5] | 0.7 [0.4–1.1] | 0.8 [0.4–1.1] | 0.0% | 0.1% | 0.0% | 3.8% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), BTR-score (VSE\* WME\*) +16 more |
| DMS | rated | 95.4 [94.9–96.0] | 87.4 [86.0–88.9] | -1.4 [-2.0–-0.7] | 3.0 [2.4–3.6] | 35 [31–40] | 100.0 [100.0–100.0] | 91.8 | 94.1 | 96.9 [96.5–97.4] | 88.4 [87.0–89.8] | -0.0 [-0.4–0.5] | 1.5 [1.0–2.0] | 53 [46–60] | 78.5 [76.7–80.2] | 77.4 [75.6–79.2] | 95.5 [94.3–96.5] | 88.8 [87.3–90.3] | 95.9 [94.9–96.8] | 90.2 [88.9–91.5] | 0.4 [0.1–0.7] | 0.4 [0.1–0.8] | 0.0% | 0.1% | 0.0% | 7.5% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), ESR (VSE\* WME\*) +22 more |
| Score | rated | 95.4 [94.9–96.0] | 86.7 [85.3–88.2] | -1.5 [-2.1–-0.8] | 3.0 [2.4–3.6] | 35 [31–40] | 100.0 [100.0–100.0] | 91.5 | 94.1 | 97.3 [96.8–97.7] | 87.5 [86.2–88.9] | 0.3 [-0.2–0.7] | 1.2 [0.7–1.6] | 59 [52–66] | 77.5 [75.5–79.2] | 75.7 [73.9–77.5] | 95.7 [94.6–96.7] | 88.3 [86.7–89.8] | 96.3 [95.4–97.2] | 90.1 [88.8–91.3] | 0.2 [0.1–0.4] | 0.5 [0.2–0.8] | 0.0% | 0.1% | 0.0% | 8.3% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), ESR (VSE\* WME\*) +22 more |
| BTR-IRV | condorcet-based | 95.4 [94.8–95.9] | 99.0 [98.6–99.3] | 0.6 [0.1–1.0] | 1.0 [0.6–1.3] | 82 [77–87] | 99.8 [99.8–99.8] | 80.2 | 80.5 | 95.7 [95.1–96.2] | 99.6 [99.3–99.7] | 0.9 [0.6–1.3] | 0.4 [0.2–0.6] | 91 [88–94] | 98.6 [98.0–99.1] | 97.2 [96.4–97.9] | 99.1 [98.6–99.5] | 98.1 [97.4–98.7] | 98.1 [97.5–98.7] | 97.2 [96.4–97.9] | 0.5 [0.2–0.9] | 0.9 [0.4–1.3] | 0.7% | 0.6% | 0.1% | 0.2% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +4 more |
| MAV | median-based | 94.5 [93.8–95.2] | 92.9 [91.8–93.8] | -1.2 [-1.9–-0.5] | 3.8 [3.2–4.5] | 38 [34–42] | 100.0 [100.0–100.0] | 93.4 | 94.4 | 94.3 [93.6–95.0] | 92.7 [91.5–93.7] | -1.4 [-2.2–-0.7] | 4.1 [3.3–4.7] | 37 [33–42] | 85.1 [83.7–86.8] | 85.2 [83.8–86.7] | 90.2 [88.9–91.7] | 90.5 [89.0–92.0] | 90.8 [89.6–92.0] | 91.0 [89.8–92.2] | 1.1 [0.7–1.6] | 1.1 [0.7–1.5] | 0.0% | 0.1% | 0.0% | 3.3% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +20 more |
| Borda | positional | 94.3 [93.5–95.0] | 93.5 [92.6–94.3] | -1.3 [-2.0–-0.6] | 4.0 [3.3–4.7] | 40 [37–43] | 99.3 [99.3–99.4] | 77.4 | 79.1 | 94.1 [93.3–94.8] | 94.1 [93.3–94.9] | -1.3 [-2.1–-0.6] | 3.9 [3.3–4.6] | 42 [39–45] | 84.5 [82.9–86.0] | 83.7 [82.1–85.3] | 90.7 [89.3–92.2] | 89.9 [88.4–91.4] | 91.0 [89.8–92.3] | 90.5 [89.1–91.7] | 0.8 [0.4–1.1] | 0.8 [0.4–1.1] | 3.0% | 2.7% | 0.0% | 2.5% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +19 more |
| ER-IRV | plurality-based | 93.4 [92.5–94.2] | 95.6 [94.8–96.4] | -1.7 [-2.4–-0.9] | 3.8 [3.1–4.6] | 50 [45–56] | 99.7 [99.7–99.7] | 80.1 | 80.7 | 93.2 [92.3–94.0] | 95.9 [95.1–96.6] | -1.8 [-2.5–-1.0] | 3.8 [3.1–4.5] | 52 [46–58] | 92.9 [91.8–94.0] | 92.2 [91.0–93.3] | 94.0 [92.9–95.2] | 93.3 [92.0–94.5] | 93.5 [92.5–94.6] | 92.8 [91.7–94.0] | 1.5 [0.9–2.0] | 1.2 [0.8–1.7] | 1.0% | 1.5% | 0.1% | 0.7% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +11 more |
| TTR | two-round | 92.5 [91.6–93.3] | 93.9 [92.8–94.9] | -2.7 [-3.6–-1.9] | 5.1 [4.4–6.0] | 41 [37–45] | 99.3 [99.2–99.4] | 50.1 | 50.8 | 92.8 [91.8–93.6] | 94.6 [93.6–95.5] | -2.3 [-3.2–-1.6] | 4.7 [3.9–5.5] | 44 [40–49] | 91.0 [89.7–92.3] | 90.2 [88.9–91.5] | 92.3 [90.9–93.6] | 91.7 [90.4–93.0] | 92.0 [90.7–93.1] | 91.2 [89.9–92.4] | 1.4 [0.9–1.9] | 1.6 [1.1–2.1] | 2.3% | 0.0% | 0.6% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +22 more |
| IRV | plurality-based | 92.5 [91.6–93.3] | 94.1 [93.1–95.1] | -2.7 [-3.5–-1.8] | 5.1 [4.1–5.9] | 42 [38–47] | 99.2 [99.2–99.3] | 79.5 | 80.7 | 93.2 [92.3–94.0] | 95.9 [95.1–96.6] | -1.8 [-2.5–-1.0] | 3.8 [3.1–4.5] | 52 [46–58] | 92.9 [91.8–94.0] | 90.4 [89.1–91.8] | 94.0 [92.9–95.2] | 91.8 [90.3–93.2] | 93.5 [92.5–94.6] | 91.6 [90.3–92.8] | 1.5 [0.9–2.0] | 1.6 [1.1–2.2] | 3.2% | 1.3% | 0.7% | 0.1% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +22 more |
| IRV-3 | plurality-based | 91.4 [90.4–92.4] | 92.3 [91.0–93.4] | -3.8 [-4.8–-2.9] | 6.4 [5.4–7.3] | 35 [31–39] | 99.5 [99.4–99.5] | 67.4 | 68.3 | 92.5 [91.6–93.4] | 94.4 [93.5–95.2] | -2.6 [-3.4–-1.8] | 4.9 [4.1–5.7] | 43 [39–47] | 90.4 [89.1–91.7] | 88.2 [86.8–89.6] | 91.4 [90.0–92.7] | 89.3 [87.8–90.8] | 91.4 [90.2–92.6] | 89.5 [88.1–90.8] | 1.4 [0.9–1.9] | 1.8 [1.2–2.4] | 3.1% | 0.7% | 0.7% | 0.1% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +27 more |
| Supplementary vote | plurality-based | 88.7 [87.5–89.8] | 87.7 [86.2–89.2] | -6.9 [-8.1–-5.8] | 9.6 [8.4–10.8] | 24 [21–27] | 99.6 [99.6–99.6] | 79.1 | 79.7 | 88.8 [87.7–90.0] | 88.4 [86.8–89.8] | -6.7 [-7.9–-5.6] | 9.3 [8.2–10.5] | 25 [23–28] | 81.8 [80.1–83.5] | 81.3 [79.6–83.1] | 82.4 [80.4–84.3] | 81.9 [80.0–83.8] | 83.7 [82.2–85.4] | 83.1 [81.4–84.8] | 2.9 [2.2–3.8] | 3.1 [2.4–3.9] | 2.4% | 0.8% | 0.2% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +32 more |
| Asset | delegated | 88.7 [87.5–89.8] | 86.4 [84.9–87.9] | -7.2 [-8.4–-6.1] | 9.8 [8.7–10.9] | 22 [19–24] | 99.4 [99.4–99.5] | 51.0 | 51.6 | 88.5 [87.3–89.6] | 86.7 [85.3–88.1] | -7.3 [-8.5–-6.1] | 9.9 [8.8–11.0] | 22 [20–25] | 76.9 [75.1–78.8] | 77.2 [75.3–78.9] | 80.5 [78.5–82.5] | 81.2 [79.2–83.0] | 81.6 [79.9–83.3] | 82.0 [80.2–83.6] | 3.2 [2.5–4.0] | 3.1 [2.4–4.0] | 2.2% | 0.0% | 0.3% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +32 more |
| Plurality | plurality-based | 80.1 [78.2–82.0] | 75.6 [73.7–77.7] | -16.1 [-17.9–-14.3] | 18.3 [16.4–20.3] | 11 [9.7–12] | 99.6 [99.6–99.6] | 51.6 | 52.0 | 78.0 [75.9–79.9] | 74.8 [72.8–76.8] | -18.0 [-20.0–-16.0] | 20.5 [18.5–22.5] | 10 [9.3–12] | 66.8 [64.8–68.8] | 67.6 [65.5–69.6] | 68.2 [65.8–70.3] | 69.1 [66.8–71.5] | 70.8 [68.7–72.7] | 71.9 [69.8–73.9] | 7.8 [6.7–9.0] | 6.6 [5.6–7.7] | 4.9% | 0.0% | 0.1% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +34 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Score | 8.6% |
| IMDS | 8.5% |
| ESR | 8.4% |
| Borda | 8.3% |
| Plurality | 8.1% |
| DMS | 7.8% |
| IRV | 5.7% |
| IRV-3 | 5.4% |
| 321 | 5.1% |
| Delegated 321 | 5.1% |
| Score runoff | 4.8% |
| ESR-MARS | 4.4% |
| Approval | 4.4% |
| Approval cond. runoff | 4.3% |
| MARS | 4.2% |
| MARS-T | 4.2% |
| STAR | 4.1% |
| MJ | 4.1% |
| ARA | 3.9% |
| TTR | 3.9% |
| Supplementary vote | 3.8% |
| Asset | 3.7% |
| MAV | 3.6% |
| ER-IRV | 3.5% |
| Soft Minimax-40 | 2.9% |
| Approval runoff | 2.6% |
| Smith//Score | 2.1% |
| BTR-score | 1.9% |
| BTR-IRV | 1.7% |
| Soft Minimax | 1.5% |
| Soft Minimax-D | 1.5% |
| MMPO | 1.0% |
| MMWV | 1.0% |
| Minimax | 1.0% |
| Schulze | 1.0% |
| Copeland | 0.9% |
| Ranked pairs | 0.9% |
| Kemeny-Young | 0.8% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Plurality | 35.3% | 1.09 | 0.89 | 94% |
| Supplementary vote | 23.8% | 1.06 | 0.59 | 99% |
| TTR | 20.6% | 0.52 | 0.59 | 99% |
| Asset | 18.1% | 0.28 | 0.07 | 100% |
| MAV | 18.1% | 0.45 | 0.09 | 100% |
| Score runoff | 17.4% | 0.07 | 0.25 | 100% |
| IRV-3 | 17.3% | 0.50 | 0.24 | 100% |
| IRV | 16.0% | 0.22 | 0.05 | 100% |
| ER-IRV | 16.0% | 0.22 | 0.05 | 100% |
| Approval | 15.3% | 0.46 | 0.39 | 100% |
| MJ | 14.6% | 0.36 | 0.05 | 100% |
| Approval cond. runoff | 14.1% | 0.42 | 0.41 | 100% |
| Approval runoff | 13.6% | 0.28 | 0.35 | 100% |
| STAR | 11.6% | 0.10 | 0.18 | 100% |
| Borda | 10.7% | 0.06 | 0.56 | 100% |
| 321 | 10.5% | 0.18 | 0.33 | 99% |
| Delegated 321 | 10.5% | 0.18 | 0.33 | 99% |
| DMS | 7.6% | 0.12 | 0.05 | 100% |
| ESR | 7.4% | 0.15 | 0.09 | 100% |
| Soft Minimax | 7.0% | 0.12 | 0.09 | 100% |
| IMDS | 7.0% | 0.12 | 0.10 | 100% |
| Soft Minimax-40 | 6.3% | 0.11 | 0.08 | 100% |
| Score | 6.2% | 0.10 | 0.07 | 100% |
| ARA | 5.6% | 0.10 | 0.08 | 100% |
| Smith//Score | 5.5% | 0.10 | 0.06 | 100% |
| BTR-score | 5.5% | 0.10 | 0.06 | 100% |
| MARS | 5.5% | 0.10 | 0.06 | 100% |
| ESR-MARS | 5.3% | 0.10 | 0.06 | 100% |
| MARS-T | 5.0% | 0.09 | 0.06 | 100% |
| Soft Minimax-D | 5.0% | 0.09 | 0.06 | 100% |
| Copeland | 3.9% | 0.01 | 0.07 | 100% |
| Schulze | 3.0% | 0.01 | 0.04 | 100% |
| Minimax | 3.0% | 0.01 | 0.04 | 100% |
| MMWV | 3.0% | 0.01 | 0.04 | 100% |
| MMPO | 3.0% | 0.01 | 0.04 | 100% |
| Ranked pairs | 3.0% | 0.02 | 0.04 | 100% |
| BTR-IRV | 2.9% | 0.02 | 0.04 | 100% |
| Kemeny-Young | 2.8% | 0.01 | 0.05 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 1957 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 43 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| MARS | 87.3% (86.7%) | 10.2% (10.6%) | 2.5% (2.7%) | 69.8% (65.1%) | 95.5% (95.4%) | 85.9% |
| Soft Minimax-D | 88.2% (88.1%) | 9.1% (9.7%) | 2.6% (2.2%) | 67.4% (67.4%) | 97.9% (97.8%) | 88.1% |
| ESR-MARS | 87.1% (86.6%) | 10.5% (10.4%) | 2.4% (3.0%) | 69.8% (60.5%) | 94.5% (95.0%) | 85.2% |
| MARS-T | 87.2% (86.7%) | 10.6% (10.6%) | 2.2% (2.7%) | 69.8% (65.1%) | 94.1% (94.9%) | 84.7% |
| Soft Minimax-40 | 87.8% (87.1%) | 9.7% (10.1%) | 2.5% (2.8%) | 67.4% (60.5%) | 92.8% (93.3%) | 83.6% |
| ARA | 86.9% (86.4%) | 10.1% (10.5%) | 3.1% (3.2%) | 67.4% (67.4%) | 90.4% (90.6%) | 81.8% |
| STAR | 86.7% (87.1%) | 9.9% (10.1%) | 3.4% (2.9%) | 74.4% (69.8%) | 91.7% (88.7%) | 81.7% |
| BTR-score | 87.8% (87.8%) | 9.3% (9.6%) | 2.9% (2.6%) | 74.4% (79.1%) | 100.0% (100.0%) | 89.0% |
| Smith//Score | 87.9% (87.8%) | 9.4% (9.7%) | 2.7% (2.6%) | 79.1% (79.1%) | 100.0% (100.0%) | 89.0% |
| Soft Minimax | 88.2% (88.3%) | 9.1% (9.4%) | 2.7% (2.3%) | 74.4% (72.1%) | 100.0% (100.0%) | 89.7% |
| IMDS | 87.9% (87.9%) | 9.2% (9.4%) | 2.9% (2.7%) | 74.4% (69.8%) | 99.9% (99.9%) | 89.6% |
| ESR | 88.1% (87.7%) | 9.7% (10.1%) | 2.2% (2.2%) | 72.1% (67.4%) | 96.8% (98.5%) | 87.5% |
| 321 | 74.6% (72.0%) | 18.3% (19.3%) | 7.1% (8.7%) | 86.0% (86.0%) | 97.1% (98.4%) | 78.4% |
| Delegated 321 | 74.6% (72.0%) | 18.3% (19.3%) | 7.1% (8.7%) | 86.0% (86.0%) | 97.1% (98.4%) | 78.4% |
| Minimax | 97.3% (98.2%) | 1.6% (1.2%) | 1.0% (0.7%) | 51.2% (46.5%) | 100.0% (100.0%) | 97.8% |
| Schulze | 97.3% (98.2%) | 1.6% (1.2%) | 1.1% (0.7%) | 51.2% (46.5%) | 100.0% (100.0%) | 97.7% |
| Ranked pairs | 97.5% (98.2%) | 1.5% (1.2%) | 1.0% (0.6%) | 53.5% (53.5%) | 100.0% (100.0%) | 98.0% |
| MMWV | 97.3% (98.2%) | 1.6% (1.2%) | 1.1% (0.7%) | 46.5% (46.5%) | 100.0% (100.0%) | 97.8% |
| MMPO | 97.2% (98.2%) | 1.6% (1.2%) | 1.1% (0.7%) | 48.8% (46.5%) | 100.0% (100.0%) | 97.7% |
| Kemeny-Young | 97.4% (98.2%) | 1.6% (1.2%) | 1.0% (0.6%) | 48.8% (48.8%) | 100.0% (100.0%) | 97.8% |
| Copeland | 97.5% (98.1%) | 1.5% (1.2%) | 1.0% (0.7%) | 53.5% (51.2%) | 100.0% (100.0%) | 98.0% |
| MJ | 87.1% (87.3%) | 10.1% (9.9%) | 2.8% (2.8%) | 76.7% (65.1%) | 89.8% (89.2%) | 86.5% |
| DMS | 86.7% (86.6%) | 10.2% (10.5%) | 3.1% (3.0%) | 62.8% (58.1%) | 84.4% (87.5%) | 77.4% |
| Score | 86.7% (86.2%) | 9.8% (10.2%) | 3.6% (3.6%) | 69.8% (58.1%) | 83.1% (87.2%) | 75.7% |
| BTR-IRV | 97.0% (98.5%) | 2.1% (1.1%) | 0.9% (0.4%) | 67.4% (62.8%) | 100.0% (100.0%) | 97.2% |
| MAV | 86.6% (86.4%) | 10.6% (10.4%) | 2.8% (3.3%) | 65.1% (53.5%) | 89.7% (88.8%) | 85.2% |
| Borda | 95.8% (98.8%) | 2.2% (0.9%) | 2.0% (0.3%) | 37.2% (11.6%) | 85.5% (85.1%) | 83.7% |
| ER-IRV | 96.5% (98.7%) | 2.9% (0.9%) | 0.6% (0.5%) | 58.1% (41.9%) | 94.2% (94.1%) | 92.2% |
| IRV | 95.1% (98.7%) | 3.8% (0.9%) | 1.0% (0.5%) | 60.5% (41.9%) | 93.6% (94.1%) | 90.4% |
| IRV-3 | 89.2% (91.2%) | 8.4% (6.9%) | 2.4% (1.9%) | 83.7% (81.4%) | 97.1% (97.7%) | 88.2% |
| Supplementary vote | 89.7% (91.1%) | 10.1% (8.7%) | 0.3% (0.2%) | 60.5% (51.2%) | 90.0% (89.9%) | 81.3% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| MARS | 0.1% (0.1%) | 11.7% (11.1%) | 0.8% (0.8%) |
| Soft Minimax-D | 0.2% (0.0%) | 11.0% (10.6%) | 0.8% (0.8%) |
| ESR-MARS | 0.1% (0.1%) | 11.2% (11.2%) | 0.8% (0.8%) |
| MARS-T | 0.1% (0.0%) | 11.3% (10.9%) | 0.8% (0.8%) |
| Score runoff | 0.4% (0.4%) | — (—) | — (—) |
| Soft Minimax-40 | 1.1% (0.7%) | 11.5% (11.0%) | 0.9% (0.8%) |
| ARA | 1.4% (0.8%) | 12.0% (11.6%) | 0.9% (0.8%) |
| STAR | 0.2% (0.3%) | 13.7% (14.4%) | 1.0% (1.0%) |
| BTR-score | 0.1% (0.0%) | 11.3% (10.9%) | 0.9% (0.8%) |
| Smith//Score | 0.0% (0.0%) | 11.3% (10.9%) | 0.8% (0.8%) |
| Soft Minimax | 1.9% (1.5%) | 11.6% (11.2%) | 0.9% (0.9%) |
| IMDS | 1.7% (1.6%) | 12.4% (11.5%) | 1.0% (1.0%) |
| ESR | 0.9% (0.9%) | 11.2% (11.2%) | 0.9% (1.0%) |
| 321 | 0.4% (0.9%) | 22.2% (23.4%) | 1.6% (1.9%) |
| Delegated 321 | 0.4% (0.9%) | 22.2% (23.4%) | 1.6% (1.9%) |
| Approval runoff | 1.2% (1.2%) | — (—) | — (—) |
| Approval cond. runoff | 1.7% (0.9%) | — (—) | — (—) |
| Minimax | 0.9% (0.8%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Schulze | 0.9% (0.8%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Ranked pairs | 0.7% (0.5%) | 0.2% (0.0%) | 0.0% (0.0%) |
| MMWV | 0.9% (0.8%) | 0.5% (0.0%) | 0.0% (0.0%) |
| MMPO | 1.0% (0.8%) | 0.5% (0.0%) | 0.0% (0.0%) |
| Approval | 1.7% (0.9%) | — (—) | — (—) |
| Kemeny-Young | 0.6% (0.4%) | 0.3% (0.0%) | 0.0% (0.0%) |
| Copeland | 1.2% (1.1%) | 0.5% (0.0%) | 0.0% (0.0%) |
| MJ | 0.0% (0.0%) | 10.6% (10.7%) | 0.9% (0.9%) |
| DMS | 0.0% (0.0%) | 10.8% (10.1%) | 0.8% (0.7%) |
| Score | 0.4% (0.5%) | 12.4% (11.5%) | 0.9% (0.8%) |
| BTR-IRV | 0.1% (0.0%) | 0.2% (0.0%) | 0.0% (0.0%) |
| MAV | 0.6% (0.8%) | 10.5% (11.5%) | 0.9% (1.0%) |
| Borda | 0.5% (0.4%) | 3.6% (0.0%) | 0.2% (0.0%) |
| ER-IRV | 1.1% (0.8%) | 0.4% (0.0%) | 0.0% (0.0%) |
| TTR | 0.9% (0.7%) | — (—) | — (—) |
| IRV | 0.4% (0.8%) | 0.1% (0.0%) | 0.0% (0.0%) |
| IRV-3 | 0.8% (0.7%) | 10.5% (10.3%) | 0.7% (0.7%) |
| Supplementary vote | 0.4% (0.4%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Asset | 1.1% (1.0%) | — (—) | — (—) |
| Plurality | 1.2% (0.8%) | — (—) | — (—) |

![strategy arrows](arrows-c4.png)

![strategy shifts](shift-c4.png)

![sincerity](sincerity-c4.png)

![fidelity](fidelity-c4.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c4.png)

Scatter and chain plots leave out Condorcet-random-drop, Geometric median, Random ballot, Random pair runoff, Anti-plurality, TRASH, CW else Score loser, CL else Score winner, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
