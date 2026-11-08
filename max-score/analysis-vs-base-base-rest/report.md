# Ballot Front Finder Lab — analysis

Input: base, base-rest, max-score (simulator version 2.1.0; analysis version 2.1.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

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

![sweep over max score](chain-max_score.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis); for conditions with fewer elections than the base, a further row gives the agreement expected between the base and a run of that size. A condition near or above its row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 0.92 | 0.99 | 0.87 | 0.99 | 0.97 |
| noise reference: c1 vs a run of 2000 elections | 0.88 | 0.99 | 0.82 | 0.99 | 0.96 |
| c2: max score 99 | 0.81 | 0.93 | 0.70 | 0.94 | 0.94 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 97.5% | 69.9% | 14.9% | 8.7 |
| c2: max score 99 | 97.9% | 69.8% | 14.2% | 8.7 |

## c1: base

4000 elections (seeds 1), 3900 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 97.5%; it also has the highest total utility in 69.9% of those; electing it loses 14.9% of VSE; favourite ÷ least liked 8.7 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 83.9 to 84.5; with the best VSE (100) WME 79.5 (nothing lower is possible there either); the most balanced point VSE 95.1 / WME 96.2. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 91.0 (at WME 92.5); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 90.9 (at WME 92.4); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -53.4 / WME -48.1. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 83.9 / WME 99.8. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 87.6 / 96.3; normalized utility winner 91.0 / 92.5; best worst margin 84.2 / 100.0; Condorcet winner, else random ballot 83.0 / 98.8; Condorcet winner, else utility winner 85.4 / 99.6; random pair, majority 15.4 / 2.6; nearest the median voter 88.7 / 96.9; nearest the mean voter 75.2 / 92.3.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): ESR-MARS, Score runoff, ARA, Soft Minimax-40, MARS, Soft Minimax-D, BTR-score, MMWVxScore, Soft Minimax, MMWV+Score, Black, MMPOWV, MMWV, Ranked pairs, Schulze.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): ARA, Soft Minimax-40, Soft Minimax-D, BTR-score, MMWVxScore, Soft Minimax, MMWV, Ranked pairs, Schulze, Minimax, Schulze-ER, Condorcet-random-drop, Minimax-ER.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 83.0 / 98.8; its mirror on the utility side 89.9 / 91.3; random pair 15.4 / 2.6. No better than a lottery of them (Lead ≤ 0): with learned ballots Geometric median, MAV, 321, Delegated 321, DMS, MJ, Score, STLR, Approval runoff, Approval cond. runoff, IDS, Approval, ARA-Top, MDDA, MAMPO, Reciprocal score, DAC, DSC, ER-IRV, MCA, Supplementary vote, TTR, Dowdall, IRV-3, IRV, Coombs, Asset, Borda-ER, Borda, Asset-foresight, Plurality, CW else Score loser, Random pair runoff, Anti-plurality, Random ballot, TRASH, CL else Score winner; with honest ballots MAV, Approval runoff, Approval cond. runoff, IDS, Approval, ARA-Top, MDDA, MAMPO, Reciprocal score, DAC, DSC, ER-IRV, MCA, Supplementary vote, TTR, Dowdall, IRV-3, IRV, Asset, Borda-ER, Borda, Asset-foresight, Plurality, CW else Score loser, Random pair runoff, Anti-plurality, Random ballot, TRASH, CL else Score winner.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ESR-MARS | hybrid | 87.9 [87.0–88.8] | 94.8 [94.2–95.4] | 0.6 [0.2–1.1] | 1.9 [1.2–2.6] | 64 [60–69] | 100.0 [100.0–100.0] | 93.0 | 94.3 | 87.9 [86.9–88.9] | 96.5 [96.0–96.9] | 1.4 [1.0–1.8] | 1.2 [0.6–1.8] | 73 [69–79] | 90.2 [89.3–91.1] | 88.0 [86.9–89.1] | 98.5 [98.1–98.9] | 96.2 [95.6–96.9] | 97.7 [97.2–98.2] | 95.7 [95.0–96.3] | 3.4 [2.8–3.9] | 3.4 [2.9–4.0] | 0.0% | 0.2% | 0.0% | 5.5% | STAR (WME\*), Score runoff (WME\*), ARA (WME\*) +2 more |
| Score runoff | two-round | 87.7 [86.7–88.7] | 95.9 [95.4–96.4] | 1.1 [0.7–1.5] | 1.5 [0.9–2.1] | 69 [64–74] | 99.9 [99.9–99.9] | 92.6 | 94.2 | 88.2 [87.1–89.1] | 96.4 [96.0–96.8] | 1.5 [1.1–1.9] | 1.1 [0.5–1.7] | 75 [70–80] | 90.2 [89.3–91.1] | 89.5 [88.6–90.5] | 99.4 [99.1–99.6] | 98.2 [97.7–98.6] | 98.5 [98.2–98.9] | 97.3 [96.8–97.8] | 3.3 [2.7–3.8] | 3.6 [3.0–4.1] | 0.0% | 1.3% | 0.0% | 3.3% | MARS (WME\*), ARA (WME\*), Soft Minimax-40 (WME\*) |
| Soft Minimax-L | hybrid | 87.7 [86.6–88.6] | 95.8 [95.2–96.3] | 1.0 [0.5–1.4] | 1.7 [1.0–2.3] | 67 [63–72] | 100.0 [100.0–100.0] | 93.9 | 94.3 | 87.8 [86.8–88.7] | 96.2 [95.8–96.7] | 1.3 [0.8–1.7] | 1.4 [0.8–2.0] | 71 [66–76] | 89.6 [88.7–90.6] | 89.2 [88.2–90.1] | 98.3 [97.8–98.7] | 97.7 [97.2–98.2] | 97.5 [97.0–98.0] | 96.9 [96.4–97.4] | 3.4 [2.8–3.9] | 3.5 [2.9–4.1] | 0.0% | 0.1% | 0.0% | 1.4% | MARS (WME\*), MARS-T (WME\*), ARA (WME\*) +1 more |
| Geometric median | references | 87.7 [86.7–88.7] | 93.1 [92.4–93.7] | -0.3 [-0.8–0.2] | 2.7 [1.8–3.4] | 57 [53–62] | 100.0 [100.0–100.0] | 92.7 | 94.5 | 87.8 [86.8–88.8] | 93.8 [93.1–94.4] | 0.1 [-0.4–0.6] | 2.4 [1.5–3.0] | 60 [56–65] | 85.8 [84.7–86.8] | 85.0 [83.8–86.0] | 95.5 [94.8–96.2] | 94.5 [93.7–95.3] | 95.3 [94.7–96.0] | 94.4 [93.7–95.1] | 3.9 [3.3–4.5] | 3.9 [3.3–4.5] | 0.0% | 0.1% | 0.0% | 7.9% | STAR (WME\*), Score runoff (WME\*), MARS (WME\*) +7 more |
| ARA | hybrid | 87.6 [86.6–88.6] | 96.4 [96.0–96.8] | 1.2 [0.8–1.7] | 1.4 [0.8–2.0] | 71 [66–76] | 100.0 [100.0–100.0] | 93.4 | 94.3 | 87.8 [86.8–88.7] | 96.9 [96.5–97.3] | 1.6 [1.2–2.0] | 1.1 [0.5–1.7] | 76 [72–81] | 90.7 [89.8–91.7] | 89.9 [88.9–90.8] | 98.0 [97.6–98.5] | 97.4 [96.9–98.0] | 97.5 [97.0–98.0] | 96.8 [96.3–97.3] | 3.2 [2.7–3.8] | 3.3 [2.8–3.9] | 0.0% | 0.1% | 0.0% | 3.7% | — |
| MAV | median-based | 87.5 [86.6–88.5] | 93.1 [92.4–93.8] | -0.4 [-0.9–0.2] | 2.8 [2.0–3.5] | 56 [52–60] | 100.0 [100.0–100.0] | 93.4 | 94.5 | 87.6 [86.7–88.6] | 93.5 [92.8–94.1] | -0.1 [-0.7–0.4] | 2.6 [1.8–3.3] | 58 [54–62] | 85.8 [84.7–86.9] | 85.3 [84.2–86.4] | 94.6 [93.8–95.4] | 93.8 [93.0–94.7] | 94.3 [93.6–95.0] | 93.8 [93.0–94.5] | 4.0 [3.4–4.7] | 3.9 [3.3–4.5] | 0.0% | 0.1% | 0.0% | 5.0% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +9 more |
| 321 | hybrid | 87.4 [86.5–88.4] | 93.3 [92.6–93.9] | -0.4 [-1.0–0.2] | 2.8 [2.0–3.5] | 56 [52–61] | 100.0 [100.0–100.0] | 85.6 | 86.7 | 88.0 [87.0–89.0] | 94.5 [94.0–95.1] | 0.6 [0.1–1.1] | 2.0 [1.3–2.6] | 63 [59–69] | 87.4 [86.4–88.4] | 86.0 [84.9–87.0] | 96.4 [95.8–97.0] | 94.7 [93.9–95.5] | 95.5 [94.8–96.1] | 94.0 [93.3–94.7] | 3.3 [2.8–3.9] | 3.4 [2.9–4.0] | 0.0% | 0.5% | 0.0% | 4.0% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +9 more |
| Delegated 321 | delegated | 87.4 [86.5–88.4] | 93.3 [92.6–93.9] | -0.4 [-1.0–0.2] | 2.8 [2.0–3.5] | 56 [52–61] | — | — | — | 88.0 [87.0–89.0] | 94.5 [94.0–95.1] | 0.6 [0.1–1.1] | 2.0 [1.3–2.6] | 63 [59–69] | 87.4 [86.4–88.4] | 86.0 [84.9–87.0] | 96.4 [95.8–97.0] | 94.7 [93.9–95.5] | 95.5 [94.8–96.1] | 94.0 [93.3–94.7] | 3.3 [2.8–3.9] | 3.4 [2.9–4.0] | 0.0% | 0.5% | 0.0% | 4.0% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +9 more |
| Soft Minimax-40 | hybrid | 87.4 [86.4–88.4] | 96.5 [96.1–97.0] | 1.2 [0.8–1.6] | 1.5 [0.8–2.1] | 70 [66–75] | 100.0 [100.0–100.0] | 93.6 | 94.3 | 87.3 [86.3–88.3] | 96.9 [96.5–97.3] | 1.3 [1.0–1.7] | 1.4 [0.7–1.9] | 72 [69–77] | 91.2 [90.3–92.1] | 90.6 [89.7–91.5] | 98.5 [98.1–98.9] | 97.9 [97.5–98.4] | 97.5 [97.0–97.9] | 97.0 [96.5–97.5] | 3.1 [2.5–3.6] | 3.0 [2.5–3.5] | 0.0% | 0.2% | 0.0% | 2.6% | — |
| STAR | hybrid | 87.4 [86.4–88.4] | 95.6 [95.1–96.2] | 0.8 [0.3–1.2] | 1.9 [1.2–2.5] | 64 [61–69] | 99.9 [99.9–100.0] | 93.2 | 94.3 | 87.9 [87.0–88.9] | 96.3 [95.9–96.8] | 1.4 [1.0–1.8] | 1.3 [0.7–1.9] | 72 [68–77] | 90.0 [89.1–90.9] | 88.9 [88.0–89.9] | 98.9 [98.5–99.3] | 97.7 [97.2–98.2] | 98.0 [97.6–98.5] | 97.0 [96.4–97.5] | 3.4 [2.9–4.0] | 3.6 [3.0–4.2] | 0.0% | 0.6% | 0.0% | 3.0% | MARS (WME\*), MARS-T (WME\*), ARA (WME\*) +2 more |
| MARS-T | hybrid | 87.4 [86.3–88.4] | 96.4 [95.9–96.8] | 1.1 [0.7–1.5] | 1.6 [0.9–2.2] | 69 [65–73] | 100.0 [100.0–100.0] | 93.2 | 94.2 | 87.5 [86.5–88.5] | 97.4 [97.0–97.7] | 1.7 [1.3–2.1] | 1.1 [0.4–1.6] | 78 [74–82] | 91.9 [91.1–92.7] | 90.2 [89.3–91.2] | 98.9 [98.5–99.2] | 97.4 [96.9–98.0] | 98.0 [97.5–98.4] | 96.7 [96.1–97.3] | 3.5 [2.9–4.0] | 3.5 [2.9–4.1] | 0.0% | 0.2% | 0.0% | 4.7% | MARS (WME\*) |
| DMS | rated | 87.3 [86.4–88.2] | 92.3 [91.6–93.0] | -0.9 [-1.4–-0.3] | 3.1 [2.3–3.8] | 53 [49–57] | 100.0 [100.0–100.0] | 92.7 | 94.4 | 88.4 [87.5–89.3] | 94.9 [94.4–95.4] | 0.9 [0.5–1.4] | 1.6 [0.9–2.2] | 68 [63–74] | 86.7 [85.7–87.8] | 84.0 [82.9–85.2] | 97.1 [96.5–97.6] | 93.1 [92.3–94.0] | 96.6 [96.0–97.2] | 93.1 [92.3–93.9] | 3.2 [2.7–3.8] | 3.6 [3.0–4.2] | 0.0% | 0.1% | 0.0% | 7.3% | Approval runoff (WME\*), MJ (WME\*), MAV (WME\*) +14 more |
| MARS | hybrid | 87.2 [86.2–88.3] | 96.8 [96.3–97.2] | 1.2 [0.8–1.7] | 1.5 [0.8–2.0] | 71 [67–75] | 100.0 [100.0–100.0] | 93.2 | 94.3 | 87.3 [86.3–88.4] | 97.5 [97.1–97.8] | 1.6 [1.3–2.1] | 1.1 [0.5–1.6] | 78 [74–82] | 92.1 [91.2–92.9] | 90.7 [89.8–91.6] | 98.9 [98.5–99.2] | 97.8 [97.2–98.3] | 98.1 [97.6–98.5] | 97.1 [96.5–97.6] | 3.6 [3.0–4.1] | 3.6 [3.0–4.2] | 0.0% | 0.2% | 0.0% | 4.7% | Soft Minimax-D (WME\*) |
| MJ | median-based | 87.1 [86.2–88.2] | 93.6 [92.9–94.2] | -0.3 [-0.8–0.2] | 2.9 [2.1–3.5] | 55 [52–60] | 100.0 [100.0–100.0] | 93.2 | 94.4 | 87.3 [86.3–88.4] | 94.3 [93.7–94.8] | 0.1 [-0.4–0.6] | 2.5 [1.8–3.1] | 59 [55–63] | 86.7 [85.7–87.8] | 86.0 [85.0–87.1] | 95.0 [94.3–95.8] | 94.1 [93.3–94.9] | 94.8 [94.2–95.6] | 93.9 [93.2–94.7] | 4.2 [3.5–4.8] | 4.1 [3.5–4.7] | 0.0% | 0.1% | 0.0% | 5.3% | ESR-MARS (VSE\* WME\*), ESR (WME\*), STAR (WME\*) +10 more |
| IC-MARS | hybrid | 87.1 [86.1–88.1] | 95.3 [94.7–95.8] | 0.4 [-0.0–0.9] | 2.2 [1.5–2.8] | 62 [58–66] | 100.0 [100.0–100.0] | 92.9 | 94.3 | 87.6 [86.6–88.6] | 97.0 [96.6–97.3] | 1.5 [1.1–2.0] | 1.2 [0.5–1.8] | 75 [71–80] | 90.5 [89.6–91.3] | 87.6 [86.6–88.6] | 98.4 [97.9–98.8] | 94.9 [94.1–95.6] | 97.7 [97.2–98.1] | 94.6 [93.9–95.3] | 3.3 [2.7–3.8] | 3.6 [3.0–4.1] | 0.0% | 0.2% | 0.0% | 5.8% | Score runoff (WME\*), MARS (WME\*), MARS-T (WME\*) +4 more |
| Score | rated | 87.1 [86.1–88.1] | 92.8 [92.1–93.4] | -0.8 [-1.3–-0.2] | 3.2 [2.3–3.9] | 52 [48–57] | 100.0 [100.0–100.0] | 92.4 | 94.3 | 88.2 [87.1–89.1] | 95.7 [95.2–96.1] | 1.2 [0.8–1.6] | 1.4 [0.8–2.0] | 70 [66–76] | 87.8 [86.8–88.8] | 83.7 [82.5–84.8] | 97.1 [96.6–97.7] | 91.9 [91.1–92.9] | 96.7 [96.2–97.3] | 92.3 [91.5–93.1] | 3.1 [2.6–3.7] | 3.6 [3.1–4.3] | 0.0% | 0.1% | 0.0% | 8.5% | ESR-MARS (VSE\* WME\*), Smith//Score (WME\*), MJ (WME\*) +15 more |
| STLR | hybrid | 87.1 [86.1–88.1] | 94.1 [93.5–94.6] | -0.1 [-0.7–0.4] | 2.7 [2.0–3.4] | 57 [53–61] | 100.0 [100.0–100.0] | 92.5 | 94.2 | 87.9 [86.9–88.8] | 95.7 [95.2–96.2] | 1.1 [0.6–1.5] | 1.6 [1.0–2.1] | 68 [64–73] | 87.4 [86.4–88.5] | 85.4 [84.2–86.5] | 96.9 [96.2–97.4] | 93.6 [92.7–94.4] | 96.5 [95.9–97.0] | 93.6 [92.8–94.4] | 3.0 [2.5–3.6] | 3.1 [2.5–3.6] | 0.0% | 0.3% | 0.0% | 6.8% | ESR-MARS (VSE\* WME\*), Smith//Score (WME\*), STAR (WME\*) +14 more |
| Approval runoff | two-round | 86.9 [85.8–87.8] | 93.5 [92.9–94.2] | -0.5 [-1.1–-0.0] | 3.1 [2.3–3.8] | 54 [50–58] | 99.9 [99.9–99.9] | 72.6 | 73.2 | 87.1 [86.1–88.1] | 93.2 [92.6–93.9] | -0.5 [-1.0–0.0] | 3.0 [2.2–3.7] | 54 [50–59] | 86.8 [85.8–87.7] | 87.2 [86.2–88.2] | 95.4 [94.8–96.1] | 95.6 [94.9–96.2] | 94.7 [94.0–95.4] | 94.8 [94.1–95.4] | 4.1 [3.5–4.8] | 4.2 [3.5–4.8] | 0.0% | 1.0% | 0.0% | 1.5% | Score runoff (VSE\* WME\*), ESR-MARS (VSE\* WME\*), Soft Minimax-L (VSE\* WME\*) +18 more |
| ESR | rated | 86.8 [85.8–87.8] | 94.7 [94.1–95.3] | 0.0 [-0.4–0.5] | 2.6 [1.9–3.3] | 58 [55–62] | 100.0 [100.0–100.0] | 92.5 | 94.3 | 87.2 [86.2–88.2] | 96.5 [96.1–97.0] | 1.1 [0.7–1.5] | 1.6 [0.9–2.2] | 69 [65–74] | 91.1 [90.1–92.0] | 88.4 [87.4–89.3] | 98.6 [98.2–99.0] | 95.5 [94.7–96.2] | 97.6 [97.1–98.1] | 94.8 [94.1–95.5] | 3.5 [3.0–4.2] | 3.9 [3.3–4.5] | 0.0% | 0.1% | 0.0% | 8.4% | Score runoff (VSE\* WME\*), ARA (VSE\* WME\*), Soft Minimax-L (VSE\* WME\*) +12 more |
| Soft Minimax-D | hybrid | 86.8 [85.8–87.9] | 97.3 [96.9–97.7] | 1.3 [0.9–1.7] | 1.4 [0.8–1.9] | 73 [69–77] | 100.0 [100.0–100.0] | 93.8 | 94.2 | 86.9 [85.9–87.9] | 97.7 [97.3–98.0] | 1.5 [1.2–1.9] | 1.2 [0.6–1.6] | 77 [73–81] | 93.0 [92.2–93.8] | 92.4 [91.6–93.3] | 98.8 [98.4–99.1] | 98.3 [97.9–98.8] | 97.9 [97.4–98.3] | 97.4 [96.9–97.9] | 3.3 [2.7–3.9] | 3.4 [2.8–4.0] | 0.0% | 0.1% | 0.0% | 1.4% | — |
| ARA-C | hybrid | 86.5 [85.4–87.6] | 95.6 [95.1–96.1] | 0.3 [-0.2–0.8] | 2.4 [1.7–3.1] | 60 [57–64] | 100.0 [100.0–100.0] | 93.7 | 94.4 | 86.6 [85.5–87.7] | 95.8 [95.3–96.3] | 0.4 [0.0–0.9] | 2.3 [1.5–2.9] | 62 [59–65] | 89.0 [88.0–90.0] | 88.7 [87.7–89.7] | 97.3 [96.7–97.9] | 96.8 [96.2–97.5] | 96.3 [95.7–96.9] | 96.0 [95.3–96.6] | 4.0 [3.4–4.7] | 4.0 [3.4–4.6] | 0.0% | 0.1% | 0.0% | 3.0% | MARS (VSE\* WME\*), MARS-T (VSE\* WME\*), ARA (VSE\* WME\*) +14 more |
| Approval cond. runoff | two-round | 86.5 [85.3–87.5] | 92.8 [92.2–93.5] | -1.1 [-1.6–-0.4] | 3.6 [2.7–4.4] | 50 [46–54] | 99.9 [99.9–99.9] | 73.0 | 73.5 | 87.1 [86.0–88.0] | 93.1 [92.4–93.8] | -0.6 [-1.2–-0.1] | 3.1 [2.3–3.8] | 53 [49–58] | 86.4 [85.3–87.4] | 86.4 [85.4–87.4] | 95.1 [94.4–95.8] | 94.6 [93.9–95.4] | 94.4 [93.7–95.1] | 94.0 [93.3–94.7] | 4.2 [3.5–4.8] | 4.2 [3.6–4.9] | 0.0% | 0.7% | 0.0% | 2.0% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), MARS (VSE\* WME\*) +26 more |
| BTR-score | hybrid | 86.4 [85.3–87.5] | 97.6 [97.2–98.0] | 1.2 [0.9–1.7] | 1.4 [0.9–1.9] | 74 [70–78] | 100.0 [100.0–100.0] | 93.7 | 94.2 | 86.4 [85.3–87.5] | 97.9 [97.6–98.2] | 1.3 [1.0–1.7] | 1.3 [0.7–1.7] | 76 [73–80] | 93.6 [92.9–94.4] | 93.1 [92.2–93.9] | 98.7 [98.3–99.2] | 98.4 [97.9–98.8] | 97.8 [97.4–98.3] | 97.5 [97.0–98.0] | 3.6 [3.0–4.2] | 3.6 [3.1–4.3] | 0.0% | 0.2% | 0.0% | 2.0% | — |
| IDS | rated | 86.4 [85.2–87.5] | 93.5 [92.8–94.1] | -0.8 [-1.3–-0.2] | 3.3 [2.6–4.1] | 51 [48–55] | 100.0 [100.0–100.0] | 92.8 | 94.5 | 86.5 [85.4–87.6] | 93.6 [92.9–94.2] | -0.7 [-1.2–-0.1] | 3.2 [2.5–4.0] | 52 [49–56] | 86.0 [84.9–87.0] | 85.8 [84.7–86.8] | 94.9 [94.1–95.7] | 94.6 [93.8–95.4] | 94.3 [93.6–95.0] | 94.0 [93.2–94.7] | 4.4 [3.8–5.1] | 4.4 [3.8–5.1] | 0.0% | 0.1% | 0.0% | 7.9% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), MARS (VSE\* WME\*) +23 more |
| Approval | approval-based | 86.3 [85.2–87.4] | 90.3 [89.5–91.1] | -2.4 [-3.0–-1.7] | 4.4 [3.4–5.3] | 44 [41–48] | 100.0 [100.0–100.0] | 74.3 | 74.5 | 86.3 [85.2–87.4] | 90.4 [89.6–91.2] | -2.3 [-3.0–-1.6] | 4.3 [3.4–5.3] | 44 [41–48] | 81.5 [80.2–82.7] | 81.7 [80.5–82.8] | 91.8 [90.9–92.8] | 91.7 [90.8–92.6] | 91.8 [90.8–92.6] | 91.7 [90.8–92.5] | 4.7 [4.0–5.3] | 4.5 [3.9–5.2] | 0.0% | 0.1% | 0.0% | 2.4% | MAV (VSE\* WME\*), DMS (VSE\* WME\*), STAR (VSE\* WME\*) +32 more |
| ARA-Top | hybrid | 86.3 [85.1–87.3] | 94.2 [93.6–94.8] | -0.5 [-1.0–-0.0] | 3.1 [2.4–3.8] | 54 [50–57] | 100.0 [100.0–100.0] | 93.9 | 94.5 | 86.4 [85.3–87.5] | 94.3 [93.8–94.9] | -0.3 [-0.8–0.1] | 3.0 [2.2–3.7] | 55 [51–58] | 86.9 [85.8–88.0] | 86.8 [85.8–87.9] | 95.5 [94.8–96.2] | 95.2 [94.5–95.9] | 94.8 [94.1–95.5] | 94.5 [93.8–95.2] | 4.5 [3.9–5.1] | 4.6 [4.0–5.2] | 0.0% | 0.1% | 0.0% | 2.9% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), MARS (VSE\* WME\*) +18 more |
| Smith//Score | condorcet-based | 86.2 [85.1–87.4] | 97.5 [97.2–97.9] | 1.1 [0.7–1.5] | 1.5 [1.0–2.0] | 72 [69–77] | 100.0 [100.0–100.0] | 93.7 | 94.2 | 86.4 [85.3–87.5] | 97.9 [97.6–98.2] | 1.3 [0.9–1.7] | 1.3 [0.8–1.7] | 76 [72–80] | 93.6 [92.8–94.4] | 92.9 [92.2–93.8] | 98.7 [98.3–99.2] | 98.3 [97.8–98.8] | 97.8 [97.4–98.3] | 97.4 [96.9–97.9] | 3.6 [3.0–4.2] | 3.7 [3.1–4.3] | 0.0% | 0.2% | 0.0% | 2.2% | Soft Minimax-D (VSE\*), MMWV+Score (WME\*) |
| MDDA | approval-based | 86.2 [85.2–87.3] | 90.5 [89.6–91.3] | -2.3 [-3.0–-1.6] | 4.4 [3.5–5.2] | 44 [41–48] | 100.0 [100.0–100.0] | 75.4 | 75.6 | 86.2 [85.1–87.3] | 90.5 [89.7–91.3] | -2.3 [-2.9–-1.6] | 4.3 [3.4–5.3] | 44 [41–48] | 81.8 [80.6–82.9] | 82.0 [80.8–83.1] | 91.9 [91.0–92.8] | 91.8 [90.9–92.7] | 91.8 [90.9–92.7] | 91.8 [90.9–92.7] | 4.7 [4.0–5.3] | 4.6 [4.0–5.2] | 0.3% | 0.0% | 0.1% | 2.1% | MAV (VSE\* WME\*), DMS (VSE\* WME\*), STAR (VSE\* WME\*) +32 more |
| Score+ | hybrid | 86.2 [85.1–87.4] | 96.3 [95.9–96.7] | 0.5 [0.0–1.0] | 2.2 [1.5–2.8] | 63 [60–67] | 100.0 [100.0–100.0] | 92.4 | 94.3 | 86.7 [85.6–87.8] | 97.2 [96.8–97.6] | 1.2 [0.8–1.6] | 1.5 [0.9–2.0] | 72 [68–76] | 92.0 [91.2–92.8] | 90.6 [89.7–91.4] | 98.2 [97.7–98.6] | 96.8 [96.2–97.3] | 97.2 [96.7–97.7] | 96.0 [95.4–96.5] | 3.5 [2.9–4.0] | 3.8 [3.2–4.4] | 0.0% | 0.1% | 0.0% | 8.3% | Smith//Score (WME\*), IMDS (WME\*), Score runoff (VSE\*) +11 more |
| MMWVxScore | hybrid | 86.2 [85.1–87.3] | 97.7 [97.4–98.1] | 1.2 [0.8–1.6] | 1.4 [0.9–1.9] | 74 [70–78] | 100.0 [100.0–100.0] | 93.7 | 94.2 | 86.4 [85.3–87.5] | 98.0 [97.7–98.3] | 1.4 [1.0–1.7] | 1.2 [0.7–1.6] | 77 [74–81] | 93.8 [93.0–94.6] | 93.2 [92.4–94.0] | 98.8 [98.4–99.2] | 98.4 [97.9–98.8] | 98.0 [97.5–98.4] | 97.5 [97.0–98.0] | 3.5 [2.9–4.1] | 3.6 [3.0–4.2] | 0.0% | 0.2% | 0.0% | 1.8% | — |
| Soft Minimax | hybrid | 86.2 [85.1–87.3] | 97.7 [97.4–98.1] | 1.1 [0.8–1.5] | 1.4 [0.9–1.9] | 74 [71–77] | 100.0 [100.0–100.0] | 93.9 | 94.3 | 86.1 [85.0–87.2] | 98.0 [97.7–98.3] | 1.2 [0.9–1.6] | 1.3 [0.8–1.7] | 76 [73–80] | 93.9 [93.2–94.6] | 93.3 [92.5–94.1] | 98.5 [98.1–99.0] | 98.1 [97.7–98.6] | 97.7 [97.2–98.1] | 97.3 [96.8–97.8] | 3.5 [2.9–4.0] | 3.5 [2.9–4.0] | 0.0% | 0.1% | 0.0% | 1.4% | — |
| Smith//Approval | condorcet-based | 86.2 [85.1–87.3] | 97.6 [97.2–97.9] | 1.1 [0.7–1.5] | 1.5 [1.0–2.0] | 72 [69–76] | 100.0 [100.0–100.0] | 93.8 | 94.2 | 86.2 [85.1–87.3] | 97.8 [97.5–98.1] | 1.2 [0.9–1.6] | 1.4 [0.9–1.8] | 75 [71–78] | 93.3 [92.5–94.1] | 92.7 [91.9–93.5] | 98.4 [97.9–98.8] | 98.0 [97.5–98.5] | 97.5 [97.0–98.0] | 97.2 [96.7–97.7] | 3.6 [3.0–4.2] | 3.7 [3.1–4.2] | 0.0% | 0.1% | 0.0% | 1.7% | Soft Minimax-D (VSE\*) |
| MMWV+Score | hybrid | 86.1 [85.0–87.2] | 97.8 [97.4–98.1] | 1.1 [0.8–1.6] | 1.4 [0.9–1.9] | 74 [71–78] | 100.0 [100.0–100.0] | 93.7 | 94.2 | 86.3 [85.2–87.4] | 98.0 [97.7–98.3] | 1.4 [1.0–1.8] | 1.2 [0.7–1.7] | 77 [74–81] | 93.8 [93.0–94.6] | 93.3 [92.5–94.1] | 98.8 [98.4–99.2] | 98.4 [98.0–98.9] | 98.0 [97.5–98.4] | 97.6 [97.1–98.0] | 3.5 [2.9–4.1] | 3.6 [3.0–4.2] | 0.0% | 0.2% | 0.0% | 1.8% | BTR-score (VSE\*) |
| MAMPO | approval-based | 86.0 [84.9–87.1] | 90.7 [89.9–91.4] | -2.3 [-3.0–-1.7] | 4.5 [3.6–5.4] | 43 [40–47] | 100.0 [100.0–100.0] | 75.5 | 75.6 | 86.0 [84.9–87.1] | 90.6 [89.8–91.3] | -2.3 [-3.0–-1.7] | 4.5 [3.6–5.4] | 43 [40–47] | 81.9 [80.6–83.1] | 82.2 [81.0–83.4] | 91.6 [90.7–92.6] | 91.6 [90.7–92.6] | 91.6 [90.7–92.5] | 91.6 [90.7–92.5] | 4.8 [4.1–5.5] | 4.7 [4.1–5.3] | 0.3% | 0.0% | 0.1% | 2.0% | Approval runoff (VSE\* WME\*), Score (VSE\* WME\*), MJ (VSE\* WME\*) +32 more |
| Reciprocal score | rated | 85.9 [84.8–87.0] | 92.9 [92.2–93.5] | -1.3 [-1.9–-0.7] | 3.9 [3.1–4.7] | 47 [44–51] | 99.9 [99.9–99.9] | 93.5 | 94.5 | 86.2 [85.1–87.3] | 93.6 [93.0–94.2] | -0.8 [-1.3–-0.3] | 3.4 [2.6–4.1] | 51 [48–55] | 85.5 [84.4–86.5] | 84.6 [83.5–85.6] | 94.5 [93.8–95.3] | 93.6 [92.8–94.5] | 94.2 [93.5–94.9] | 93.4 [92.7–94.2] | 4.5 [3.9–5.1] | 4.7 [4.0–5.3] | 0.0% | 0.7% | 0.0% | 2.5% | MJ (VSE\* WME\*), ESR (VSE\* WME\*), STAR (VSE\* WME\*) +29 more |
| IMDS | rated | 85.9 [84.7–86.9] | 97.3 [96.9–97.7] | 0.8 [0.4–1.1] | 1.8 [1.3–2.3] | 68 [65–72] | 100.0 [100.0–100.0] | 92.4 | 94.3 | 85.9 [84.8–87.0] | 97.9 [97.6–98.2] | 1.1 [0.7–1.5] | 1.4 [0.9–1.8] | 74 [71–78] | 93.9 [93.1–94.6] | 92.7 [91.9–93.5] | 98.5 [98.1–98.9] | 97.7 [97.2–98.2] | 97.5 [97.0–98.0] | 96.7 [96.1–97.2] | 3.5 [2.9–4.1] | 3.6 [3.0–4.2] | 0.0% | 0.1% | 0.0% | 8.3% | BTR-score (VSE\*), Soft Minimax (WME\*), Soft Minimax-D (VSE\*) +2 more |
| INS | rated | 85.8 [84.7–87.0] | 97.3 [96.9–97.6] | 0.7 [0.4–1.1] | 1.9 [1.3–2.3] | 68 [65–72] | 100.0 [100.0–100.0] | 92.4 | 94.3 | 85.9 [84.8–87.1] | 97.8 [97.5–98.2] | 1.1 [0.7–1.4] | 1.5 [0.9–1.9] | 74 [71–78] | 93.8 [93.0–94.5] | 92.6 [91.8–93.4] | 98.5 [98.1–98.9] | 97.7 [97.2–98.2] | 97.4 [96.9–97.9] | 96.6 [96.0–97.1] | 3.5 [2.9–4.1] | 3.6 [3.0–4.2] | 0.0% | 0.1% | 0.0% | 8.4% | BTR-score (VSE\*), Soft Minimax (WME\*), Soft Minimax-D (VSE\*) +2 more |
| DAC | plurality-based | 85.7 [84.6–86.8] | 90.8 [89.9–91.6] | -2.4 [-3.1–-1.8] | 4.7 [3.8–5.6] | 42 [39–46] | 99.6 [99.6–99.6] | 79.2 | 80.1 | 86.5 [85.5–87.6] | 93.4 [92.8–94.0] | -0.7 [-1.3–-0.2] | 3.3 [2.5–4.0] | 52 [49–56] | 85.0 [83.9–86.1] | 83.0 [81.9–84.1] | 92.1 [91.1–93.0] | 90.2 [89.2–91.3] | 92.8 [92.0–93.5] | 90.9 [90.0–91.7] | 4.1 [3.5–4.8] | 4.6 [3.9–5.2] | 0.5% | 4.1% | 0.1% | 1.2% | Approval runoff (VSE\* WME\*), Score (VSE\* WME\*), MJ (VSE\* WME\*) +32 more |
| DSC | plurality-based | 85.6 [84.6–86.7] | 91.1 [90.3–91.9] | -2.3 [-3.0–-1.7] | 4.7 [3.7–5.5] | 43 [40–46] | 99.5 [99.5–99.5] | 79.2 | 80.1 | 86.5 [85.5–87.6] | 93.4 [92.8–94.0] | -0.7 [-1.3–-0.2] | 3.3 [2.5–4.0] | 52 [49–56] | 85.0 [83.9–86.1] | 83.4 [82.2–84.5] | 92.1 [91.1–93.0] | 90.1 [89.1–91.1] | 92.8 [92.0–93.5] | 90.8 [89.8–91.6] | 4.1 [3.5–4.8] | 4.3 [3.7–5.0] | 2.0% | 3.1% | 0.1% | 0.4% | Approval runoff (VSE\* WME\*), Score (VSE\* WME\*), MJ (VSE\* WME\*) +32 more |
| ER-IRV | plurality-based | 85.5 [84.4–86.6] | 94.7 [94.0–95.3] | -0.6 [-1.1–-0.1] | 3.3 [2.6–4.1] | 53 [49–57] | 99.7 [99.7–99.7] | 78.5 | 79.0 | 86.0 [84.9–87.0] | 95.4 [94.8–96.0] | -0.1 [-0.5–0.3] | 2.8 [2.1–3.4] | 58 [54–61] | 92.5 [91.7–93.3] | 91.5 [90.7–92.3] | 97.0 [96.4–97.6] | 96.0 [95.3–96.6] | 95.6 [94.9–96.2] | 94.8 [94.0–95.5] | 3.8 [3.2–4.3] | 4.2 [3.5–4.8] | 0.6% | 2.0% | 0.2% | 0.4% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), BTR-score (VSE\* WME\*) +19 more |
| MCA | median-based | 84.9 [83.8–85.9] | 91.0 [90.2–91.8] | -2.7 [-3.4–-2.1] | 5.2 [4.4–6.0] | 40 [37–43] | 100.0 [100.0–100.0] | 85.3 | 86.6 | 83.2 [82.1–84.3] | 92.9 [92.1–93.6] | -2.7 [-3.4–-2.1] | 5.5 [4.7–6.2] | 41 [38–44] | 84.9 [83.8–86.0] | 82.7 [81.4–83.8] | 90.2 [89.2–91.2] | 89.1 [88.0–90.2] | 90.0 [89.1–90.9] | 89.2 [88.2–90.1] | 4.2 [3.6–4.8] | 3.8 [3.2–4.4] | 0.0% | 0.1% | 0.0% | 7.0% | Smith//Score (VSE\* WME\*), Approval runoff (VSE\* WME\*), Approval cond. runoff (VSE\* WME\*) +53 more |
| Supplementary vote | plurality-based | 84.9 [83.8–86.0] | 87.8 [86.7–88.8] | -4.2 [-5.0–-3.4] | 5.9 [5.0–6.9] | 36 [33–39] | 99.6 [99.6–99.6] | 78.1 | 78.7 | 85.6 [84.4–86.6] | 88.6 [87.6–89.6] | -3.5 [-4.3–-2.8] | 5.3 [4.4–6.2] | 39 [36–42] | 82.0 [80.8–83.1] | 81.5 [80.3–82.7] | 89.8 [88.8–90.9] | 89.2 [88.1–90.3] | 89.4 [88.5–90.4] | 88.9 [87.9–89.9] | 4.5 [3.9–5.2] | 4.9 [4.2–5.5] | 1.9% | 1.2% | 0.2% | 0.0% | Smith//Score (VSE\* WME\*), Approval (VSE\* WME\*), Approval runoff (VSE\* WME\*) +58 more |
| Black | condorcet-based | 84.6 [83.5–85.8] | 98.8 [98.5–99.0] | 0.8 [0.5–1.2] | 1.1 [0.8–1.4] | 80 [78–84] | 99.7 [99.7–99.7] | 78.0 | 78.5 | 84.4 [83.2–85.5] | 99.5 [99.4–99.7] | 1.1 [0.8–1.4] | 0.4 [0.3–0.6] | 91 [89–93] | 98.6 [98.1–98.9] | 96.7 [96.1–97.3] | 99.3 [99.0–99.6] | 98.5 [98.0–98.8] | 98.3 [97.9–98.8] | 97.4 [96.9–97.9] | 4.4 [3.8–5.0] | 4.2 [3.5–4.8] | 0.9% | 1.7% | 0.0% | 0.8% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +8 more |
| TTR | two-round | 84.5 [83.4–85.6] | 93.9 [93.2–94.7] | -1.5 [-2.1–-1.0] | 4.2 [3.5–5.0] | 47 [44–51] | 98.8 [98.7–98.9] | 64.5 | 65.5 | 85.1 [84.0–86.2] | 94.1 [93.4–94.8] | -1.2 [-1.7–-0.6] | 3.9 [3.1–4.6] | 49 [46–53] | 91.0 [90.1–91.9] | 90.5 [89.6–91.4] | 95.5 [94.7–96.2] | 95.0 [94.2–95.7] | 93.9 [93.1–94.6] | 93.6 [92.8–94.3] | 4.4 [3.8–5.1] | 4.6 [3.9–5.2] | 2.0% | 0.0% | 1.0% | 0.0% | Smith//Score (VSE\* WME\* sincerity), IMDS (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity) +48 more |
| MMPOWV | minimax-family | 84.5 [83.4–85.7] | 99.2 [99.0–99.4] | 1.0 [0.7–1.4] | 0.7 [0.5–0.9] | 87 [84–90] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.7 [97.2–98.1] | 99.5 [99.2–99.7] | 99.0 [98.6–99.3] | 98.7 [98.4–99.1] | 98.1 [97.6–98.5] | 4.3 [3.7–5.0] | 4.2 [3.5–4.8] | 0.1% | 1.2% | 0.0% | 0.1% | Schulze (WME\*), Minimax (WME\*) |
| MMWV | minimax-family | 84.5 [83.4–85.6] | 99.3 [99.1–99.5] | 1.0 [0.7–1.4] | 0.7 [0.4–0.9] | 87 [84–90] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.8 [97.3–98.2] | 99.5 [99.2–99.7] | 99.0 [98.7–99.3] | 98.7 [98.4–99.1] | 98.2 [97.8–98.5] | 4.3 [3.7–5.0] | 4.2 [3.6–4.9] | 0.1% | 1.2% | 0.0% | 0.1% | — |
| Ranked pairs | condorcet | 84.5 [83.4–85.6] | 99.3 [99.1–99.5] | 1.0 [0.7–1.4] | 0.6 [0.4–0.8] | 88 [85–91] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.1–85.5] | 99.6 [99.5–99.7] | 1.1 [0.8–1.4] | 0.4 [0.2–0.5] | 92 [90–94] | 98.6 [98.2–99.0] | 97.9 [97.4–98.3] | 99.5 [99.2–99.7] | 99.1 [98.7–99.4] | 98.6 [98.2–98.9] | 98.1 [97.6–98.5] | 4.3 [3.7–5.0] | 4.3 [3.7–5.0] | 0.4% | 0.7% | 0.0% | 0.1% | — |
| Schulze | condorcet | 84.4 [83.3–85.6] | 99.4 [99.2–99.5] | 1.0 [0.7–1.4] | 0.6 [0.4–0.8] | 89 [86–91] | 99.9 [99.8–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.9 [97.4–98.3] | 99.5 [99.2–99.7] | 99.0 [98.7–99.3] | 98.7 [98.4–99.1] | 98.2 [97.8–98.5] | 4.3 [3.7–5.0] | 4.2 [3.6–4.9] | 0.4% | 0.8% | 0.0% | 0.1% | — |
| MMPO | minimax-family | 84.4 [83.3–85.6] | 99.3 [99.1–99.4] | 1.0 [0.6–1.4] | 0.7 [0.5–0.9] | 87 [84–90] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.7 [97.2–98.2] | 99.5 [99.2–99.7] | 99.0 [98.6–99.3] | 98.7 [98.4–99.1] | 98.1 [97.7–98.5] | 4.3 [3.7–5.0] | 4.2 [3.6–4.9] | 0.1% | 1.2% | 0.0% | 0.1% | Schulze (WME\*), Minimax (WME\*) |
| Minimax | minimax-family | 84.4 [83.3–85.5] | 99.3 [99.2–99.5] | 1.0 [0.7–1.4] | 0.6 [0.4–0.8] | 88 [85–91] | 99.9 [99.8–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.9 [97.4–98.3] | 99.5 [99.2–99.7] | 99.1 [98.8–99.4] | 98.7 [98.4–99.1] | 98.2 [97.8–98.6] | 4.3 [3.7–5.0] | 4.2 [3.6–4.9] | 0.4% | 0.8% | 0.0% | 0.1% | — |
| Schulze-ER | condorcet | 84.4 [83.3–85.5] | 99.3 [99.1–99.5] | 1.0 [0.6–1.3] | 0.7 [0.4–0.8] | 88 [85–90] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.7 [97.3–98.2] | 99.5 [99.2–99.7] | 99.0 [98.7–99.3] | 98.7 [98.4–99.1] | 98.2 [97.8–98.5] | 4.3 [3.7–5.0] | 4.2 [3.6–4.9] | 0.1% | 1.1% | 0.0% | 0.2% | — |
| Condorcet-random-drop | references | 84.4 [83.3–85.5] | 99.4 [99.2–99.5] | 1.0 [0.7–1.3] | 0.6 [0.4–0.8] | 88 [86–91] | 99.8 [99.8–99.9] | 78.3 | 78.5 | 84.3 [83.1–85.4] | 99.7 [99.6–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 94 [93–95] | 98.6 [98.2–98.9] | 97.9 [97.4–98.2] | 99.5 [99.2–99.7] | 99.1 [98.8–99.4] | 98.7 [98.4–99.0] | 98.3 [97.9–98.6] | 4.1 [3.5–4.8] | 4.1 [3.5–4.7] | 0.5% | 0.8% | 0.0% | 0.1% | — |
| Minimax-ER | minimax-family | 84.3 [83.2–85.5] | 99.3 [99.1–99.5] | 0.9 [0.6–1.3] | 0.7 [0.5–0.9] | 87 [85–90] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.2–85.5] | 99.7 [99.5–99.8] | 1.1 [0.8–1.4] | 0.3 [0.2–0.4] | 93 [91–95] | 98.6 [98.2–98.9] | 97.7 [97.2–98.1] | 99.5 [99.2–99.7] | 99.0 [98.7–99.4] | 98.7 [98.4–99.1] | 98.2 [97.8–98.5] | 4.3 [3.7–5.0] | 4.2 [3.5–4.8] | 0.1% | 1.1% | 0.0% | 0.2% | — |
| Dowdall | positional | 84.3 [83.1–85.4] | 89.8 [89.0–90.6] | -3.6 [-4.4–-2.9] | 6.0 [5.0–6.9] | 36 [33–40] | 99.4 [99.4–99.4] | 76.9 | 78.5 | 85.2 [84.0–86.3] | 91.7 [91.0–92.4] | -2.3 [-3.0–-1.6] | 4.8 [3.9–5.6] | 42 [39–46] | 82.2 [81.0–83.4] | 80.0 [78.7–81.2] | 90.9 [89.9–91.9] | 88.6 [87.5–89.7] | 90.5 [89.5–91.3] | 88.4 [87.5–89.4] | 4.9 [4.2–5.6] | 5.2 [4.5–5.9] | 3.3% | 3.3% | 0.0% | 4.5% | Smith//Score (VSE\* WME\*), ER-IRV (VSE\* WME\*), Approval runoff (VSE\* WME\*) +63 more |
| Copeland//Margins | condorcet | 84.3 [83.2–85.4] | 99.1 [98.9–99.3] | 0.8 [0.5–1.1] | 0.9 [0.6–1.1] | 84 [81–87] | 99.8 [99.8–99.8] | 78.2 | 78.5 | 84.2 [83.0–85.4] | 99.5 [99.3–99.6] | 1.0 [0.7–1.3] | 0.5 [0.3–0.6] | 90 [88–93] | 98.6 [98.2–98.9] | 97.4 [96.9–97.9] | 99.5 [99.2–99.7] | 99.1 [98.7–99.4] | 98.3 [97.9–98.7] | 97.9 [97.4–98.3] | 4.3 [3.7–5.0] | 4.3 [3.7–4.9] | 0.5% | 0.9% | 0.1% | 0.3% | Schulze (WME\*), Kemeny-Young (WME\*), Minimax (WME\*) +1 more |
| BTR-IRV | condorcet-based | 84.3 [83.2–85.5] | 98.5 [98.2–98.8] | 0.5 [0.2–0.9] | 1.4 [1.0–1.7] | 77 [74–81] | 99.7 [99.7–99.8] | 78.2 | 78.6 | 84.2 [83.0–85.3] | 99.4 [99.2–99.5] | 0.9 [0.6–1.2] | 0.6 [0.4–0.8] | 88 [85–91] | 98.6 [98.3–99.0] | 96.6 [95.9–97.1] | 99.5 [99.2–99.7] | 98.5 [98.1–98.9] | 98.3 [97.8–98.7] | 97.2 [96.6–97.7] | 4.5 [3.9–5.2] | 4.5 [3.8–5.1] | 0.7% | 1.1% | 0.1% | 0.3% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +13 more |
| Copeland//Margins-ER | condorcet | 84.3 [83.1–85.4] | 99.1 [98.9–99.3] | 0.8 [0.5–1.2] | 0.9 [0.6–1.1] | 85 [82–88] | 99.8 [99.8–99.8] | 78.2 | 78.5 | 84.2 [83.0–85.4] | 99.5 [99.3–99.6] | 1.0 [0.7–1.3] | 0.5 [0.3–0.6] | 90 [88–93] | 98.6 [98.2–98.9] | 97.4 [96.9–97.9] | 99.5 [99.2–99.7] | 99.0 [98.7–99.3] | 98.3 [97.9–98.7] | 97.8 [97.4–98.3] | 4.3 [3.7–5.0] | 4.3 [3.7–4.9] | 0.1% | 1.2% | 0.1% | 0.3% | Schulze (WME\*), Kemeny-Young (WME\*), Condorcet-random-drop (WME\*) |
| Kemeny-Young | condorcet | 84.2 [83.0–85.3] | 99.3 [99.2–99.5] | 0.9 [0.5–1.2] | 0.6 [0.4–0.8] | 88 [85–91] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.3 [83.1–85.4] | 99.6 [99.5–99.7] | 1.1 [0.8–1.4] | 0.4 [0.2–0.5] | 93 [90–94] | 98.6 [98.2–98.9] | 97.8 [97.3–98.2] | 99.5 [99.2–99.7] | 98.9 [98.5–99.3] | 98.7 [98.3–99.0] | 98.0 [97.6–98.4] | 4.4 [3.8–5.1] | 4.5 [3.9–5.1] | 0.4% | 0.7% | 0.0% | 0.1% | Ranked pairs (VSE\*), MMPOWV (VSE\*) |
| BTR-Borda | condorcet-based | 84.1 [82.9–85.3] | 98.9 [98.7–99.2] | 0.6 [0.2–1.0] | 1.0 [0.7–1.3] | 82 [79–85] | 99.7 [99.7–99.8] | 78.1 | 78.5 | 84.2 [83.0–85.3] | 99.5 [99.4–99.7] | 1.0 [0.7–1.3] | 0.5 [0.3–0.6] | 91 [89–93] | 98.5 [98.1–98.9] | 97.1 [96.5–97.6] | 99.4 [99.1–99.7] | 98.5 [98.1–98.9] | 98.3 [97.9–98.7] | 97.5 [97.0–98.0] | 4.4 [3.7–5.0] | 4.4 [3.8–5.1] | 0.7% | 1.3% | 0.1% | 0.6% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +9 more |
| Benham | condorcet-based | 84.1 [82.9–85.3] | 98.3 [98.0–98.6] | 0.3 [-0.0–0.7] | 1.6 [1.2–1.9] | 75 [71–78] | 99.7 [99.7–99.7] | 78.2 | 78.6 | 83.7 [82.5–84.9] | 99.5 [99.3–99.6] | 0.7 [0.5–1.0] | 0.5 [0.4–0.7] | 90 [87–92] | 98.6 [98.1–99.0] | 96.4 [95.8–97.0] | 99.4 [99.2–99.7] | 98.3 [97.9–98.7] | 98.0 [97.6–98.5] | 96.8 [96.2–97.3] | 4.6 [3.9–5.2] | 4.2 [3.6–4.9] | 0.8% | 1.1% | 0.1% | 0.3% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +15 more |
| Nanson | condorcet-based | 84.1 [82.9–85.3] | 99.2 [99.0–99.4] | 0.8 [0.4–1.1] | 0.8 [0.5–1.0] | 86 [83–89] | 99.9 [99.8–99.9] | 78.3 | 78.5 | 84.1 [82.9–85.3] | 99.5 [99.3–99.6] | 0.9 [0.6–1.2] | 0.5 [0.3–0.7] | 90 [87–93] | 98.6 [98.2–99.0] | 97.9 [97.4–98.3] | 99.5 [99.3–99.8] | 99.1 [98.7–99.4] | 98.5 [98.1–98.9] | 97.9 [97.4–98.3] | 4.5 [3.9–5.1] | 4.5 [3.8–5.1] | 0.4% | 0.6% | 0.0% | 0.6% | Ranked pairs (VSE\*), MMWV (VSE\*), MMPOWV (VSE\*) |
| Copeland | condorcet | 84.0 [82.8–85.2] | 99.1 [98.9–99.3] | 0.7 [0.4–1.0] | 0.9 [0.6–1.1] | 84 [81–88] | 99.9 [99.9–99.9] | 78.3 | 78.5 | 84.0 [82.8–85.2] | 99.4 [99.3–99.6] | 0.8 [0.6–1.1] | 0.6 [0.4–0.7] | 89 [87–92] | 98.5 [98.2–98.9] | 97.9 [97.4–98.3] | 99.4 [99.1–99.6] | 99.1 [98.7–99.3] | 98.1 [97.6–98.5] | 97.7 [97.3–98.1] | 4.3 [3.7–5.0] | 4.3 [3.7–5.0] | 0.4% | 0.7% | 0.0% | 0.1% | Schulze (VSE\* WME\*), Ranked pairs (VSE\*), MMWV (VSE\*) +1 more |
| SICT | condorcet | 83.9 [82.7–85.0] | 98.5 [98.1–98.8] | 0.3 [-0.1–0.7] | 1.5 [1.1–1.8] | 76 [72–80] | 99.8 [99.8–99.8] | 78.1 | 78.6 | 84.0 [82.7–85.2] | 99.5 [99.3–99.6] | 0.8 [0.6–1.2] | 0.5 [0.4–0.7] | 90 [87–92] | 98.7 [98.3–99.0] | 96.6 [96.0–97.1] | 99.5 [99.3–99.8] | 98.6 [98.2–99.0] | 98.3 [97.9–98.7] | 97.2 [96.7–97.8] | 4.6 [4.0–5.3] | 4.6 [4.0–5.3] | 0.2% | 2.0% | 0.1% | 0.5% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Minimax (VSE\* WME\*) +14 more |
| Baldwin-ER | condorcet-based | 83.8 [82.6–85.0] | 98.9 [98.6–99.1] | 0.5 [0.1–0.8] | 1.1 [0.8–1.4] | 81 [77–84] | 99.9 [99.9–99.9] | 78.3 | 78.6 | 83.9 [82.7–85.1] | 99.3 [99.0–99.5] | 0.7 [0.4–1.0] | 0.7 [0.5–1.0] | 86 [82–90] | 98.6 [98.2–98.9] | 97.5 [97.0–98.0] | 99.4 [99.1–99.7] | 98.9 [98.5–99.2] | 98.0 [97.6–98.5] | 97.5 [97.0–98.0] | 4.7 [4.0–5.3] | 4.7 [4.0–5.3] | 0.2% | 0.8% | 0.1% | 0.4% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Minimax (VSE\* WME\*) +9 more |
| Baldwin | condorcet-based | 83.8 [82.6–85.0] | 98.8 [98.5–99.1] | 0.4 [0.0–0.8] | 1.2 [0.8–1.4] | 80 [77–84] | 99.9 [99.8–99.9] | 78.3 | 78.6 | 83.9 [82.7–85.1] | 99.3 [99.0–99.5] | 0.7 [0.4–1.0] | 0.7 [0.5–1.0] | 86 [82–90] | 98.6 [98.2–98.9] | 97.4 [96.9–97.9] | 99.4 [99.1–99.7] | 98.7 [98.4–99.1] | 98.0 [97.6–98.5] | 97.3 [96.8–97.8] | 4.7 [4.0–5.3] | 4.7 [4.0–5.3] | 0.4% | 0.6% | 0.1% | 0.3% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Minimax (VSE\* WME\*) +12 more |
| IRV-3 | plurality-based | 83.8 [82.5–85.0] | 91.4 [90.4–92.3] | -3.1 [-4.0–-2.4] | 5.8 [4.8–6.8] | 38 [35–42] | 99.2 [99.1–99.2] | 76.7 | 77.9 | 86.3 [85.3–87.3] | 94.3 [93.6–94.9] | -0.4 [-0.9–0.0] | 3.1 [2.4–3.8] | 54 [51–58] | 90.2 [89.3–91.2] | 87.4 [86.4–88.4] | 95.7 [95.0–96.4] | 92.6 [91.8–93.5] | 94.4 [93.7–95.2] | 91.7 [90.9–92.5] | 3.9 [3.4–4.5] | 5.1 [4.5–5.8] | 2.6% | 1.0% | 1.3% | 0.1% | Smith//Score (VSE\* WME\*), ER-IRV (VSE\* WME\*), Approval runoff (VSE\* WME\*) +60 more |
| IRV | plurality-based | 83.6 [82.3–84.8] | 92.5 [91.7–93.3] | -2.7 [-3.4–-2.0] | 5.4 [4.5–6.3] | 41 [38–44] | 99.0 [99.0–99.1] | 77.7 | 79.0 | 86.0 [84.9–87.0] | 95.4 [94.8–96.0] | -0.1 [-0.5–0.3] | 2.8 [2.1–3.4] | 58 [54–61] | 92.5 [91.7–93.3] | 89.1 [88.0–90.0] | 97.0 [96.4–97.6] | 93.6 [92.8–94.4] | 95.6 [94.9–96.2] | 92.5 [91.7–93.3] | 3.8 [3.2–4.3] | 4.8 [4.2–5.5] | 2.6% | 1.8% | 1.4% | 0.1% | Ranked pairs (VSE\* WME\*), MMWV (VSE\* WME\*), Smith//Score (VSE\* WME\*) +55 more |
| Coombs | plurality-based | 83.1 [81.9–84.3] | 96.5 [96.0–97.0] | -1.1 [-1.7–-0.4] | 3.2 [2.6–3.8] | 58 [55–62] | 99.3 [99.2–99.3] | 77.2 | 78.3 | 84.2 [83.0–85.5] | 98.7 [98.5–98.9] | 0.6 [0.2–1.0] | 1.2 [0.9–1.4] | 79 [77–82] | 95.1 [94.4–95.8] | 92.3 [91.5–93.2] | 96.5 [95.8–97.1] | 94.5 [93.8–95.3] | 95.7 [95.1–96.3] | 93.6 [92.9–94.3] | 4.0 [3.4–4.7] | 4.6 [4.0–5.3] | 1.7% | 2.0% | 0.9% | 1.6% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +33 more |
| Asset | delegated | 82.0 [80.6–83.3] | 85.4 [84.3–86.6] | -7.0 [-7.9–-6.0] | 8.8 [7.6–9.9] | 28 [26–30] | 99.2 [99.2–99.3] | 66.8 | 67.5 | 82.8 [81.6–84.0] | 86.3 [85.2–87.3] | -6.1 [-7.0–-5.2] | 8.0 [6.9–9.0] | 30 [28–32] | 76.8 [75.4–78.1] | 76.0 [74.7–77.4] | 81.8 [80.4–83.1] | 81.0 [79.6–82.3] | 82.0 [80.8–83.2] | 81.4 [80.1–82.6] | 4.0 [3.4–4.6] | 4.5 [3.8–5.1] | 1.7% | 0.0% | 0.5% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +67 more |
| Borda-ER | positional | 81.9 [80.5–83.1] | 91.0 [90.2–91.7] | -4.3 [-5.1–-3.6] | 7.0 [6.1–7.9] | 34 [32–37] | 99.2 [99.2–99.2] | 75.5 | 77.8 | 82.7 [81.5–83.9] | 94.2 [93.6–94.8] | -2.4 [-3.1–-1.7] | 5.0 [4.3–5.6] | 45 [43–48] | 84.5 [83.4–85.6] | 81.0 [79.8–82.2] | 88.3 [87.3–89.5] | 86.7 [85.5–87.8] | 88.6 [87.7–89.6] | 87.1 [86.0–88.1] | 3.3 [2.8–3.9] | 4.4 [3.7–5.0] | 1.7% | 6.6% | 0.0% | 4.7% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +63 more |
| Borda | positional | 81.7 [80.5–83.1] | 91.1 [90.4–91.9] | -4.3 [-5.1–-3.6] | 7.0 [6.1–7.9] | 34 [32–37] | 99.1 [99.1–99.2] | 75.9 | 77.8 | 82.7 [81.5–83.9] | 94.2 [93.6–94.8] | -2.4 [-3.1–-1.7] | 5.0 [4.3–5.6] | 45 [43–48] | 84.5 [83.4–85.6] | 81.0 [79.8–82.2] | 88.3 [87.3–89.5] | 86.6 [85.5–87.8] | 88.6 [87.7–89.6] | 87.2 [86.1–88.2] | 3.3 [2.8–3.9] | 4.4 [3.8–5.0] | 3.3% | 5.1% | 0.0% | 4.4% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +63 more |
| Asset-foresight | delegated | 80.6 [79.3–81.9] | 87.3 [86.3–88.4] | -6.8 [-7.6–-5.8] | 9.3 [8.3–10.2] | 27 [26–29] | 99.3 [99.3–99.4] | 66.0 | 66.7 | 81.2 [80.0–82.5] | 87.8 [86.8–88.8] | -6.2 [-7.0–-5.3] | 8.8 [7.7–9.7] | 29 [27–31] | 78.0 [76.7–79.3] | 77.5 [76.2–78.9] | 81.9 [80.5–83.2] | 81.4 [80.1–82.8] | 82.1 [80.9–83.3] | 81.8 [80.6–83.0] | 4.6 [3.9–5.3] | 4.8 [4.1–5.5] | 1.7% | 0.0% | 0.4% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +67 more |
| Plurality | plurality-based | 76.7 [75.1–78.2] | 75.8 [74.4–77.3] | -13.2 [-14.7–-11.7] | 14.4 [12.8–15.9] | 17 [16–19] | 99.5 [99.5–99.5] | 64.3 | 64.9 | 74.3 [72.6–76.1] | 74.9 [73.5–76.2] | -15.6 [-17.2–-13.8] | 16.7 [15.0–18.4] | 15 [14–16] | 66.7 [65.2–68.2] | 67.1 [65.6–68.6] | 75.3 [73.8–76.8] | 75.9 [74.4–77.5] | 75.8 [74.4–77.1] | 76.7 [75.4–77.9] | 8.8 [8.0–9.7] | 7.8 [7.0–8.7] | 4.4% | 0.0% | 0.1% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +69 more |
| CW else Score loser | references | 73.1 [70.8–75.3] | 85.9 [84.4–87.5] | -11.3 [-13.1–-9.6] | 13.6 [11.9–15.3] | 23 [21–26] | 100.0 [100.0–100.0] | 93.2 | 94.3 | 74.4 [72.0–76.6] | 87.7 [86.0–89.3] | -9.8 [-11.7–-8.0] | 12.0 [10.3–13.7] | 26 [23–29] | 88.6 [87.6–89.6] | 87.3 [86.2–88.3] | 93.2 [92.3–94.0] | 91.9 [91.0–92.9] | 92.6 [91.8–93.4] | 91.4 [90.5–92.3] | 6.8 [6.0–7.6] | 7.3 [6.5–8.1] | 0.0% | 0.4% | 0.0% | 3.2% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +68 more |
| Random pair runoff | references | 50.5 [49.9–50.9] | 43.9 [43.5–44.3] | -39.5 [-40.2–-38.7] | 40.6 [40.1–41.1] | 5.3 [5.2–5.4] | 99.9 [99.9–99.9] | 76.5 | 76.7 | 50.3 [49.7–50.8] | 43.5 [43.1–44.0] | -39.7 [-40.4–-38.9] | 40.8 [40.3–41.3] | 5.3 [5.2–5.4] | 52.0 [51.5–52.5] | 52.2 [51.7–52.7] | 55.6 [55.1–56.1] | 55.9 [55.4–56.3] | 58.4 [57.9–58.9] | 58.6 [58.1–59.0] | 3.2 [2.6–3.8] | 3.2 [2.7–3.9] | 0.3% | 0.5% | 0.0% | 0.2% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +70 more |
| Anti-plurality | references | 3.4 [0.6–5.6] | 4.9 [2.7–6.9] | -86.5 [-88.8–-84.1] | 87.6 [85.4–90.3] | 1.2 [1.1–1.3] | 97.9 [97.8–98.0] | 45.3 | 47.8 | 4.2 [2.0–6.2] | 6.7 [4.8–8.3] | -85.2 [-87.3–-83.3] | 86.7 [84.8–88.8] | 1.3 [1.2–1.4] | 33.5 [32.6–34.4] | 33.3 [32.3–34.3] | 33.9 [32.9–34.9] | 33.7 [32.5–34.8] | 37.6 [36.8–38.6] | 37.4 [36.3–38.4] | 18.9 [17.8–20.2] | 22.2 [20.9–23.5] | 0.0% | 24.3% | 0.0% | 0.0% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), Kemeny-Young (VSE\* WME\* sincerity) +72 more |
| Random ballot | references | -11.4 [-12.0–-10.7] | -16.5 [-17.2–-15.8] | -101.3 [-102.4–-100.2] | 102.4 [101.8–103.1] | 1.0 [1.0–1.0] | 100.0 [100.0–100.0] | 93.6 | 93.8 | -11.5 [-12.2–-10.9] | -16.7 [-17.4–-16.0] | -101.5 [-102.6–-100.3] | 102.6 [101.9–103.2] | 1.0 [1.0–1.0] | 25.2 [24.9–25.5] | 25.2 [25.0–25.5] | 26.0 [25.7–26.3] | 26.1 [25.8–26.4] | 29.6 [29.3–30.0] | 29.7 [29.4–30.1] | 23.2 [21.8–24.5] | 23.1 [21.8–24.4] | 0.0% | 0.0% | 0.0% | 0.4% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +71 more |
| TRASH | references | -29.5 [-32.6–-26.4] | -29.5 [-31.7–-27.1] | -119.4 [-122.5–-116.0] | 120.5 [117.4–123.6] | 1.0 [1.0–1.0] | — | — | — | -22.9 [-25.5–-20.4] | -26.8 [-28.8–-24.7] | -112.9 [-115.5–-110.1] | 114.0 [111.5–116.5] | 1.0 [1.0–1.0] | 14.1 [13.2–14.8] | 15.6 [14.5–16.5] | 14.9 [13.9–15.8] | 15.8 [14.6–16.9] | 18.7 [17.8–19.6] | 19.2 [18.1–20.3] | 35.8 [34.3–37.3] | 42.0 [40.5–43.5] | 16.7% | 6.9% | 3.9% | 8.3% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +74 more |
| CL else Score winner | references | -82.6 [-87.6–-77.0] | -47.4 [-51.2–-43.3] | -146.1 [-149.9–-142.1] | 147.4 [143.3–151.2] | 1.0 [1.0–1.0] | 99.9 [99.9–99.9] | 92.4 | 94.4 | -74.4 [-79.5–-68.9] | -40.7 [-44.7–-36.6] | -139.5 [-143.4–-135.4] | 140.7 [136.6–144.7] | 1.0 [1.0–1.0] | 34.2 [32.8–35.7] | 31.1 [29.6–32.6] | 38.9 [37.2–40.7] | 34.8 [33.2–36.5] | 38.9 [37.5–40.4] | 35.1 [33.6–36.7] | 58.2 [56.6–59.7] | 60.7 [59.1–62.2] | 0.0% | 0.8% | 0.0% | 5.2% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +73 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| TRASH | 36.8% |
| Anti-plurality | 25.0% |
| Borda-ER | 13.1% |
| Borda | 13.1% |
| Dowdall | 11.6% |
| Score | 9.2% |
| ESR | 9.1% |
| INS | 9.0% |
| IMDS | 9.0% |
| Score+ | 9.0% |
| IDS | 8.6% |
| Geometric median | 8.6% |
| Plurality | 8.6% |
| MCA | 8.2% |
| DMS | 8.0% |
| STLR | 7.5% |
| DAC | 6.7% |
| Coombs | 6.4% |
| IC-MARS | 6.4% |
| DSC | 6.3% |
| IRV | 6.3% |
| CL else Score winner | 6.2% |
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
| MDDA | 4.7% |
| TTR | 4.6% |
| Approval cond. runoff | 4.5% |
| MAMPO | 4.4% |
| Asset | 4.2% |
| Asset-foresight | 4.1% |
| ARA | 4.1% |
| CW else Score loser | 4.0% |
| STAR | 3.8% |
| Supplementary vote | 3.6% |
| Reciprocal score | 3.6% |
| Approval runoff | 3.5% |
| ER-IRV | 3.5% |
| Black | 3.5% |
| ARA-Top | 3.4% |
| ARA-C | 3.4% |
| Soft Minimax-40 | 2.9% |
| SICT | 2.8% |
| BTR-Borda | 2.8% |
| Smith//Score | 2.5% |
| BTR-IRV | 2.4% |
| Benham | 2.4% |
| BTR-score | 2.4% |
| MMWVxScore | 2.2% |
| MMWV+Score | 2.1% |
| Smith//Approval | 2.0% |
| Nanson | 1.8% |
| Copeland//Margins | 1.8% |
| Copeland//Margins-ER | 1.8% |
| Soft Minimax-D | 1.6% |
| Soft Minimax | 1.6% |
| Soft Minimax-L | 1.6% |
| Baldwin-ER | 1.6% |
| Baldwin | 1.5% |
| Condorcet-random-drop | 1.5% |
| Minimax-ER | 1.5% |
| Random pair runoff | 1.5% |
| Minimax | 1.4% |
| Schulze-ER | 1.4% |
| MMPO | 1.4% |
| MMWV | 1.4% |
| Schulze | 1.4% |
| MMPOWV | 1.4% |
| Copeland | 1.3% |
| Ranked pairs | 1.3% |
| Kemeny-Young | 1.3% |
| Random ballot | 0.5% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Random ballot | 100.0% | 0.00 | 2.00 | 100% |
| TRASH | 90.0% | 1.03 | 1.94 | 97% |
| Anti-plurality | 88.5% | 0.35 | 1.99 | 99% |
| Random pair runoff | 67.5% | 0.32 | 1.04 | 99% |
| CL else Score winner | 44.1% | 0.31 | 0.66 | 90% |
| Plurality | 34.6% | 1.12 | 0.92 | 94% |
| Supplementary vote | 23.8% | 1.07 | 0.59 | 99% |
| DSC | 22.2% | 0.69 | 0.08 | 100% |
| DAC | 22.2% | 0.69 | 0.08 | 100% |
| TTR | 20.8% | 0.53 | 0.58 | 99% |
| MDDA | 20.3% | 0.71 | 0.28 | 100% |
| Approval | 20.3% | 0.71 | 0.29 | 100% |
| MAMPO | 20.2% | 0.71 | 0.26 | 100% |
| Asset | 18.5% | 0.27 | 0.08 | 100% |
| Asset-foresight | 18.0% | 0.21 | 0.29 | 100% |
| Dowdall | 17.8% | 0.57 | 0.42 | 99% |
| MCA | 17.5% | 0.34 | 0.17 | 99% |
| IRV-3 | 17.4% | 0.51 | 0.25 | 100% |
| Approval runoff | 17.4% | 0.44 | 0.37 | 100% |
| Approval cond. runoff | 16.9% | 0.51 | 0.38 | 100% |
| MAV | 16.3% | 0.47 | 0.07 | 100% |
| Reciprocal score | 16.3% | 0.46 | 0.07 | 100% |
| IRV | 15.5% | 0.22 | 0.05 | 100% |
| ER-IRV | 15.5% | 0.22 | 0.05 | 100% |
| MJ | 15.4% | 0.40 | 0.06 | 100% |
| IDS | 15.0% | 0.46 | 0.06 | 100% |
| 321 | 14.8% | 0.38 | 0.42 | 100% |
| Delegated 321 | 14.8% | 0.38 | 0.42 | 100% |
| ARA-Top | 14.6% | 0.48 | 0.08 | 100% |
| DMS | 14.5% | 0.34 | 0.06 | 100% |
| CW else Score loser | 13.9% | 0.38 | 0.41 | 96% |
| Geometric median | 13.6% | 0.44 | 0.15 | 100% |
| ARA-C | 12.3% | 0.38 | 0.16 | 100% |
| ESR | 11.7% | 0.32 | 0.08 | 100% |
| Score | 11.7% | 0.30 | 0.08 | 100% |
| Score runoff | 11.2% | 0.19 | 0.22 | 100% |
| Borda | 10.8% | 0.05 | 0.56 | 100% |
| Borda-ER | 10.8% | 0.05 | 0.56 | 100% |
| Soft Minimax-40 | 10.7% | 0.29 | 0.08 | 100% |
| STLR | 10.6% | 0.24 | 0.23 | 100% |
| ARA | 10.5% | 0.29 | 0.09 | 100% |
| ESR-MARS | 10.4% | 0.28 | 0.07 | 100% |
| Soft Minimax-L | 10.1% | 0.27 | 0.06 | 100% |
| Score+ | 10.1% | 0.25 | 0.08 | 100% |
| STAR | 9.9% | 0.27 | 0.18 | 100% |
| INS | 9.5% | 0.25 | 0.09 | 100% |
| IC-MARS | 9.5% | 0.26 | 0.08 | 100% |
| IMDS | 9.4% | 0.25 | 0.10 | 100% |
| Soft Minimax | 8.7% | 0.24 | 0.09 | 100% |
| MARS-T | 8.5% | 0.22 | 0.06 | 100% |
| MARS | 8.5% | 0.23 | 0.07 | 100% |
| Smith//Approval | 8.3% | 0.22 | 0.07 | 100% |
| Soft Minimax-D | 7.9% | 0.21 | 0.06 | 100% |
| Smith//Score | 7.6% | 0.21 | 0.06 | 100% |
| BTR-score | 7.5% | 0.21 | 0.06 | 100% |
| MMWV+Score | 7.1% | 0.20 | 0.06 | 100% |
| MMWVxScore | 7.1% | 0.20 | 0.06 | 100% |
| Coombs | 6.1% | 0.01 | 0.15 | 100% |
| Copeland | 4.0% | 0.01 | 0.07 | 100% |
| Condorcet-random-drop | 3.5% | 0.01 | 0.06 | 100% |
| Benham | 3.2% | 0.02 | 0.03 | 100% |
| SICT | 3.2% | 0.05 | 0.05 | 100% |
| BTR-IRV | 3.0% | 0.03 | 0.04 | 100% |
| Ranked pairs | 3.0% | 0.02 | 0.05 | 100% |
| Black | 2.9% | 0.02 | 0.06 | 100% |
| Baldwin | 2.9% | 0.02 | 0.03 | 100% |
| Baldwin-ER | 2.9% | 0.02 | 0.03 | 100% |
| Copeland//Margins-ER | 2.9% | 0.02 | 0.06 | 100% |
| Copeland//Margins | 2.9% | 0.02 | 0.06 | 100% |
| BTR-Borda | 2.8% | 0.02 | 0.06 | 100% |
| Schulze | 2.8% | 0.01 | 0.04 | 100% |
| Minimax | 2.8% | 0.01 | 0.04 | 100% |
| MMWV | 2.8% | 0.01 | 0.04 | 100% |
| MMPO | 2.8% | 0.01 | 0.04 | 100% |
| Schulze-ER | 2.8% | 0.01 | 0.04 | 100% |
| Minimax-ER | 2.8% | 0.01 | 0.04 | 100% |
| MMPOWV | 2.8% | 0.01 | 0.04 | 100% |
| Kemeny-Young | 2.6% | 0.02 | 0.05 | 100% |
| Nanson | 2.5% | 0.02 | 0.05 | 100% |

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
| Schulze-ER | — | — | — | — |
| Copeland//Margins-ER | — | — | — | — |
| Copeland//Margins | — | — | — | — |
| SICT | — | — | — | — |
| Condorcet-random-drop | — | — | — | — |
| Minimax-ER | — | — | — | — |
| MMPOWV | — | — | — | — |
| Black | — | — | — | — |
| Baldwin | — | — | — | — |
| Baldwin-ER | — | — | — | — |
| Nanson | — | — | — | — |
| BTR-Borda | — | — | — | — |
| Benham | — | — | — | — |
| DSC | — | — | — | — |
| DAC | — | — | — | — |
| Coombs | — | — | — | — |
| MDDA | — | — | — | — |
| MAMPO | — | — | — | — |
| MCA | — | — | — | — |
| Reciprocal score | — | — | — | — |
| INS | — | — | — | — |
| Geometric median | — | — | — | — |
| Random ballot | — | — | — | — |
| IC-MARS | — | — | — | — |
| ARA-Top | — | — | — | — |
| ARA-C | — | — | — | — |
| Smith//Approval | — | — | — | — |
| Score+ | — | — | — | — |
| Asset-foresight | — | — | — | — |
| MMWV+Score | — | — | — | — |
| MMWVxScore | — | — | — | — |
| STLR | — | — | — | — |
| Soft Minimax-L | — | — | — | — |
| Borda-ER | — | — | — | — |
| Dowdall | — | — | — | — |
| Random pair runoff | — | — | — | — |
| Anti-plurality | — | — | — | — |
| TRASH | — | — | — | — |
| CW else Score loser | — | — | — | — |
| CL else Score winner | — | — | — | — |
| IDS | — | — | — | — |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 3900 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 100 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| ESR-MARS | 90.1% (90.9%) | 7.2% (6.9%) | 2.7% (2.2%) | 81.0% (76.0%) | 95.2% (96.6%) | 88.0% |
| Soft Minimax-L | 90.6% (90.8%) | 7.2% (7.0%) | 2.2% (2.1%) | 66.0% (67.0%) | 95.0% (95.8%) | 89.2% |
| Geometric median | 89.2% (89.8%) | 7.6% (7.3%) | 3.3% (2.9%) | 68.0% (63.0%) | 92.4% (93.1%) | 85.0% |
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
| IC-MARS | 90.0% (91.0%) | 7.0% (6.6%) | 3.0% (2.4%) | 77.0% (76.0%) | 93.4% (95.9%) | 87.6% |
| Score | 89.7% (90.3%) | 7.3% (7.0%) | 3.1% (2.7%) | 74.0% (73.0%) | 89.2% (93.0%) | 83.7% |
| STLR | 87.8% (88.3%) | 7.5% (6.6%) | 4.7% (5.1%) | 68.0% (65.0%) | 90.9% (92.4%) | 85.4% |
| ESR | 90.9% (91.8%) | 6.8% (6.5%) | 2.2% (1.6%) | 84.0% (79.0%) | 95.7% (97.8%) | 88.4% |
| Soft Minimax-D | 91.7% (92.2%) | 6.4% (6.2%) | 1.8% (1.7%) | 76.0% (69.0%) | 99.6% (99.7%) | 92.4% |
| ARA-C | 89.9% (90.1%) | 8.0% (7.6%) | 2.1% (2.3%) | 71.0% (72.0%) | 96.3% (96.6%) | 88.7% |
| BTR-score | 91.9% (92.4%) | 6.1% (5.8%) | 1.9% (1.8%) | 79.0% (76.0%) | 100.0% (100.0%) | 93.1% |
| IDS | 89.6% (90.1%) | 8.2% (7.9%) | 2.2% (2.0%) | 71.0% (69.0%) | 94.0% (94.0%) | 85.8% |
| ARA-Top | 90.1% (90.2%) | 8.0% (8.1%) | 1.9% (1.7%) | 80.0% (77.0%) | 94.9% (94.9%) | 86.8% |
| Smith//Score | 91.8% (92.4%) | 6.2% (5.8%) | 2.0% (1.8%) | 77.0% (77.0%) | 100.0% (100.0%) | 92.9% |
| MDDA | 77.9% (77.1%) | 19.6% (20.6%) | 2.6% (2.3%) | 96.0% (99.0%) | 94.0% (94.4%) | 82.0% |
| Score+ | 90.2% (91.1%) | 7.2% (6.7%) | 2.7% (2.3%) | 81.0% (74.0%) | 96.5% (97.3%) | 90.6% |
| MMWVxScore | 91.7% (92.2%) | 5.9% (5.7%) | 2.3% (2.1%) | 72.0% (71.0%) | 100.0% (100.0%) | 93.2% |
| Soft Minimax | 92.2% (92.8%) | 5.8% (5.4%) | 2.0% (1.7%) | 71.0% (67.0%) | 100.0% (100.0%) | 93.3% |
| Smith//Approval | 92.2% (92.8%) | 6.4% (6.2%) | 1.5% (1.1%) | 77.0% (74.0%) | 100.0% (100.0%) | 92.7% |
| MMWV+Score | 92.0% (92.2%) | 5.9% (5.7%) | 2.1% (2.1%) | 72.0% (71.0%) | 100.0% (100.0%) | 93.3% |
| MAMPO | 78.3% (77.6%) | 19.5% (20.3%) | 2.2% (2.1%) | 96.0% (99.0%) | 95.5% (96.0%) | 82.2% |
| Reciprocal score | 89.1% (89.7%) | 8.8% (8.1%) | 2.2% (2.2%) | 68.0% (67.0%) | 92.1% (92.8%) | 84.6% |
| IMDS | 91.6% (92.8%) | 6.1% (5.4%) | 2.3% (1.7%) | 77.0% (76.0%) | 99.7% (99.8%) | 92.7% |
| INS | 91.7% (92.9%) | 6.1% (5.4%) | 2.2% (1.7%) | 78.0% (78.0%) | 99.6% (99.8%) | 92.6% |
| DAC | 96.7% (98.9%) | 2.5% (1.0%) | 0.8% (0.0%) | 41.0% (33.0%) | 84.3% (85.8%) | 83.0% |
| DSC | 96.1% (98.9%) | 3.1% (1.0%) | 0.8% (0.0%) | 45.0% (33.0%) | 85.4% (85.8%) | 83.4% |
| ER-IRV | 96.9% (98.9%) | 2.2% (0.8%) | 0.9% (0.3%) | 54.0% (44.0%) | 93.3% (93.5%) | 91.5% |
| MCA | 81.7% (80.5%) | 13.8% (13.8%) | 4.4% (5.6%) | 79.0% (77.0%) | 86.5% (84.0%) | 82.7% |
| Supplementary vote | 90.3% (91.4%) | 9.1% (8.3%) | 0.6% (0.2%) | 65.0% (57.0%) | 89.2% (89.3%) | 81.5% |
| Black | 96.3% (98.4%) | 1.9% (1.2%) | 1.8% (0.4%) | 64.0% (49.0%) | 100.0% (100.0%) | 96.7% |
| MMPOWV | 97.3% (98.4%) | 1.6% (1.2%) | 1.1% (0.4%) | 41.0% (37.0%) | 100.0% (100.0%) | 97.7% |
| MMWV | 97.4% (98.4%) | 1.5% (1.2%) | 1.1% (0.4%) | 40.0% (37.0%) | 100.0% (100.0%) | 97.8% |
| Ranked pairs | 97.6% (98.4%) | 1.5% (1.2%) | 0.9% (0.4%) | 45.0% (40.0%) | 100.0% (100.0%) | 97.9% |
| Schulze | 97.5% (98.4%) | 1.5% (1.2%) | 1.0% (0.4%) | 41.0% (37.0%) | 100.0% (100.0%) | 97.9% |
| MMPO | 97.3% (98.4%) | 1.6% (1.2%) | 1.1% (0.4%) | 41.0% (37.0%) | 100.0% (100.0%) | 97.7% |
| Minimax | 97.4% (98.4%) | 1.5% (1.2%) | 1.0% (0.4%) | 40.0% (37.0%) | 100.0% (100.0%) | 97.9% |
| Schulze-ER | 97.4% (98.4%) | 1.6% (1.2%) | 1.1% (0.4%) | 41.0% (37.0%) | 100.0% (100.0%) | 97.7% |
| Condorcet-random-drop | 97.3% (98.2%) | 1.5% (1.1%) | 1.2% (0.7%) | 31.0% (23.0%) | 100.0% (100.0%) | 97.9% |
| Minimax-ER | 97.3% (98.4%) | 1.6% (1.2%) | 1.1% (0.4%) | 40.0% (37.0%) | 100.0% (100.0%) | 97.7% |
| Dowdall | 92.5% (96.1%) | 5.4% (3.8%) | 2.0% (0.2%) | 58.0% (43.0%) | 83.4% (84.6%) | 80.0% |
| Copeland//Margins | 97.0% (98.5%) | 1.6% (1.1%) | 1.4% (0.4%) | 47.0% (47.0%) | 100.0% (100.0%) | 97.4% |
| BTR-IRV | 96.2% (98.6%) | 2.2% (1.2%) | 1.6% (0.3%) | 70.0% (64.0%) | 100.0% (100.0%) | 96.6% |
| Copeland//Margins-ER | 97.0% (98.5%) | 1.6% (1.1%) | 1.4% (0.4%) | 48.0% (47.0%) | 100.0% (100.0%) | 97.4% |
| Kemeny-Young | 97.4% (98.4%) | 1.7% (1.2%) | 1.0% (0.4%) | 48.0% (42.0%) | 100.0% (100.0%) | 97.8% |
| BTR-Borda | 96.6% (98.4%) | 1.9% (1.2%) | 1.5% (0.4%) | 53.0% (45.0%) | 100.0% (100.0%) | 97.1% |
| Benham | 96.0% (98.5%) | 1.9% (1.1%) | 2.1% (0.4%) | 68.0% (65.0%) | 100.0% (100.0%) | 96.4% |
| Nanson | 97.2% (98.5%) | 1.7% (1.2%) | 1.1% (0.3%) | 51.0% (45.0%) | 100.0% (100.0%) | 97.9% |
| Copeland | 97.5% (98.3%) | 1.4% (1.2%) | 1.1% (0.6%) | 41.0% (40.0%) | 100.0% (100.0%) | 97.9% |
| SICT | 96.3% (98.6%) | 2.2% (1.2%) | 1.5% (0.2%) | 77.0% (74.0%) | 99.7% (100.0%) | 96.6% |
| Baldwin-ER | 97.2% (98.5%) | 1.9% (1.2%) | 0.9% (0.2%) | 56.0% (51.0%) | 100.0% (100.0%) | 97.5% |
| Baldwin | 97.1% (98.5%) | 2.0% (1.2%) | 0.9% (0.2%) | 58.0% (51.0%) | 100.0% (100.0%) | 97.4% |
| IRV-3 | 88.4% (91.1%) | 9.1% (6.8%) | 2.5% (2.1%) | 83.0% (81.0%) | 96.8% (97.5%) | 87.4% |
| IRV | 94.5% (98.9%) | 4.2% (0.8%) | 1.3% (0.3%) | 61.0% (44.0%) | 92.7% (93.5%) | 89.1% |
| Coombs | 94.4% (98.6%) | 3.5% (1.1%) | 2.2% (0.4%) | 40.0% (25.0%) | 96.3% (96.2%) | 92.3% |
| Borda-ER | 93.1% (98.8%) | 2.6% (1.0%) | 4.3% (0.2%) | 40.0% (11.0%) | 84.1% (85.1%) | 81.0% |
| Borda | 93.7% (98.8%) | 2.5% (1.0%) | 3.8% (0.2%) | 37.0% (11.0%) | 83.6% (85.1%) | 81.0% |
| CW else Score loser | 87.2% (88.5%) | 8.6% (7.7%) | 4.2% (3.8%) | 94.0% (94.0%) | 100.0% (100.0%) | 87.3% |
| Random pair runoff | 97.8% (98.2%) | 1.2% (1.1%) | 1.0% (0.7%) | 7.0% (6.0%) | 52.4% (52.2%) | 52.2% |
| Random ballot | 81.5% (81.2%) | 8.1% (8.0%) | 10.4% (10.8%) | 52.0% (52.0%) | 24.6% (24.5%) | 25.2% |
| TRASH | 50.0% (77.5%) | 42.6% (21.7%) | 7.4% (0.8%) | 71.0% (44.0%) | 14.7% (19.0%) | 15.6% |
| CL else Score winner | 84.6% (86.1%) | 11.5% (10.2%) | 3.9% (3.7%) | 70.0% (68.0%) | 34.6% (38.0%) | 31.1% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| ESR-MARS | 0.1% (0.0%) | 11.1% (10.7%) | 0.9% (0.9%) |
| Score runoff | 0.3% (0.2%) | — (—) | — (—) |
| Soft Minimax-L | 0.0% (0.0%) | 10.8% (10.6%) | 0.9% (0.9%) |
| Geometric median | 0.0% (0.0%) | 12.3% (11.9%) | 1.1% (1.0%) |
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
| IC-MARS | 0.4% (0.1%) | 11.4% (10.8%) | 0.9% (0.9%) |
| Score | 0.4% (0.1%) | 11.8% (11.1%) | 1.0% (0.9%) |
| STLR | 0.1% (0.1%) | 15.5% (15.4%) | 1.2% (1.2%) |
| Approval runoff | 0.8% (0.4%) | — (—) | — (—) |
| ESR | 0.4% (0.2%) | 11.3% (10.6%) | 1.0% (0.9%) |
| Soft Minimax-D | 0.1% (0.1%) | 10.9% (11.1%) | 0.9% (0.9%) |
| ARA-C | 0.4% (0.2%) | 12.3% (12.3%) | 1.0% (1.0%) |
| Approval cond. runoff | 0.7% (0.4%) | — (—) | — (—) |
| BTR-score | 0.0% (0.0%) | 10.7% (10.7%) | 0.9% (0.9%) |
| IDS | 0.3% (0.1%) | 9.8% (9.8%) | 0.8% (0.9%) |
| Approval | 1.1% (0.6%) | — (—) | — (—) |
| ARA-Top | 0.2% (0.1%) | 9.4% (9.7%) | 0.9% (0.9%) |
| Smith//Score | 0.0% (0.0%) | 10.7% (10.6%) | 0.9% (0.9%) |
| MDDA | 1.1% (0.6%) | 19.7% (19.3%) | 1.6% (1.6%) |
| Score+ | 0.2% (0.1%) | 11.5% (10.8%) | 0.9% (0.9%) |
| MMWVxScore | 0.0% (0.0%) | 10.8% (11.1%) | 0.9% (0.9%) |
| Soft Minimax | 1.1% (0.9%) | 11.2% (11.2%) | 1.0% (1.0%) |
| Smith//Approval | 0.1% (0.1%) | 10.5% (10.2%) | 0.9% (0.8%) |
| MMWV+Score | 0.0% (0.0%) | 10.5% (11.0%) | 0.9% (0.9%) |
| MAMPO | 0.9% (0.4%) | 19.1% (18.9%) | 1.6% (1.6%) |
| Reciprocal score | 0.0% (0.0%) | 10.0% (9.9%) | 0.9% (0.8%) |
| IMDS | 1.4% (0.8%) | 11.7% (11.3%) | 1.0% (1.0%) |
| INS | 1.4% (0.8%) | 11.6% (11.2%) | 1.0% (1.0%) |
| DAC | 1.2% (0.6%) | 1.3% (0.0%) | 0.1% (0.0%) |
| DSC | 1.2% (0.6%) | 0.4% (0.0%) | 0.0% (0.0%) |
| ER-IRV | 1.2% (0.8%) | 0.7% (0.0%) | 0.0% (0.0%) |
| MCA | 1.8% (2.5%) | 17.5% (18.9%) | 1.4% (1.5%) |
| Supplementary vote | 0.8% (0.8%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Black | 0.1% (0.1%) | 0.9% (0.0%) | 0.1% (0.0%) |
| TTR | 1.1% (0.7%) | — (—) | — (—) |
| MMPOWV | 0.9% (0.9%) | 0.5% (0.0%) | 0.0% (0.0%) |
| MMWV | 0.9% (0.9%) | 0.5% (0.0%) | 0.0% (0.0%) |
| Ranked pairs | 0.8% (0.8%) | 0.1% (0.0%) | 0.0% (0.0%) |
| Schulze | 0.8% (0.9%) | 0.2% (0.0%) | 0.0% (0.0%) |
| MMPO | 0.9% (0.9%) | 0.5% (0.0%) | 0.0% (0.0%) |
| Minimax | 0.9% (0.9%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Schulze-ER | 0.9% (0.9%) | 0.6% (0.0%) | 0.0% (0.0%) |
| Condorcet-random-drop | 2.9% (2.6%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Minimax-ER | 0.9% (0.9%) | 0.6% (0.0%) | 0.0% (0.0%) |
| Dowdall | 0.0% (0.0%) | 2.1% (0.0%) | 0.2% (0.0%) |
| Copeland//Margins | 0.0% (0.0%) | 0.5% (0.0%) | 0.0% (0.0%) |
| BTR-IRV | 0.1% (0.1%) | 0.3% (0.0%) | 0.0% (0.0%) |
| Copeland//Margins-ER | 0.0% (0.0%) | 0.9% (0.0%) | 0.1% (0.0%) |
| Kemeny-Young | 0.5% (0.5%) | 0.2% (0.0%) | 0.0% (0.0%) |
| BTR-Borda | 0.0% (0.0%) | 0.7% (0.0%) | 0.1% (0.0%) |
| Benham | 0.2% (0.1%) | 0.3% (0.0%) | 0.0% (0.0%) |
| Nanson | 0.1% (0.1%) | 0.6% (0.0%) | 0.0% (0.0%) |
| Copeland | 1.4% (1.1%) | 0.6% (0.0%) | 0.1% (0.0%) |
| SICT | 0.1% (0.0%) | 0.9% (0.0%) | 0.1% (0.0%) |
| Baldwin-ER | 0.3% (0.1%) | 0.8% (0.0%) | 0.1% (0.0%) |
| Baldwin | 0.2% (0.1%) | 0.4% (0.0%) | 0.0% (0.0%) |
| IRV-3 | 0.8% (0.8%) | 10.5% (10.5%) | 0.8% (0.7%) |
| IRV | 0.8% (0.8%) | 0.2% (0.0%) | 0.0% (0.0%) |
| Coombs | 0.6% (0.9%) | 1.4% (0.0%) | 0.1% (0.0%) |
| Asset | 1.2% (1.1%) | — (—) | — (—) |
| Borda-ER | 0.5% (0.4%) | 4.5% (0.0%) | 0.3% (0.0%) |
| Borda | 0.5% (0.4%) | 3.7% (0.0%) | 0.2% (0.0%) |
| Asset-foresight | 1.5% (1.9%) | — (—) | — (—) |
| Plurality | 1.4% (1.1%) | — (—) | — (—) |
| CW else Score loser | 0.0% (0.1%) | 16.2% (15.3%) | 1.1% (1.0%) |
| Random pair runoff | 100.0% (100.0%) | 1.5% (0.0%) | 0.1% (0.0%) |
| Anti-plurality | 74.1% (86.8%) | — (—) | — (—) |
| Random ballot | 100.0% (100.0%) | 57.6% (57.4%) | 2.8% (2.8%) |
| TRASH | 27.8% (64.5%) | 7.3% (0.0%) | 0.4% (0.0%) |
| CL else Score winner | 0.3% (0.1%) | 47.3% (48.7%) | 3.0% (3.1%) |

![strategy arrows](arrows-c1.png)

![strategy shifts](shift-c1.png)

![sincerity](sincerity-c1.png)

![fidelity](fidelity-c1.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c1.png)

## c2: max score 99

2000 elections (seeds 1), 1957 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..99; nomination: exits 1, clones 2. Electorates: Condorcet winner in 97.9%; it also has the highest total utility in 69.8% of those; electing it loses 14.2% of VSE; favourite ÷ least liked 8.7 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 84.6 to 85.1; with the best VSE (100) WME 78.7 (nothing lower is possible there either); the most balanced point VSE 95.0 / WME 96.4. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 90.9 (at WME 92.5); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–99) at most VSE 90.8 (at WME 92.5); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -53.6 / WME -47.7. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 84.6 / WME 99.9. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 87.9 / 96.3; normalized utility winner 90.9 / 92.5; best worst margin 84.8 / 100.0; Condorcet winner, else random ballot 84.0 / 99.0; Condorcet winner, else utility winner 86.0 / 99.6; random pair, majority 15.3 / 2.9; nearest the median voter 88.6 / 96.7; nearest the mean voter 75.6 / 92.4.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Soft Minimax-L, Score runoff, Soft Minimax-40, Soft Minimax, IC-MARS, MARS, Soft Minimax-D, MMWVxScore, MMWV+Score.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Score runoff, Soft Minimax-40, Soft Minimax, Soft Minimax-D, MMWVxScore, MMWV+Score, Smith//Score, Smith//Approval.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 84.0 / 99.0; its mirror on the utility side 89.9 / 91.6; random pair 15.3 / 2.9. No better than a lottery of them (Lead ≤ 0): with learned ballots DMS, Score, IDS, ESR-MARS, ESR, STLR, MAV, MJ, Reciprocal score, ARA-Top; with honest ballots IDS, MAV, MJ, Reciprocal score, ARA-Top.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Soft Minimax-L | hybrid | 87.8 [86.4–89.1] | 96.0 [95.4–96.7] | 0.8 [0.1–1.4] | 1.4 [0.5–2.3] | 70 [63–77] | 100.0 [100.0–100.0] | 99.2 | 99.7 | 87.7 [86.2–89.0] | 96.3 [95.7–96.9] | 0.8 [0.2–1.5] | 1.3 [0.4–2.2] | 71 [64–77] | 89.3 [87.8–90.6] | 88.8 [87.4–90.2] | 98.1 [97.4–98.8] | 97.4 [96.6–98.2] | 97.5 [96.8–98.2] | 96.9 [96.0–97.7] | 3.4 [2.6–4.2] | 3.5 [2.7–4.3] | 0.0% | 0.1% | 0.0% | 1.7% | Soft Minimax-40 (WME\*) |
| STAR | hybrid | 87.6 [86.1–89.0] | 95.9 [95.1–96.6] | 0.6 [0.0–1.2] | 1.5 [0.6–2.5] | 67 [61–75] | 99.9 [99.9–99.9] | 98.3 | 99.7 | 88.1 [86.7–89.5] | 96.9 [96.3–97.4] | 1.3 [0.8–1.9] | 0.8 [-0.0–1.7] | 78 [71–88] | 90.7 [89.5–92.0] | 89.6 [88.3–91.0] | 99.7 [99.4–99.9] | 98.1 [97.4–98.7] | 99.0 [98.5–99.4] | 97.5 [96.8–98.2] | 3.2 [2.5–4.0] | 3.6 [2.8–4.5] | 0.0% | 0.8% | 0.0% | 3.3% | Score runoff (WME\*), Soft Minimax-40 (WME\*) |
| Score runoff | two-round | 87.5 [86.0–88.9] | 96.6 [95.9–97.2] | 0.9 [0.3–1.4] | 1.3 [0.4–2.3] | 71 [65–78] | 99.9 [99.8–99.9] | 97.8 | 99.7 | 88.2 [86.8–89.6] | 96.9 [96.3–97.4] | 1.4 [0.9–2.0] | 0.8 [-0.1–1.7] | 79 [72–90] | 90.8 [89.5–92.0] | 90.5 [89.2–91.8] | 99.8 [99.6–100.0] | 98.7 [98.1–99.2] | 99.1 [98.6–99.4] | 98.0 [97.4–98.7] | 3.2 [2.5–4.0] | 3.4 [2.6–4.2] | 0.0% | 1.5% | 0.0% | 3.4% | — |
| Soft Minimax-40 | hybrid | 87.4 [86.0–88.7] | 96.9 [96.3–97.5] | 1.0 [0.4–1.5] | 1.2 [0.4–2.1] | 73 [67–79] | 100.0 [100.0–100.0] | 98.8 | 99.7 | 87.4 [85.9–88.7] | 97.3 [96.8–97.8] | 1.1 [0.6–1.7] | 1.1 [0.3–1.9] | 76 [71–82] | 91.6 [90.4–92.8] | 90.9 [89.6–92.2] | 98.9 [98.3–99.4] | 98.1 [97.4–98.7] | 98.2 [97.5–98.8] | 97.4 [96.7–98.0] | 3.2 [2.5–4.0] | 3.2 [2.5–4.0] | 0.0% | 0.1% | 0.0% | 2.9% | — |
| DMS | rated | 87.1 [85.7–88.4] | 92.7 [91.7–93.7] | -1.1 [-1.8–-0.3] | 2.9 [1.9–4.0] | 53 [47–59] | 100.0 [100.0–100.0] | 97.5 | 99.7 | 87.8 [86.3–89.0] | 94.9 [94.1–95.5] | 0.2 [-0.4–0.9] | 1.8 [0.9–2.8] | 63 [59–70] | 86.1 [84.5–87.5] | 84.2 [82.6–85.8] | 96.5 [95.6–97.5] | 92.8 [91.4–93.9] | 96.2 [95.3–97.0] | 92.9 [91.8–94.0] | 3.5 [2.7–4.2] | 3.5 [2.8–4.3] | 0.0% | 0.1% | 0.0% | 7.9% | ESR (WME\*), STAR (WME\*), Score runoff (WME\*) +7 more |
| Score | rated | 87.1 [85.5–88.5] | 92.1 [91.1–93.1] | -1.4 [-2.1–-0.6] | 3.1 [2.0–4.3] | 51 [45–58] | 100.0 [100.0–100.0] | 97.4 | 99.7 | 87.3 [85.8–88.7] | 95.5 [94.8–96.1] | 0.3 [-0.3–0.9] | 1.8 [1.0–2.8] | 64 [59–70] | 86.8 [85.3–88.4] | 82.2 [80.4–83.9] | 96.8 [95.9–97.6] | 91.3 [89.9–92.6] | 96.3 [95.5–97.1] | 91.7 [90.5–92.8] | 3.5 [2.6–4.3] | 3.4 [2.6–4.2] | 0.0% | 0.1% | 0.0% | 8.8% | MJ (WME\*), ESR (WME\*), STAR (WME\*) +9 more |
| IDS | rated | 86.6 [85.1–88.1] | 93.3 [92.3–94.2] | -1.1 [-1.8–-0.3] | 3.1 [1.9–4.1] | 52 [47–58] | 100.0 [100.0–100.0] | 97.5 | 99.7 | 86.8 [85.3–88.3] | 93.5 [92.6–94.4] | -0.9 [-1.6–-0.2] | 2.9 [1.8–4.0] | 53 [48–59] | 85.7 [84.1–87.3] | 85.5 [83.9–87.1] | 94.5 [93.3–95.6] | 93.9 [92.8–95.1] | 94.3 [93.3–95.3] | 93.8 [92.8–94.8] | 4.3 [3.4–5.2] | 4.3 [3.5–5.2] | 0.0% | 0.1% | 0.0% | 8.2% | Soft Minimax-L (VSE\* WME\*), ESR (WME\*), STAR (WME\*) +11 more |
| Soft Minimax | hybrid | 86.6 [85.1–88.1] | 98.2 [97.7–98.5] | 1.1 [0.6–1.6] | 1.0 [0.2–1.6] | 80 [75–86] | 100.0 [100.0–100.0] | 99.2 | 99.7 | 86.7 [85.1–88.1] | 98.3 [97.9–98.7] | 1.2 [0.7–1.7] | 0.9 [0.1–1.4] | 82 [78–88] | 94.1 [93.1–95.1] | 93.6 [92.5–94.7] | 98.9 [98.5–99.4] | 98.5 [97.8–99.0] | 98.3 [97.7–98.9] | 97.9 [97.3–98.5] | 3.4 [2.6–4.2] | 3.2 [2.5–4.1] | 0.0% | 0.1% | 0.0% | 1.7% | — |
| ESR-MARS | hybrid | 86.6 [85.1–88.0] | 95.5 [94.8–96.2] | -0.1 [-0.7–0.5] | 2.3 [1.4–3.2] | 60 [56–65] | 100.0 [100.0–100.0] | 98.2 | 99.7 | 86.9 [85.3–88.2] | 97.5 [96.9–97.9] | 0.9 [0.4–1.4] | 1.3 [0.5–2.0] | 74 [69–80] | 91.9 [90.7–93.2] | 89.1 [87.7–90.4] | 99.0 [98.5–99.4] | 96.2 [95.2–97.2] | 98.0 [97.2–98.6] | 95.5 [94.5–96.4] | 3.5 [2.8–4.4] | 3.5 [2.8–4.4] | 0.0% | 0.2% | 0.0% | 5.4% | Score runoff (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*), STAR (VSE\*) +9 more |
| ESR | rated | 86.4 [84.9–87.9] | 95.6 [94.7–96.3] | -0.2 [-0.8–0.4] | 2.4 [1.3–3.4] | 60 [55–65] | 100.0 [100.0–100.0] | 97.4 | 99.7 | 86.5 [84.9–87.9] | 97.4 [96.8–97.9] | 0.7 [0.1–1.2] | 1.5 [0.6–2.2] | 72 [66–77] | 92.7 [91.5–93.9] | 89.9 [88.6–91.2] | 99.0 [98.5–99.4] | 96.5 [95.6–97.4] | 97.7 [97.0–98.4] | 95.8 [94.8–96.7] | 3.9 [3.0–4.8] | 3.9 [3.1–4.8] | 0.0% | 0.1% | 0.0% | 8.8% | Score runoff (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*), STAR (VSE\*) +9 more |
| STLR | hybrid | 86.4 [84.9–87.9] | 94.0 [93.1–94.8] | -0.9 [-1.6–-0.1] | 3.0 [1.9–4.0] | 53 [47–59] | 100.0 [99.9–100.0] | 97.5 | 99.7 | 88.0 [86.6–89.3] | 95.5 [94.8–96.1] | 0.6 [0.0–1.3] | 1.5 [0.5–2.4] | 68 [62–76] | 86.6 [85.1–88.1] | 84.6 [83.0–86.2] | 96.8 [95.9–97.7] | 93.5 [92.3–94.7] | 96.5 [95.6–97.3] | 93.6 [92.6–94.7] | 3.1 [2.4–3.9] | 3.5 [2.7–4.3] | 0.0% | 0.5% | 0.0% | 7.3% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), Soft Minimax-L (VSE\* WME\*) +14 more |
| MAV | median-based | 86.4 [84.9–87.8] | 92.7 [91.7–93.7] | -1.5 [-2.2–-0.7] | 3.5 [2.4–4.5] | 49 [44–55] | 100.0 [100.0–100.0] | 97.7 | 99.7 | 86.6 [85.1–88.0] | 94.7 [93.8–95.4] | -0.5 [-1.1–0.2] | 2.6 [1.6–3.6] | 57 [52–62] | 87.3 [85.9–88.8] | 85.4 [83.9–87.0] | 95.2 [94.2–96.2] | 93.1 [91.9–94.4] | 94.8 [93.9–95.8] | 93.0 [91.8–94.1] | 4.1 [3.3–5.0] | 4.3 [3.5–5.2] | 0.0% | 0.1% | 0.0% | 7.2% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +14 more |
| MJ | median-based | 86.3 [84.9–87.7] | 93.3 [92.4–94.3] | -1.2 [-1.9–-0.5] | 3.3 [2.3–4.3] | 50 [46–56] | 100.0 [100.0–100.0] | 97.7 | 99.7 | 86.5 [85.0–87.9] | 94.5 [93.7–95.3] | -0.6 [-1.2–0.1] | 2.7 [1.7–3.7] | 56 [51–61] | 87.2 [85.8–88.7] | 86.0 [84.5–87.5] | 95.1 [94.1–96.1] | 93.4 [92.2–94.6] | 94.8 [94.0–95.8] | 93.5 [92.4–94.5] | 4.0 [3.2–5.0] | 4.2 [3.4–5.1] | 0.0% | 0.1% | 0.0% | 7.3% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +12 more |
| IC-MARS | hybrid | 86.3 [84.8–87.6] | 98.2 [97.8–98.6] | 0.9 [0.3–1.5] | 1.1 [0.4–1.7] | 79 [74–86] | 100.0 [100.0–100.0] | 98.4 | 99.7 | 85.9 [84.3–87.3] | 99.1 [98.8–99.3] | 1.1 [0.6–1.6] | 0.6 [0.1–1.0] | 88 [84–92] | 95.1 [94.2–96.1] | 93.5 [92.4–94.6] | 99.2 [98.7–99.6] | 98.3 [97.7–98.9] | 98.2 [97.6–98.8] | 97.3 [96.6–98.0] | 3.8 [2.9–4.6] | 3.5 [2.7–4.3] | 0.0% | 0.3% | 0.0% | 4.5% | Soft Minimax-D (WME\*) |
| ARA-C | hybrid | 86.0 [84.4–87.6] | 97.6 [97.1–98.1] | 0.5 [-0.1–1.2] | 1.5 [0.7–2.2] | 72 [67–78] | 100.0 [100.0–100.0] | 98.9 | 99.7 | 86.3 [84.7–87.9] | 98.0 [97.5–98.3] | 0.8 [0.2–1.4] | 1.2 [0.5–1.9] | 76 [71–82] | 92.4 [91.2–93.5] | 92.0 [90.8–93.2] | 98.2 [97.5–98.8] | 97.7 [97.0–98.4] | 97.3 [96.6–97.9] | 97.0 [96.2–97.7] | 3.9 [3.1–4.7] | 4.0 [3.2–5.0] | 0.0% | 0.2% | 0.0% | 3.0% | Smith//Score (WME\*), MARS (WME\*), IC-MARS (WME\*) +4 more |
| MARS | hybrid | 86.0 [84.4–87.4] | 98.4 [98.0–98.7] | 0.8 [0.3–1.3] | 1.0 [0.4–1.6] | 80 [75–84] | 100.0 [100.0–100.0] | 98.4 | 99.7 | 85.9 [84.2–87.3] | 99.1 [98.8–99.3] | 1.1 [0.6–1.6] | 0.6 [0.1–1.0] | 88 [84–92] | 95.0 [94.1–96.0] | 93.2 [92.1–94.3] | 99.2 [98.7–99.6] | 98.0 [97.3–98.7] | 98.2 [97.5–98.8] | 97.2 [96.5–97.9] | 3.8 [2.9–4.6] | 3.7 [2.9–4.5] | 0.0% | 0.2% | 0.0% | 4.4% | MMWV+Score (WME\*), MMWVxScore (WME\*), Soft Minimax-D (WME\*) |
| MARS-T | hybrid | 86.0 [84.4–87.4] | 98.4 [98.0–98.7] | 0.8 [0.3–1.3] | 1.0 [0.4–1.6] | 80 [75–84] | 100.0 [100.0–100.0] | 98.4 | 99.7 | 85.9 [84.2–87.3] | 99.1 [98.8–99.3] | 1.1 [0.6–1.6] | 0.6 [0.1–1.0] | 88 [84–92] | 95.0 [94.1–96.0] | 93.2 [92.0–94.2] | 99.2 [98.7–99.6] | 97.9 [97.2–98.6] | 98.2 [97.5–98.8] | 97.1 [96.4–97.8] | 3.8 [2.9–4.6] | 3.7 [2.9–4.5] | 0.0% | 0.2% | 0.0% | 4.4% | MMWV+Score (WME\*), MMWVxScore (WME\*), Soft Minimax-D (WME\*) |
| ARA | hybrid | 85.9 [84.4–87.4] | 98.1 [97.6–98.4] | 0.6 [0.1–1.3] | 1.3 [0.6–1.9] | 76 [70–81] | 100.0 [100.0–100.0] | 98.7 | 99.7 | 86.0 [84.4–87.5] | 98.6 [98.2–98.9] | 0.9 [0.4–1.4] | 0.9 [0.3–1.4] | 81 [77–87] | 93.8 [92.7–94.9] | 92.8 [91.6–93.9] | 98.3 [97.7–99.0] | 98.2 [97.5–98.8] | 97.5 [96.8–98.2] | 97.3 [96.6–98.0] | 3.5 [2.6–4.2] | 3.5 [2.6–4.3] | 0.0% | 0.2% | 0.0% | 3.3% | Smith//Score (WME\*), Smith//Approval (WME\*), MMWV+Score (WME\*) +2 more |
| Score+ | hybrid | 85.8 [84.1–87.2] | 97.3 [96.7–97.8] | 0.2 [-0.4–0.8] | 1.8 [1.1–2.6] | 68 [63–74] | 100.0 [100.0–100.0] | 97.4 | 99.7 | 85.8 [84.2–87.3] | 98.2 [97.8–98.6] | 0.6 [0.1–1.2] | 1.2 [0.6–1.8] | 77 [73–82] | 94.0 [93.0–95.1] | 92.4 [91.1–93.5] | 98.2 [97.6–98.8] | 96.7 [95.8–97.5] | 97.4 [96.7–98.0] | 96.0 [95.1–96.9] | 3.8 [2.9–4.7] | 3.8 [2.9–4.5] | 0.0% | 0.1% | 0.0% | 8.8% | Soft Minimax (VSE\* WME\*), Smith//Score (WME\*), MARS (WME\*) +8 more |
| Soft Minimax-D | hybrid | 85.6 [84.1–87.0] | 99.2 [99.0–99.5] | 1.0 [0.5–1.5] | 0.6 [0.1–0.9] | 89 [85–93] | 100.0 [100.0–100.0] | 99.2 | 99.7 | 85.5 [84.0–86.9] | 99.5 [99.2–99.6] | 1.0 [0.5–1.5] | 0.4 [-0.0–0.7] | 92 [88–95] | 97.5 [96.9–98.2] | 96.6 [95.8–97.4] | 99.3 [98.9–99.7] | 98.9 [98.5–99.4] | 98.7 [98.1–99.1] | 98.2 [97.5–98.7] | 4.0 [3.1–4.8] | 3.7 [2.9–4.5] | 0.0% | 0.1% | 0.0% | 1.7% | — |
| Reciprocal score | rated | 85.3 [83.7–87.0] | 91.9 [90.9–93.0] | -2.4 [-3.3–-1.5] | 4.4 [3.1–5.6] | 43 [39–48] | 99.9 [99.9–99.9] | 98.4 | 99.7 | 85.5 [83.8–87.1] | 92.7 [91.7–93.7] | -2.0 [-2.8–-1.1] | 4.0 [2.8–5.2] | 45 [41–51] | 84.4 [82.8–86.0] | 83.3 [81.7–85.1] | 93.5 [92.2–94.7] | 92.3 [91.0–93.6] | 93.5 [92.4–94.5] | 92.5 [91.3–93.5] | 4.9 [4.0–5.9] | 5.0 [4.0–5.9] | 0.0% | 0.8% | 0.0% | 3.1% | STAR (VSE\* WME\*), Score runoff (VSE\* WME\*), Soft Minimax-40 (VSE\* WME\*) +24 more |
| MMWVxScore | hybrid | 85.2 [83.7–86.7] | 99.5 [99.3–99.7] | 0.9 [0.4–1.4] | 0.5 [0.0–0.7] | 91 [88–94] | 100.0 [100.0–100.0] | 99.3 | 99.7 | 85.3 [83.7–86.8] | 99.7 [99.6–99.8] | 1.0 [0.6–1.5] | 0.3 [-0.1–0.4] | 94 [92–96] | 98.0 [97.3–98.6] | 97.6 [96.9–98.3] | 99.4 [99.0–99.8] | 99.2 [98.7–99.6] | 98.7 [98.2–99.2] | 98.5 [98.0–99.0] | 4.0 [3.1–4.9] | 4.0 [3.1–4.8] | 0.0% | 0.2% | 0.0% | 1.2% | — |
| MMWV+Score | hybrid | 85.2 [83.6–86.6] | 99.5 [99.3–99.7] | 0.9 [0.4–1.4] | 0.5 [0.0–0.7] | 91 [88–94] | 100.0 [100.0–100.0] | 99.3 | 99.7 | 85.2 [83.6–86.7] | 99.7 [99.6–99.8] | 0.9 [0.5–1.4] | 0.3 [-0.1–0.4] | 94 [92–96] | 98.0 [97.3–98.6] | 97.6 [96.9–98.3] | 99.4 [99.0–99.8] | 99.3 [98.8–99.6] | 98.7 [98.1–99.2] | 98.6 [98.0–99.1] | 4.0 [3.2–5.0] | 4.0 [3.1–4.9] | 0.0% | 0.2% | 0.0% | 1.2% | — |
| Smith//Score | condorcet-based | 85.0 [83.5–86.6] | 99.4 [99.2–99.6] | 0.8 [0.3–1.2] | 0.5 [0.1–0.8] | 90 [86–93] | 100.0 [100.0–100.0] | 99.1 | 99.7 | 85.0 [83.4–86.5] | 99.7 [99.6–99.8] | 0.8 [0.5–1.3] | 0.3 [-0.0–0.4] | 93 [92–95] | 97.9 [97.2–98.5] | 97.2 [96.4–97.9] | 99.4 [99.0–99.8] | 99.1 [98.6–99.6] | 98.8 [98.3–99.3] | 98.5 [98.0–99.0] | 4.2 [3.4–5.1] | 4.2 [3.4–5.1] | 0.0% | 0.2% | 0.0% | 1.7% | — |
| ARA-Top | hybrid | 85.0 [83.2–86.7] | 95.5 [94.7–96.1] | -1.0 [-1.9–-0.2] | 3.2 [2.2–4.2] | 55 [50–60] | 100.0 [100.0–100.0] | 98.9 | 99.7 | 85.1 [83.3–86.8] | 95.6 [94.9–96.3] | -0.9 [-1.8–-0.1] | 3.1 [2.0–4.0] | 56 [51–61] | 87.6 [86.1–89.0] | 87.0 [85.5–88.5] | 94.8 [93.6–95.8] | 94.2 [93.0–95.2] | 94.2 [93.1–95.2] | 93.7 [92.7–94.8] | 4.8 [3.8–5.7] | 4.7 [3.7–5.7] | 0.0% | 0.2% | 0.0% | 3.0% | Score runoff (VSE\* WME\*), IC-MARS (VSE\* WME\*), Soft Minimax (VSE\* WME\*) +18 more |
| Smith//Approval | condorcet-based | 84.9 [83.4–86.6] | 99.4 [99.2–99.6] | 0.7 [0.2–1.2] | 0.6 [0.1–0.8] | 89 [86–93] | 100.0 [100.0–100.0] | 99.3 | 99.7 | 84.7 [83.1–86.3] | 99.6 [99.4–99.7] | 0.7 [0.4–1.1] | 0.4 [0.0–0.6] | 92 [89–95] | 98.0 [97.4–98.6] | 97.4 [96.6–98.0] | 99.4 [99.0–99.7] | 99.1 [98.6–99.5] | 98.7 [98.1–99.1] | 98.3 [97.7–98.8] | 4.4 [3.5–5.3] | 4.2 [3.4–5.2] | 0.0% | 0.2% | 0.0% | 1.1% | — |
| INS | rated | 84.9 [83.3–86.5] | 98.7 [98.3–99.0] | 0.3 [-0.1–0.8] | 1.1 [0.6–1.6] | 79 [75–84] | 100.0 [100.0–100.0] | 97.3 | 99.7 | 84.7 [83.1–86.3] | 99.2 [98.8–99.4] | 0.5 [0.1–0.9] | 0.8 [0.3–1.1] | 85 [81–90] | 97.2 [96.5–97.9] | 95.8 [95.0–96.7] | 99.2 [98.7–99.6] | 98.5 [97.8–99.0] | 97.8 [97.1–98.4] | 97.2 [96.4–97.8] | 3.9 [3.1–4.8] | 3.8 [3.0–4.6] | 0.0% | 0.1% | 0.0% | 8.8% | Soft Minimax-D (VSE\* WME\*), Smith//Score (WME\*), BTR-score (WME\*) +6 more |
| IMDS | rated | 84.8 [83.2–86.4] | 98.6 [98.2–98.9] | 0.3 [-0.2–0.7] | 1.2 [0.7–1.7] | 78 [74–83] | 100.0 [100.0–100.0] | 97.4 | 99.7 | 84.6 [82.9–86.2] | 99.1 [98.8–99.4] | 0.4 [0.0–0.8] | 0.8 [0.4–1.1] | 85 [81–89] | 96.6 [95.7–97.3] | 95.3 [94.3–96.2] | 98.8 [98.2–99.2] | 98.0 [97.4–98.6] | 97.5 [96.9–98.2] | 96.8 [96.0–97.5] | 3.8 [2.9–4.5] | 3.6 [2.8–4.3] | 0.0% | 0.1% | 0.0% | 8.8% | Soft Minimax-D (VSE\* WME\*), Smith//Score (WME\*), BTR-score (WME\*) +7 more |
| BTR-score | hybrid | 84.8 [83.2–86.4] | 99.4 [99.1–99.6] | 0.6 [0.2–1.1] | 0.6 [0.2–0.8] | 89 [85–92] | 100.0 [100.0–100.0] | 99.1 | 99.7 | 85.0 [83.5–86.6] | 99.7 [99.5–99.8] | 0.9 [0.5–1.3] | 0.3 [-0.1–0.4] | 93 [92–95] | 97.9 [97.2–98.5] | 96.9 [96.1–97.6] | 99.4 [99.0–99.8] | 98.9 [98.4–99.4] | 98.6 [98.0–99.1] | 98.2 [97.5–98.7] | 4.3 [3.5–5.2] | 4.3 [3.5–5.2] | 0.0% | 0.3% | 0.0% | 1.6% | Soft Minimax-D (VSE\*) |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Score+ | 9.1% |
| ESR | 9.1% |
| Score | 9.1% |
| INS | 9.1% |
| IMDS | 9.0% |
| IDS | 8.5% |
| DMS | 8.3% |
| STLR | 7.8% |
| MJ | 7.5% |
| MAV | 7.5% |
| ESR-MARS | 5.8% |
| Score runoff | 5.0% |
| IC-MARS | 4.9% |
| MARS-T | 4.8% |
| MARS | 4.8% |
| Reciprocal score | 4.1% |
| STAR | 4.1% |
| ARA | 3.5% |
| ARA-Top | 3.4% |
| ARA-C | 3.3% |
| Soft Minimax-40 | 3.1% |
| Smith//Score | 2.0% |
| BTR-score | 1.9% |
| Soft Minimax-L | 1.9% |
| Soft Minimax-D | 1.8% |
| Soft Minimax | 1.8% |
| MMWVxScore | 1.4% |
| MMWV+Score | 1.4% |
| Smith//Approval | 1.3% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Reciprocal score | 17.2% | 0.49 | 0.18 | 100% |
| MAV | 16.2% | 0.41 | 0.08 | 100% |
| MJ | 16.0% | 0.39 | 0.06 | 100% |
| ARA-Top | 14.1% | 0.43 | 0.30 | 99% |
| IDS | 14.0% | 0.42 | 0.15 | 100% |
| DMS | 13.5% | 0.31 | 0.04 | 100% |
| STLR | 11.5% | 0.17 | 0.21 | 100% |
| Score | 10.9% | 0.26 | 0.04 | 100% |
| Score runoff | 10.4% | 0.16 | 0.19 | 100% |
| STAR | 10.1% | 0.16 | 0.18 | 100% |
| ARA-C | 9.8% | 0.31 | 0.14 | 100% |
| ESR | 9.8% | 0.25 | 0.09 | 100% |
| Soft Minimax-L | 9.4% | 0.25 | 0.04 | 100% |
| Score+ | 9.3% | 0.17 | 0.06 | 100% |
| ESR-MARS | 9.0% | 0.25 | 0.05 | 100% |
| Soft Minimax-40 | 9.0% | 0.23 | 0.04 | 100% |
| ARA | 8.0% | 0.19 | 0.07 | 100% |
| Soft Minimax | 6.4% | 0.14 | 0.04 | 100% |
| IMDS | 5.9% | 0.08 | 0.08 | 100% |
| MARS-T | 5.7% | 0.15 | 0.04 | 100% |
| MARS | 5.6% | 0.14 | 0.04 | 100% |
| IC-MARS | 5.6% | 0.14 | 0.04 | 100% |
| INS | 5.0% | 0.06 | 0.05 | 100% |
| Soft Minimax-D | 3.8% | 0.08 | 0.04 | 100% |
| Smith//Approval | 3.4% | 0.06 | 0.05 | 100% |
| BTR-score | 2.9% | 0.05 | 0.04 | 100% |
| MMWVxScore | 2.7% | 0.04 | 0.04 | 100% |
| Smith//Score | 2.6% | 0.05 | 0.04 | 100% |
| MMWV+Score | 2.6% | 0.04 | 0.04 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 1957 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 43 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Soft Minimax-L | 96.5% (97.0%) | 1.7% (1.3%) | 1.7% (1.7%) | 44.2% (39.5%) | 90.7% (90.9%) | 88.8% |
| STAR | 94.9% (96.6%) | 3.1% (2.1%) | 2.0% (1.3%) | 41.9% (32.6%) | 93.1% (92.3%) | 89.6% |
| Soft Minimax-40 | 96.8% (97.4%) | 1.9% (1.3%) | 1.3% (1.3%) | 41.9% (37.2%) | 92.4% (93.0%) | 90.9% |
| DMS | 95.7% (96.7%) | 2.6% (1.8%) | 1.7% (1.5%) | 41.9% (25.6%) | 85.8% (87.9%) | 84.2% |
| Score | 95.4% (96.9%) | 2.8% (1.6%) | 1.9% (1.5%) | 39.5% (25.6%) | 84.5% (88.5%) | 82.2% |
| IDS | 95.2% (96.7%) | 2.8% (1.8%) | 2.0% (1.5%) | 37.2% (30.2%) | 87.4% (87.4%) | 85.5% |
| Soft Minimax | 97.0% (97.5%) | 1.7% (1.2%) | 1.3% (1.3%) | 34.9% (30.2%) | 95.1% (95.3%) | 93.6% |
| ESR-MARS | 95.2% (96.6%) | 2.7% (1.8%) | 2.0% (1.5%) | 60.5% (48.8%) | 91.8% (93.9%) | 89.1% |
| ESR | 95.0% (96.3%) | 3.2% (2.5%) | 1.8% (1.2%) | 46.5% (41.9%) | 92.7% (95.0%) | 89.9% |
| STLR | 94.7% (96.5%) | 3.1% (2.0%) | 2.1% (1.4%) | 34.9% (14.0%) | 87.0% (88.4%) | 84.6% |
| MAV | 96.1% (97.0%) | 2.2% (1.5%) | 1.6% (1.5%) | 44.2% (34.9%) | 87.5% (89.0%) | 85.4% |
| MJ | 96.1% (97.1%) | 2.0% (1.4%) | 1.9% (1.5%) | 41.9% (32.6%) | 87.7% (88.6%) | 86.0% |
| IC-MARS | 95.0% (96.8%) | 2.6% (1.7%) | 2.4% (1.5%) | 58.1% (48.8%) | 96.4% (97.0%) | 93.5% |
| ARA-C | 96.2% (96.9%) | 2.1% (1.7%) | 1.6% (1.3%) | 44.2% (44.2%) | 93.8% (94.1%) | 92.0% |
| MARS | 95.6% (96.7%) | 2.4% (1.7%) | 2.0% (1.6%) | 55.8% (48.8%) | 95.9% (97.3%) | 93.2% |
| MARS-T | 95.6% (96.7%) | 2.5% (1.7%) | 2.0% (1.6%) | 55.8% (48.8%) | 95.9% (97.3%) | 93.2% |
| ARA | 96.2% (97.1%) | 2.1% (1.5%) | 1.7% (1.4%) | 44.2% (44.2%) | 94.6% (95.4%) | 92.8% |
| Score+ | 95.9% (97.1%) | 2.6% (1.6%) | 1.5% (1.3%) | 60.5% (53.5%) | 94.5% (95.6%) | 92.4% |
| Soft Minimax-D | 96.8% (97.5%) | 1.7% (1.5%) | 1.5% (1.0%) | 51.2% (46.5%) | 98.6% (99.1%) | 96.6% |
| Reciprocal score | 94.6% (96.1%) | 3.3% (2.2%) | 2.1% (1.7%) | 32.6% (23.3%) | 85.1% (86.0%) | 83.3% |
| MMWVxScore | 96.5% (97.2%) | 1.9% (1.6%) | 1.5% (1.2%) | 44.2% (41.9%) | 100.0% (100.0%) | 97.6% |
| MMWV+Score | 96.6% (97.2%) | 1.9% (1.6%) | 1.4% (1.2%) | 48.8% (44.2%) | 100.0% (100.0%) | 97.6% |
| Smith//Score | 96.4% (97.2%) | 1.8% (1.5%) | 1.8% (1.3%) | 51.2% (46.5%) | 100.0% (100.0%) | 97.2% |
| ARA-Top | 95.1% (96.3%) | 2.6% (2.0%) | 2.2% (1.7%) | 39.5% (32.6%) | 89.4% (89.4%) | 87.0% |
| Smith//Approval | 96.5% (97.2%) | 1.8% (1.5%) | 1.7% (1.3%) | 62.8% (58.1%) | 100.0% (100.0%) | 97.4% |
| INS | 95.5% (97.3%) | 2.8% (1.7%) | 1.7% (1.0%) | 53.5% (51.2%) | 99.1% (99.3%) | 95.8% |
| IMDS | 95.2% (97.2%) | 2.8% (1.4%) | 2.0% (1.4%) | 65.1% (58.1%) | 98.6% (98.4%) | 95.3% |
| BTR-score | 96.2% (97.2%) | 1.9% (1.5%) | 1.9% (1.3%) | 55.8% (53.5%) | 100.0% (100.0%) | 96.9% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| Soft Minimax-L | 0.0% (0.0%) | 6.8% (6.5%) | 0.5% (0.5%) |
| STAR | 0.0% (0.0%) | 7.7% (7.0%) | 0.6% (0.5%) |
| Score runoff | 0.0% (0.0%) | — (—) | — (—) |
| Soft Minimax-40 | 0.0% (0.0%) | 7.3% (6.5%) | 0.5% (0.5%) |
| DMS | 0.0% (0.0%) | 7.8% (6.2%) | 0.6% (0.5%) |
| Score | 0.0% (0.0%) | 8.2% (5.9%) | 0.6% (0.4%) |
| IDS | 0.0% (0.0%) | 8.5% (6.1%) | 0.7% (0.5%) |
| Soft Minimax | 0.0% (0.0%) | 6.8% (6.7%) | 0.5% (0.5%) |
| ESR-MARS | 0.0% (0.0%) | 8.2% (6.7%) | 0.6% (0.5%) |
| ESR | 0.2% (0.1%) | 9.2% (6.5%) | 0.7% (0.5%) |
| STLR | 0.1% (0.0%) | 8.3% (7.0%) | 0.6% (0.5%) |
| MAV | 0.3% (0.0%) | 7.5% (6.2%) | 0.6% (0.5%) |
| MJ | 0.0% (0.0%) | 8.2% (6.2%) | 0.7% (0.5%) |
| IC-MARS | 0.0% (0.0%) | 7.6% (6.8%) | 0.6% (0.5%) |
| ARA-C | 0.4% (0.2%) | 7.0% (6.2%) | 0.5% (0.5%) |
| MARS | 0.1% (0.0%) | 7.5% (6.8%) | 0.6% (0.5%) |
| MARS-T | 0.0% (0.0%) | 7.6% (6.9%) | 0.6% (0.5%) |
| ARA | 0.5% (0.4%) | 6.8% (6.4%) | 0.5% (0.5%) |
| Score+ | 0.0% (0.0%) | 9.0% (6.8%) | 0.6% (0.5%) |
| Soft Minimax-D | 0.1% (0.1%) | 7.2% (6.9%) | 0.5% (0.5%) |
| Reciprocal score | 0.0% (0.0%) | 7.0% (6.5%) | 0.5% (0.5%) |
| MMWVxScore | 0.0% (0.0%) | 7.7% (7.4%) | 0.5% (0.5%) |
| MMWV+Score | 0.0% (0.0%) | 7.7% (7.4%) | 0.5% (0.5%) |
| Smith//Score | 0.0% (0.0%) | 8.2% (7.3%) | 0.6% (0.5%) |
| ARA-Top | 0.7% (0.2%) | 7.6% (6.4%) | 0.6% (0.5%) |
| Smith//Approval | 0.1% (0.0%) | 7.4% (7.2%) | 0.6% (0.5%) |
| INS | 0.9% (0.9%) | 9.9% (7.3%) | 0.7% (0.5%) |
| IMDS | 0.7% (0.5%) | 10.0% (7.5%) | 0.7% (0.5%) |
| BTR-score | 0.0% (0.0%) | 7.9% (7.3%) | 0.6% (0.5%) |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

Scatter and chain plots show only max-score; all methods are in the tables and heatmaps.
