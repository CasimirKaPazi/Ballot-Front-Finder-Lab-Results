# Ballot Front Finder Lab — analysis

Input: base, hard (simulator version 2.1.0; analysis version 2.1.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

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

![sweep over candidate dispersion + laziness + poll error](chain-cand_dispersion-lazy-poll_error.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis); for conditions with fewer elections than the base, a further row gives the agreement expected between the base and a run of that size. A condition near or above its row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 0.97 | 0.99 | 0.95 | 0.99 | 0.99 |
| noise reference: c1 vs a run of 2000 elections | 0.95 | 0.99 | 0.92 | 0.99 | 0.99 |
| c2: candidate dispersion 0.5, laziness 0.25, poll error 4.0 | 0.29 | 0.96 | 0.25 | 0.93 | 0.95 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 97.5% | 69.9% | 14.9% | 8.7 |
| c2: candidate dispersion 0.5, laziness 0.25, poll error 4.0 | 85.8% | 43.6% | 37.2% | 2.2 |

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

## c2: candidate dispersion 0.5, laziness 0.25, poll error 4.0

2000 elections (seeds 1), 1716 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7, candidates: voters who stand, dispersion 0.5 (by who stands); 3 poll round(s), 5 polls of 49 voters, poll error 4.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), laziness 0.25, strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 85.8%; it also has the highest total utility in 43.6% of those; electing it loses 37.2% of VSE; favourite ÷ least liked 2.2 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 57.9 to 60.1; with the best VSE (100) WME 52.0 (nothing lower is possible there either); the most balanced point VSE 87.3 / WME 89.6. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 63.2 (at WME 91.2); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 62.8 (at WME 93.5); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE 1.3 / WME -9.2. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 57.7 / WME 98.2. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 61.0 / 92.9; normalized utility winner 63.1 / 85.8; best worst margin 59.0 / 100.0; Condorcet winner, else random ballot 54.3 / 91.9; Condorcet winner, else utility winner 67.8 / 95.6; random pair, majority 33.5 / 28.8; nearest the median voter 67.0 / 91.9; nearest the mean voter 20.6 / 76.7.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Asset, Soft Minimax-40, Soft Minimax, Minimax.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Soft Minimax, Soft Minimax-D.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 54.3 / 91.9; its mirror on the utility side 58.0 / 78.9; random pair 33.5 / 28.8. No better than a lottery of them (Lead ≤ 0): with learned ballots ER-IRV, IRV, Approval runoff, TTR, Approval cond. runoff, IRV-3, Approval, Supplementary vote, Plurality; with honest ballots Approval runoff, TTR, Approval cond. runoff, IRV-3, Approval, Supplementary vote, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Asset | delegated | 61.9 [59.0–64.7] | 59.2 [56.9–61.7] | 3.9 [0.8–7.0] | 1.3 [-1.5–4.2] | 84 [64–>99] | 98.9 [98.8–98.9] | 63.1 | 64.3 | 63.6 [60.9–66.4] | 61.1 [58.7–63.4] | 5.6 [2.6–8.3] | -0.4 [-3.2–2.3] | >99 [75–>99] | 51.6 [49.3–54.2] | 50.2 [47.9–52.9] | 57.3 [54.5–60.1] | 54.9 [52.1–57.9] | 59.0 [56.8–61.1] | 57.3 [55.1–59.6] | 17.8 [16.0–19.4] | 18.0 [16.4–19.6] | 2.9% | 0.0% | 0.8% | 0.0% | Smith//Score (WME\* sincerity), Score (WME\* sincerity), IMDS (WME\* sincerity) +14 more |
| Soft Minimax-40 | hybrid | 61.2 [58.6–63.7] | 93.3 [92.4–94.2] | 5.7 [4.4–6.9] | 1.9 [-0.5–4.3] | 79 [72–94] | 100.0 [100.0–100.0] | 93.0 | 93.8 | 61.2 [58.5–63.7] | 94.0 [93.1–94.9] | 5.9 [4.6–7.1] | 1.8 [-0.6–4.3] | 80 [74–96] | 86.6 [85.0–88.1] | 85.5 [83.9–87.1] | 96.7 [95.6–97.6] | 95.5 [94.4–96.6] | 93.2 [92.2–94.4] | 92.6 [91.5–93.7] | 18.6 [16.8–20.2] | 18.8 [17.0–20.4] | 0.0% | 0.3% | 0.0% | 2.2% | Copeland (WME\*), Soft Minimax (WME\*), Soft Minimax-D (WME\*) |
| Soft Minimax | hybrid | 60.9 [58.2–63.5] | 94.7 [94.0–95.5] | 5.8 [4.5–7.0] | 2.1 [-0.4–4.2] | 80 [76–92] | 100.0 [100.0–100.0] | 93.1 | 93.7 | 60.9 [58.3–63.4] | 95.3 [94.5–96.0] | 5.9 [4.7–7.2] | 2.0 [-0.3–3.9] | 82 [78–94] | 90.0 [88.6–91.4] | 89.0 [87.7–90.4] | 96.7 [95.7–97.7] | 95.7 [94.5–96.8] | 92.7 [91.6–93.8] | 91.6 [90.5–92.8] | 18.9 [17.2–20.6] | 18.9 [17.1–20.5] | 0.0% | 0.2% | 0.0% | 1.8% | — |
| Soft Minimax-D | hybrid | 60.7 [58.1–63.4] | 94.7 [93.9–95.5] | 5.7 [4.4–7.0] | 2.2 [-0.3–4.2] | 80 [75–91] | 100.0 [100.0–100.0] | 93.0 | 93.6 | 60.8 [58.2–63.4] | 95.2 [94.4–96.0] | 5.8 [4.6–7.1] | 2.1 [-0.3–4.0] | 81 [77–93] | 88.9 [87.4–90.4] | 88.3 [86.7–89.8] | 96.6 [95.6–97.6] | 95.9 [94.8–97.0] | 93.7 [92.6–94.7] | 92.8 [91.6–93.9] | 18.8 [17.1–20.5] | 18.9 [17.1–20.6] | 0.0% | 0.2% | 0.0% | 1.8% | — |
| STAR | hybrid | 60.7 [58.1–63.5] | 91.1 [90.1–92.1] | 4.8 [3.4–6.3] | 2.4 [-0.3–4.9] | 74 [66–86] | 100.0 [100.0–100.0] | 92.5 | 93.7 | 61.7 [59.1–64.4] | 92.7 [91.8–93.7] | 6.0 [4.7–7.4] | 1.4 [-1.3–3.9] | 82 [74–>99] | 83.4 [81.5–85.2] | 81.0 [79.0–82.8] | 97.2 [96.3–98.1] | 94.6 [93.3–95.8] | 95.2 [94.2–96.1] | 92.7 [91.6–93.9] | 18.4 [16.7–20.1] | 18.9 [17.1–20.6] | 0.0% | 0.5% | 0.0% | 3.7% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +10 more |
| Score runoff | two-round | 60.7 [58.0–63.4] | 92.0 [91.0–92.9] | 5.0 [3.6–6.3] | 2.4 [-0.2–5.0] | 75 [68–85] | 99.9 [99.9–99.9] | 92.4 | 93.7 | 61.8 [59.3–64.5] | 92.7 [91.8–93.6] | 6.1 [4.7–7.4] | 1.3 [-1.3–3.7] | 84 [74–>99] | 83.6 [81.7–85.3] | 82.8 [80.9–84.6] | 98.1 [97.2–98.8] | 96.3 [95.2–97.4] | 95.3 [94.3–96.2] | 93.7 [92.6–94.9] | 18.4 [16.7–20.1] | 19.5 [17.8–21.1] | 0.0% | 0.7% | 0.0% | 3.5% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +7 more |
| Delegated 321 | delegated | 60.7 [58.0–63.5] | 86.3 [85.0–87.7] | 3.7 [2.1–5.4] | 2.5 [-0.3–5.2] | 73 [62–90] | — | — | — | 61.3 [58.6–64.0] | 87.7 [86.3–88.9] | 4.5 [2.9–6.1] | 1.9 [-0.8–4.6] | 78 [67–96] | 77.8 [75.9–79.7] | 76.4 [74.4–78.5] | 89.4 [87.6–91.1] | 87.7 [85.8–89.5] | 88.4 [87.0–89.9] | 86.7 [85.2–88.1] | 19.1 [17.2–20.8] | 19.4 [17.5–21.1] | 0.0% | 0.9% | 0.0% | 2.7% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +15 more |
| Copeland | condorcet | 60.0 [57.4–62.6] | 94.5 [93.8–95.3] | 5.0 [3.9–6.3] | 3.0 [0.5–4.7] | 77 [73–84] | 99.8 [99.8–99.8] | 81.7 | 82.2 | 60.0 [57.5–62.6] | 95.6 [94.9–96.3] | 5.3 [4.2–6.4] | 2.7 [0.4–3.9] | 80 [77–87] | 92.4 [91.2–93.6] | 90.5 [89.1–91.7] | 97.6 [96.7–98.3] | 95.9 [94.8–96.9] | 92.5 [91.4–93.5] | 91.2 [90.0–92.3] | 18.9 [17.2–20.5] | 18.8 [17.1–20.3] | 1.2% | 0.4% | 0.2% | 0.5% | Schulze (WME\*), Minimax (WME\*) |
| MARS-T | hybrid | 60.0 [57.2–62.7] | 91.8 [90.8–92.9] | 4.4 [3.0–5.9] | 3.1 [0.5–5.7] | 71 [65–79] | 100.0 [100.0–100.0] | 92.5 | 93.7 | 60.6 [57.9–63.1] | 94.0 [93.2–94.9] | 5.4 [4.1–6.7] | 2.4 [-0.0–4.7] | 78 [73–87] | 85.5 [83.9–87.2] | 83.2 [81.3–84.9] | 96.7 [95.6–97.6] | 94.8 [93.5–95.9] | 94.0 [92.9–95.0] | 92.0 [90.8–93.2] | 18.6 [16.9–20.4] | 19.1 [17.2–20.8] | 0.0% | 0.4% | 0.0% | 3.7% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +9 more |
| ARA | hybrid | 60.0 [57.2–62.6] | 90.1 [88.8–91.2] | 4.0 [2.7–5.5] | 3.2 [0.6–5.8] | 69 [62–81] | 100.0 [100.0–100.0] | 92.9 | 93.9 | 61.4 [58.8–64.0] | 92.0 [91.0–93.0] | 5.5 [4.3–6.8] | 1.8 [-0.9–4.3] | 79 [70–97] | 83.8 [81.8–85.6] | 81.9 [79.9–83.7] | 95.6 [94.4–96.7] | 93.1 [91.6–94.4] | 91.9 [90.7–93.1] | 89.8 [88.4–91.1] | 18.2 [16.4–19.9] | 19.1 [17.3–20.9] | 0.0% | 0.3% | 0.0% | 3.0% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +13 more |
| Borda | positional | 59.9 [57.1–62.6] | 89.9 [88.8–91.0] | 4.0 [2.2–5.7] | 3.2 [0.6–5.9] | 69 [61–83] | 99.6 [99.6–99.6] | 80.7 | 81.8 | 62.1 [59.3–64.4] | 92.2 [91.3–93.0] | 6.1 [4.5–7.8] | 1.1 [-1.2–3.7] | 86 [72–>99] | 80.2 [78.3–82.1] | 77.0 [74.9–79.0] | 87.6 [85.7–89.4] | 85.5 [83.5–87.4] | 86.8 [85.3–88.3] | 84.3 [82.6–85.8] | 16.6 [15.1–18.2] | 17.8 [16.2–19.4] | 3.0% | 1.1% | 0.3% | 2.1% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +13 more |
| Smith//Score | condorcet-based | 59.9 [57.3–62.5] | 93.8 [92.8–94.7] | 4.8 [3.6–6.1] | 3.1 [0.6–5.2] | 75 [70–82] | 100.0 [100.0–100.0] | 92.7 | 93.7 | 60.5 [57.9–63.2] | 95.2 [94.4–96.0] | 5.6 [4.4–6.8] | 2.4 [-0.1–4.1] | 80 [76–89] | 90.1 [88.6–91.5] | 87.5 [85.9–89.1] | 97.6 [96.6–98.4] | 95.9 [94.8–97.0] | 94.2 [93.2–95.2] | 92.8 [91.6–93.8] | 19.0 [17.3–20.7] | 19.5 [17.8–21.2] | 0.0% | 0.4% | 0.0% | 3.1% | Schulze (WME\*), Kemeny-Young (WME\*), Minimax (WME\*) +3 more |
| 321 | hybrid | 59.9 [57.2–62.8] | 86.4 [85.0–87.8] | 3.2 [1.5–4.9] | 3.3 [0.4–5.9] | 68 [59–81] | 99.9 [99.9–100.0] | 86.1 | 87.1 | 60.8 [58.1–63.7] | 87.6 [86.3–88.9] | 4.1 [2.5–5.8] | 2.4 [-0.5–5.1] | 74 [63–91] | 77.8 [75.8–79.7] | 76.3 [74.3–78.3] | 89.5 [87.8–91.3] | 88.1 [86.3–89.8] | 88.5 [87.0–89.9] | 86.6 [85.2–88.2] | 19.4 [17.5–21.2] | 19.9 [18.1–21.6] | 0.0% | 0.5% | 0.0% | 3.1% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +18 more |
| IMDS | rated | 59.8 [57.2–62.5] | 92.6 [91.6–93.5] | 4.5 [3.2–5.7] | 3.2 [0.6–5.6] | 72 [66–81] | 100.0 [100.0–100.0] | 92.1 | 93.7 | 61.0 [58.4–63.7] | 94.5 [93.6–95.4] | 5.8 [4.6–7.0] | 2.0 [-0.6–4.2] | 80 [75–95] | 89.2 [87.8–90.8] | 86.0 [84.4–87.6] | 96.2 [95.1–97.2] | 93.8 [92.4–95.0] | 91.6 [90.4–92.8] | 89.4 [88.0–90.7] | 18.5 [16.8–20.1] | 19.8 [17.9–21.4] | 0.0% | 0.3% | 0.0% | 5.8% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +8 more |
| Score | rated | 59.8 [57.1–62.6] | 87.8 [86.6–89.0] | 3.4 [1.9–4.9] | 3.4 [0.6–6.0] | 67 [60–78] | 100.0 [100.0–100.0] | 92.2 | 93.8 | 61.3 [58.8–64.0] | 90.7 [89.7–91.7] | 5.2 [3.9–6.6] | 1.8 [-0.8–4.3] | 78 [69–92] | 78.7 [76.8–80.7] | 74.8 [72.6–76.8] | 94.3 [93.0–95.6] | 89.6 [88.0–91.2] | 93.9 [92.8–95.0] | 89.5 [88.2–90.8] | 18.4 [16.7–20.1] | 19.4 [17.5–21.1] | 0.0% | 0.4% | 0.0% | 5.8% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +17 more |
| MARS | hybrid | 59.8 [57.0–62.4] | 92.2 [91.1–93.1] | 4.3 [3.1–5.7] | 3.3 [0.7–5.9] | 70 [65–78] | 100.0 [100.0–100.0] | 92.5 | 93.7 | 60.7 [58.0–63.3] | 94.2 [93.3–95.0] | 5.5 [4.2–6.8] | 2.3 [-0.2–4.6] | 78 [74–88] | 86.1 [84.4–87.7] | 83.4 [81.6–85.2] | 96.7 [95.6–97.6] | 94.4 [93.2–95.7] | 94.1 [93.0–95.1] | 92.0 [90.7–93.1] | 18.6 [16.8–20.3] | 19.3 [17.5–21.1] | 0.0% | 0.4% | 0.0% | 3.7% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +9 more |
| Minimax | minimax-family | 59.4 [56.8–62.0] | 95.3 [94.6–96.1] | 4.8 [3.5–6.0] | 3.2 [1.0–4.3] | 78 [75–83] | 99.8 [99.8–99.8] | 82.0 | 82.5 | 59.6 [56.8–62.2] | 96.7 [96.1–97.3] | 5.2 [4.3–6.4] | 2.3 [0.7–3.3] | 83 [80–88] | 93.1 [91.9–94.2] | 90.5 [89.1–91.8] | 97.9 [97.0–98.7] | 95.7 [94.6–96.7] | 93.9 [92.8–94.8] | 92.1 [91.0–93.2] | 19.8 [17.9–21.4] | 19.6 [17.8–21.2] | 1.2% | 0.5% | 0.2% | 0.5% | Soft Minimax (VSE\*) |
| BTR-score | hybrid | 59.3 [56.6–61.9] | 93.4 [92.5–94.3] | 4.2 [2.9–5.5] | 3.7 [1.2–5.7] | 72 [68–78] | 100.0 [100.0–100.0] | 92.7 | 93.7 | 60.0 [57.2–62.6] | 94.9 [94.1–95.7] | 5.1 [3.9–6.4] | 2.9 [0.5–4.6] | 78 [74–85] | 90.2 [88.8–91.7] | 87.4 [85.8–89.0] | 97.5 [96.6–98.4] | 95.1 [94.0–96.2] | 93.8 [92.7–94.8] | 91.8 [90.7–93.1] | 19.1 [17.4–20.9] | 19.8 [18.0–21.4] | 0.0% | 0.3% | 0.0% | 3.1% | Soft Minimax (VSE\* WME\*), Soft Minimax-D (VSE\* WME\*), Ranked pairs (WME\*) +7 more |
| Ranked pairs | condorcet | 59.3 [56.6–61.9] | 94.4 [93.6–95.3] | 4.5 [3.2–5.7] | 3.6 [1.1–5.0] | 75 [71–79] | 99.8 [99.8–99.8] | 82.0 | 82.5 | 59.7 [56.9–62.3] | 96.4 [95.8–97.1] | 5.2 [4.2–6.4] | 2.5 [0.7–3.5] | 82 [79–87] | 93.0 [91.7–94.1] | 89.6 [88.2–90.9] | 97.8 [97.0–98.6] | 95.3 [94.1–96.4] | 93.4 [92.4–94.4] | 91.4 [90.2–92.6] | 19.6 [17.8–21.3] | 19.8 [18.0–21.4] | 1.2% | 0.4% | 0.2% | 0.4% | Schulze (WME\*), Minimax (WME\*), Soft Minimax (VSE\*) +1 more |
| Schulze | condorcet | 59.2 [56.6–61.8] | 95.3 [94.5–96.1] | 4.6 [3.4–5.8] | 3.3 [1.2–4.4] | 78 [74–82] | 99.8 [99.8–99.8] | 82.0 | 82.5 | 59.5 [56.8–62.2] | 96.7 [96.1–97.3] | 5.1 [4.2–6.3] | 2.3 [0.7–3.4] | 83 [80–88] | 93.1 [91.9–94.2] | 90.7 [89.3–92.0] | 97.9 [97.0–98.7] | 95.9 [94.8–96.9] | 94.0 [93.0–94.9] | 92.3 [91.1–93.4] | 19.8 [18.0–21.4] | 19.8 [17.9–21.4] | 1.2% | 0.4% | 0.2% | 0.5% | Soft Minimax (VSE\*), Soft Minimax-D (VSE\*) |
| Kemeny-Young | condorcet | 59.2 [56.5–61.8] | 95.0 [94.2–95.8] | 4.5 [3.3–5.7] | 3.5 [1.2–4.7] | 76 [73–81] | 99.8 [99.8–99.8] | 82.0 | 82.5 | 59.4 [56.7–62.0] | 96.6 [96.0–97.2] | 5.0 [4.1–6.2] | 2.5 [0.9–3.5] | 82 [80–87] | 93.1 [91.9–94.3] | 90.1 [88.8–91.5] | 98.0 [97.2–98.7] | 95.8 [94.6–96.8] | 93.5 [92.5–94.6] | 92.0 [90.7–93.1] | 19.7 [17.9–21.3] | 20.0 [18.1–21.6] | 1.1% | 0.4% | 0.2% | 0.4% | Soft Minimax (VSE\*), Soft Minimax-D (VSE\*) |
| MMPO | minimax-family | 58.8 [56.3–61.4] | 95.1 [94.3–95.7] | 4.2 [3.1–5.4] | 3.7 [1.5–4.7] | 76 [73–79] | 99.8 [99.8–99.9] | 81.7 | 82.4 | 59.5 [56.9–62.1] | 96.4 [95.9–97.0] | 5.1 [4.1–6.3] | 2.5 [0.9–3.5] | 82 [79–86] | 93.3 [92.1–94.3] | 90.8 [89.6–92.0] | 97.2 [96.3–98.1] | 95.1 [93.9–96.2] | 92.1 [90.9–93.3] | 90.3 [89.0–91.6] | 18.9 [17.1–20.6] | 19.3 [17.5–21.0] | 0.4% | 0.7% | 0.3% | 1.9% | Copeland (VSE\*), Soft Minimax (VSE\*), Soft Minimax-D (VSE\*) |
| MMWV | minimax-family | 58.7 [56.0–61.3] | 94.5 [93.7–95.3] | 4.0 [2.8–5.3] | 3.9 [1.7–5.2] | 74 [71–78] | 99.8 [99.8–99.9] | 81.8 | 82.5 | 59.5 [56.9–62.1] | 96.2 [95.6–96.9] | 5.0 [4.0–6.3] | 2.7 [0.8–3.7] | 81 [78–86] | 93.0 [91.8–94.2] | 89.7 [88.3–91.0] | 97.7 [96.8–98.4] | 94.8 [93.5–96.0] | 93.2 [92.1–94.3] | 90.7 [89.5–92.0] | 19.4 [17.5–21.0] | 19.7 [17.8–21.3] | 0.4% | 0.7% | 0.2% | 1.8% | Copeland (VSE\*), Minimax (WME\*), Soft Minimax (VSE\*) +1 more |
| BTR-IRV | condorcet-based | 58.5 [55.7–61.1] | 91.0 [90.0–92.1] | 3.1 [1.7–4.5] | 4.6 [2.0–7.1] | 64 [60–72] | 99.6 [99.5–99.6] | 81.6 | 82.5 | 59.4 [56.6–62.1] | 95.2 [94.5–95.9] | 4.7 [3.6–5.8] | 3.3 [1.0–4.5] | 77 [74–82] | 91.6 [90.4–92.9] | 85.1 [83.6–86.8] | 97.3 [96.4–98.1] | 92.5 [91.1–93.9] | 91.8 [90.6–92.9] | 87.6 [86.2–88.9] | 20.1 [18.4–21.8] | 21.0 [19.2–22.7] | 2.3% | 0.6% | 0.6% | 0.8% | Soft Minimax (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*), Soft Minimax-D (VSE\* WME\*) +12 more |
| ESR-MARS | hybrid | 58.4 [55.6–61.2] | 90.0 [88.9–91.1] | 2.8 [1.3–4.3] | 4.7 [2.0–7.3] | 63 [59–69] | 100.0 [100.0–100.0] | 92.6 | 93.8 | 60.7 [58.0–63.5] | 92.6 [91.7–93.5] | 5.1 [3.8–6.5] | 2.4 [-0.3–5.0] | 75 [69–85] | 82.4 [80.5–84.2] | 79.5 [77.5–81.5] | 96.3 [95.1–97.2] | 92.2 [90.8–93.8] | 94.2 [93.1–95.2] | 90.7 [89.5–91.9] | 18.9 [17.1–20.5] | 20.5 [18.7–22.3] | 0.0% | 0.4% | 0.0% | 4.1% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), MARS-T (VSE\* WME\*) +14 more |
| MJ | median-based | 58.3 [55.6–60.9] | 86.4 [85.0–87.9] | 1.9 [0.4–3.6] | 4.9 [2.3–7.5] | 59 [53–70] | 100.0 [100.0–100.0] | 92.7 | 94.0 | 59.6 [56.9–62.2] | 88.4 [87.1–89.7] | 3.4 [1.9–4.9] | 3.6 [1.0–6.2] | 66 [59–78] | 77.8 [75.8–79.8] | 75.9 [73.8–78.0] | 88.6 [86.8–90.4] | 86.8 [84.8–88.7] | 87.5 [86.0–89.0] | 86.0 [84.4–87.5] | 19.8 [18.1–21.5] | 20.6 [18.8–22.4] | 0.0% | 0.3% | 0.0% | 4.3% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), Soft Minimax (VSE\* WME\*) +20 more |
| ESR | rated | 57.7 [55.0–60.4] | 89.3 [88.2–90.6] | 2.1 [0.7–3.6] | 5.4 [2.7–8.0] | 60 [56–65] | 100.0 [100.0–100.0] | 92.3 | 93.9 | 59.5 [56.9–62.2] | 92.5 [91.6–93.4] | 4.2 [2.8–5.6] | 3.5 [0.9–5.8] | 70 [66–77] | 83.6 [81.8–85.3] | 79.0 [77.1–81.0] | 96.4 [95.4–97.3] | 91.0 [89.4–92.6] | 93.5 [92.4–94.6] | 89.6 [88.2–90.9] | 20.2 [18.3–22.0] | 21.4 [19.5–23.3] | 0.0% | 0.3% | 0.0% | 5.6% | Copeland (VSE\* WME\*), Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*) +16 more |
| MAV | median-based | 57.6 [54.9–60.3] | 83.9 [82.5–85.6] | 0.8 [-0.9–2.3] | 5.6 [2.9–8.3] | 56 [48–64] | 100.0 [100.0–100.0] | 92.9 | 94.2 | 58.3 [55.6–61.0] | 85.5 [84.2–86.9] | 1.7 [0.1–3.2] | 4.9 [2.2–7.5] | 59 [52–69] | 74.2 [72.1–76.3] | 72.7 [70.6–74.9] | 86.2 [84.2–88.1] | 84.0 [81.9–86.0] | 86.1 [84.5–87.7] | 84.3 [82.7–86.0] | 21.2 [19.4–23.1] | 21.1 [19.3–22.9] | 0.0% | 0.3% | 0.0% | 4.3% | Copeland (VSE\* WME\*), Smith//Score (VSE\* WME\*), Score (VSE\* WME\*) +23 more |
| DMS | rated | 57.6 [54.6–60.4] | 84.8 [83.2–86.2] | 1.0 [-0.7–2.6] | 5.6 [2.8–8.5] | 56 [49–64] | 100.0 [100.0–100.0] | 92.5 | 94.0 | 58.6 [55.7–61.4] | 86.9 [85.5–88.2] | 2.3 [0.7–3.7] | 4.6 [1.8–7.4] | 61 [54–70] | 74.8 [72.6–76.8] | 72.0 [69.8–74.3] | 92.4 [91.0–93.8] | 87.8 [86.1–89.7] | 91.4 [90.2–92.6] | 87.8 [86.3–89.2] | 21.2 [19.2–23.0] | 22.1 [20.1–23.9] | 0.0% | 0.5% | 0.0% | 5.1% | Copeland (VSE\* WME\*), Smith//Score (VSE\* WME\*), Score (VSE\* WME\*) +22 more |
| ER-IRV | plurality-based | 56.0 [53.2–58.9] | 78.5 [76.7–80.3] | -1.6 [-3.6–0.6] | 7.2 [4.3–10.0] | 49 [41–58] | 99.7 [99.6–99.7] | 82.7 | 83.7 | 57.4 [54.5–60.1] | 81.0 [79.4–82.7] | 0.0 [-1.9–1.9] | 5.8 [3.0–8.7] | 55 [46–64] | 73.5 [71.4–75.5] | 70.7 [68.6–72.8] | 82.2 [80.1–84.2] | 79.0 [76.6–81.3] | 79.7 [77.8–81.4] | 77.8 [75.9–79.6] | 22.3 [20.4–24.2] | 23.2 [21.2–25.1] | 1.1% | 0.7% | 0.6% | 2.4% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +25 more |
| IRV | plurality-based | 54.4 [51.6–57.2] | 73.4 [71.3–75.5] | -3.5 [-5.9–-0.9] | 8.8 [5.9–11.6] | 43 [36–51] | 98.8 [98.7–98.9] | 81.7 | 83.7 | 57.4 [54.5–60.1] | 81.0 [79.4–82.7] | 0.0 [-1.9–1.9] | 5.8 [3.0–8.7] | 55 [46–64] | 73.5 [71.4–75.5] | 66.9 [64.6–69.1] | 82.2 [80.1–84.2] | 75.0 [72.5–77.3] | 79.7 [77.8–81.4] | 74.0 [71.9–75.8] | 22.3 [20.4–24.2] | 24.3 [22.4–26.2] | 4.6% | 0.9% | 1.8% | 0.8% | MMWV (VSE\* WME\* sincerity), MMPO (VSE\* WME\* sincerity), Smith//Score (VSE\* WME\* sincerity) +28 more |
| Approval runoff | two-round | 53.1 [50.1–56.4] | 79.8 [78.1–81.5] | -3.6 [-5.6–-1.5] | 10.1 [6.8–13.1] | 39 [35–46] | 99.9 [99.9–100.0] | 72.5 | 72.8 | 53.1 [50.1–56.4] | 79.9 [78.2–81.6] | -3.5 [-5.5–-1.4] | 10.1 [6.8–13.0] | 40 [35–46] | 70.4 [68.4–72.6] | 70.3 [68.3–72.5] | 81.4 [79.5–83.6] | 81.2 [79.3–83.3] | 80.6 [78.9–82.3] | 80.4 [78.7–82.1] | 24.8 [22.8–26.7] | 25.1 [23.0–27.0] | 0.0% | 0.5% | 0.0% | 1.7% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +26 more |
| TTR | two-round | 52.9 [49.9–55.8] | 75.4 [73.4–77.3] | -4.7 [-6.8–-2.3] | 10.3 [7.4–13.3] | 38 [32–45] | 98.9 [98.8–99.0] | 61.9 | 62.8 | 54.2 [51.3–57.0] | 77.1 [75.2–78.9] | -3.3 [-5.3–-1.1] | 9.0 [6.2–11.9] | 42 [35–50] | 70.3 [68.1–72.2] | 68.3 [66.1–70.4] | 77.2 [74.8–79.4] | 74.5 [72.0–76.8] | 76.5 [74.7–78.2] | 74.4 [72.6–76.2] | 24.0 [22.1–25.8] | 24.7 [22.8–26.6] | 2.3% | 0.0% | 0.9% | 0.0% | Smith//Score (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity), MJ (VSE\* WME\* sincerity) +28 more |
| Approval cond. runoff | two-round | 52.7 [49.8–55.9] | 79.3 [77.6–81.0] | -4.0 [-5.9–-1.8] | 10.5 [7.3–13.4] | 38 [34–45] | 99.9 [99.9–100.0] | 72.5 | 72.9 | 52.9 [49.8–56.2] | 80.0 [78.4–81.7] | -3.7 [-5.7–-1.6] | 10.3 [7.0–13.3] | 39 [35–45] | 70.7 [68.8–72.9] | 69.9 [67.9–72.1] | 81.6 [79.5–83.8] | 81.0 [78.8–83.1] | 80.6 [79.0–82.4] | 80.2 [78.5–81.9] | 24.9 [22.9–26.7] | 25.1 [23.1–26.9] | 0.0% | 0.6% | 0.0% | 2.4% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +26 more |
| IRV-3 | plurality-based | 50.7 [47.7–53.7] | 69.5 [67.2–71.6] | -7.3 [-10.1–-4.4] | 12.5 [9.5–15.4] | 32 [27–39] | 98.7 [98.6–98.8] | 78.1 | 80.0 | 55.3 [52.4–58.2] | 75.5 [73.7–77.4] | -2.7 [-5.0–-0.1] | 7.9 [5.0–10.8] | 46 [38–55] | 67.1 [64.9–69.3] | 61.8 [59.5–64.2] | 76.1 [73.7–78.4] | 70.1 [67.6–72.8] | 75.2 [73.3–77.0] | 70.1 [68.1–72.2] | 23.8 [21.9–25.6] | 26.3 [24.4–28.1] | 4.4% | 0.9% | 1.8% | 0.4% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), Kemeny-Young (VSE\* WME\* sincerity) +30 more |
| Approval | approval-based | 43.3 [40.2–46.6] | 69.6 [67.6–71.7] | -13.5 [-15.8–-10.9] | 19.9 [16.5–23.0] | 24 [23–26] | 100.0 [100.0–100.0] | 73.7 | 74.0 | 43.4 [40.2–46.8] | 70.0 [68.1–72.0] | -13.3 [-15.7–-10.7] | 19.8 [16.4–23.0] | 24 [23–27] | 57.1 [54.8–59.4] | 56.9 [54.6–59.3] | 69.5 [67.1–72.1] | 68.8 [66.3–71.5] | 71.3 [69.3–73.1] | 70.9 [68.9–72.8] | 30.0 [27.8–31.9] | 30.3 [28.2–32.2] | 0.0% | 0.4% | 0.0% | 2.4% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +28 more |
| Supplementary vote | plurality-based | 40.3 [37.1–44.0] | 58.7 [56.4–61.3] | -17.7 [-20.9–-14.4] | 22.9 [19.1–26.1] | 16 [15–19] | 99.6 [99.5–99.6] | 82.9 | 83.6 | 42.8 [39.6–46.2] | 60.7 [58.5–63.3] | -15.2 [-18.3–-12.0] | 20.4 [16.9–23.6] | 19 [16–23] | 51.4 [49.0–53.7] | 49.8 [47.5–52.3] | 60.0 [57.4–62.7] | 58.0 [55.3–60.7] | 63.7 [61.6–65.8] | 62.0 [60.1–64.1] | 30.6 [28.6–32.7] | 32.1 [30.1–34.2] | 2.4% | 0.6% | 0.5% | 0.2% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +33 more |
| Plurality | plurality-based | 18.9 [14.9–23.1] | 40.5 [37.9–43.3] | -38.9 [-42.6–-34.7] | 44.3 [40.1–48.3] | 7.6 [6.9–8.4] | 99.4 [99.3–99.4] | 61.7 | 62.4 | 17.8 [14.0–22.1] | 40.1 [37.6–42.8] | -39.8 [-43.5–-35.6] | 45.3 [41.1–49.1] | 7.5 [6.8–8.2] | 34.3 [32.1–36.6] | 34.3 [32.2–36.7] | 41.0 [38.4–43.7] | 41.5 [38.9–44.2] | 46.9 [44.7–49.2] | 47.3 [45.1–49.6] | 41.2 [39.0–43.5] | 41.5 [39.4–43.7] | 3.8% | 0.0% | 0.4% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +34 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| IRV | 9.3% |
| IRV-3 | 8.8% |
| Borda | 7.3% |
| Plurality | 7.1% |
| IMDS | 6.8% |
| Score | 6.7% |
| ESR | 6.6% |
| DMS | 6.2% |
| Asset | 5.7% |
| MJ | 5.1% |
| MAV | 5.1% |
| ESR-MARS | 4.9% |
| BTR-IRV | 4.9% |
| ER-IRV | 4.8% |
| TTR | 4.7% |
| Score runoff | 4.5% |
| Supplementary vote | 4.5% |
| MARS | 4.5% |
| MARS-T | 4.5% |
| STAR | 4.4% |
| Delegated 321 | 4.4% |
| 321 | 4.2% |
| Approval | 4.2% |
| Approval cond. runoff | 4.1% |
| Smith//Score | 3.8% |
| BTR-score | 3.7% |
| ARA | 3.6% |
| MMPO | 3.3% |
| MMWV | 3.2% |
| Approval runoff | 2.8% |
| Minimax | 2.8% |
| Copeland | 2.8% |
| Soft Minimax-40 | 2.8% |
| Schulze | 2.7% |
| Ranked pairs | 2.7% |
| Kemeny-Young | 2.5% |
| Soft Minimax | 2.3% |
| Soft Minimax-D | 2.2% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| TTR | 53.2% | 1.31 | 1.02 | 98% |
| Supplementary vote | 48.4% | 1.78 | 0.95 | 97% |
| Plurality | 47.9% | 1.34 | 1.18 | 92% |
| IRV-3 | 46.1% | 1.43 | 0.63 | 99% |
| IRV | 43.9% | 0.88 | 0.20 | 99% |
| ER-IRV | 43.9% | 0.88 | 0.20 | 99% |
| Approval runoff | 42.0% | 1.15 | 0.71 | 100% |
| Approval cond. runoff | 41.7% | 1.16 | 0.72 | 100% |
| Asset | 41.5% | 0.78 | 0.30 | 98% |
| Approval | 38.5% | 1.29 | 0.50 | 99% |
| DMS | 36.0% | 0.71 | 0.16 | 100% |
| Delegated 321 | 35.4% | 0.91 | 0.60 | 100% |
| 321 | 35.1% | 0.91 | 0.60 | 100% |
| MAV | 31.4% | 0.80 | 0.19 | 100% |
| MJ | 26.2% | 0.67 | 0.19 | 100% |
| ARA | 22.7% | 0.57 | 0.25 | 99% |
| ESR | 21.6% | 0.58 | 0.20 | 100% |
| Score runoff | 21.1% | 0.46 | 0.39 | 100% |
| STAR | 20.2% | 0.50 | 0.40 | 100% |
| Copeland | 19.4% | 0.38 | 0.35 | 100% |
| Score | 19.4% | 0.51 | 0.17 | 100% |
| BTR-IRV | 18.9% | 0.45 | 0.26 | 99% |
| ESR-MARS | 18.9% | 0.49 | 0.16 | 100% |
| IMDS | 18.8% | 0.49 | 0.27 | 100% |
| Soft Minimax-40 | 18.0% | 0.49 | 0.21 | 100% |
| MMPO | 17.8% | 0.40 | 0.26 | 100% |
| Soft Minimax | 16.3% | 0.45 | 0.24 | 100% |
| Smith//Score | 16.0% | 0.38 | 0.17 | 100% |
| MARS-T | 16.0% | 0.39 | 0.15 | 100% |
| MARS | 15.9% | 0.40 | 0.16 | 100% |
| BTR-score | 15.5% | 0.39 | 0.17 | 100% |
| MMWV | 15.4% | 0.35 | 0.19 | 100% |
| Borda | 14.8% | 0.26 | 0.44 | 100% |
| Ranked pairs | 14.8% | 0.37 | 0.20 | 100% |
| Kemeny-Young | 14.7% | 0.37 | 0.21 | 99% |
| Soft Minimax-D | 14.6% | 0.35 | 0.16 | 100% |
| Schulze | 14.5% | 0.35 | 0.18 | 100% |
| Minimax | 14.5% | 0.35 | 0.18 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 1716 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 284 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Soft Minimax-40 | 83.2% (84.6%) | 10.9% (10.0%) | 5.9% (5.4%) | 59.5% (57.4%) | 95.9% (96.4%) | 85.5% |
| Soft Minimax | 84.6% (86.0%) | 8.6% (8.1%) | 6.8% (5.9%) | 64.1% (64.1%) | 100.0% (100.0%) | 89.0% |
| Soft Minimax-D | 84.3% (85.5%) | 9.2% (8.7%) | 6.5% (5.8%) | 59.2% (58.8%) | 99.2% (99.4%) | 88.3% |
| STAR | 81.3% (84.7%) | 11.4% (9.2%) | 7.3% (6.1%) | 63.0% (62.7%) | 91.7% (90.8%) | 81.0% |
| Delegated 321 | 73.0% (73.5%) | 17.3% (15.7%) | 9.7% (10.8%) | 73.9% (71.1%) | 93.7% (93.1%) | 76.4% |
| Copeland | 87.2% (89.0%) | 6.9% (5.9%) | 5.9% (5.0%) | 46.5% (47.5%) | 100.0% (100.0%) | 90.5% |
| MARS-T | 82.1% (84.4%) | 10.5% (9.7%) | 7.4% (5.9%) | 61.3% (58.5%) | 94.1% (95.1%) | 83.2% |
| ARA | 81.4% (83.1%) | 11.0% (10.3%) | 7.7% (6.6%) | 62.3% (62.7%) | 92.1% (92.8%) | 81.9% |
| Borda | 81.7% (87.9%) | 10.2% (6.0%) | 8.1% (6.1%) | 42.3% (33.1%) | 80.6% (79.7%) | 77.0% |
| Smith//Score | 84.3% (86.8%) | 8.9% (7.9%) | 6.8% (5.3%) | 72.5% (69.7%) | 100.0% (100.0%) | 87.5% |
| 321 | 73.6% (73.7%) | 17.5% (15.6%) | 8.9% (10.7%) | 70.4% (70.1%) | 94.1% (93.0%) | 76.3% |
| IMDS | 83.2% (86.8%) | 10.2% (8.6%) | 6.6% (4.7%) | 67.6% (65.1%) | 98.8% (99.3%) | 86.0% |
| Score | 80.2% (82.4%) | 12.5% (11.0%) | 7.3% (6.6%) | 59.2% (54.9%) | 83.9% (85.4%) | 74.8% |
| MARS | 82.0% (84.8%) | 10.7% (9.4%) | 7.3% (5.8%) | 63.0% (59.9%) | 95.1% (95.6%) | 83.4% |
| Minimax | 86.0% (89.3%) | 7.4% (5.9%) | 6.6% (4.7%) | 53.5% (52.1%) | 100.0% (100.0%) | 90.5% |
| BTR-score | 83.0% (86.5%) | 10.1% (8.2%) | 6.9% (5.4%) | 64.8% (66.2%) | 100.0% (100.0%) | 87.4% |
| Ranked pairs | 85.1% (89.3%) | 8.5% (5.9%) | 6.4% (4.7%) | 53.9% (50.4%) | 100.0% (100.0%) | 89.6% |
| Schulze | 86.1% (89.5%) | 7.5% (5.9%) | 6.4% (4.7%) | 53.9% (51.4%) | 100.0% (100.0%) | 90.7% |
| Kemeny-Young | 85.5% (89.2%) | 7.8% (5.8%) | 6.8% (5.0%) | 52.1% (49.3%) | 100.0% (100.0%) | 90.1% |
| MMPO | 84.6% (87.4%) | 7.3% (5.8%) | 8.1% (6.8%) | 55.6% (54.9%) | 97.2% (97.2%) | 90.8% |
| MMWV | 85.1% (88.7%) | 7.6% (5.7%) | 7.3% (5.6%) | 56.7% (55.6%) | 100.0% (100.0%) | 89.7% |
| BTR-IRV | 82.7% (89.8%) | 10.6% (6.5%) | 6.7% (3.7%) | 65.1% (65.1%) | 100.0% (100.0%) | 85.1% |
| ESR-MARS | 81.8% (82.9%) | 10.8% (10.5%) | 7.5% (6.6%) | 59.5% (57.4%) | 89.4% (91.8%) | 79.5% |
| MJ | 79.6% (81.7%) | 12.0% (10.5%) | 8.4% (7.8%) | 58.8% (56.7%) | 82.6% (83.2%) | 75.9% |
| ESR | 81.8% (84.1%) | 11.5% (10.3%) | 6.8% (5.6%) | 62.7% (60.6%) | 90.6% (93.5%) | 79.0% |
| MAV | 79.3% (81.0%) | 13.2% (11.5%) | 7.6% (7.5%) | 59.9% (56.7%) | 82.5% (82.5%) | 72.7% |
| DMS | 77.6% (80.7%) | 14.0% (12.3%) | 8.4% (7.0%) | 59.5% (54.9%) | 83.6% (84.0%) | 72.0% |
| ER-IRV | 82.6% (86.8%) | 11.8% (8.9%) | 5.7% (4.3%) | 57.0% (52.8%) | 83.6% (83.3%) | 70.7% |
| IRV | 76.2% (86.8%) | 16.7% (8.9%) | 7.1% (4.3%) | 63.7% (52.8%) | 84.1% (83.3%) | 66.9% |
| IRV-3 | 61.3% (67.7%) | 30.0% (25.1%) | 8.7% (7.2%) | 84.5% (82.4%) | 95.1% (95.4%) | 61.8% |
| Supplementary vote | 67.2% (71.7%) | 25.0% (22.4%) | 7.8% (5.9%) | 64.4% (61.6%) | 75.0% (74.9%) | 49.8% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| Asset | 2.8% (2.8%) | — (—) | — (—) |
| Soft Minimax-40 | 1.5% (0.9%) | 24.7% (24.6%) | 2.3% (2.3%) |
| Soft Minimax | 2.6% (2.2%) | 25.1% (25.1%) | 2.3% (2.3%) |
| Soft Minimax-D | 0.3% (0.2%) | 24.9% (24.6%) | 2.2% (2.2%) |
| STAR | 0.5% (0.4%) | 28.3% (28.1%) | 2.5% (2.5%) |
| Score runoff | 0.4% (0.1%) | — (—) | — (—) |
| Delegated 321 | 0.8% (0.4%) | 34.2% (36.4%) | 3.3% (3.5%) |
| Copeland | 5.2% (4.0%) | 21.9% (21.9%) | 1.8% (1.8%) |
| MARS-T | 0.2% (0.2%) | 25.0% (23.8%) | 2.3% (2.2%) |
| ARA | 2.4% (1.0%) | 25.2% (24.1%) | 2.3% (2.3%) |
| Borda | 1.1% (0.7%) | 24.1% (22.6%) | 1.8% (1.7%) |
| Smith//Score | 0.3% (0.1%) | 23.6% (23.5%) | 2.1% (2.1%) |
| 321 | 0.5% (0.4%) | 33.5% (36.1%) | 3.2% (3.5%) |
| IMDS | 3.0% (2.4%) | 24.9% (23.8%) | 2.2% (2.2%) |
| Score | 0.8% (0.3%) | 22.8% (23.2%) | 2.1% (2.1%) |
| MARS | 0.3% (0.2%) | 24.9% (23.9%) | 2.2% (2.2%) |
| Minimax | 2.5% (1.8%) | 21.6% (21.1%) | 1.8% (1.9%) |
| BTR-score | 0.2% (0.2%) | 24.7% (23.6%) | 2.3% (2.1%) |
| Ranked pairs | 2.2% (1.8%) | 21.8% (21.0%) | 1.9% (1.9%) |
| Schulze | 2.5% (1.8%) | 21.3% (21.1%) | 1.8% (1.8%) |
| Kemeny-Young | 1.9% (1.4%) | 21.9% (21.6%) | 1.9% (1.8%) |
| MMPO | 4.1% (3.6%) | 22.9% (21.4%) | 2.0% (1.9%) |
| MMWV | 2.3% (1.4%) | 23.1% (21.8%) | 1.9% (1.9%) |
| BTR-IRV | 3.8% (2.4%) | 21.2% (20.8%) | 1.9% (1.8%) |
| ESR-MARS | 0.2% (0.1%) | 23.3% (22.8%) | 2.1% (2.1%) |
| MJ | 0.1% (0.1%) | 21.8% (22.2%) | 2.1% (2.1%) |
| ESR | 1.1% (0.6%) | 23.3% (23.1%) | 2.2% (2.2%) |
| MAV | 0.3% (0.3%) | 21.3% (21.9%) | 2.1% (2.1%) |
| DMS | 0.1% (0.1%) | 22.0% (21.3%) | 2.1% (2.0%) |
| ER-IRV | 4.2% (3.4%) | 17.9% (18.4%) | 1.7% (1.8%) |
| IRV | 3.9% (3.4%) | 18.2% (18.4%) | 1.7% (1.8%) |
| Approval runoff | 1.4% (0.9%) | — (—) | — (—) |
| TTR | 3.5% (3.2%) | — (—) | — (—) |
| Approval cond. runoff | 1.1% (0.9%) | — (—) | — (—) |
| IRV-3 | 2.4% (1.6%) | 19.1% (20.4%) | 1.8% (2.0%) |
| Approval | 2.2% (1.6%) | — (—) | — (—) |
| Supplementary vote | 2.2% (2.1%) | 15.8% (16.0%) | 1.6% (1.6%) |
| Plurality | 2.9% (2.3%) | — (—) | — (—) |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

Scatter and chain plots leave out Condorcet-random-drop, Geometric median, Random ballot, Random pair runoff, Anti-plurality, TRASH, CW else Score loser, CL else Score winner, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
