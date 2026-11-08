# Ballot Front Finder Lab — analysis

Input: base, candidates/01-cands=4, candidates/02-cands=8, candidates/03-cands=10 (simulator version 2.1.0; analysis version 2.1.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

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

![sweep over candidates](chain-cands.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis); for conditions with fewer elections than the base, a further row gives the agreement expected between the base and a run of that size. A condition near or above its row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 0.97 | 0.99 | 0.95 | 0.99 | 0.99 |
| noise reference: c1 vs a run of 2000 elections | 0.95 | 0.99 | 0.92 | 0.99 | 0.99 |
| c2: candidates 4 | 0.96 | 0.96 | 0.90 | 0.98 | 0.97 |
| c3: candidates 8 | 0.96 | 0.98 | 0.92 | 0.98 | 0.98 |
| c4: candidates 10 | 0.95 | 0.97 | 0.92 | 0.96 | 0.96 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 97.5% | 69.9% | 14.9% | 8.7 |
| c2: candidates 4 | 98.8% | 76.3% | 15.2% | 5.6 |
| c3: candidates 8 | 96.4% | 64.6% | 14.0% | 11.5 |
| c4: candidates 10 | 95.3% | 62.4% | 13.3% | 13.9 |

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

## c2: candidates 4

2000 elections (seeds 1), 1975 with a Condorcet winner. 99 voters, 4 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 98.8%; it also has the highest total utility in 76.3% of those; electing it loses 15.2% of VSE; favourite ÷ least liked 5.6 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 84.3 to 84.5; with the best VSE (100) WME 80.0 (nothing lower is possible there either); the most balanced point VSE 95.3 / WME 96.0. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 89.2 (at WME 93.9); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 88.8 (at WME 94.5); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -69.1 / WME -67.2. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 84.2 / WME 99.9. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 86.8 / 97.0; normalized utility winner 89.2 / 93.9; best worst margin 84.4 / 100.0; Condorcet winner, else random ballot 84.0 / 99.6; Condorcet winner, else utility winner 84.9 / 99.8; random pair, majority 20.3 / 7.6; nearest the median voter 88.7 / 96.9; nearest the mean voter 76.2 / 92.1.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): DMS, Score, ESR-MARS, Soft Minimax-40, BTR-score, Soft Minimax-D, Soft Minimax, MMWV, Copeland.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Soft Minimax-40, Soft Minimax-D, Soft Minimax, MMWV, Copeland.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 84.0 / 99.6; its mirror on the utility side 88.8 / 93.5; random pair 20.3 / 7.6. No better than a lottery of them (Lead ≤ 0): with learned ballots DMS, Score, MAV, 321, Delegated 321, ESR, STAR, MJ, Approval, Approval runoff, ER-IRV, BTR-IRV, Approval cond. runoff, TTR, IRV, IRV-3, Supplementary vote, Asset, Borda, Plurality; with honest ballots Approval, Approval runoff, ER-IRV, Approval cond. runoff, TTR, IRV, IRV-3, Supplementary vote, Asset, Borda, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| DMS | rated | 86.6 [85.1–88.1] | 95.7 [94.9–96.6] | -0.2 [-1.0–0.5] | 1.5 [0.4–2.6] | 69 [60–82] | 100.0 [100.0–100.0] | 95.1 | 96.2 | 87.0 [85.3–88.5] | 96.9 [96.3–97.6] | 0.5 [-0.1–1.1] | 0.9 [-0.1–1.8] | 79 [69–>99] | 92.8 [91.6–93.9] | 90.9 [89.6–92.1] | 98.6 [98.0–99.1] | 96.1 [95.1–97.0] | 98.3 [97.7–98.9] | 96.1 [95.2–97.0] | 4.8 [3.9–5.7] | 5.0 [4.0–5.9] | 0.0% | 0.0% | 0.0% | 5.3% | MAV (WME\*), ESR (WME\*), Score runoff (WME\*) +5 more |
| Score | rated | 86.1 [84.3–87.8] | 95.9 [95.1–96.7] | -0.4 [-1.1–0.2] | 1.8 [0.7–2.8] | 65 [58–77] | 100.0 [100.0–100.0] | 95.0 | 96.2 | 86.3 [84.4–88.0] | 97.2 [96.6–97.7] | 0.2 [-0.4–0.8] | 1.1 [0.1–2.1] | 74 [65–90] | 92.8 [91.6–93.8] | 91.2 [89.9–92.3] | 98.4 [97.8–99.0] | 95.7 [94.8–96.6] | 98.2 [97.6–98.8] | 95.9 [95.0–96.7] | 4.9 [4.0–5.8] | 5.2 [4.3–6.2] | 0.0% | 0.0% | 0.0% | 5.9% | Smith//Score (WME\*), MJ (WME\*), MAV (WME\*) +12 more |
| ESR-MARS | hybrid | 86.1 [84.2–87.9] | 97.4 [96.8–98.0] | 0.2 [-0.3–0.9] | 1.1 [0.1–2.0] | 74 [66–88] | 100.0 [100.0–100.0] | 95.3 | 96.2 | 86.3 [84.4–88.0] | 98.5 [98.1–98.9] | 0.8 [0.3–1.3] | 0.4 [-0.5–1.2] | 87 [79–>99] | 95.2 [94.1–96.0] | 93.6 [92.6–94.7] | 99.1 [98.7–99.5] | 97.5 [96.8–98.2] | 98.6 [98.0–99.1] | 97.1 [96.4–97.8] | 5.0 [4.0–5.9] | 5.2 [4.3–6.2] | 0.0% | 0.1% | 0.0% | 3.9% | Smith//Score (WME\*), BTR-score (WME\*), MARS (WME\*) +3 more |
| Soft Minimax-40 | hybrid | 86.0 [84.3–87.7] | 98.6 [98.2–99.0] | 0.7 [0.2–1.2] | 0.5 [-0.4–1.2] | 86 [79–>99] | 100.0 [100.0–100.0] | 95.7 | 96.2 | 86.1 [84.4–87.8] | 98.9 [98.5–99.2] | 0.9 [0.4–1.4] | 0.3 [-0.5–1.0] | 91 [84–>99] | 96.5 [95.7–97.3] | 96.2 [95.4–97.0] | 99.4 [99.1–99.7] | 99.2 [98.8–99.6] | 98.7 [98.2–99.1] | 98.5 [98.0–99.0] | 5.2 [4.3–6.2] | 5.3 [4.3–6.3] | 0.0% | 0.1% | 0.0% | 1.8% | — |
| Score runoff | two-round | 85.9 [84.2–87.8] | 97.6 [97.0–98.1] | 0.2 [-0.3–0.7] | 1.0 [0.1–1.9] | 75 [68–85] | 99.9 [99.9–99.9] | 95.1 | 96.1 | 85.8 [83.9–87.6] | 97.8 [97.3–98.3] | 0.2 [-0.3–0.7] | 1.0 [0.0–1.8] | 76 [69–85] | 94.5 [93.4–95.4] | 94.3 [93.1–95.2] | 99.7 [99.4–99.9] | 98.9 [98.4–99.3] | 98.9 [98.4–99.3] | 98.2 [97.6–98.8] | 5.4 [4.5–6.3] | 5.5 [4.5–6.5] | 0.0% | 0.9% | 0.0% | 1.7% | Smith//Score (WME\*), IMDS (WME\*), BTR-score (WME\*) +3 more |
| MAV | median-based | 85.7 [84.0–87.5] | 97.3 [96.6–97.9] | -0.1 [-0.7–0.5] | 1.4 [0.4–2.2] | 70 [63–79] | 100.0 [100.0–100.0] | 95.6 | 96.3 | 85.7 [83.9–87.4] | 97.7 [97.1–98.2] | 0.1 [-0.5–0.6] | 1.1 [0.2–1.9] | 74 [67–82] | 94.6 [93.6–95.5] | 94.2 [93.2–95.1] | 98.3 [97.7–98.9] | 98.0 [97.3–98.6] | 97.6 [97.0–98.3] | 97.5 [96.7–98.1] | 6.3 [5.2–7.3] | 6.2 [5.1–7.2] | 0.0% | 0.0% | 0.0% | 3.2% | Smith//Score (WME\*), IMDS (WME\*), BTR-score (WME\*) +5 more |
| 321 | hybrid | 85.7 [83.9–87.4] | 97.3 [96.6–97.9] | -0.1 [-0.7–0.6] | 1.4 [0.4–2.2] | 70 [63–80] | 100.0 [99.9–100.0] | 90.1 | 90.8 | 86.3 [84.6–87.9] | 97.8 [97.2–98.3] | 0.5 [-0.0–1.0] | 0.8 [-0.1–1.6] | 79 [72–95] | 94.7 [93.7–95.6] | 94.1 [93.1–95.1] | 98.9 [98.4–99.3] | 98.2 [97.5–98.7] | 98.3 [97.7–98.9] | 97.5 [96.8–98.2] | 5.3 [4.3–6.2] | 5.6 [4.6–6.6] | 0.0% | 0.3% | 0.0% | 2.4% | Smith//Score (WME\*), IMDS (WME\*), BTR-score (WME\*) +6 more |
| Delegated 321 | delegated | 85.7 [83.9–87.4] | 97.3 [96.6–97.9] | -0.1 [-0.7–0.6] | 1.4 [0.4–2.2] | 70 [63–80] | — | — | — | 86.3 [84.6–87.9] | 97.8 [97.2–98.3] | 0.5 [-0.0–1.0] | 0.8 [-0.1–1.6] | 79 [72–95] | 94.7 [93.7–95.6] | 94.1 [93.1–95.1] | 98.9 [98.4–99.3] | 98.2 [97.5–98.7] | 98.3 [97.7–98.9] | 97.5 [96.8–98.2] | 5.3 [4.3–6.2] | 5.6 [4.6–6.6] | 0.0% | 0.3% | 0.0% | 2.4% | Smith//Score (WME\*), IMDS (WME\*), BTR-score (WME\*) +6 more |
| MARS-T | hybrid | 85.6 [83.7–87.4] | 98.2 [97.6–98.6] | 0.3 [-0.2–0.8] | 0.9 [0.0–1.7] | 78 [71–88] | 100.0 [100.0–100.0] | 95.4 | 96.1 | 86.0 [84.2–87.8] | 99.0 [98.7–99.3] | 0.8 [0.5–1.3] | 0.2 [-0.6–1.0] | 92 [85–>99] | 95.9 [95.1–96.7] | 95.0 [94.1–95.9] | 99.1 [98.7–99.5] | 98.3 [97.6–98.8] | 98.7 [98.1–99.2] | 97.8 [97.2–98.4] | 5.0 [4.0–5.9] | 5.4 [4.4–6.5] | 0.0% | 0.1% | 0.0% | 3.4% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax-D (WME\*) |
| ARA | hybrid | 85.6 [83.8–87.4] | 97.9 [97.3–98.5] | 0.2 [-0.3–0.7] | 1.0 [0.2–1.8] | 75 [69–85] | 100.0 [100.0–100.0] | 95.5 | 96.2 | 85.7 [84.0–87.4] | 98.5 [98.1–98.9] | 0.5 [0.0–1.0] | 0.6 [-0.2–1.4] | 82 [77–94] | 95.6 [94.6–96.4] | 95.0 [94.1–96.0] | 99.0 [98.6–99.5] | 98.5 [97.9–99.1] | 98.5 [98.0–99.0] | 98.1 [97.4–98.6] | 5.5 [4.5–6.5] | 5.5 [4.5–6.5] | 0.0% | 0.1% | 0.0% | 2.9% | Smith//Score (WME\*), IMDS (WME\*), BTR-score (WME\*) +3 more |
| MARS | hybrid | 85.5 [83.6–87.3] | 98.2 [97.8–98.6] | 0.3 [-0.2–0.7] | 0.9 [0.0–1.7] | 78 [72–87] | 100.0 [100.0–100.0] | 95.4 | 96.1 | 85.9 [84.1–87.7] | 98.9 [98.5–99.2] | 0.8 [0.4–1.2] | 0.3 [-0.5–1.1] | 89 [82–>99] | 95.8 [94.9–96.6] | 95.1 [94.1–96.0] | 99.1 [98.7–99.5] | 98.4 [97.9–99.0] | 98.6 [98.0–99.1] | 97.9 [97.2–98.5] | 5.1 [4.2–6.0] | 5.5 [4.5–6.5] | 0.0% | 0.1% | 0.0% | 3.4% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax-D (WME\*) |
| ESR | rated | 85.5 [83.6–87.3] | 97.6 [97.0–98.2] | -0.0 [-0.5–0.5] | 1.3 [0.3–2.1] | 72 [65–81] | 100.0 [100.0–100.0] | 95.0 | 96.2 | 85.5 [83.6–87.3] | 98.7 [98.2–99.1] | 0.5 [0.1–0.9] | 0.6 [-0.2–1.4] | 83 [77–96] | 96.5 [95.7–97.2] | 94.7 [93.7–95.6] | 99.3 [98.9–99.6] | 97.8 [97.1–98.4] | 98.6 [98.1–99.1] | 97.2 [96.5–97.9] | 5.4 [4.4–6.4] | 5.7 [4.7–6.7] | 0.0% | 0.0% | 0.0% | 5.8% | Smith//Score (WME\*), IMDS (WME\*), BTR-score (WME\*) +3 more |
| STAR | hybrid | 85.3 [83.6–87.1] | 97.4 [96.7–98.0] | -0.2 [-0.9–0.5] | 1.4 [0.4–2.3] | 69 [62–79] | 100.0 [99.9–100.0] | 95.6 | 96.1 | 86.1 [84.4–87.7] | 98.0 [97.5–98.4] | 0.5 [-0.1–1.0] | 0.8 [-0.2–1.6] | 79 [73–93] | 94.4 [93.3–95.4] | 93.8 [92.7–94.7] | 99.3 [98.9–99.7] | 98.4 [97.8–98.9] | 98.7 [98.2–99.2] | 97.9 [97.2–98.5] | 5.1 [4.2–6.2] | 5.8 [4.8–6.8] | 0.0% | 0.4% | 0.0% | 1.6% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +13 more |
| MJ | median-based | 85.3 [83.5–87.1] | 97.4 [96.6–98.0] | -0.3 [-0.9–0.3] | 1.5 [0.5–2.4] | 68 [62–77] | 100.0 [100.0–100.0] | 95.6 | 96.3 | 85.6 [83.9–87.4] | 97.9 [97.4–98.4] | 0.1 [-0.3–0.7] | 1.0 [0.1–1.8] | 75 [69–83] | 95.0 [94.0–95.9] | 94.5 [93.5–95.5] | 98.6 [98.1–99.1] | 98.2 [97.5–98.8] | 98.0 [97.4–98.6] | 97.6 [96.9–98.2] | 6.2 [5.1–7.2] | 6.2 [5.2–7.3] | 0.0% | 0.0% | 0.0% | 3.3% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +13 more |
| BTR-score | hybrid | 85.2 [83.4–87.1] | 99.1 [98.8–99.4] | 0.5 [0.1–0.9] | 0.5 [-0.3–1.0] | 87 [82–95] | 100.0 [100.0–100.0] | 95.8 | 96.1 | 85.4 [83.5–87.3] | 99.3 [99.0–99.5] | 0.6 [0.3–1.1] | 0.3 [-0.5–0.8] | 92 [87–>99] | 97.2 [96.5–97.9] | 96.7 [95.9–97.5] | 99.2 [98.8–99.6] | 99.1 [98.6–99.5] | 98.7 [98.1–99.2] | 98.5 [97.9–99.0] | 5.5 [4.5–6.5] | 5.5 [4.5–6.5] | 0.0% | 0.2% | 0.0% | 1.2% | Soft Minimax (WME\*) |
| Soft Minimax-D | hybrid | 85.2 [83.3–87.1] | 99.2 [98.9–99.4] | 0.5 [0.1–0.9] | 0.4 [-0.3–1.0] | 89 [83–96] | 100.0 [100.0–100.0] | 95.8 | 96.1 | 85.3 [83.5–87.1] | 99.3 [99.1–99.5] | 0.6 [0.2–1.0] | 0.3 [-0.4–0.8] | 92 [87–>99] | 97.1 [96.4–97.8] | 96.9 [96.1–97.6] | 99.2 [98.8–99.6] | 99.2 [98.8–99.6] | 98.6 [98.1–99.1] | 98.5 [98.0–99.0] | 5.4 [4.5–6.3] | 5.5 [4.5–6.5] | 0.0% | 0.1% | 0.0% | 0.9% | — |
| Smith//Score | condorcet-based | 85.2 [83.3–87.1] | 99.1 [98.8–99.4] | 0.5 [0.1–0.9] | 0.5 [-0.3–1.0] | 88 [82–95] | 100.0 [100.0–100.0] | 95.8 | 96.1 | 85.4 [83.5–87.3] | 99.3 [99.0–99.6] | 0.7 [0.3–1.1] | 0.3 [-0.5–0.8] | 92 [87–>99] | 97.2 [96.5–97.9] | 96.7 [95.9–97.4] | 99.2 [98.8–99.6] | 99.0 [98.6–99.4] | 98.7 [98.2–99.2] | 98.5 [97.9–99.0] | 5.4 [4.4–6.3] | 5.5 [4.6–6.6] | 0.0% | 0.1% | 0.0% | 1.3% | Soft Minimax (WME\*) |
| Approval | approval-based | 85.0 [83.1–86.9] | 95.2 [94.3–96.1] | -1.4 [-2.2–-0.5] | 2.7 [1.4–3.9] | 56 [49–64] | 100.0 [100.0–100.0] | 80.8 | 80.9 | 85.0 [83.0–86.8] | 95.1 [94.2–96.1] | -1.4 [-2.2–-0.5] | 2.7 [1.5–3.9] | 55 [49–64] | 91.2 [90.0–92.5] | 91.2 [89.9–92.4] | 95.9 [95.0–96.8] | 96.0 [95.1–96.8] | 95.4 [94.5–96.3] | 95.5 [94.6–96.5] | 6.3 [5.3–7.4] | 6.3 [5.3–7.4] | 0.0% | 0.0% | 0.0% | 1.3% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +26 more |
| IMDS | rated | 84.9 [83.0–86.8] | 98.7 [98.3–99.1] | 0.1 [-0.3–0.6] | 0.8 [0.1–1.5] | 80 [75–86] | 100.0 [100.0–100.0] | 94.9 | 96.1 | 85.0 [83.1–86.8] | 99.2 [98.9–99.4] | 0.4 [0.0–0.7] | 0.5 [-0.2–1.0] | 87 [82–93] | 97.2 [96.5–97.8] | 96.5 [95.6–97.2] | 99.2 [98.8–99.5] | 98.8 [98.3–99.3] | 98.3 [97.7–98.8] | 97.9 [97.3–98.5] | 5.6 [4.7–6.6] | 5.7 [4.7–6.7] | 0.0% | 0.0% | 0.0% | 5.9% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +9 more |
| Soft Minimax | hybrid | 84.9 [83.0–86.8] | 99.3 [99.1–99.6] | 0.4 [0.0–0.8] | 0.4 [-0.2–0.8] | 90 [85–95] | 100.0 [100.0–100.0] | 95.8 | 96.1 | 85.0 [83.1–86.9] | 99.4 [99.2–99.6] | 0.5 [0.2–0.9] | 0.3 [-0.3–0.7] | 92 [88–97] | 97.4 [96.7–98.1] | 97.3 [96.6–97.9] | 99.1 [98.7–99.5] | 99.1 [98.7–99.5] | 98.7 [98.2–99.1] | 98.5 [98.0–99.0] | 5.5 [4.5–6.4] | 5.5 [4.5–6.5] | 0.0% | 0.1% | 0.0% | 0.9% | — |
| Approval runoff | two-round | 84.8 [82.9–86.7] | 96.7 [95.9–97.5] | -0.8 [-1.5–-0.2] | 2.1 [1.0–3.1] | 62 [56–70] | 99.9 [99.8–99.9] | 79.0 | 79.5 | 85.1 [83.2–86.9] | 96.7 [95.9–97.5] | -0.6 [-1.3–0.0] | 1.9 [0.8–2.9] | 63 [58–73] | 94.3 [93.3–95.3] | 94.3 [93.3–95.3] | 97.8 [97.0–98.4] | 97.6 [96.9–98.3] | 96.8 [96.0–97.6] | 96.7 [95.9–97.5] | 6.1 [5.1–7.2] | 6.2 [5.2–7.3] | 0.0% | 0.8% | 0.0% | 0.8% | Soft Minimax-40 (VSE\* WME\*), Ranked pairs (WME\*), Schulze (WME\*) +18 more |
| MMWV | minimax-family | 84.5 [82.6–86.3] | 99.5 [99.3–99.7] | 0.2 [-0.1–0.6] | 0.4 [-0.2–0.7] | 91 [86–95] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 84.5 [82.6–86.3] | 99.8 [99.7–99.9] | 0.4 [0.1–0.7] | 0.1 [-0.3–0.2] | 96 [95–98] | 99.1 [98.7–99.5] | 98.5 [98.0–99.1] | 99.5 [99.1–99.8] | 99.3 [99.0–99.7] | 99.2 [98.8–99.5] | 98.9 [98.5–99.3] | 5.8 [4.8–6.8] | 5.9 [4.9–6.9] | 0.0% | 0.8% | 0.0% | 0.1% | — |
| MMPO | minimax-family | 84.4 [82.6–86.3] | 99.4 [99.0–99.6] | 0.2 [-0.2–0.5] | 0.5 [-0.1–0.9] | 88 [83–93] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 84.5 [82.6–86.3] | 99.8 [99.7–99.9] | 0.4 [0.1–0.7] | 0.1 [-0.3–0.2] | 96 [95–98] | 99.1 [98.7–99.5] | 98.5 [98.0–99.0] | 99.5 [99.1–99.8] | 99.3 [98.8–99.6] | 99.2 [98.8–99.5] | 98.8 [98.4–99.3] | 5.8 [4.8–6.8] | 5.9 [4.9–6.9] | 0.0% | 0.8% | 0.0% | 0.1% | Smith//Score (VSE\*), BTR-score (VSE\*), Soft Minimax-D (VSE\*) |
| ER-IRV | plurality-based | 84.4 [82.5–86.3] | 97.3 [96.5–97.9] | -0.8 [-1.4–-0.2] | 1.9 [0.9–2.9] | 64 [58–72] | 99.8 [99.7–99.8] | 82.4 | 82.8 | 85.0 [83.1–86.8] | 98.3 [97.8–98.8] | -0.0 [-0.4–0.4] | 1.1 [0.3–1.8] | 76 [71–84] | 97.2 [96.4–97.9] | 96.3 [95.4–97.1] | 98.7 [98.2–99.2] | 97.9 [97.2–98.6] | 97.6 [96.9–98.2] | 96.8 [95.9–97.5] | 5.9 [4.9–7.0] | 6.2 [5.2–7.4] | 0.4% | 0.5% | 0.2% | 0.4% | MARS (VSE\* WME\*), MARS-T (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +18 more |
| Kemeny-Young | condorcet | 84.4 [82.6–86.2] | 99.4 [99.1–99.7] | 0.2 [-0.2–0.5] | 0.5 [-0.1–0.8] | 89 [84–94] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 84.5 [82.7–86.3] | 99.8 [99.7–99.9] | 0.4 [0.2–0.7] | 0.1 [-0.3–0.2] | 96 [95–98] | 99.1 [98.7–99.5] | 98.5 [98.0–99.0] | 99.5 [99.1–99.8] | 99.2 [98.8–99.5] | 99.1 [98.7–99.5] | 98.7 [98.2–99.2] | 5.7 [4.7–6.7] | 5.9 [4.9–7.0] | 0.2% | 0.6% | 0.0% | 0.0% | Smith//Score (VSE\*), Soft Minimax-D (VSE\*) |
| Minimax | minimax-family | 84.4 [82.5–86.2] | 99.4 [99.2–99.7] | 0.2 [-0.2–0.5] | 0.5 [-0.1–0.8] | 89 [84–94] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 84.5 [82.6–86.3] | 99.8 [99.7–99.9] | 0.4 [0.1–0.7] | 0.1 [-0.3–0.2] | 96 [95–98] | 99.1 [98.7–99.5] | 98.5 [98.1–99.0] | 99.5 [99.1–99.8] | 99.3 [98.9–99.7] | 99.2 [98.8–99.5] | 98.9 [98.4–99.3] | 5.8 [4.8–6.8] | 5.9 [5.0–7.0] | 0.2% | 0.7% | 0.0% | 0.1% | Smith//Score (VSE\*), BTR-score (VSE\*), Soft Minimax-D (VSE\*) |
| Ranked pairs | condorcet | 84.4 [82.5–86.2] | 99.4 [99.1–99.7] | 0.1 [-0.2–0.5] | 0.5 [-0.1–0.8] | 89 [84–94] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 84.4 [82.6–86.3] | 99.8 [99.7–99.9] | 0.4 [0.1–0.6] | 0.2 [-0.3–0.3] | 96 [94–98] | 99.1 [98.7–99.5] | 98.6 [98.1–99.1] | 99.5 [99.1–99.8] | 99.3 [98.8–99.6] | 99.1 [98.7–99.5] | 98.8 [98.3–99.2] | 5.8 [4.8–6.8] | 5.9 [5.0–7.0] | 0.2% | 0.6% | 0.0% | 0.0% | Smith//Score (VSE\*), Soft Minimax-D (VSE\*) |
| Schulze | condorcet | 84.3 [82.5–86.2] | 99.5 [99.2–99.7] | 0.1 [-0.2–0.5] | 0.5 [-0.1–0.8] | 90 [85–94] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 84.5 [82.6–86.3] | 99.8 [99.7–99.9] | 0.4 [0.1–0.7] | 0.1 [-0.3–0.2] | 96 [95–98] | 99.1 [98.7–99.5] | 98.6 [98.2–99.1] | 99.5 [99.1–99.8] | 99.3 [98.9–99.7] | 99.2 [98.8–99.5] | 98.9 [98.4–99.3] | 5.8 [4.8–6.8] | 6.0 [5.0–7.0] | 0.2% | 0.7% | 0.0% | 0.1% | MMPO (VSE\*), Soft Minimax-D (VSE\*) |
| BTR-IRV | condorcet-based | 84.3 [82.4–86.2] | 98.7 [98.2–99.1] | -0.2 [-0.7–0.3] | 1.0 [0.3–1.6] | 78 [72–85] | 99.8 [99.8–99.8] | 82.1 | 82.4 | 84.3 [82.4–86.1] | 99.6 [99.4–99.8] | 0.2 [-0.1–0.4] | 0.4 [-0.1–0.6] | 92 [88–96] | 99.2 [98.8–99.5] | 97.5 [96.8–98.1] | 99.4 [99.0–99.8] | 98.7 [98.2–99.2] | 98.8 [98.2–99.2] | 98.0 [97.4–98.6] | 6.0 [5.1–7.1] | 6.3 [5.3–7.3] | 0.4% | 0.9% | 0.0% | 0.1% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +10 more |
| Copeland | condorcet | 84.3 [82.5–86.1] | 99.6 [99.4–99.8] | 0.2 [-0.1–0.4] | 0.4 [-0.1–0.6] | 92 [89–95] | 99.9 [99.9–99.9] | 82.1 | 82.3 | 84.3 [82.4–86.1] | 99.7 [99.5–99.9] | 0.2 [0.0–0.5] | 0.3 [-0.2–0.4] | 94 [90–97] | 99.1 [98.7–99.5] | 98.9 [98.4–99.3] | 99.5 [99.1–99.8] | 99.4 [99.0–99.7] | 98.9 [98.5–99.3] | 98.8 [98.3–99.2] | 5.9 [4.9–7.0] | 5.9 [4.9–7.0] | 0.1% | 0.4% | 0.0% | 0.1% | — |
| Approval cond. runoff | two-round | 84.3 [82.2–86.1] | 96.4 [95.6–97.3] | -1.2 [-2.0–-0.5] | 2.4 [1.5–3.5] | 58 [53–65] | 99.9 [99.9–100.0] | 79.7 | 80.1 | 85.0 [83.0–86.8] | 96.7 [95.9–97.5] | -0.8 [-1.4–-0.1] | 2.0 [1.0–3.0] | 62 [57–71] | 94.1 [93.1–95.1] | 94.0 [93.0–95.0] | 97.6 [96.8–98.3] | 97.3 [96.5–98.0] | 96.6 [95.7–97.4] | 96.4 [95.5–97.2] | 6.1 [5.1–7.2] | 6.6 [5.5–7.7] | 0.0% | 0.4% | 0.0% | 1.1% | ESR (VSE\* WME\*), Score runoff (VSE\* WME\*), MARS (VSE\* WME\*) +23 more |
| TTR | two-round | 83.7 [81.7–85.5] | 97.4 [96.7–98.1] | -1.2 [-1.8–-0.5] | 2.1 [1.3–2.9] | 64 [58–71] | 99.1 [99.0–99.3] | 71.0 | 71.8 | 84.8 [82.9–86.7] | 98.2 [97.7–98.7] | -0.1 [-0.5–0.3] | 1.2 [0.4–1.9] | 74 [69–82] | 97.1 [96.3–97.8] | 96.3 [95.5–97.1] | 98.7 [98.1–99.2] | 97.9 [97.2–98.5] | 97.6 [97.0–98.3] | 96.8 [96.0–97.6] | 6.2 [5.1–7.2] | 6.8 [5.7–7.8] | 1.0% | 0.0% | 0.8% | 0.0% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), BTR-score (VSE\* WME\*) +23 more |
| IRV | plurality-based | 83.3 [81.3–85.2] | 96.2 [95.3–97.1] | -1.9 [-2.6–-1.1] | 3.0 [1.9–4.0] | 55 [50–62] | 99.3 [99.2–99.4] | 81.9 | 82.8 | 85.0 [83.1–86.8] | 98.3 [97.8–98.8] | -0.0 [-0.4–0.4] | 1.1 [0.3–1.8] | 76 [71–84] | 97.2 [96.4–97.9] | 95.1 [94.1–96.0] | 98.7 [98.2–99.2] | 96.8 [96.0–97.6] | 97.6 [96.9–98.2] | 95.7 [94.8–96.5] | 5.9 [4.9–7.0] | 6.7 [5.7–7.8] | 1.1% | 0.5% | 1.0% | 0.1% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +28 more |
| IRV-3 | plurality-based | 83.1 [81.0–85.0] | 95.7 [94.7–96.7] | -2.2 [-3.1–-1.4] | 3.4 [2.2–4.5] | 51 [46–59] | 99.3 [99.2–99.4] | 80.7 | 81.6 | 84.8 [82.8–86.7] | 97.8 [97.2–98.4] | -0.3 [-0.8–0.2] | 1.4 [0.5–2.2] | 71 [64–79] | 96.7 [95.9–97.5] | 94.9 [93.9–95.9] | 98.2 [97.6–98.8] | 96.4 [95.5–97.3] | 97.2 [96.4–97.9] | 95.3 [94.4–96.2] | 5.9 [4.8–6.9] | 6.6 [5.5–7.8] | 1.2% | 0.5% | 1.0% | 0.1% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +29 more |
| Supplementary vote | plurality-based | 82.9 [80.6–84.9] | 92.5 [91.1–93.9] | -3.7 [-5.0–-2.6] | 5.1 [3.5–6.7] | 39 [34–46] | 99.7 [99.6–99.7] | 81.7 | 82.2 | 84.4 [82.3–86.2] | 93.7 [92.4–95.0] | -2.4 [-3.4–-1.4] | 3.7 [2.2–5.1] | 47 [41–56] | 92.7 [91.5–93.9] | 91.8 [90.6–93.1] | 95.5 [94.5–96.5] | 94.6 [93.5–95.7] | 94.7 [93.7–95.7] | 93.7 [92.6–94.8] | 6.0 [5.1–7.1] | 6.8 [5.8–8.0] | 1.1% | 0.4% | 0.3% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +31 more |
| Asset | delegated | 82.3 [80.1–84.4] | 91.9 [90.7–93.0] | -4.4 [-5.5–-3.1] | 5.7 [4.2–7.1] | 37 [33–42] | 99.3 [99.2–99.4] | 72.9 | 73.5 | 82.5 [80.5–84.5] | 92.4 [91.1–93.4] | -4.0 [-5.1–-2.8] | 5.4 [3.9–6.7] | 38 [34–44] | 86.9 [85.4–88.3] | 86.3 [84.9–87.9] | 89.6 [88.2–91.0] | 89.0 [87.6–90.5] | 89.9 [88.5–91.2] | 89.2 [87.8–90.6] | 6.5 [5.4–7.5] | 6.6 [5.5–7.7] | 0.9% | 0.0% | 0.4% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +32 more |
| Borda | positional | 81.6 [79.4–83.5] | 93.5 [92.5–94.5] | -4.0 [-5.2–-3.0] | 5.3 [4.1–6.4] | 40 [36–45] | 99.1 [99.1–99.2] | 78.9 | 80.6 | 81.1 [79.1–83.3] | 94.1 [93.2–95.0] | -4.0 [-5.1–-2.8] | 5.1 [3.9–6.2] | 42 [39–46] | 87.9 [86.5–89.4] | 87.3 [85.8–88.7] | 90.5 [89.1–91.9] | 90.9 [89.6–92.2] | 90.9 [89.6–92.2] | 90.9 [89.6–92.1] | 6.3 [5.3–7.5] | 6.6 [5.5–7.6] | 2.2% | 5.4% | 0.0% | 3.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +31 more |
| Plurality | plurality-based | 76.4 [73.7–78.8] | 83.6 [81.8–85.5] | -11.3 [-13.1–-9.4] | 12.2 [10.2–14.5] | 20 [18–23] | 99.6 [99.6–99.7] | 68.8 | 69.2 | 75.0 [72.0–77.6] | 83.5 [81.5–85.3] | -12.1 [-14.1–-10.3] | 13.3 [11.1–15.7] | 19 [17–22] | 79.3 [77.5–81.1] | 79.2 [77.5–81.0] | 84.1 [82.4–85.7] | 84.5 [82.9–86.2] | 84.2 [82.5–85.7] | 84.5 [83.0–86.2] | 9.6 [8.2–10.8] | 9.0 [7.6–10.3] | 3.9% | 0.0% | 0.1% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +34 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Borda | 10.9% |
| Score | 6.9% |
| IMDS | 6.8% |
| Plurality | 6.8% |
| ESR | 6.7% |
| DMS | 6.3% |
| ESR-MARS | 4.7% |
| MARS | 4.0% |
| MARS-T | 4.0% |
| MJ | 3.9% |
| MAV | 3.9% |
| 321 | 3.4% |
| Delegated 321 | 3.4% |
| Approval | 3.4% |
| ARA | 3.4% |
| IRV | 3.0% |
| Approval cond. runoff | 3.0% |
| IRV-3 | 3.0% |
| Score runoff | 2.8% |
| TTR | 2.5% |
| Asset | 2.3% |
| STAR | 2.2% |
| Soft Minimax-40 | 2.2% |
| Approval runoff | 2.1% |
| Supplementary vote | 2.1% |
| Smith//Score | 1.7% |
| ER-IRV | 1.7% |
| BTR-score | 1.6% |
| BTR-IRV | 1.5% |
| Soft Minimax-D | 1.2% |
| Soft Minimax | 1.2% |
| Minimax | 1.0% |
| Schulze | 1.0% |
| MMPO | 1.0% |
| MMWV | 1.0% |
| Kemeny-Young | 1.0% |
| Ranked pairs | 1.0% |
| Copeland | 0.6% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Plurality | 24.3% | 0.46 | 0.74 | 95% |
| Approval | 14.2% | 0.32 | 0.16 | 100% |
| Supplementary vote | 12.7% | 0.42 | 0.31 | 99% |
| MAV | 11.2% | 0.20 | 0.03 | 100% |
| Approval cond. runoff | 10.8% | 0.24 | 0.20 | 100% |
| Approval runoff | 10.2% | 0.19 | 0.20 | 100% |
| TTR | 10.1% | 0.20 | 0.28 | 100% |
| MJ | 9.9% | 0.17 | 0.03 | 100% |
| Asset | 9.8% | 0.11 | 0.03 | 100% |
| 321 | 9.7% | 0.17 | 0.31 | 100% |
| Delegated 321 | 9.7% | 0.17 | 0.31 | 100% |
| Borda | 9.3% | 0.03 | 0.54 | 100% |
| IRV | 9.2% | 0.10 | 0.02 | 100% |
| ER-IRV | 9.2% | 0.10 | 0.02 | 100% |
| DMS | 9.0% | 0.14 | 0.03 | 100% |
| IRV-3 | 9.0% | 0.14 | 0.07 | 100% |
| ESR | 8.8% | 0.13 | 0.04 | 100% |
| Score | 8.2% | 0.12 | 0.04 | 100% |
| ARA | 8.1% | 0.13 | 0.05 | 100% |
| STAR | 7.8% | 0.13 | 0.12 | 100% |
| ESR-MARS | 7.8% | 0.12 | 0.03 | 100% |
| Soft Minimax-40 | 7.5% | 0.12 | 0.04 | 100% |
| IMDS | 7.1% | 0.10 | 0.05 | 100% |
| Score runoff | 6.9% | 0.09 | 0.15 | 100% |
| Soft Minimax | 6.8% | 0.10 | 0.04 | 100% |
| MARS | 6.5% | 0.10 | 0.03 | 100% |
| MARS-T | 6.4% | 0.10 | 0.03 | 100% |
| BTR-score | 6.1% | 0.09 | 0.02 | 100% |
| Smith//Score | 6.0% | 0.09 | 0.02 | 100% |
| Soft Minimax-D | 6.0% | 0.09 | 0.02 | 100% |
| Copeland | 2.2% | 0.01 | 0.04 | 100% |
| BTR-IRV | 1.6% | 0.01 | 0.02 | 100% |
| Ranked pairs | 1.5% | 0.01 | 0.03 | 100% |
| Kemeny-Young | 1.5% | 0.00 | 0.03 | 100% |
| Schulze | 1.5% | 0.00 | 0.02 | 100% |
| Minimax | 1.5% | 0.00 | 0.02 | 100% |
| MMWV | 1.5% | 0.00 | 0.02 | 100% |
| MMPO | 1.5% | 0.00 | 0.02 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 1975 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 25 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| DMS | 94.1% (94.4%) | 4.6% (4.3%) | 1.3% (1.4%) | 72.0% (68.0%) | 94.0% (95.7%) | 90.9% |
| Score | 94.3% (94.4%) | 4.2% (3.9%) | 1.5% (1.7%) | 76.0% (68.0%) | 94.0% (95.6%) | 91.2% |
| ESR-MARS | 94.9% (95.3%) | 3.6% (3.4%) | 1.5% (1.3%) | 76.0% (72.0%) | 96.8% (98.1%) | 93.6% |
| Soft Minimax-40 | 95.7% (95.7%) | 3.0% (3.0%) | 1.2% (1.3%) | 64.0% (60.0%) | 98.7% (99.1%) | 96.2% |
| MAV | 94.6% (94.7%) | 3.6% (3.6%) | 1.8% (1.6%) | 68.0% (64.0%) | 96.3% (96.8%) | 94.2% |
| 321 | 91.0% (89.6%) | 4.1% (3.5%) | 4.9% (6.8%) | 84.0% (76.0%) | 99.5% (99.6%) | 94.1% |
| Delegated 321 | 91.0% (89.6%) | 4.1% (3.5%) | 4.9% (6.8%) | 84.0% (76.0%) | 99.5% (99.6%) | 94.1% |
| MARS-T | 95.1% (95.3%) | 3.4% (3.2%) | 1.4% (1.5%) | 80.0% (76.0%) | 97.8% (98.5%) | 95.0% |
| ARA | 95.3% (95.3%) | 3.2% (3.1%) | 1.5% (1.6%) | 80.0% (80.0%) | 97.7% (97.7%) | 95.0% |
| MARS | 95.2% (95.4%) | 3.4% (3.2%) | 1.3% (1.4%) | 76.0% (76.0%) | 97.9% (98.6%) | 95.1% |
| ESR | 95.8% (96.3%) | 2.9% (2.6%) | 1.3% (1.1%) | 76.0% (72.0%) | 97.6% (99.1%) | 94.7% |
| STAR | 95.3% (96.2%) | 3.7% (2.7%) | 1.0% (1.2%) | 72.0% (64.0%) | 97.1% (96.3%) | 93.8% |
| MJ | 94.4% (94.6%) | 3.8% (3.7%) | 1.8% (1.7%) | 72.0% (64.0%) | 96.3% (96.7%) | 94.5% |
| BTR-score | 96.3% (96.6%) | 2.5% (2.2%) | 1.2% (1.2%) | 72.0% (64.0%) | 100.0% (100.0%) | 96.7% |
| Soft Minimax-D | 96.2% (96.4%) | 2.6% (2.6%) | 1.2% (1.0%) | 68.0% (60.0%) | 99.7% (99.9%) | 96.9% |
| Smith//Score | 96.2% (96.6%) | 2.6% (2.2%) | 1.3% (1.2%) | 64.0% (56.0%) | 100.0% (100.0%) | 96.7% |
| IMDS | 95.9% (96.7%) | 2.8% (2.4%) | 1.3% (1.0%) | 88.0% (88.0%) | 99.8% (99.8%) | 96.5% |
| Soft Minimax | 96.7% (96.8%) | 2.3% (2.3%) | 1.0% (0.9%) | 64.0% (52.0%) | 100.0% (100.0%) | 97.3% |
| MMWV | 98.2% (99.0%) | 1.0% (0.7%) | 0.8% (0.3%) | 40.0% (32.0%) | 100.0% (100.0%) | 98.5% |
| MMPO | 98.1% (99.0%) | 1.1% (0.7%) | 0.8% (0.3%) | 40.0% (32.0%) | 100.0% (100.0%) | 98.5% |
| ER-IRV | 98.2% (99.3%) | 1.5% (0.5%) | 0.3% (0.2%) | 80.0% (68.0%) | 97.2% (97.9%) | 96.3% |
| Kemeny-Young | 98.2% (99.0%) | 1.0% (0.7%) | 0.8% (0.3%) | 32.0% (20.0%) | 100.0% (100.0%) | 98.5% |
| Minimax | 98.3% (99.0%) | 1.0% (0.7%) | 0.7% (0.3%) | 44.0% (32.0%) | 100.0% (100.0%) | 98.5% |
| Ranked pairs | 98.3% (99.0%) | 1.0% (0.7%) | 0.7% (0.3%) | 44.0% (32.0%) | 100.0% (100.0%) | 98.6% |
| Schulze | 98.4% (99.0%) | 1.0% (0.7%) | 0.6% (0.3%) | 44.0% (32.0%) | 100.0% (100.0%) | 98.6% |
| BTR-IRV | 97.4% (99.1%) | 1.7% (0.7%) | 1.0% (0.2%) | 60.0% (56.0%) | 100.0% (100.0%) | 97.5% |
| Copeland | 98.6% (98.9%) | 0.9% (0.7%) | 0.5% (0.4%) | 56.0% (52.0%) | 100.0% (100.0%) | 98.9% |
| IRV | 97.0% (99.3%) | 2.7% (0.5%) | 0.3% (0.2%) | 76.0% (68.0%) | 97.0% (97.9%) | 95.1% |
| IRV-3 | 96.4% (98.5%) | 3.1% (1.1%) | 0.6% (0.4%) | 64.0% (60.0%) | 97.0% (97.9%) | 94.9% |
| Supplementary vote | 93.0% (94.5%) | 6.5% (5.3%) | 0.5% (0.2%) | 60.0% (60.0%) | 97.5% (97.9%) | 91.8% |
| Borda | 95.4% (99.1%) | 2.0% (0.8%) | 2.6% (0.1%) | 36.0% (16.0%) | 89.4% (88.2%) | 87.3% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| DMS | 0.0% (0.0%) | 4.0% (3.9%) | 0.7% (0.7%) |
| Score | 0.2% (0.3%) | 4.6% (4.3%) | 0.8% (0.8%) |
| ESR-MARS | 0.1% (0.0%) | 4.2% (3.9%) | 0.8% (0.7%) |
| Soft Minimax-40 | 0.4% (0.4%) | 4.0% (3.9%) | 0.7% (0.7%) |
| Score runoff | 0.1% (0.1%) | — (—) | — (—) |
| MAV | 0.0% (0.1%) | 4.0% (4.1%) | 0.8% (0.8%) |
| 321 | 0.1% (0.1%) | 11.5% (13.4%) | 1.6% (1.8%) |
| Delegated 321 | 0.1% (0.1%) | 11.5% (13.4%) | 1.6% (1.8%) |
| MARS-T | 0.0% (0.1%) | 4.5% (4.0%) | 0.8% (0.7%) |
| ARA | 0.4% (0.4%) | 4.5% (4.2%) | 0.8% (0.8%) |
| MARS | 0.0% (0.1%) | 4.4% (4.0%) | 0.8% (0.7%) |
| ESR | 0.5% (0.3%) | 4.2% (4.0%) | 0.7% (0.7%) |
| STAR | 0.1% (0.1%) | 5.3% (5.3%) | 0.9% (0.9%) |
| MJ | 0.0% (0.0%) | 4.5% (4.3%) | 0.9% (0.8%) |
| BTR-score | 0.1% (0.1%) | 3.5% (4.0%) | 0.7% (0.8%) |
| Soft Minimax-D | 0.1% (0.1%) | 3.8% (3.5%) | 0.7% (0.6%) |
| Smith//Score | 0.0% (0.1%) | 3.7% (4.0%) | 0.7% (0.8%) |
| Approval | 0.6% (0.6%) | — (—) | — (—) |
| IMDS | 1.1% (0.8%) | 4.2% (4.0%) | 0.7% (0.7%) |
| Soft Minimax | 0.7% (0.7%) | 4.2% (4.0%) | 0.7% (0.7%) |
| Approval runoff | 0.5% (0.5%) | — (—) | — (—) |
| MMWV | 0.4% (0.4%) | 0.1% (0.0%) | 0.0% (0.0%) |
| MMPO | 0.5% (0.4%) | 0.1% (0.0%) | 0.0% (0.0%) |
| ER-IRV | 0.4% (0.1%) | 0.1% (0.0%) | 0.0% (0.0%) |
| Kemeny-Young | 0.4% (0.4%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Minimax | 0.4% (0.4%) | 0.1% (0.0%) | 0.0% (0.0%) |
| Ranked pairs | 0.4% (0.4%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Schulze | 0.4% (0.4%) | 0.1% (0.0%) | 0.0% (0.0%) |
| BTR-IRV | 0.0% (0.0%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Copeland | 0.6% (0.4%) | 0.1% (0.0%) | 0.0% (0.0%) |
| Approval cond. runoff | 0.8% (0.4%) | — (—) | — (—) |
| TTR | 0.4% (0.2%) | — (—) | — (—) |
| IRV | 0.2% (0.1%) | 0.1% (0.0%) | 0.0% (0.0%) |
| IRV-3 | 0.4% (0.4%) | 0.4% (0.3%) | 0.0% (0.0%) |
| Supplementary vote | 0.7% (0.4%) | 0.1% (0.0%) | 0.0% (0.0%) |
| Asset | 0.2% (0.4%) | — (—) | — (—) |
| Borda | 0.5% (0.4%) | 1.2% (0.0%) | 0.1% (0.0%) |
| Plurality | 1.3% (1.1%) | — (—) | — (—) |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

## c3: candidates 8

2000 elections (seeds 1), 1927 with a Condorcet winner. 99 voters, 8 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 96.4%; it also has the highest total utility in 64.6% of those; electing it loses 14.0% of VSE; favourite ÷ least liked 11.5 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 84.4 to 85.3; with the best VSE (100) WME 77.6 (nothing lower is possible there either); the most balanced point VSE 95.1 / WME 96.2. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 92.5 (at WME 90.6); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 92.3 (at WME 90.1); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -44.3 / WME -37.0. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 84.7 / WME 99.8. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 88.7 / 95.3; normalized utility winner 92.5 / 90.6; best worst margin 84.8 / 100.0; Condorcet winner, else random ballot 83.1 / 98.0; Condorcet winner, else utility winner 86.5 / 99.5; random pair, majority 15.8 / 3.8; nearest the median voter 88.3 / 96.9; nearest the mean voter 75.0 / 92.3.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): ESR-MARS, Soft Minimax-40, ARA, Soft Minimax-D, Smith//Score, BTR-score, Soft Minimax, Copeland, Kemeny-Young.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Soft Minimax-40, Soft Minimax-D, Soft Minimax, Kemeny-Young, Schulze, Ranked pairs.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 83.1 / 98.0; its mirror on the utility side 90.7 / 88.7; random pair 15.8 / 3.8. No better than a lottery of them (Lead ≤ 0): with learned ballots DMS, Approval cond. runoff, MJ, Score, MAV, Approval, ER-IRV, TTR, Supplementary vote, IRV, IRV-3, Asset, Borda, Plurality; with honest ballots Approval cond. runoff, MAV, Approval, ER-IRV, TTR, Supplementary vote, IRV, IRV-3, Asset, Borda, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ESR-MARS | hybrid | 89.6 [88.4–90.7] | 93.4 [92.6–94.3] | 1.6 [0.9–2.2] | 1.8 [1.0–2.8] | 64 [59–71] | 100.0 [100.0–100.0] | 91.5 | 93.1 | 90.0 [88.8–91.1] | 95.0 [94.3–95.7] | 2.5 [1.8–3.1] | 1.2 [0.3–2.0] | 75 [69–83] | 86.5 [85.0–88.1] | 84.0 [82.4–85.6] | 98.3 [97.6–98.9] | 95.5 [94.4–96.5] | 97.5 [96.8–98.2] | 95.2 [94.2–96.2] | 2.2 [1.6–2.9] | 2.4 [1.7–3.1] | 0.0% | 0.3% | 0.0% | 6.4% | STAR (WME\*), Score runoff (WME\*), Soft Minimax-40 (WME\*) |
| Soft Minimax-40 | hybrid | 89.6 [88.4–90.6] | 95.0 [94.3–95.7] | 2.2 [1.6–2.9] | 1.5 [0.7–2.2] | 71 [65–78] | 100.0 [100.0–100.0] | 92.1 | 93.1 | 89.6 [88.4–90.7] | 95.2 [94.5–95.9] | 2.3 [1.7–3.0] | 1.4 [0.6–2.1] | 72 [67–79] | 87.0 [85.5–88.5] | 86.5 [85.0–87.9] | 98.6 [98.0–99.1] | 98.0 [97.3–98.6] | 97.7 [97.1–98.3] | 97.2 [96.5–97.9] | 2.0 [1.4–2.6] | 1.9 [1.4–2.6] | 0.0% | 0.2% | 0.0% | 3.3% | — |
| ESR | rated | 89.4 [88.3–90.6] | 92.6 [91.7–93.5] | 1.1 [0.4–1.9] | 2.2 [1.3–3.1] | 60 [55–67] | 100.0 [100.0–100.0] | 90.9 | 93.1 | 90.0 [88.8–91.0] | 94.9 [94.2–95.6] | 2.4 [1.8–3.0] | 1.2 [0.4–2.0] | 74 [68–81] | 86.3 [84.8–87.8] | 82.9 [81.3–84.7] | 98.1 [97.2–98.8] | 94.4 [93.2–95.5] | 97.4 [96.6–98.0] | 94.0 [92.9–95.0] | 2.1 [1.5–2.7] | 2.4 [1.7–3.1] | 0.0% | 0.2% | 0.0% | 9.4% | STAR (WME\*), Score runoff (WME\*), MARS-T (WME\*) +2 more |
| STAR | hybrid | 89.4 [88.1–90.6] | 94.4 [93.6–95.1] | 1.9 [1.2–2.5] | 1.8 [0.9–2.7] | 67 [61–73] | 100.0 [99.9–100.0] | 91.6 | 93.0 | 89.7 [88.4–90.8] | 95.2 [94.5–95.8] | 2.4 [1.8–3.0] | 1.3 [0.5–2.1] | 73 [67–80] | 86.4 [84.9–87.9] | 85.1 [83.6–86.7] | 98.5 [97.9–99.1] | 96.6 [95.7–97.4] | 97.8 [97.2–98.4] | 96.0 [95.1–96.9] | 2.0 [1.4–2.6] | 1.9 [1.3–2.5] | 0.0% | 0.7% | 0.0% | 4.2% | ARA (WME\*) |
| DMS | rated | 89.1 [87.8–90.2] | 89.7 [88.5–90.8] | -0.4 [-1.1–0.4] | 3.2 [2.1–4.2] | 52 [46–58] | 100.0 [100.0–100.0] | 91.1 | 93.1 | 90.2 [89.1–91.3] | 91.3 [90.3–92.3] | 0.9 [0.3–1.6] | 1.9 [1.0–2.9] | 63 [58–71] | 80.8 [79.1–82.4] | 78.9 [77.2–80.7] | 95.8 [94.7–96.8] | 91.6 [90.1–93.0] | 95.2 [94.2–96.2] | 91.8 [90.5–93.0] | 2.4 [1.7–3.0] | 2.5 [1.9–3.2] | 0.0% | 0.2% | 0.0% | 8.1% | Approval runoff (WME\*), ESR (WME\*), STAR (WME\*) +9 more |
| Score runoff | two-round | 89.1 [87.8–90.2] | 94.5 [93.7–95.2] | 1.7 [1.0–2.5] | 2.0 [1.1–2.8] | 64 [60–71] | 99.9 [99.9–99.9] | 91.0 | 93.0 | 89.7 [88.5–90.8] | 95.1 [94.4–95.8] | 2.4 [1.7–3.0] | 1.3 [0.5–2.1] | 73 [68–79] | 86.8 [85.4–88.3] | 86.1 [84.7–87.6] | 99.1 [98.6–99.5] | 96.5 [95.4–97.4] | 98.3 [97.7–98.8] | 96.2 [95.4–97.0] | 2.1 [1.5–2.8] | 2.1 [1.4–2.7] | 0.0% | 1.5% | 0.0% | 4.6% | MARS (WME\*), Soft Minimax-D (WME\*) |
| 321 | hybrid | 89.0 [87.8–90.2] | 91.0 [89.8–92.0] | 0.1 [-0.6–0.9] | 2.9 [2.0–4.0] | 54 [48–60] | 100.0 [100.0–100.0] | 82.9 | 84.1 | 89.8 [88.7–90.9] | 92.6 [91.6–93.4] | 1.2 [0.6–2.0] | 2.0 [1.1–2.8] | 63 [57–70] | 82.8 [81.3–84.5] | 80.7 [79.0–82.4] | 95.3 [94.3–96.3] | 93.1 [91.7–94.3] | 94.6 [93.5–95.6] | 92.6 [91.3–93.7] | 1.8 [1.3–2.5] | 2.1 [1.6–2.9] | 0.0% | 0.4% | 0.0% | 5.1% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +6 more |
| Delegated 321 | delegated | 89.0 [87.8–90.2] | 91.0 [89.8–92.0] | 0.1 [-0.6–0.9] | 2.9 [2.0–4.0] | 54 [48–60] | — | — | — | 89.8 [88.7–90.9] | 92.6 [91.6–93.4] | 1.2 [0.6–2.0] | 2.0 [1.1–2.8] | 63 [57–70] | 82.8 [81.3–84.5] | 80.7 [79.0–82.4] | 95.3 [94.3–96.3] | 93.1 [91.7–94.3] | 94.6 [93.5–95.6] | 92.6 [91.3–93.7] | 1.8 [1.3–2.5] | 2.1 [1.6–2.9] | 0.0% | 0.4% | 0.0% | 5.1% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +6 more |
| ARA | hybrid | 88.8 [87.6–90.0] | 95.2 [94.6–95.9] | 1.9 [1.3–2.6] | 1.8 [1.0–2.6] | 67 [62–72] | 100.0 [100.0–100.0] | 91.9 | 93.0 | 89.1 [87.9–90.2] | 96.1 [95.6–96.8] | 2.5 [1.9–3.1] | 1.3 [0.6–1.9] | 74 [69–80] | 88.7 [87.5–90.2] | 86.9 [85.5–88.5] | 98.2 [97.6–98.9] | 96.9 [96.0–97.7] | 97.1 [96.4–97.8] | 95.7 [94.8–96.5] | 1.9 [1.4–2.6] | 2.1 [1.4–2.7] | 0.0% | 0.1% | 0.0% | 4.3% | Soft Minimax-40 (VSE\*), Soft Minimax-D (WME\*) |
| MARS-T | hybrid | 88.7 [87.5–89.9] | 95.0 [94.3–95.7] | 1.8 [1.1–2.5] | 2.0 [1.2–2.8] | 65 [61–71] | 100.0 [100.0–100.0] | 91.7 | 93.0 | 89.1 [87.9–90.3] | 96.3 [95.8–96.9] | 2.6 [2.0–3.2] | 1.2 [0.5–1.9] | 76 [71–82] | 88.8 [87.6–90.3] | 86.5 [85.0–88.0] | 98.7 [98.2–99.3] | 96.4 [95.4–97.3] | 98.0 [97.4–98.6] | 96.0 [95.1–96.9] | 2.0 [1.4–2.7] | 2.2 [1.6–2.9] | 0.0% | 0.2% | 0.0% | 5.1% | STAR (VSE\*), MARS (WME\*), Soft Minimax-40 (VSE\*) +1 more |
| Soft Minimax-D | hybrid | 88.6 [87.3–89.8] | 96.5 [95.9–97.0] | 2.4 [1.8–3.0] | 1.4 [0.7–2.1] | 73 [68–79] | 100.0 [100.0–100.0] | 92.4 | 93.0 | 88.8 [87.5–90.0] | 96.7 [96.2–97.3] | 2.6 [2.0–3.2] | 1.2 [0.5–1.8] | 76 [71–82] | 89.8 [88.4–91.1] | 89.3 [88.0–90.7] | 98.7 [98.1–99.2] | 98.3 [97.7–98.9] | 97.5 [96.8–98.1] | 97.0 [96.4–97.8] | 1.9 [1.3–2.6] | 2.1 [1.5–2.8] | 0.0% | 0.2% | 0.0% | 1.8% | — |
| MARS | hybrid | 88.6 [87.2–89.7] | 95.4 [94.8–96.1] | 1.9 [1.2–2.5] | 1.9 [1.2–2.7] | 66 [62–72] | 100.0 [100.0–100.0] | 91.7 | 93.0 | 88.9 [87.6–90.0] | 96.6 [96.1–97.1] | 2.6 [2.0–3.2] | 1.2 [0.5–1.9] | 76 [71–82] | 89.3 [88.1–90.8] | 87.3 [85.9–88.8] | 98.8 [98.2–99.3] | 96.9 [96.0–97.8] | 98.0 [97.4–98.6] | 96.4 [95.6–97.2] | 2.1 [1.4–2.8] | 2.1 [1.6–2.8] | 0.0% | 0.2% | 0.0% | 5.1% | Soft Minimax-40 (VSE\*), Soft Minimax-D (WME\*) |
| Approval runoff | two-round | 88.6 [87.3–89.7] | 91.4 [90.4–92.4] | 0.0 [-0.7–0.8] | 3.2 [2.2–4.1] | 52 [47–57] | 99.9 [99.9–99.9] | 69.3 | 69.8 | 88.9 [87.6–90.0] | 91.1 [90.0–92.1] | 0.1 [-0.7–0.9] | 3.0 [2.1–4.0] | 53 [48–59] | 81.7 [80.0–83.4] | 82.3 [80.7–84.1] | 93.9 [92.6–95.1] | 93.8 [92.5–94.9] | 93.3 [92.3–94.4] | 93.5 [92.3–94.5] | 2.7 [2.0–3.5] | 2.7 [2.0–3.5] | 0.0% | 1.1% | 0.0% | 2.2% | ESR (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*), ESR-MARS (VSE\* WME\*) +10 more |
| Approval cond. runoff | two-round | 88.5 [87.2–89.7] | 90.8 [89.8–91.8] | -0.2 [-1.1–0.6] | 3.3 [2.4–4.3] | 50 [45–56] | 99.9 [99.9–99.9] | 69.5 | 70.0 | 89.0 [87.7–90.1] | 90.7 [89.7–91.7] | -0.0 [-0.8–0.7] | 3.0 [2.1–4.0] | 53 [48–58] | 81.3 [79.6–83.1] | 81.7 [80.0–83.4] | 93.7 [92.4–94.9] | 93.3 [92.1–94.6] | 93.2 [92.0–94.2] | 92.9 [91.7–94.0] | 2.8 [2.1–3.5] | 2.9 [2.1–3.6] | 0.0% | 0.8% | 0.0% | 2.8% | Soft Minimax-40 (VSE\* WME\*), ESR-MARS (VSE\* WME\*), Smith//Score (WME\*) +10 more |
| MJ | median-based | 88.4 [87.1–89.6] | 90.7 [89.6–91.7] | -0.3 [-1.1–0.5] | 3.4 [2.5–4.5] | 49 [45–55] | 100.0 [100.0–100.0] | 91.5 | 93.1 | 89.0 [87.8–90.1] | 92.4 [91.5–93.3] | 0.7 [0.0–1.4] | 2.6 [1.7–3.6] | 57 [52–62] | 81.6 [79.9–83.4] | 79.4 [77.7–81.1] | 93.2 [92.0–94.6] | 90.9 [89.4–92.3] | 92.3 [91.2–93.5] | 90.3 [89.0–91.5] | 2.4 [1.8–3.1] | 2.4 [1.7–3.1] | 0.0% | 0.1% | 0.0% | 6.5% | ESR (VSE\* WME\*), STAR (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +10 more |
| Score | rated | 88.1 [86.9–89.3] | 90.1 [89.1–91.2] | -0.8 [-1.6–0.1] | 3.8 [2.9–4.8] | 46 [43–52] | 100.0 [100.0–100.0] | 90.8 | 93.1 | 89.6 [88.4–90.8] | 93.9 [93.1–94.7] | 1.7 [1.1–2.4] | 1.7 [0.8–2.7] | 66 [61–73] | 84.0 [82.4–85.6] | 78.9 [77.2–80.8] | 97.3 [96.4–98.1] | 90.1 [88.7–91.6] | 96.4 [95.5–97.2] | 90.4 [89.1–91.6] | 2.1 [1.6–2.9] | 2.2 [1.7–2.9] | 0.0% | 0.2% | 0.0% | 9.5% | ESR (VSE\* WME\*), STAR (VSE\* WME\*), Score runoff (VSE\* WME\*) +12 more |
| MAV | median-based | 88.0 [86.7–89.2] | 89.9 [88.8–90.9] | -0.9 [-1.7–-0.0] | 3.9 [2.9–5.0] | 46 [42–51] | 100.0 [100.0–100.0] | 91.7 | 93.3 | 88.6 [87.3–89.7] | 90.7 [89.6–91.7] | -0.3 [-1.0–0.5] | 3.3 [2.4–4.3] | 50 [46–56] | 78.5 [76.5–80.1] | 77.6 [75.7–79.5] | 91.5 [90.1–92.8] | 89.9 [88.3–91.3] | 91.0 [89.7–92.0] | 89.5 [88.2–90.8] | 2.9 [2.2–3.6] | 2.8 [2.1–3.5] | 0.0% | 0.1% | 0.0% | 6.2% | ESR (VSE\* WME\*), STAR (VSE\* WME\*), Score runoff (VSE\* WME\*) +14 more |
| Smith//Score | condorcet-based | 87.8 [86.5–89.0] | 96.6 [96.1–97.2] | 2.0 [1.4–2.6] | 1.7 [1.0–2.4] | 68 [64–74] | 100.0 [100.0–100.0] | 92.2 | 93.0 | 88.1 [86.8–89.4] | 97.0 [96.5–97.5] | 2.3 [1.8–2.9] | 1.3 [0.7–2.0] | 74 [69–80] | 90.7 [89.4–92.0] | 89.9 [88.6–91.4] | 98.6 [98.0–99.1] | 98.0 [97.3–98.7] | 97.6 [96.9–98.2] | 96.9 [96.1–97.5] | 2.1 [1.5–2.9] | 2.2 [1.6–2.9] | 0.0% | 0.2% | 0.0% | 2.8% | Soft Minimax (WME\*), Soft Minimax-D (VSE\*) |
| BTR-score | hybrid | 87.7 [86.4–89.0] | 96.7 [96.1–97.2] | 2.0 [1.4–2.6] | 1.7 [1.0–2.4] | 69 [64–74] | 100.0 [100.0–100.0] | 92.2 | 93.0 | 87.9 [86.5–89.1] | 97.0 [96.5–97.5] | 2.2 [1.6–2.8] | 1.5 [0.8–2.1] | 72 [68–78] | 90.7 [89.4–92.1] | 90.0 [88.6–91.4] | 98.5 [97.9–99.1] | 98.1 [97.4–98.7] | 97.2 [96.4–97.8] | 96.7 [95.9–97.4] | 2.2 [1.6–2.9] | 2.3 [1.7–3.0] | 0.0% | 0.2% | 0.0% | 2.6% | Soft Minimax (WME\*), Soft Minimax-D (VSE\*) |
| Approval | approval-based | 87.6 [86.2–88.9] | 87.6 [86.3–88.7] | -2.2 [-3.1–-1.2] | 4.7 [3.5–5.9] | 42 [38–46] | 100.0 [100.0–100.0] | 70.6 | 71.0 | 87.7 [86.3–89.0] | 88.3 [87.1–89.4] | -1.8 [-2.7–-0.9] | 4.5 [3.4–5.7] | 43 [39–47] | 75.9 [74.0–77.8] | 75.4 [73.5–77.2] | 89.6 [88.1–91.1] | 88.6 [86.9–90.2] | 89.6 [88.3–90.9] | 88.6 [87.1–89.9] | 4.0 [3.1–4.9] | 3.9 [3.0–4.8] | 0.0% | 0.2% | 0.0% | 3.5% | Approval runoff (VSE\* WME\*), DMS (VSE\* WME\*), ESR (VSE\* WME\*) +18 more |
| Soft Minimax | hybrid | 87.6 [86.2–88.8] | 97.2 [96.6–97.7] | 2.1 [1.5–2.7] | 1.5 [0.9–2.1] | 72 [67–77] | 100.0 [100.0–100.0] | 92.5 | 93.0 | 87.5 [86.2–88.7] | 97.3 [96.9–97.8] | 2.1 [1.6–2.7] | 1.4 [0.8–2.0] | 73 [69–78] | 91.4 [90.2–92.7] | 91.2 [89.9–92.5] | 98.8 [98.3–99.3] | 98.6 [98.0–99.2] | 97.5 [96.8–98.2] | 97.2 [96.5–97.9] | 2.1 [1.4–2.8] | 2.1 [1.5–2.8] | 0.0% | 0.2% | 0.0% | 1.8% | — |
| IMDS | rated | 87.5 [86.1–88.7] | 96.0 [95.4–96.7] | 1.6 [1.0–2.1] | 2.1 [1.4–2.9] | 63 [60–69] | 100.0 [100.0–100.0] | 90.7 | 93.0 | 87.8 [86.5–88.9] | 97.0 [96.5–97.6] | 2.2 [1.6–2.7] | 1.5 [0.9–2.1] | 72 [67–77] | 91.2 [90.0–92.5] | 89.1 [87.7–90.6] | 98.9 [98.4–99.4] | 97.4 [96.6–98.1] | 97.3 [96.7–98.0] | 95.8 [94.9–96.6] | 2.1 [1.5–2.9] | 2.4 [1.7–3.0] | 0.0% | 0.2% | 0.0% | 9.4% | Smith//Score (WME\*), BTR-score (WME\*), MARS (VSE\*) +2 more |
| ER-IRV | plurality-based | 86.0 [84.6–87.3] | 92.8 [91.7–93.7] | -0.8 [-1.5–0.0] | 4.5 [3.6–5.5] | 45 [42–49] | 99.6 [99.6–99.7] | 75.9 | 76.7 | 86.6 [85.3–87.9] | 93.4 [92.4–94.3] | -0.1 [-0.8–0.6] | 3.9 [3.0–4.8] | 49 [45–53] | 88.4 [87.0–89.8] | 87.2 [85.6–88.6] | 95.3 [94.2–96.3] | 94.4 [93.2–95.5] | 93.2 [92.0–94.3] | 92.4 [91.3–93.6] | 2.3 [1.6–3.0] | 2.6 [1.9–3.4] | 1.0% | 3.4% | 0.2% | 0.4% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), STAR (VSE\* WME\*) +10 more |
| TTR | two-round | 85.1 [83.5–86.3] | 90.4 [89.3–91.7] | -2.3 [-3.2–-1.4] | 6.0 [4.9–7.1] | 38 [35–41] | 98.8 [98.6–98.9] | 61.9 | 62.9 | 84.8 [83.4–86.2] | 90.1 [89.0–91.3] | -2.6 [-3.5–-1.6] | 6.3 [5.1–7.3] | 37 [34–40] | 83.9 [82.3–85.5] | 84.2 [82.6–85.8] | 90.3 [88.8–91.7] | 90.4 [88.9–91.9] | 88.9 [87.6–90.3] | 89.4 [88.1–90.8] | 3.2 [2.5–4.0] | 2.8 [2.1–3.6] | 2.9% | 0.0% | 1.0% | 0.0% | Smith//Score (VSE\* WME\* sincerity), IMDS (VSE\* WME\* sincerity), ESR (VSE\* WME\* sincerity) +27 more |
| Supplementary vote | plurality-based | 84.9 [83.4–86.4] | 82.7 [81.2–84.3] | -5.7 [-7.0–-4.3] | 7.6 [6.1–9.1] | 31 [28–35] | 99.5 [99.4–99.5] | 76.2 | 77.0 | 85.6 [84.1–87.0] | 83.3 [81.7–84.8] | -5.0 [-6.3–-3.6] | 6.9 [5.5–8.3] | 33 [30–37] | 71.7 [69.8–73.8] | 71.5 [69.6–73.5] | 83.5 [81.5–85.4] | 83.0 [81.0–84.8] | 84.1 [82.3–85.7] | 83.5 [81.8–85.2] | 4.2 [3.3–5.1] | 4.3 [3.5–5.2] | 3.1% | 2.1% | 0.3% | 0.0% | Smith//Score (VSE\* WME\*), Approval (VSE\* WME\*), Approval runoff (VSE\* WME\*) +31 more |
| Copeland | condorcet | 84.9 [83.4–86.2] | 99.0 [98.7–99.3] | 1.5 [0.9–2.0] | 0.9 [0.6–1.2] | 83 [79–87] | 99.8 [99.8–99.8] | 75.7 | 76.0 | 84.7 [83.1–86.0] | 99.5 [99.2–99.6] | 1.5 [1.1–2.1] | 0.5 [0.3–0.7] | 89 [86–92] | 98.2 [97.6–98.7] | 96.8 [96.1–97.5] | 99.3 [98.9–99.7] | 98.8 [98.3–99.3] | 97.4 [96.8–98.1] | 96.7 [96.0–97.5] | 2.9 [2.1–3.6] | 2.6 [2.0–3.4] | 0.6% | 0.9% | 0.0% | 0.2% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) |
| IRV | plurality-based | 84.9 [83.3–86.3] | 90.3 [89.1–91.5] | -2.5 [-3.4–-1.4] | 6.2 [5.0–7.3] | 37 [34–41] | 99.0 [99.0–99.1] | 75.2 | 76.7 | 86.6 [85.3–87.9] | 93.4 [92.4–94.3] | -0.1 [-0.8–0.6] | 3.9 [3.0–4.8] | 49 [45–53] | 88.4 [87.0–89.8] | 84.4 [82.8–86.0] | 95.3 [94.2–96.3] | 91.3 [89.8–92.7] | 93.2 [92.0–94.3] | 89.6 [88.3–90.9] | 2.3 [1.6–3.0] | 3.1 [2.4–4.0] | 3.8% | 2.5% | 1.3% | 0.2% | Smith//Score (VSE\* WME\*), ER-IRV (VSE\* WME\*), IMDS (VSE\* WME\*) +27 more |
| BTR-IRV | condorcet-based | 84.9 [83.3–86.2] | 98.5 [98.2–98.9] | 1.2 [0.6–1.8] | 1.4 [0.9–1.7] | 77 [72–81] | 99.7 [99.7–99.7] | 75.6 | 76.0 | 85.0 [83.5–86.4] | 99.5 [99.3–99.6] | 1.7 [1.2–2.3] | 0.5 [0.3–0.7] | 90 [87–93] | 98.2 [97.7–98.8] | 95.7 [94.9–96.7] | 99.5 [99.2–99.9] | 98.5 [97.9–99.1] | 98.0 [97.4–98.6] | 96.8 [96.0–97.5] | 2.8 [2.1–3.5] | 3.0 [2.3–3.8] | 1.2% | 1.3% | 0.1% | 0.4% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +4 more |
| Kemeny-Young | condorcet | 84.8 [83.3–86.2] | 99.5 [99.2–99.6] | 1.6 [1.1–2.2] | 0.5 [0.3–0.7] | 89 [86–93] | 99.9 [99.8–99.9] | 75.8 | 76.0 | 84.7 [83.2–86.1] | 99.7 [99.5–99.8] | 1.7 [1.3–2.2] | 0.3 [0.2–0.5] | 93 [90–95] | 98.1 [97.6–98.8] | 97.6 [96.9–98.2] | 99.3 [98.9–99.7] | 98.7 [98.2–99.2] | 97.9 [97.2–98.4] | 97.5 [96.8–98.1] | 3.0 [2.4–3.8] | 3.0 [2.3–3.9] | 0.5% | 0.8% | 0.0% | 0.2% | — |
| Schulze | condorcet | 84.7 [83.2–86.0] | 99.3 [99.0–99.5] | 1.5 [1.0–2.1] | 0.7 [0.4–0.9] | 87 [83–90] | 99.8 [99.8–99.9] | 75.8 | 76.0 | 84.7 [83.2–86.1] | 99.6 [99.4–99.8] | 1.6 [1.2–2.2] | 0.4 [0.2–0.5] | 92 [89–95] | 98.1 [97.5–98.7] | 97.0 [96.3–97.7] | 99.3 [98.9–99.6] | 98.6 [98.0–99.1] | 97.9 [97.3–98.5] | 97.2 [96.5–97.9] | 2.9 [2.2–3.6] | 3.0 [2.3–3.8] | 0.6% | 0.9% | 0.0% | 0.1% | — |
| Minimax | minimax-family | 84.7 [83.2–86.0] | 99.3 [99.0–99.5] | 1.5 [0.9–2.0] | 0.7 [0.5–1.0] | 86 [82–90] | 99.8 [99.8–99.9] | 75.7 | 76.0 | 84.7 [83.2–86.1] | 99.6 [99.4–99.8] | 1.6 [1.2–2.2] | 0.4 [0.2–0.5] | 92 [89–95] | 98.1 [97.5–98.7] | 96.9 [96.1–97.6] | 99.3 [98.9–99.6] | 98.4 [97.8–99.0] | 97.9 [97.3–98.5] | 97.2 [96.5–97.9] | 2.9 [2.2–3.6] | 2.9 [2.2–3.8] | 0.7% | 1.0% | 0.0% | 0.1% | Kemeny-Young (WME\*) |
| Ranked pairs | condorcet | 84.5 [82.9–85.9] | 99.4 [99.1–99.6] | 1.4 [1.0–2.0] | 0.6 [0.4–0.9] | 88 [84–91] | 99.9 [99.8–99.9] | 75.8 | 76.0 | 84.8 [83.3–86.1] | 99.6 [99.4–99.8] | 1.7 [1.2–2.2] | 0.4 [0.2–0.6] | 92 [89–94] | 98.1 [97.5–98.7] | 97.5 [96.8–98.1] | 99.2 [98.8–99.6] | 98.8 [98.3–99.3] | 97.7 [97.1–98.3] | 97.3 [96.5–97.9] | 2.9 [2.2–3.6] | 3.1 [2.4–3.9] | 0.5% | 0.8% | 0.1% | 0.2% | — |
| MMWV | minimax-family | 84.5 [83.0–85.8] | 99.2 [99.0–99.5] | 1.3 [0.9–1.9] | 0.7 [0.5–1.0] | 86 [82–89] | 99.9 [99.8–99.9] | 75.7 | 76.0 | 84.7 [83.2–86.1] | 99.6 [99.4–99.8] | 1.6 [1.2–2.2] | 0.4 [0.2–0.5] | 92 [89–95] | 98.1 [97.5–98.7] | 96.9 [96.2–97.7] | 99.3 [98.9–99.6] | 98.4 [97.7–99.0] | 97.9 [97.3–98.5] | 97.1 [96.4–97.8] | 2.9 [2.2–3.6] | 3.0 [2.3–3.8] | 0.1% | 1.5% | 0.0% | 0.1% | Schulze (VSE\*), Kemeny-Young (WME\*), Minimax (VSE\*) |
| MMPO | minimax-family | 84.4 [82.8–85.7] | 99.3 [99.0–99.5] | 1.3 [0.8–1.9] | 0.7 [0.5–1.0] | 86 [82–89] | 99.9 [99.8–99.9] | 75.7 | 76.0 | 84.7 [83.2–86.1] | 99.6 [99.4–99.8] | 1.6 [1.2–2.2] | 0.4 [0.2–0.5] | 92 [89–95] | 98.1 [97.5–98.7] | 96.9 [96.2–97.7] | 99.3 [98.9–99.6] | 98.4 [97.7–99.0] | 97.9 [97.3–98.5] | 97.1 [96.4–97.8] | 2.9 [2.2–3.6] | 3.0 [2.3–3.8] | 0.1% | 1.6% | 0.0% | 0.1% | Schulze (VSE\*), Kemeny-Young (WME\*), Copeland (VSE\*) +1 more |
| IRV-3 | plurality-based | 84.0 [82.3–85.5] | 86.6 [85.2–88.0] | -4.6 [-5.9–-3.4] | 7.8 [6.5–9.3] | 31 [28–34] | 99.1 [99.1–99.2] | 73.5 | 74.8 | 87.4 [86.0–88.6] | 90.7 [89.6–91.8] | -0.9 [-1.7–-0.1] | 4.2 [3.2–5.4] | 45 [41–49] | 82.2 [80.5–83.8] | 78.1 [76.2–79.8] | 92.0 [90.6–93.2] | 87.1 [85.4–88.7] | 91.1 [89.8–92.3] | 86.5 [84.9–87.9] | 2.8 [2.1–3.5] | 4.2 [3.3–5.1] | 3.8% | 1.1% | 1.4% | 0.1% | Smith//Score (VSE\* WME\*), ER-IRV (VSE\* WME\*), Approval runoff (VSE\* WME\*) +30 more |
| Asset | delegated | 82.3 [80.8–83.9] | 82.0 [80.4–83.6] | -7.6 [-8.9–-6.3] | 10.0 [8.6–11.5] | 26 [23–28] | 99.1 [99.1–99.2] | 64.1 | 65.0 | 83.7 [82.2–85.1] | 83.1 [81.6–84.6] | -6.3 [-7.6–-5.1] | 8.7 [7.4–10.1] | 29 [26–31] | 71.2 [69.2–73.2] | 70.3 [68.2–72.3] | 78.9 [76.8–81.0] | 77.8 [75.8–79.9] | 78.7 [77.0–80.5] | 77.2 [75.4–78.9] | 2.4 [1.8–3.1] | 2.9 [2.1–3.7] | 2.4% | 0.0% | 0.5% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +33 more |
| Borda | positional | 82.1 [80.3–83.7] | 91.1 [90.1–92.0] | -3.6 [-4.7–-2.6] | 7.3 [6.3–8.3] | 35 [32–38] | 99.2 [99.2–99.3] | 73.8 | 75.6 | 83.5 [81.9–84.9] | 94.9 [94.2–95.6] | -1.2 [-2.0–-0.3] | 4.4 [3.7–5.2] | 49 [46–53] | 84.7 [83.2–86.4] | 78.6 [76.9–80.4] | 89.0 [87.5–90.5] | 85.1 [83.4–86.8] | 89.1 [87.7–90.3] | 85.5 [84.0–87.1] | 2.1 [1.5–2.7] | 2.8 [2.0–3.5] | 3.9% | 4.7% | 0.0% | 4.6% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +29 more |
| Plurality | plurality-based | 75.1 [73.0–77.1] | 69.1 [67.1–71.0] | -15.6 [-17.6–-13.5] | 17.4 [15.4–19.5] | 15 [14–16] | 99.4 [99.3–99.4] | 62.6 | 63.3 | 73.3 [71.0–75.5] | 68.2 [66.1–70.1] | -17.3 [-19.8–-14.9] | 19.2 [17.0–21.5] | 14 [12–15] | 56.6 [54.5–58.8] | 57.3 [55.2–59.5] | 66.4 [64.0–68.8] | 67.0 [64.6–69.3] | 68.3 [66.3–70.4] | 69.4 [67.3–71.3] | 8.7 [7.4–9.9] | 8.2 [7.0–9.4] | 5.2% | 0.0% | 0.2% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +34 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Borda | 13.3% |
| Plurality | 10.3% |
| ESR | 10.0% |
| Score | 10.0% |
| IMDS | 9.9% |
| DMS | 8.7% |
| IRV | 8.2% |
| IRV-3 | 8.1% |
| ESR-MARS | 7.0% |
| MJ | 6.8% |
| MAV | 6.6% |
| Score runoff | 6.3% |
| Approval | 6.2% |
| 321 | 6.2% |
| Delegated 321 | 6.2% |
| TTR | 6.0% |
| Supplementary vote | 5.9% |
| MARS-T | 5.5% |
| MARS | 5.5% |
| Asset | 5.3% |
| ER-IRV | 5.2% |
| Approval cond. runoff | 5.2% |
| STAR | 5.0% |
| ARA | 4.6% |
| Approval runoff | 4.4% |
| Soft Minimax-40 | 3.6% |
| BTR-IRV | 3.2% |
| Smith//Score | 3.1% |
| BTR-score | 2.9% |
| Soft Minimax-D | 2.1% |
| Soft Minimax | 2.0% |
| MMWV | 1.8% |
| Minimax | 1.8% |
| MMPO | 1.8% |
| Copeland | 1.8% |
| Schulze | 1.8% |
| Ranked pairs | 1.6% |
| Kemeny-Young | 1.6% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Plurality | 39.6% | 1.77 | 1.05 | 95% |
| Supplementary vote | 31.8% | 1.74 | 0.78 | 99% |
| TTR | 29.8% | 0.99 | 0.85 | 99% |
| Approval | 25.1% | 1.14 | 0.43 | 100% |
| Asset | 25.1% | 0.45 | 0.12 | 100% |
| IRV-3 | 23.2% | 0.94 | 0.42 | 100% |
| Approval runoff | 22.8% | 0.75 | 0.52 | 100% |
| Approval cond. runoff | 21.8% | 0.84 | 0.53 | 100% |
| IRV | 20.4% | 0.35 | 0.07 | 100% |
| ER-IRV | 20.4% | 0.35 | 0.07 | 100% |
| MAV | 18.5% | 0.67 | 0.10 | 100% |
| MJ | 17.6% | 0.53 | 0.10 | 100% |
| DMS | 17.6% | 0.52 | 0.09 | 100% |
| 321 | 17.3% | 0.64 | 0.51 | 100% |
| Delegated 321 | 17.3% | 0.64 | 0.51 | 100% |
| Score runoff | 14.8% | 0.32 | 0.27 | 100% |
| Score | 13.5% | 0.49 | 0.12 | 100% |
| ESR | 12.8% | 0.51 | 0.12 | 100% |
| STAR | 11.7% | 0.43 | 0.22 | 100% |
| ARA | 11.7% | 0.42 | 0.13 | 100% |
| ESR-MARS | 11.7% | 0.44 | 0.09 | 100% |
| Soft Minimax-40 | 11.6% | 0.45 | 0.14 | 100% |
| Borda | 10.8% | 0.08 | 0.56 | 100% |
| IMDS | 10.6% | 0.38 | 0.15 | 100% |
| Soft Minimax | 10.2% | 0.38 | 0.13 | 100% |
| BTR-score | 9.4% | 0.33 | 0.08 | 100% |
| Smith//Score | 9.3% | 0.33 | 0.08 | 100% |
| MARS | 9.1% | 0.35 | 0.09 | 100% |
| Soft Minimax-D | 9.0% | 0.32 | 0.08 | 100% |
| MARS-T | 8.8% | 0.33 | 0.07 | 100% |
| Copeland | 5.7% | 0.03 | 0.09 | 100% |
| Ranked pairs | 4.3% | 0.05 | 0.06 | 100% |
| BTR-IRV | 4.2% | 0.04 | 0.05 | 100% |
| Schulze | 4.0% | 0.02 | 0.05 | 100% |
| Minimax | 4.0% | 0.02 | 0.05 | 100% |
| MMWV | 4.0% | 0.02 | 0.05 | 100% |
| MMPO | 4.0% | 0.02 | 0.05 | 100% |
| Kemeny-Young | 3.6% | 0.02 | 0.07 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 1927 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 73 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| ESR-MARS | 87.1% (87.5%) | 10.4% (10.1%) | 2.4% (2.4%) | 75.3% (68.5%) | 93.4% (95.8%) | 84.0% |
| Soft Minimax-40 | 87.1% (86.9%) | 10.0% (9.5%) | 2.9% (3.6%) | 75.3% (75.3%) | 96.3% (96.8%) | 86.5% |
| ESR | 86.5% (88.3%) | 10.8% (9.7%) | 2.7% (2.0%) | 80.8% (75.3%) | 93.7% (96.5%) | 82.9% |
| STAR | 87.3% (88.2%) | 10.0% (9.5%) | 2.8% (2.3%) | 86.3% (79.5%) | 94.6% (94.5%) | 85.1% |
| DMS | 85.2% (85.8%) | 11.3% (10.3%) | 3.5% (3.8%) | 75.3% (75.3%) | 87.7% (90.8%) | 78.9% |
| 321 | 77.3% (76.8%) | 14.0% (12.8%) | 8.7% (10.4%) | 79.5% (82.2%) | 97.4% (98.0%) | 80.7% |
| Delegated 321 | 77.3% (76.8%) | 14.0% (12.8%) | 8.7% (10.4%) | 79.5% (82.2%) | 97.4% (98.0%) | 80.7% |
| ARA | 86.7% (87.1%) | 9.4% (9.3%) | 3.9% (3.6%) | 79.5% (79.5%) | 95.1% (95.6%) | 86.9% |
| MARS-T | 87.1% (88.1%) | 9.9% (9.5%) | 3.0% (2.4%) | 78.1% (75.3%) | 96.0% (98.0%) | 86.5% |
| Soft Minimax-D | 88.6% (89.0%) | 9.3% (9.0%) | 2.1% (2.0%) | 75.3% (78.1%) | 99.6% (99.7%) | 89.3% |
| MARS | 87.0% (88.1%) | 9.9% (9.4%) | 3.1% (2.5%) | 79.5% (75.3%) | 96.7% (98.2%) | 87.3% |
| MJ | 85.0% (85.6%) | 11.8% (10.8%) | 3.2% (3.6%) | 80.8% (82.2%) | 86.2% (87.7%) | 79.4% |
| Score | 85.8% (86.6%) | 11.2% (10.2%) | 3.0% (3.2%) | 76.7% (69.9%) | 86.2% (91.8%) | 78.9% |
| MAV | 85.5% (85.7%) | 11.8% (11.1%) | 2.8% (3.2%) | 80.8% (72.6%) | 86.0% (86.9%) | 77.6% |
| Smith//Score | 89.1% (89.7%) | 9.0% (8.7%) | 2.0% (1.6%) | 79.5% (76.7%) | 100.0% (100.0%) | 89.9% |
| BTR-score | 89.3% (89.7%) | 9.2% (8.8%) | 1.6% (1.5%) | 80.8% (79.5%) | 100.0% (100.0%) | 90.0% |
| Soft Minimax | 89.9% (90.2%) | 7.9% (7.9%) | 2.1% (1.9%) | 79.5% (79.5%) | 100.0% (100.0%) | 91.2% |
| IMDS | 87.5% (89.7%) | 9.2% (7.8%) | 3.2% (2.5%) | 82.2% (82.2%) | 99.8% (99.6%) | 89.1% |
| ER-IRV | 94.8% (98.4%) | 3.7% (1.1%) | 1.6% (0.5%) | 52.1% (32.9%) | 90.3% (89.8%) | 87.2% |
| Supplementary vote | 85.8% (88.8%) | 12.7% (10.8%) | 1.5% (0.4%) | 67.1% (56.2%) | 81.8% (81.0%) | 71.5% |
| Copeland | 96.0% (97.6%) | 2.2% (1.4%) | 1.8% (1.0%) | 53.4% (50.7%) | 100.0% (100.0%) | 96.8% |
| IRV | 93.4% (98.4%) | 4.6% (1.1%) | 2.1% (0.5%) | 49.3% (32.9%) | 88.3% (89.8%) | 84.4% |
| BTR-IRV | 95.4% (98.1%) | 3.0% (1.5%) | 1.7% (0.4%) | 68.5% (65.8%) | 100.0% (100.0%) | 95.7% |
| Kemeny-Young | 96.5% (97.7%) | 1.8% (1.3%) | 1.7% (1.0%) | 35.6% (30.1%) | 100.0% (100.0%) | 97.6% |
| Schulze | 96.2% (97.6%) | 2.0% (1.4%) | 1.8% (1.0%) | 47.9% (42.5%) | 100.0% (100.0%) | 97.0% |
| Minimax | 96.2% (97.6%) | 2.2% (1.4%) | 1.7% (1.0%) | 47.9% (42.5%) | 100.0% (100.0%) | 96.9% |
| Ranked pairs | 96.6% (97.6%) | 1.9% (1.4%) | 1.6% (1.0%) | 42.5% (42.5%) | 100.0% (100.0%) | 97.5% |
| MMWV | 96.1% (97.6%) | 2.1% (1.4%) | 1.9% (1.0%) | 47.9% (42.5%) | 100.0% (100.0%) | 96.9% |
| MMPO | 96.0% (97.6%) | 2.0% (1.4%) | 2.0% (1.0%) | 47.9% (42.5%) | 99.9% (100.0%) | 96.9% |
| IRV-3 | 78.9% (81.9%) | 17.4% (15.2%) | 3.7% (2.9%) | 89.0% (80.8%) | 97.1% (97.9%) | 78.1% |
| Borda | 91.3% (98.9%) | 3.9% (1.0%) | 4.8% (0.2%) | 41.1% (15.1%) | 81.5% (85.2%) | 78.6% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| ESR-MARS | 0.0% (0.0%) | 20.2% (20.6%) | 1.0% (1.0%) |
| Soft Minimax-40 | 0.8% (0.2%) | 21.7% (21.5%) | 1.1% (1.1%) |
| ESR | 0.4% (0.1%) | 23.4% (20.7%) | 1.1% (1.0%) |
| STAR | 0.1% (0.1%) | 21.4% (22.5%) | 1.0% (1.1%) |
| DMS | 0.0% (0.0%) | 19.9% (20.1%) | 1.0% (1.0%) |
| Score runoff | 0.4% (0.3%) | — (—) | — (—) |
| 321 | 0.2% (0.1%) | 35.8% (38.0%) | 1.8% (2.0%) |
| Delegated 321 | 0.2% (0.1%) | 35.8% (38.0%) | 1.8% (2.0%) |
| ARA | 0.9% (0.5%) | 21.7% (22.1%) | 1.1% (1.1%) |
| MARS-T | 0.1% (0.0%) | 20.5% (20.8%) | 1.0% (1.0%) |
| Soft Minimax-D | 0.1% (0.0%) | 20.8% (20.8%) | 1.0% (1.0%) |
| MARS | 0.2% (0.1%) | 20.3% (21.1%) | 1.0% (1.0%) |
| Approval runoff | 0.9% (0.4%) | — (—) | — (—) |
| Approval cond. runoff | 0.9% (0.4%) | — (—) | — (—) |
| MJ | 0.0% (0.0%) | 19.2% (19.9%) | 0.9% (1.0%) |
| Score | 0.6% (0.1%) | 23.6% (20.9%) | 1.1% (1.0%) |
| MAV | 0.2% (0.0%) | 19.3% (19.4%) | 1.0% (0.9%) |
| Smith//Score | 0.0% (0.1%) | 20.1% (20.4%) | 0.9% (1.0%) |
| BTR-score | 0.0% (0.0%) | 20.2% (20.4%) | 1.0% (1.0%) |
| Approval | 1.5% (0.6%) | — (—) | — (—) |
| Soft Minimax | 1.1% (0.7%) | 21.4% (21.2%) | 1.1% (1.0%) |
| IMDS | 1.4% (0.8%) | 24.2% (21.9%) | 1.2% (1.1%) |
| ER-IRV | 1.6% (1.2%) | 1.9% (0.0%) | 0.1% (0.0%) |
| TTR | 2.0% (1.9%) | — (—) | — (—) |
| Supplementary vote | 1.4% (0.9%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Copeland | 1.8% (1.4%) | 1.5% (0.0%) | 0.1% (0.0%) |
| IRV | 1.2% (1.2%) | 0.9% (0.0%) | 0.0% (0.0%) |
| BTR-IRV | 0.2% (0.1%) | 0.8% (0.0%) | 0.0% (0.0%) |
| Kemeny-Young | 1.0% (1.0%) | 0.9% (0.0%) | 0.0% (0.0%) |
| Schulze | 1.4% (1.4%) | 0.7% (0.0%) | 0.0% (0.0%) |
| Minimax | 1.4% (1.4%) | 0.5% (0.0%) | 0.0% (0.0%) |
| Ranked pairs | 1.2% (1.1%) | 1.2% (0.0%) | 0.1% (0.0%) |
| MMWV | 1.1% (1.4%) | 1.6% (0.0%) | 0.1% (0.0%) |
| MMPO | 1.2% (1.4%) | 1.4% (0.0%) | 0.1% (0.0%) |
| IRV-3 | 1.3% (0.7%) | 24.1% (24.9%) | 1.1% (1.1%) |
| Asset | 3.5% (1.8%) | — (—) | — (—) |
| Borda | 0.7% (0.6%) | 6.5% (0.0%) | 0.3% (0.0%) |
| Plurality | 1.7% (0.8%) | — (—) | — (—) |

![strategy arrows](arrows-c3.png)

![strategy shifts](shift-c3.png)

![sincerity](sincerity-c3.png)

![fidelity](fidelity-c3.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c3.png)

## c4: candidates 10

2000 elections (seeds 1), 1906 with a Condorcet winner. 99 voters, 10 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 95.3%; it also has the highest total utility in 62.4% of those; electing it loses 13.3% of VSE; favourite ÷ least liked 13.9 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 85.0 to 85.9; with the best VSE (100) WME 77.8 (nothing lower is possible there either); the most balanced point VSE 94.8 / WME 96.6. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 93.4 (at WME 89.5); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 93.2 (at WME 89.3); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -38.8 / WME -31.2. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 85.3 / WME 99.7. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 89.4 / 94.7; normalized utility winner 93.4 / 89.5; best worst margin 85.4 / 100.0; Condorcet winner, else random ballot 83.1 / 97.1; Condorcet winner, else utility winner 87.2 / 99.2; random pair, majority 16.1 / 4.1; nearest the median voter 88.5 / 97.2; nearest the mean voter 74.8 / 92.3.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): ESR-MARS, STAR, ARA, Soft Minimax-40, MARS-T, MARS, Soft Minimax-D, Soft Minimax, MMWV, Kemeny-Young.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): ARA, Soft Minimax-40, Soft Minimax-D, MMWV, MMPO, Kemeny-Young, Minimax, Schulze, Ranked pairs.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 83.1 / 97.1; its mirror on the utility side 90.7 / 87.0; random pair 16.1 / 4.1. No better than a lottery of them (Lead ≤ 0): with learned ballots DMS, MAV, MJ, Approval cond. runoff, Approval, ER-IRV, IRV, IRV-3, TTR, Asset, Borda, Supplementary vote, Plurality; with honest ballots Approval runoff, Approval cond. runoff, Approval, ER-IRV, IRV, IRV-3, TTR, Asset, Borda, Supplementary vote, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ESR-MARS | hybrid | 91.0 [90.1–91.9] | 91.8 [90.9–92.8] | 2.2 [1.5–3.0] | 1.9 [1.1–2.7] | 64 [59–71] | 100.0 [100.0–100.0] | 90.3 | 92.1 | 91.3 [90.4–92.2] | 94.0 [93.1–94.7] | 3.3 [2.6–4.0] | 1.1 [0.4–1.8] | 75 [69–80] | 82.7 [80.9–84.4] | 78.9 [77.1–80.8] | 97.7 [96.9–98.4] | 93.8 [92.6–95.0] | 96.8 [96.0–97.5] | 93.3 [92.2–94.5] | 1.3 [0.8–1.8] | 1.1 [0.7–1.6] | 0.0% | 0.3% | 0.0% | 7.0% | STAR (WME\*), ARA (WME\*), Soft Minimax-40 (WME\*) |
| STAR | hybrid | 90.8 [89.9–91.7] | 92.9 [92.1–93.7] | 2.6 [1.9–3.3] | 1.8 [1.0–2.5] | 65 [61–72] | 100.0 [100.0–100.0] | 90.5 | 92.1 | 91.3 [90.4–92.2] | 93.9 [93.2–94.6] | 3.3 [2.6–4.0] | 1.1 [0.4–1.9] | 75 [69–80] | 82.6 [80.9–84.3] | 81.1 [79.4–82.8] | 98.4 [97.8–99.0] | 95.4 [94.2–96.4] | 97.5 [96.9–98.2] | 94.9 [94.0–95.9] | 1.2 [0.8–1.8] | 1.5 [1.0–2.1] | 0.0% | 0.7% | 0.0% | 4.9% | ARA (WME\*), Soft Minimax-40 (WME\*) |
| 321 | hybrid | 90.7 [89.8–91.6] | 89.5 [88.4–90.5] | 1.0 [0.3–1.8] | 2.5 [1.7–3.4] | 57 [52–63] | 100.0 [100.0–100.0] | 80.9 | 82.2 | 91.5 [90.6–92.4] | 90.7 [89.7–91.6] | 2.0 [1.3–2.7] | 1.6 [0.8–2.4] | 66 [61–74] | 78.2 [76.4–79.9] | 76.0 [74.0–77.8] | 94.0 [92.8–95.3] | 91.0 [89.6–92.5] | 93.3 [92.2–94.4] | 91.0 [89.7–92.3] | 1.4 [0.9–1.8] | 1.4 [0.9–1.9] | 0.0% | 0.5% | 0.0% | 5.7% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +3 more |
| Delegated 321 | delegated | 90.7 [89.8–91.6] | 89.5 [88.4–90.5] | 1.0 [0.3–1.8] | 2.5 [1.7–3.4] | 57 [52–63] | — | — | — | 91.5 [90.6–92.4] | 90.7 [89.7–91.6] | 2.0 [1.3–2.7] | 1.6 [0.8–2.4] | 66 [61–74] | 78.2 [76.4–79.9] | 76.0 [74.0–77.8] | 94.0 [92.8–95.3] | 91.0 [89.6–92.5] | 93.3 [92.2–94.4] | 91.0 [89.7–92.3] | 1.4 [0.9–1.8] | 1.4 [0.9–1.9] | 0.0% | 0.5% | 0.0% | 5.7% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +3 more |
| ARA | hybrid | 90.5 [89.6–91.4] | 94.1 [93.3–94.7] | 2.9 [2.2–3.7] | 1.7 [1.0–2.4] | 68 [63–74] | 100.0 [100.0–100.0] | 90.8 | 92.1 | 90.7 [89.8–91.5] | 95.1 [94.4–95.7] | 3.5 [2.8–4.2] | 1.3 [0.6–1.9] | 75 [69–80] | 84.3 [82.7–86.0] | 83.1 [81.5–84.7] | 97.1 [96.2–98.0] | 95.9 [94.9–96.8] | 96.4 [95.6–97.2] | 95.0 [94.1–95.9] | 1.1 [0.6–1.5] | 1.1 [0.6–1.6] | 0.0% | 0.2% | 0.0% | 4.6% | — |
| ESR | rated | 90.5 [89.5–91.4] | 91.6 [90.7–92.6] | 1.9 [1.1–2.6] | 2.3 [1.5–3.1] | 60 [55–66] | 100.0 [100.0–100.0] | 89.7 | 92.2 | 91.1 [90.1–92.0] | 93.5 [92.7–94.3] | 3.0 [2.3–3.7] | 1.4 [0.6–2.2] | 71 [64–78] | 82.5 [80.8–84.2] | 79.3 [77.5–81.2] | 97.7 [97.0–98.5] | 93.6 [92.3–94.9] | 96.4 [95.6–97.2] | 93.1 [92.0–94.2] | 1.6 [1.0–2.1] | 1.8 [1.2–2.4] | 0.0% | 0.2% | 0.0% | 10.0% | STAR (WME\*), Score runoff (WME\*), MARS-T (WME\*) +2 more |
| Soft Minimax-40 | hybrid | 90.4 [89.4–91.4] | 94.2 [93.4–94.9] | 2.9 [2.2–3.7] | 1.7 [0.9–2.4] | 68 [63–74] | 100.0 [100.0–100.0] | 91.2 | 92.2 | 90.5 [89.4–91.4] | 94.8 [94.1–95.4] | 3.2 [2.5–3.9] | 1.5 [0.8–2.2] | 71 [65–77] | 84.3 [82.8–85.9] | 83.5 [81.8–85.0] | 97.7 [96.9–98.4] | 97.0 [96.1–97.8] | 97.0 [96.3–97.8] | 96.1 [95.2–96.9] | 1.6 [1.0–2.1] | 1.6 [1.1–2.1] | 0.0% | 0.2% | 0.0% | 3.7% | — |
| DMS | rated | 90.2 [89.1–91.1] | 87.0 [85.8–88.2] | -0.3 [-1.1–0.5] | 3.2 [2.3–4.2] | 51 [45–56] | 100.0 [100.0–100.0] | 90.1 | 92.3 | 91.7 [90.7–92.6] | 88.8 [87.8–89.8] | 1.3 [0.6–2.1] | 1.7 [0.8–2.5] | 65 [59–73] | 74.1 [72.0–76.0] | 71.6 [69.5–73.5] | 93.7 [92.3–94.9] | 88.6 [87.0–90.2] | 93.8 [92.5–94.8] | 89.1 [87.8–90.5] | 1.7 [1.1–2.2] | 1.8 [1.3–2.4] | 0.0% | 0.2% | 0.0% | 8.4% | ESR-MARS (VSE\* WME\*), Smith//Score (WME\*), Approval runoff (WME\*) +11 more |
| MAV | median-based | 90.0 [88.9–90.9] | 87.9 [86.7–88.9] | -0.0 [-0.9–0.8] | 3.3 [2.4–4.2] | 50 [45–56] | 100.0 [100.0–100.0] | 90.4 | 92.3 | 90.6 [89.6–91.5] | 88.3 [87.2–89.3] | 0.5 [-0.3–1.3] | 2.7 [1.8–3.6] | 55 [50–62] | 73.1 [71.2–75.0] | 73.3 [71.2–75.1] | 88.6 [87.0–90.2] | 88.1 [86.3–89.7] | 89.4 [88.2–90.8] | 88.6 [87.2–90.0] | 1.6 [1.1–2.1] | 1.7 [1.1–2.2] | 0.0% | 0.2% | 0.0% | 6.8% | ESR-MARS (VSE\* WME\*), Smith//Score (WME\*), Approval runoff (WME\*) +11 more |
| Score runoff | two-round | 89.9 [88.9–90.9] | 93.5 [92.7–94.3] | 2.3 [1.6–3.0] | 2.3 [1.5–3.1] | 61 [57–66] | 99.9 [99.9–99.9] | 89.9 | 92.0 | 91.4 [90.5–92.3] | 94.1 [93.3–94.7] | 3.4 [2.8–4.1] | 1.0 [0.3–1.7] | 77 [72–83] | 83.4 [81.7–85.0] | 83.9 [82.2–85.5] | 98.9 [98.3–99.4] | 96.3 [95.4–97.2] | 98.2 [97.7–98.8] | 95.5 [94.5–96.3] | 1.4 [0.9–1.8] | 1.8 [1.2–2.3] | 0.0% | 1.4% | 0.0% | 5.5% | Smith//Score (WME\*), STAR (VSE\*), MARS (WME\*) +2 more |
| MARS-T | hybrid | 89.8 [88.7–90.8] | 94.6 [93.9–95.3] | 2.7 [2.0–3.5] | 2.0 [1.3–2.8] | 64 [60–71] | 100.0 [100.0–100.0] | 90.7 | 92.1 | 90.0 [88.9–91.0] | 95.6 [95.0–96.2] | 3.3 [2.6–4.0] | 1.5 [0.8–2.2] | 72 [67–77] | 86.0 [84.5–87.5] | 84.2 [82.6–85.8] | 98.6 [97.9–99.2] | 96.7 [95.7–97.5] | 97.8 [97.2–98.4] | 95.7 [94.8–96.5] | 1.6 [1.1–2.1] | 1.9 [1.3–2.5] | 0.0% | 0.1% | 0.0% | 5.6% | Smith//Score (WME\*), Soft Minimax-D (WME\*) |
| Score | rated | 89.6 [88.6–90.6] | 89.5 [88.4–90.6] | 0.5 [-0.4–1.3] | 3.4 [2.5–4.3] | 50 [45–55] | 100.0 [100.0–100.0] | 89.7 | 92.2 | 91.2 [90.3–92.1] | 92.9 [92.0–93.7] | 2.8 [2.1–3.5] | 1.5 [0.7–2.2] | 70 [64–76] | 80.6 [78.8–82.5] | 75.7 [73.7–77.6] | 97.0 [96.2–97.8] | 89.4 [87.6–90.9] | 96.3 [95.5–97.1] | 89.8 [88.4–91.1] | 1.2 [0.7–1.7] | 1.4 [0.9–1.9] | 0.0% | 0.2% | 0.0% | 10.1% | ESR (VSE\* WME\*), STAR (VSE\* WME\*), ARA (VSE\* WME\*) +12 more |
| MARS | hybrid | 89.6 [88.4–90.6] | 94.9 [94.3–95.6] | 2.8 [2.0–3.5] | 2.0 [1.3–2.8] | 64 [61–70] | 100.0 [100.0–100.0] | 90.7 | 92.1 | 89.7 [88.6–90.8] | 95.9 [95.3–96.5] | 3.3 [2.5–4.0] | 1.6 [0.9–2.3] | 71 [66–76] | 86.5 [85.0–88.1] | 84.9 [83.4–86.5] | 98.6 [97.9–99.1] | 97.1 [96.3–98.0] | 97.8 [97.2–98.4] | 96.2 [95.4–97.0] | 1.7 [1.1–2.2] | 2.0 [1.5–2.6] | 0.0% | 0.1% | 0.0% | 5.5% | Smith//Score (WME\*), BTR-score (WME\*), Soft Minimax-D (WME\*) |
| Approval runoff | two-round | 89.4 [88.3–90.5] | 89.4 [88.3–90.5] | 0.3 [-0.5–1.1] | 3.6 [2.6–4.6] | 49 [44–54] | 99.9 [99.9–99.9] | 67.0 | 67.6 | 89.1 [87.9–90.2] | 88.4 [87.2–89.6] | -0.3 [-1.2–0.6] | 4.0 [3.0–5.1] | 46 [42–51] | 75.8 [73.7–77.9] | 76.8 [74.9–78.7] | 91.0 [89.5–92.5] | 91.5 [90.1–92.9] | 90.2 [88.9–91.5] | 90.7 [89.5–92.0] | 2.4 [1.8–3.0] | 2.0 [1.4–2.6] | 0.0% | 1.3% | 0.0% | 2.9% | ESR (VSE\* WME\*), STAR (VSE\* WME\*), ARA (VSE\* WME\*) +12 more |
| Soft Minimax-D | hybrid | 89.2 [88.1–90.3] | 96.2 [95.7–96.8] | 3.1 [2.4–3.9] | 1.7 [1.0–2.4] | 70 [65–75] | 100.0 [100.0–100.0] | 91.5 | 92.1 | 89.2 [88.1–90.3] | 96.3 [95.7–96.9] | 3.2 [2.5–3.9] | 1.6 [1.0–2.3] | 70 [66–76] | 88.0 [86.5–89.5] | 87.8 [86.3–89.3] | 98.4 [97.8–99.0] | 98.2 [97.4–98.8] | 97.2 [96.4–97.9] | 96.9 [96.2–97.7] | 1.6 [1.1–2.1] | 1.6 [1.1–2.1] | 0.0% | 0.1% | 0.0% | 1.9% | — |
| Smith//Score | condorcet-based | 89.2 [88.1–90.3] | 95.9 [95.2–96.4] | 3.0 [2.3–3.7] | 1.9 [1.2–2.6] | 68 [63–73] | 100.0 [100.0–100.0] | 91.2 | 92.0 | 89.2 [88.0–90.3] | 96.2 [95.6–96.7] | 3.1 [2.4–3.8] | 1.7 [1.1–2.4] | 69 [65–74] | 88.0 [86.5–89.5] | 87.5 [86.0–89.0] | 98.5 [97.9–99.1] | 97.7 [97.0–98.5] | 97.2 [96.5–98.0] | 96.6 [95.9–97.4] | 1.8 [1.2–2.5] | 1.9 [1.4–2.6] | 0.0% | 0.1% | 0.0% | 3.0% | Soft Minimax (WME\*) |
| BTR-score | hybrid | 88.9 [87.7–90.0] | 95.9 [95.3–96.5] | 2.8 [2.1–3.5] | 2.0 [1.4–2.7] | 65 [62–71] | 100.0 [100.0–100.0] | 91.2 | 92.0 | 88.9 [87.7–90.0] | 96.4 [95.8–96.9] | 3.0 [2.3–3.7] | 1.8 [1.1–2.5] | 68 [64–74] | 88.2 [86.7–89.6] | 87.4 [85.9–88.8] | 98.5 [97.9–99.1] | 97.5 [96.7–98.3] | 97.4 [96.8–98.0] | 96.5 [95.6–97.2] | 1.8 [1.2–2.4] | 1.9 [1.4–2.6] | 0.0% | 0.2% | 0.0% | 2.8% | Soft Minimax (WME\*), Soft Minimax-D (WME\*) |
| MJ | median-based | 88.8 [87.7–89.8] | 89.4 [88.3–90.5] | -0.1 [-0.9–0.8] | 4.2 [3.2–5.1] | 45 [42–50] | 100.0 [100.0–100.0] | 90.2 | 92.2 | 89.3 [88.2–90.3] | 91.2 [90.2–92.1] | 1.0 [0.2–1.8] | 3.3 [2.5–4.2] | 51 [47–57] | 77.3 [75.3–79.2] | 74.8 [72.7–76.8] | 90.6 [89.2–92.2] | 87.5 [85.8–89.2] | 90.6 [89.4–92.0] | 88.2 [86.9–89.7] | 1.9 [1.4–2.5] | 1.8 [1.2–2.4] | 0.0% | 0.2% | 0.0% | 7.2% | ESR (VSE\* WME\*), STAR (VSE\* WME\*), Score runoff (VSE\* WME\*) +12 more |
| Approval cond. runoff | two-round | 88.7 [87.5–89.9] | 88.8 [87.7–89.9] | -0.4 [-1.2–0.6] | 4.3 [3.2–5.3] | 44 [41–50] | 99.9 [99.9–99.9] | 67.2 | 67.8 | 89.2 [88.0–90.3] | 88.4 [87.2–89.6] | -0.3 [-1.2–0.6] | 3.9 [2.9–5.0] | 46 [42–52] | 75.4 [73.5–77.4] | 76.3 [74.6–78.3] | 90.8 [89.4–92.3] | 90.8 [89.3–92.3] | 90.2 [89.0–91.6] | 90.4 [89.1–91.7] | 2.3 [1.7–2.9] | 2.3 [1.7–3.0] | 0.0% | 0.9% | 0.0% | 3.4% | ESR (VSE\* WME\*), STAR (VSE\* WME\*), Score runoff (VSE\* WME\*) +14 more |
| Soft Minimax | hybrid | 88.7 [87.4–89.8] | 96.5 [95.9–97.0] | 2.9 [2.2–3.6] | 1.9 [1.2–2.5] | 68 [64–73] | 100.0 [100.0–100.0] | 91.5 | 92.1 | 88.8 [87.4–89.9] | 96.6 [96.1–97.2] | 3.0 [2.3–3.7] | 1.8 [1.1–2.4] | 70 [65–75] | 89.0 [87.6–90.3] | 88.6 [87.2–89.9] | 98.7 [98.0–99.2] | 98.2 [97.6–98.9] | 97.2 [96.5–97.9] | 96.9 [96.1–97.6] | 1.6 [1.1–2.1] | 1.6 [1.1–2.1] | 0.0% | 0.1% | 0.0% | 1.9% | Soft Minimax-D (VSE\*) |
| IMDS | rated | 88.6 [87.4–89.8] | 95.2 [94.5–95.9] | 2.3 [1.6–3.1] | 2.5 [1.7–3.2] | 61 [57–65] | 100.0 [100.0–100.0] | 89.5 | 92.1 | 89.0 [87.8–90.1] | 96.1 [95.5–96.7] | 2.9 [2.3–3.6] | 1.9 [1.2–2.6] | 67 [63–73] | 88.6 [87.2–90.1] | 86.7 [85.1–88.2] | 98.3 [97.7–99.0] | 97.1 [96.2–97.9] | 96.7 [95.9–97.5] | 94.9 [94.0–95.8] | 1.3 [0.8–1.8] | 1.8 [1.2–2.4] | 0.0% | 0.2% | 0.0% | 10.1% | Soft Minimax-D (VSE\* WME\*), Smith//Score (WME\*), BTR-score (WME\*) +3 more |
| Approval | approval-based | 88.2 [87.0–89.5] | 85.0 [83.6–86.4] | -2.3 [-3.2–-1.2] | 5.2 [3.9–6.3] | 40 [36–44] | 100.0 [100.0–100.0] | 68.0 | 68.4 | 88.3 [86.9–89.6] | 85.3 [84.0–86.6] | -2.1 [-3.1–-1.0] | 5.1 [3.8–6.4] | 40 [36–45] | 70.1 [68.0–72.0] | 69.9 [67.8–71.8] | 86.9 [85.1–88.6] | 85.9 [84.1–87.7] | 86.9 [85.4–88.4] | 85.8 [84.2–87.3] | 3.1 [2.4–3.9] | 2.9 [2.2–3.6] | 0.0% | 0.3% | 0.0% | 4.5% | Approval runoff (VSE\* WME\*), Score (VSE\* WME\*), MAV (VSE\* WME\*) +19 more |
| ER-IRV | plurality-based | 87.2 [85.8–88.5] | 89.7 [88.4–90.8] | -0.9 [-1.7–-0.0] | 5.3 [4.2–6.4] | 40 [37–44] | 99.6 [99.5–99.6] | 73.6 | 74.5 | 87.6 [86.3–88.8] | 91.0 [89.9–92.0] | -0.1 [-0.9–0.6] | 4.7 [3.7–5.7] | 44 [40–47] | 83.1 [81.4–84.7] | 80.7 [78.9–82.4] | 92.8 [91.5–94.1] | 90.7 [89.1–92.1] | 90.7 [89.5–92.1] | 88.9 [87.5–90.4] | 1.8 [1.2–2.4] | 1.8 [1.2–2.4] | 1.4% | 4.9% | 0.2% | 0.6% | Smith//Score (VSE\* WME\*), IMDS (VSE\* WME\*), ESR (VSE\* WME\*) +16 more |
| IRV | plurality-based | 86.2 [84.8–87.4] | 88.0 [86.6–89.1] | -2.2 [-3.1–-1.3] | 6.5 [5.4–7.6] | 35 [32–39] | 99.1 [99.1–99.2] | 73.2 | 74.5 | 87.6 [86.3–88.8] | 91.0 [89.9–92.0] | -0.1 [-0.9–0.6] | 4.7 [3.7–5.7] | 44 [40–47] | 83.1 [81.4–84.7] | 79.2 [77.3–80.8] | 92.8 [91.5–94.1] | 89.2 [87.7–90.8] | 90.7 [89.5–92.1] | 87.2 [85.8–88.6] | 1.8 [1.2–2.4] | 2.0 [1.4–2.6] | 4.7% | 3.0% | 1.0% | 0.2% | Smith//Score (VSE\* WME\*), Approval runoff (VSE\* WME\*), Score (VSE\* WME\*) +23 more |
| MMWV | minimax-family | 85.2 [83.7–86.5] | 99.2 [98.9–99.4] | 2.1 [1.6–2.7] | 0.8 [0.6–1.1] | 85 [81–89] | 99.9 [99.9–99.9] | 73.5 | 73.7 | 85.1 [83.7–86.4] | 99.5 [99.3–99.6] | 2.4 [1.8–2.9] | 0.5 [0.3–0.7] | 90 [87–93] | 97.6 [96.9–98.2] | 96.6 [95.9–97.4] | 98.8 [98.2–99.3] | 98.2 [97.5–98.8] | 97.3 [96.6–98.0] | 96.6 [95.8–97.3] | 2.5 [1.9–3.1] | 2.5 [1.9–3.2] | 0.1% | 1.5% | 0.0% | 0.1% | — |
| MMPO | minimax-family | 85.1 [83.7–86.4] | 99.1 [98.8–99.4] | 2.1 [1.5–2.7] | 0.9 [0.6–1.1] | 84 [80–88] | 99.9 [99.9–99.9] | 73.5 | 73.7 | 85.1 [83.7–86.4] | 99.5 [99.3–99.6] | 2.4 [1.8–2.9] | 0.5 [0.3–0.7] | 90 [87–93] | 97.6 [96.9–98.2] | 96.7 [95.9–97.4] | 98.8 [98.2–99.3] | 98.2 [97.5–98.9] | 97.3 [96.6–98.0] | 96.6 [95.8–97.3] | 2.5 [1.9–3.1] | 2.5 [1.9–3.3] | 0.1% | 1.5% | 0.0% | 0.1% | — |
| Copeland | condorcet | 85.1 [83.7–86.3] | 98.9 [98.6–99.1] | 1.9 [1.4–2.5] | 1.1 [0.8–1.4] | 81 [77–85] | 99.8 [99.8–99.8] | 73.5 | 73.7 | 85.1 [83.7–86.4] | 99.3 [99.1–99.5] | 2.2 [1.7–2.8] | 0.7 [0.5–0.9] | 87 [84–89] | 97.6 [96.9–98.3] | 96.1 [95.2–96.9] | 98.8 [98.2–99.3] | 98.1 [97.4–98.7] | 96.8 [96.0–97.5] | 96.0 [95.1–96.8] | 2.4 [1.7–3.0] | 2.4 [1.8–3.0] | 0.8% | 0.9% | 0.0% | 0.2% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +2 more |
| Kemeny-Young | condorcet | 85.1 [83.6–86.4] | 99.3 [99.1–99.5] | 2.2 [1.7–2.9] | 0.7 [0.4–0.9] | 88 [84–91] | 99.9 [99.9–99.9] | 73.5 | 73.8 | 85.2 [83.7–86.6] | 99.5 [99.3–99.6] | 2.4 [1.8–3.0] | 0.5 [0.3–0.7] | 90 [87–93] | 97.6 [96.9–98.3] | 96.9 [96.1–97.6] | 98.9 [98.3–99.4] | 98.4 [97.7–99.0] | 97.4 [96.7–98.1] | 96.8 [96.0–97.6] | 2.6 [2.0–3.4] | 2.6 [2.0–3.4] | 0.6% | 0.8% | 0.0% | 0.1% | — |
| Minimax | minimax-family | 84.9 [83.5–86.3] | 99.2 [99.0–99.5] | 2.1 [1.6–2.7] | 0.8 [0.5–1.0] | 86 [82–90] | 99.9 [99.8–99.9] | 73.5 | 73.7 | 85.1 [83.7–86.4] | 99.5 [99.3–99.6] | 2.4 [1.8–2.9] | 0.5 [0.3–0.7] | 90 [87–93] | 97.6 [96.9–98.2] | 96.9 [96.2–97.6] | 98.8 [98.2–99.3] | 98.3 [97.7–98.9] | 97.3 [96.6–98.0] | 96.8 [96.0–97.5] | 2.5 [1.9–3.1] | 2.5 [1.9–3.3] | 0.8% | 0.8% | 0.0% | 0.2% | — |
| Schulze | condorcet | 84.9 [83.5–86.3] | 99.2 [99.0–99.5] | 2.1 [1.6–2.7] | 0.8 [0.5–1.0] | 86 [82–90] | 99.9 [99.8–99.9] | 73.5 | 73.7 | 85.1 [83.7–86.4] | 99.5 [99.3–99.6] | 2.4 [1.8–2.9] | 0.5 [0.3–0.7] | 90 [87–93] | 97.6 [96.9–98.2] | 96.9 [96.2–97.6] | 98.8 [98.2–99.3] | 98.3 [97.7–98.9] | 97.3 [96.6–98.0] | 96.8 [95.9–97.5] | 2.5 [1.9–3.1] | 2.5 [1.9–3.3] | 0.8% | 0.8% | 0.0% | 0.2% | — |
| Ranked pairs | condorcet | 84.9 [83.4–86.1] | 99.3 [99.0–99.4] | 2.1 [1.6–2.7] | 0.7 [0.5–1.0] | 86 [83–90] | 99.9 [99.8–99.9] | 73.6 | 73.8 | 85.1 [83.6–86.4] | 99.5 [99.3–99.6] | 2.4 [1.8–2.9] | 0.5 [0.3–0.7] | 90 [87–93] | 97.6 [96.9–98.2] | 96.8 [96.0–97.5] | 98.8 [98.3–99.3] | 98.3 [97.6–98.9] | 97.4 [96.6–98.0] | 96.7 [95.9–97.4] | 2.5 [1.9–3.2] | 2.6 [1.9–3.3] | 0.6% | 0.8% | 0.1% | 0.2% | — |
| IRV-3 | plurality-based | 84.7 [83.1–86.1] | 83.2 [81.7–84.6] | -5.1 [-6.4–-3.9] | 8.5 [7.2–10.0] | 29 [26–32] | 99.2 [99.1–99.2] | 71.6 | 72.8 | 88.0 [86.8–89.1] | 87.3 [86.1–88.4] | -1.4 [-2.3–-0.5] | 5.1 [4.1–6.1] | 40 [37–44] | 74.5 [72.5–76.2] | 71.2 [69.0–73.1] | 87.7 [85.9–89.3] | 83.6 [81.6–85.3] | 87.3 [85.8–88.8] | 83.2 [81.4–84.6] | 2.2 [1.6–2.9] | 3.3 [2.5–4.1] | 4.4% | 1.3% | 1.1% | 0.1% | Smith//Score (VSE\* WME\*), IRV (VSE\* WME\*), ER-IRV (VSE\* WME\*) +31 more |
| BTR-IRV | condorcet-based | 84.6 [83.1–86.0] | 97.8 [97.3–98.2] | 1.2 [0.5–1.8] | 2.2 [1.7–2.6] | 69 [64–73] | 99.7 [99.7–99.7] | 73.3 | 73.8 | 85.1 [83.7–86.4] | 99.1 [98.9–99.4] | 2.0 [1.5–2.6] | 0.9 [0.6–1.1] | 85 [81–88] | 97.4 [96.7–98.1] | 93.7 [92.7–94.7] | 98.7 [98.1–99.2] | 96.9 [95.9–97.7] | 96.6 [95.7–97.3] | 94.2 [93.0–95.1] | 2.5 [1.9–3.2] | 2.6 [1.9–3.3] | 1.7% | 1.4% | 0.2% | 0.5% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +4 more |
| TTR | two-round | 84.5 [82.9–85.9] | 86.7 [85.4–88.1] | -3.7 [-4.7–-2.7] | 8.1 [6.8–9.3] | 31 [29–33] | 98.8 [98.7–98.9] | 60.7 | 61.7 | 85.6 [84.2–86.8] | 86.6 [85.3–88.0] | -3.1 [-4.0–-2.1] | 7.2 [6.1–8.4] | 32 [30–36] | 77.9 [76.3–79.9] | 78.2 [76.4–80.0] | 86.8 [85.1–88.4] | 86.6 [84.8–88.2] | 84.9 [83.5–86.5] | 84.7 [83.1–86.3] | 2.3 [1.7–2.9] | 2.7 [2.0–3.5] | 3.6% | 0.0% | 0.9% | 0.0% | Smith//Score (VSE\* WME\* sincerity), Approval runoff (VSE\* WME\* sincerity), Approval cond. runoff (VSE\* WME\* sincerity) +29 more |
| Asset | delegated | 84.1 [82.6–85.5] | 80.8 [79.3–82.2] | -6.4 [-7.7–-5.1] | 9.3 [7.9–10.7] | 28 [25–30] | 99.2 [99.1–99.2] | 62.7 | 63.5 | 85.3 [83.9–86.6] | 81.2 [79.6–82.7] | -5.5 [-6.7–-4.0] | 8.1 [6.8–9.5] | 30 [27–33] | 66.5 [64.2–68.6] | 66.0 [63.9–68.1] | 75.7 [73.2–77.8] | 74.9 [72.6–77.0] | 76.0 [74.1–78.0] | 75.4 [73.6–77.3] | 2.1 [1.5–2.9] | 2.5 [1.8–3.2] | 2.9% | 0.0% | 0.5% | 0.0% | Smith//Score (VSE\* WME\*), IRV (VSE\* WME\*), ER-IRV (VSE\* WME\*) +32 more |
| Borda | positional | 83.6 [82.2–84.9] | 90.6 [89.6–91.7] | -2.5 [-3.6–-1.5] | 7.3 [6.3–8.3] | 35 [32–38] | 99.3 [99.3–99.3] | 71.7 | 73.5 | 84.3 [82.9–85.6] | 94.7 [93.9–95.3] | -0.4 [-1.2–0.5] | 4.7 [3.9–5.4] | 48 [45–52] | 82.7 [81.0–84.3] | 77.3 [75.6–79.4] | 86.9 [85.2–88.8] | 83.2 [81.3–85.1] | 87.2 [85.9–88.7] | 83.9 [82.4–85.4] | 1.6 [1.0–2.1] | 2.2 [1.6–2.9] | 4.5% | 4.1% | 0.1% | 5.4% | Kemeny-Young (VSE\* WME\*), Copeland (VSE\* WME\*), MMWV (VSE\* WME\*) +24 more |
| Supplementary vote | plurality-based | 82.2 [80.6–83.9] | 77.2 [75.6–78.9] | -8.5 [-10.1–-6.7] | 11.2 [9.5–12.8] | 24 [21–26] | 99.5 [99.4–99.5] | 74.7 | 75.4 | 82.8 [81.2–84.5] | 78.1 [76.6–79.8] | -7.9 [-9.4–-6.2] | 10.6 [8.9–12.2] | 25 [22–28] | 62.9 [60.9–64.9] | 62.0 [59.9–64.1] | 75.9 [73.9–78.1] | 74.5 [72.3–76.9] | 77.0 [75.2–78.8] | 76.0 [74.3–77.9] | 5.1 [4.1–6.0] | 5.0 [4.0–5.9] | 3.4% | 2.7% | 0.3% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +33 more |
| Plurality | plurality-based | 73.0 [70.7–75.4] | 63.6 [61.6–65.6] | -17.7 [-19.9–-15.4] | 20.4 [18.1–22.7] | 13 [12–15] | 99.2 [99.2–99.3] | 61.6 | 62.4 | 68.6 [66.0–71.1] | 61.9 [59.9–63.9] | -22.1 [-24.7–-19.6] | 24.8 [22.3–27.4] | 11 [9.6–12] | 47.1 [44.9–49.3] | 48.7 [46.6–51.0] | 57.4 [54.8–59.8] | 59.4 [56.8–61.9] | 60.6 [58.5–62.8] | 62.6 [60.6–64.8] | 10.0 [8.6–11.3] | 7.9 [6.7–9.1] | 5.8% | 0.0% | 0.2% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +34 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Borda | 14.2% |
| Plurality | 11.2% |
| Score | 10.6% |
| IMDS | 10.6% |
| ESR | 10.4% |
| IRV | 9.2% |
| DMS | 9.0% |
| IRV-3 | 8.8% |
| MJ | 7.5% |
| ESR-MARS | 7.5% |
| ER-IRV | 7.4% |
| Approval | 7.2% |
| TTR | 7.1% |
| MAV | 7.1% |
| Score runoff | 7.0% |
| 321 | 6.8% |
| Delegated 321 | 6.8% |
| Supplementary vote | 6.7% |
| Asset | 6.0% |
| Approval cond. runoff | 5.9% |
| MARS-T | 5.8% |
| MARS | 5.8% |
| STAR | 5.7% |
| Approval runoff | 5.3% |
| ARA | 4.9% |
| Soft Minimax-40 | 4.0% |
| BTR-IRV | 3.9% |
| Smith//Score | 3.3% |
| BTR-score | 3.1% |
| Copeland | 2.1% |
| Soft Minimax | 2.1% |
| Soft Minimax-D | 2.1% |
| Schulze | 1.9% |
| Minimax | 1.9% |
| MMWV | 1.8% |
| MMPO | 1.8% |
| Ranked pairs | 1.7% |
| Kemeny-Young | 1.6% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Plurality | 45.9% | 2.44 | 1.17 | 95% |
| Supplementary vote | 37.9% | 2.46 | 0.93 | 99% |
| TTR | 37.6% | 1.55 | 1.05 | 99% |
| Asset | 30.4% | 0.71 | 0.19 | 99% |
| IRV-3 | 29.4% | 1.51 | 0.61 | 100% |
| Approval | 27.8% | 1.51 | 0.53 | 100% |
| Approval cond. runoff | 27.5% | 1.15 | 0.63 | 100% |
| Approval runoff | 27.2% | 1.07 | 0.63 | 100% |
| IRV | 23.8% | 0.47 | 0.12 | 100% |
| ER-IRV | 23.8% | 0.47 | 0.12 | 100% |
| 321 | 21.3% | 0.84 | 0.52 | 100% |
| Delegated 321 | 21.3% | 0.84 | 0.52 | 100% |
| DMS | 20.7% | 0.79 | 0.11 | 100% |
| MAV | 19.7% | 0.81 | 0.15 | 100% |
| MJ | 18.4% | 0.67 | 0.12 | 100% |
| Score runoff | 16.4% | 0.45 | 0.32 | 100% |
| ESR | 14.1% | 0.67 | 0.12 | 100% |
| Score | 13.5% | 0.69 | 0.15 | 100% |
| Soft Minimax-40 | 13.2% | 0.71 | 0.16 | 100% |
| STAR | 12.8% | 0.58 | 0.26 | 100% |
| ESR-MARS | 12.8% | 0.61 | 0.10 | 100% |
| ARA | 12.6% | 0.60 | 0.16 | 100% |
| IMDS | 11.1% | 0.56 | 0.17 | 100% |
| Soft Minimax | 10.6% | 0.54 | 0.16 | 100% |
| Borda | 10.2% | 0.11 | 0.53 | 100% |
| Soft Minimax-D | 9.7% | 0.49 | 0.11 | 100% |
| MARS | 9.4% | 0.54 | 0.12 | 100% |
| BTR-score | 8.9% | 0.44 | 0.10 | 100% |
| MARS-T | 8.9% | 0.51 | 0.10 | 100% |
| Smith//Score | 8.5% | 0.44 | 0.10 | 100% |
| Copeland | 7.2% | 0.03 | 0.12 | 100% |
| BTR-IRV | 5.4% | 0.07 | 0.07 | 100% |
| Ranked pairs | 5.2% | 0.08 | 0.07 | 99% |
| Schulze | 4.9% | 0.02 | 0.05 | 100% |
| Minimax | 4.9% | 0.02 | 0.05 | 100% |
| MMWV | 4.9% | 0.02 | 0.05 | 100% |
| MMPO | 4.9% | 0.02 | 0.05 | 100% |
| Kemeny-Young | 4.5% | 0.03 | 0.08 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 1906 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 94 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| ESR-MARS | 83.8% (84.8%) | 12.1% (11.2%) | 4.0% (3.9%) | 80.9% (78.7%) | 90.8% (94.6%) | 78.9% |
| STAR | 84.5% (86.0%) | 11.6% (10.8%) | 3.9% (3.1%) | 81.9% (81.9%) | 92.8% (92.2%) | 81.1% |
| 321 | 72.7% (73.9%) | 18.5% (16.4%) | 8.8% (9.8%) | 79.8% (75.5%) | 96.8% (97.0%) | 76.0% |
| Delegated 321 | 72.7% (73.9%) | 18.5% (16.4%) | 8.8% (9.8%) | 79.8% (75.5%) | 96.8% (97.0%) | 76.0% |
| ARA | 84.9% (85.0%) | 10.9% (10.8%) | 4.2% (4.2%) | 72.3% (73.4%) | 93.4% (94.0%) | 83.1% |
| ESR | 84.1% (84.7%) | 12.3% (11.7%) | 3.7% (3.6%) | 81.9% (85.1%) | 91.7% (94.7%) | 79.3% |
| Soft Minimax-40 | 84.5% (84.6%) | 11.9% (11.3%) | 3.7% (4.1%) | 83.0% (83.0%) | 94.4% (95.8%) | 83.5% |
| DMS | 81.9% (82.4%) | 13.4% (12.4%) | 4.7% (5.2%) | 81.9% (77.7%) | 82.0% (86.3%) | 71.6% |
| MAV | 81.7% (82.1%) | 14.1% (13.0%) | 4.1% (4.9%) | 73.4% (73.4%) | 83.4% (83.3%) | 73.3% |
| MARS-T | 84.7% (85.4%) | 12.1% (11.4%) | 3.2% (3.2%) | 83.0% (83.0%) | 95.6% (97.8%) | 84.2% |
| Score | 82.8% (83.3%) | 12.3% (11.6%) | 4.9% (5.0%) | 70.2% (73.4%) | 84.5% (91.5%) | 75.7% |
| MARS | 84.8% (85.7%) | 11.5% (10.8%) | 3.6% (3.5%) | 78.7% (77.7%) | 96.0% (98.3%) | 84.9% |
| Soft Minimax-D | 86.8% (86.9%) | 11.0% (10.8%) | 2.2% (2.3%) | 85.1% (87.2%) | 99.6% (99.8%) | 87.8% |
| Smith//Score | 86.3% (86.9%) | 11.0% (10.7%) | 2.7% (2.4%) | 84.0% (84.0%) | 100.0% (100.0%) | 87.5% |
| BTR-score | 86.0% (86.9%) | 11.2% (10.8%) | 2.8% (2.4%) | 87.2% (87.2%) | 100.0% (100.0%) | 87.4% |
| MJ | 81.6% (82.2%) | 13.8% (12.9%) | 4.6% (4.9%) | 80.9% (74.5%) | 83.4% (83.8%) | 74.8% |
| Soft Minimax | 87.2% (87.6%) | 10.5% (10.0%) | 2.3% (2.4%) | 86.2% (87.2%) | 100.0% (100.0%) | 88.6% |
| IMDS | 85.8% (87.4%) | 11.1% (9.8%) | 3.1% (2.8%) | 90.4% (93.6%) | 99.6% (99.7%) | 86.7% |
| ER-IRV | 92.1% (97.8%) | 5.5% (1.8%) | 2.4% (0.3%) | 41.5% (26.6%) | 85.3% (84.6%) | 80.7% |
| IRV | 91.1% (97.8%) | 7.1% (1.8%) | 1.8% (0.3%) | 54.3% (26.6%) | 84.2% (84.6%) | 79.2% |
| MMWV | 95.6% (97.2%) | 2.4% (1.9%) | 2.0% (0.9%) | 42.6% (42.6%) | 100.0% (100.0%) | 96.6% |
| MMPO | 95.6% (97.2%) | 2.4% (1.9%) | 2.0% (0.9%) | 43.6% (42.6%) | 100.0% (100.0%) | 96.7% |
| Copeland | 94.9% (96.7%) | 2.5% (1.9%) | 2.6% (1.3%) | 46.8% (45.7%) | 100.0% (100.0%) | 96.1% |
| Kemeny-Young | 96.1% (97.3%) | 2.1% (1.9%) | 1.8% (0.8%) | 39.4% (34.0%) | 100.0% (100.0%) | 96.9% |
| Minimax | 95.8% (97.2%) | 2.3% (1.9%) | 1.9% (0.9%) | 43.6% (42.6%) | 100.0% (100.0%) | 96.9% |
| Schulze | 95.7% (97.2%) | 2.3% (1.9%) | 2.0% (0.9%) | 42.6% (42.6%) | 100.0% (100.0%) | 96.9% |
| Ranked pairs | 96.0% (97.2%) | 2.3% (1.9%) | 1.7% (0.9%) | 39.4% (37.2%) | 100.0% (100.0%) | 96.8% |
| IRV-3 | 70.9% (73.6%) | 24.3% (22.7%) | 4.8% (3.7%) | 91.5% (92.6%) | 97.5% (97.9%) | 71.2% |
| BTR-IRV | 93.2% (97.4%) | 4.1% (2.1%) | 2.7% (0.5%) | 70.2% (64.9%) | 100.0% (100.0%) | 93.7% |
| Borda | 89.9% (98.4%) | 4.6% (1.3%) | 5.5% (0.3%) | 33.0% (9.6%) | 80.6% (83.1%) | 77.3% |
| Supplementary vote | 82.6% (85.9%) | 15.5% (13.6%) | 1.8% (0.5%) | 54.3% (50.0%) | 75.1% (74.5%) | 62.0% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| ESR-MARS | 0.4% (0.1%) | 30.9% (29.7%) | 1.0% (1.0%) |
| STAR | 0.5% (0.1%) | 33.6% (32.1%) | 1.1% (1.1%) |
| 321 | 0.7% (0.3%) | 50.0% (50.9%) | 1.8% (1.9%) |
| Delegated 321 | 0.7% (0.3%) | 50.0% (50.9%) | 1.8% (1.9%) |
| ARA | 1.4% (0.2%) | 30.7% (30.0%) | 1.0% (1.0%) |
| ESR | 0.4% (0.1%) | 31.5% (28.7%) | 1.1% (1.0%) |
| Soft Minimax-40 | 0.7% (0.3%) | 29.0% (29.2%) | 1.0% (1.0%) |
| DMS | 0.0% (0.0%) | 29.5% (28.2%) | 0.9% (0.9%) |
| MAV | 0.2% (0.1%) | 30.6% (28.7%) | 1.0% (0.9%) |
| Score runoff | 0.4% (0.0%) | — (—) | — (—) |
| MARS-T | 0.1% (0.0%) | 29.9% (29.6%) | 1.0% (1.0%) |
| Score | 0.7% (0.1%) | 33.4% (30.1%) | 1.1% (1.0%) |
| MARS | 0.4% (0.0%) | 30.0% (30.0%) | 1.0% (1.0%) |
| Approval runoff | 1.5% (0.5%) | — (—) | — (—) |
| Soft Minimax-D | 0.1% (0.0%) | 29.0% (28.6%) | 0.9% (0.9%) |
| Smith//Score | 0.0% (0.0%) | 29.6% (29.2%) | 0.9% (0.9%) |
| BTR-score | 0.0% (0.0%) | 29.8% (29.1%) | 0.9% (0.9%) |
| MJ | 0.0% (0.0%) | 30.3% (29.0%) | 1.0% (0.9%) |
| Approval cond. runoff | 1.4% (0.5%) | — (—) | — (—) |
| Soft Minimax | 0.9% (0.6%) | 29.9% (30.1%) | 1.0% (1.0%) |
| IMDS | 1.4% (0.6%) | 32.4% (29.8%) | 1.0% (1.0%) |
| Approval | 2.2% (0.8%) | — (—) | — (—) |
| ER-IRV | 2.1% (1.9%) | 4.1% (0.0%) | 0.1% (0.0%) |
| IRV | 2.5% (1.9%) | 2.0% (0.0%) | 0.1% (0.0%) |
| MMWV | 1.7% (1.3%) | 2.4% (0.0%) | 0.1% (0.0%) |
| MMPO | 1.7% (1.3%) | 2.4% (0.0%) | 0.1% (0.0%) |
| Copeland | 2.6% (2.0%) | 3.0% (0.0%) | 0.1% (0.0%) |
| Kemeny-Young | 1.1% (0.9%) | 1.6% (0.0%) | 0.1% (0.0%) |
| Minimax | 1.5% (1.3%) | 1.2% (0.0%) | 0.0% (0.0%) |
| Schulze | 1.6% (1.3%) | 1.4% (0.0%) | 0.0% (0.0%) |
| Ranked pairs | 1.4% (1.3%) | 1.7% (0.0%) | 0.1% (0.0%) |
| IRV-3 | 2.2% (0.7%) | 41.9% (43.9%) | 1.4% (1.5%) |
| BTR-IRV | 0.6% (0.1%) | 1.6% (0.0%) | 0.1% (0.0%) |
| TTR | 1.2% (1.1%) | — (—) | — (—) |
| Asset | 3.6% (2.4%) | — (—) | — (—) |
| Borda | 0.7% (0.7%) | 14.5% (0.0%) | 0.4% (0.0%) |
| Supplementary vote | 1.4% (1.0%) | 0.1% (0.0%) | 0.0% (0.0%) |
| Plurality | 1.9% (1.1%) | — (—) | — (—) |

![strategy arrows](arrows-c4.png)

![strategy shifts](shift-c4.png)

![sincerity](sincerity-c4.png)

![fidelity](fidelity-c4.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c4.png)

Scatter and chain plots leave out Condorcet-random-drop, Geometric median, Random ballot, Random pair runoff, Anti-plurality, TRASH, CW else Score loser, CL else Score winner, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
