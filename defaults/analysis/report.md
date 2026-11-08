# Ballot Front Finder Lab — analysis

Input: defaults (simulator version 2.1.0; analysis version 2.1.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

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

## c1: single condition

200 elections (seeds 1), 196 with a Condorcet winner. 99 voters, 6 candidates, model noise: 6 dimensions, voters dense where a fractal noise is high, strength 5.0, width 0.7, candidates: voters who stand; 3 poll round(s), 5 polls of 49 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.0, adaptive share 1.0, scores 0..5; nomination: exits 1, clones 2. Electorates: Condorcet winner in 98.0%; it also has the highest total utility in 73.0% of those; electing it loses 9.9% of VSE; favourite ÷ least liked 8.7 (median voter). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 89.8 to 90.1; with the best VSE (100) WME 81.8 (nothing lower is possible there either); the most balanced point VSE 95.3 / WME 98.4. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 92.2 (at WME 93.4); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–5) at most VSE 92.7 (at WME 94.0); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -52.5 / WME -45.4. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 89.9 / WME 99.9. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 91.1 / 96.7; normalized utility winner 92.2 / 93.4; best worst margin 90.0 / 100.0; Condorcet winner, else random ballot 88.9 / 98.9; Condorcet winner, else utility winner 90.2 / 99.6; random pair, majority 16.9 / 5.3; nearest the median voter 91.7 / 97.4; nearest the mean voter 77.6 / 90.5.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): ARA, Score runoff, MARS, BTR-score, BTR-IRV, Schulze, Minimax, Ranked pairs.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): BTR-score, BTR-IRV, Schulze, Minimax, Ranked pairs, Copeland.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 88.9 / 98.9; its mirror on the utility side 91.2 / 92.3; random pair 16.9 / 5.3. No better than a lottery of them (Lead ≤ 0): with learned ballots Approval, ER-IRV, IRV-3, 321, MJ, IRV, STAR, TTR, Score, Borda, Plurality; with honest ballots Approval, MJ, Score, Borda, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ARA | hybrid | 92.1 [88.5–94.9] | 96.4 [94.5–98.2] | 1.8 [-0.6–3.8] | -0.1 [-2.4–3.0] | >99 [70–>99] | 100.0 [100.0–100.0] | 93.4 | 94.2 | 91.4 [87.9–94.2] | 97.8 [96.3–99.0] | 1.6 [-0.7–3.5] | 0.5 [-1.8–2.8] | 94 [72–>99] | 91.8 [87.4–95.4] | 90.3 [86.3–94.2] | 98.2 [95.9–100.0] | 97.3 [94.7–99.4] | 97.5 [95.0–99.5] | 96.8 [94.0–99.0] | 2.5 [0.5–5.0] | 2.5 [0.5–5.0] | 0.0% | 0.1% | 0.0% | 3.6% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +8 more |
| Score runoff | two-round | 92.1 [89.3–94.9] | 96.7 [94.8–98.4] | 1.8 [0.6–3.3] | -0.1 [-2.1–2.5] | >99 [77–>99] | 99.8 [99.7–99.9] | 92.3 | 94.1 | 91.0 [87.8–94.1] | 96.2 [94.0–98.0] | 0.8 [-0.5–2.1] | 1.0 [-1.2–3.8] | 84 [61–>99] | 91.3 [87.2–94.9] | 92.9 [89.1–96.4] | 100.0 [100.0–100.0] | 99.4 [98.1–100.0] | 99.0 [97.5–100.0] | 98.5 [96.5–100.0] | 3.0 [1.0–5.5] | 2.5 [0.5–5.0] | 0.0% | 1.6% | 0.0% | 4.0% | Schulze (WME\*), Minimax (WME\*), BTR-IRV (WME\*) +3 more |
| Supplementary vote | plurality-based | 91.9 [89.0–94.5] | 89.9 [85.6–93.6] | 0.7 [-1.8–3.6] | 0.2 [-2.0–3.2] | 95 [50–>99] | 99.5 [99.4–99.7] | 77.5 | 78.1 | 91.5 [88.5–94.2] | 89.4 [84.9–92.9] | 0.3 [-2.1–3.1] | 0.7 [-1.5–3.6] | 88 [47–>99] | 82.1 [76.5–87.2] | 83.7 [78.5–88.7] | 89.8 [84.9–94.0] | 90.4 [85.7–94.5] | 89.0 [84.5–93.0] | 89.5 [85.0–93.5] | 2.5 [0.5–4.5] | 2.0 [0.5–4.0] | 1.6% | 1.0% | 0.4% | 0.0% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +15 more |
| Approval cond. runoff | two-round | 91.8 [88.4–94.7] | 91.2 [87.4–94.7] | 0.6 [-2.1–3.4] | 0.3 [-2.0–3.7] | 93 [52–>99] | 99.9 [99.8–100.0] | 73.2 | 73.6 | 91.5 [88.0–94.6] | 91.7 [88.0–95.1] | 0.3 [-2.3–2.9] | 0.6 [-1.7–4.0] | 88 [51–>99] | 84.7 [79.6–89.4] | 84.6 [79.4–89.2] | 93.1 [89.2–96.8] | 92.4 [88.3–96.1] | 92.8 [89.0–96.0] | 92.2 [88.2–95.5] | 2.0 [0.5–4.0] | 2.0 [0.5–4.0] | 0.0% | 0.7% | 0.0% | 1.9% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +12 more |
| MARS | hybrid | 91.8 [88.7–94.9] | 97.2 [95.5–98.6] | 1.7 [0.3–3.2] | 0.2 [-2.0–2.7] | 98 [80–>99] | 100.0 [100.0–100.0] | 93.1 | 94.2 | 91.4 [88.3–94.5] | 98.0 [96.6–99.1] | 1.6 [0.0–3.2] | 0.5 [-1.8–2.6] | 94 [76–>99] | 92.9 [88.8–96.4] | 91.8 [87.7–95.4] | 99.4 [98.1–100.0] | 100.0 [100.0–100.0] | 99.0 [97.5–100.0] | 99.5 [98.5–100.0] | 3.0 [1.0–5.0] | 3.0 [1.0–5.0] | 0.0% | 0.2% | 0.0% | 4.4% | Ranked pairs (WME\*), Schulze (WME\*), Copeland (WME\*) +4 more |
| BTR-score | hybrid | 91.6 [88.7–94.4] | 98.7 [97.6–99.6] | 2.0 [0.7–3.4] | 0.3 [-2.0–1.6] | 97 [83–>99] | 100.0 [99.9–100.0] | 93.5 | 94.2 | 91.2 [88.2–94.2] | 99.1 [98.3–99.7] | 1.7 [0.6–3.1] | 0.5 [-1.7–1.4] | 93 [83–>99] | 96.4 [93.4–98.5] | 95.9 [92.9–98.5] | 99.4 [98.1–100.0] | 99.4 [98.1–100.0] | 99.0 [97.5–100.0] | 99.0 [97.5–100.0] | 3.5 [1.5–6.0] | 3.0 [1.0–5.5] | 0.0% | 0.3% | 0.0% | 2.2% | — |
| Soft Minimax-D | hybrid | 91.6 [88.5–94.3] | 98.3 [97.0–99.5] | 1.8 [0.4–3.4] | 0.3 [-1.9–2.0] | 96 [82–>99] | 100.0 [100.0–100.0] | 93.8 | 94.2 | 91.3 [88.2–94.2] | 98.7 [97.7–99.5] | 1.7 [0.3–3.3] | 0.6 [-1.8–1.9] | 93 [82–>99] | 95.7 [92.6–98.2] | 95.2 [92.2–98.0] | 99.1 [97.5–100.0] | 99.1 [97.5–100.0] | 98.8 [97.0–100.0] | 98.8 [97.0–100.0] | 3.0 [1.0–5.5] | 2.5 [0.5–5.0] | 0.0% | 0.2% | 0.0% | 1.3% | Ranked pairs (WME\*), Schulze (WME\*), Copeland (WME\*) +1 more |
| MAV | median-based | 91.3 [88.2–94.4] | 94.4 [91.7–96.7] | 0.6 [-1.1–2.5] | 0.8 [-1.4–3.8] | 86 [55–>99] | 100.0 [100.0–100.0] | 93.5 | 94.5 | 91.1 [87.9–94.2] | 94.6 [91.9–96.9] | 0.5 [-1.2–2.3] | 1.0 [-1.2–4.0] | 83 [55–>99] | 88.3 [83.7–92.3] | 87.8 [83.2–92.0] | 97.0 [94.2–99.4] | 96.4 [93.4–98.8] | 96.0 [93.0–98.5] | 96.0 [93.0–98.5] | 2.5 [0.5–4.5] | 2.5 [0.5–4.5] | 0.0% | 0.1% | 0.0% | 4.5% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +10 more |
| Approval runoff | two-round | 91.2 [87.7–94.3] | 91.4 [87.9–94.7] | 0.0 [-2.6–2.7] | 0.9 [-1.5–4.4] | 83 [46–>99] | 99.8 [99.7–99.9] | 72.6 | 73.3 | 91.7 [88.2–94.7] | 91.3 [87.7–94.7] | 0.5 [-2.2–3.0] | 0.5 [-1.8–3.8] | 90 [52–>99] | 84.4 [78.9–89.3] | 85.5 [80.1–90.4] | 93.7 [89.9–97.1] | 93.7 [89.8–97.2] | 93.2 [89.8–96.5] | 93.2 [89.8–96.5] | 2.0 [0.5–4.0] | 2.0 [0.5–4.0] | 0.0% | 1.2% | 0.0% | 1.4% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +12 more |
| Smith//Score | condorcet-based | 91.1 [87.9–94.1] | 98.5 [97.3–99.5] | 1.5 [-0.0–3.0] | 0.8 [-1.6–2.2] | 91 [75–>99] | 100.0 [99.9–100.0] | 93.5 | 94.2 | 91.2 [88.2–94.2] | 99.1 [98.3–99.7] | 1.7 [0.6–3.1] | 0.5 [-1.7–1.4] | 93 [83–>99] | 96.4 [93.4–98.5] | 95.4 [92.3–98.0] | 99.4 [98.1–100.0] | 99.4 [98.1–100.0] | 99.0 [97.5–100.0] | 99.0 [97.5–100.0] | 3.5 [1.5–6.0] | 3.0 [1.0–5.5] | 0.0% | 0.3% | 0.0% | 2.3% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +2 more |
| Approval | approval-based | 91.0 [87.1–94.5] | 90.5 [86.2–94.3] | -0.2 [-3.0–2.3] | 1.1 [-1.4–5.1] | 78 [47–>99] | 100.0 [100.0–100.0] | 74.4 | 74.6 | 91.0 [87.0–94.5] | 91.1 [87.3–94.6] | -0.2 [-2.9–2.4] | 1.2 [-1.4–5.2] | 77 [49–>99] | 82.1 [76.3–87.2] | 82.1 [76.4–87.3] | 92.2 [87.9–95.9] | 91.6 [87.3–95.6] | 92.5 [88.5–96.0] | 92.0 [88.0–95.5] | 3.5 [1.0–6.5] | 3.5 [1.0–6.5] | 0.0% | 0.2% | 0.0% | 2.4% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +13 more |
| ER-IRV | plurality-based | 90.7 [87.5–93.7] | 93.0 [88.6–96.5] | -0.2 [-2.7–2.5] | 1.4 [-1.1–4.5] | 65 [44–>99] | 99.6 [99.4–99.7] | 78.4 | 79.1 | 91.5 [88.3–94.3] | 95.9 [93.3–98.1] | 1.1 [-0.5–3.2] | 0.6 [-1.7–3.2] | 90 [61–>99] | 93.4 [89.7–96.5] | 91.8 [88.3–95.4] | 96.4 [93.1–98.8] | 94.3 [91.0–97.5] | 95.8 [92.8–98.0] | 93.6 [90.2–96.6] | 2.5 [0.5–5.0] | 3.5 [1.5–6.0] | 0.9% | 2.4% | 0.3% | 0.6% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +11 more |
| BTR-IRV | condorcet-based | 90.6 [87.4–93.7] | 99.3 [98.0–100.0] | 1.3 [0.3–2.7] | 0.7 [-1.2–1.6] | 90 [80–>99] | 99.8 [99.7–99.9] | 78.2 | 78.5 | 90.4 [87.2–93.6] | 99.8 [99.5–100.0] | 1.3 [0.3–2.6] | 0.2 [-1.2–0.4] | 97 [93–>99] | 99.5 [98.5–100.0] | 99.0 [97.4–100.0] | 100.0 [100.0–100.0] | 99.4 [98.1–100.0] | 99.5 [98.5–100.0] | 99.0 [97.5–100.0] | 3.5 [1.5–6.0] | 3.5 [1.5–6.0] | 0.6% | 0.6% | 0.3% | 0.2% | — |
| Schulze | condorcet | 90.4 [87.2–93.4] | 99.4 [98.6–100.0] | 1.2 [-0.0–2.7] | 0.5 [-1.1–1.3] | 92 [83–>99] | 99.8 [99.8–99.9] | 78.2 | 78.5 | 90.2 [87.0–93.4] | 99.9 [99.8–100.0] | 1.2 [0.2–2.6] | 0.1 [-1.1–0.2] | 98 [95–>99] | 99.0 [97.4–100.0] | 98.1 [96.2–100.0] | 99.4 [98.1–100.0] | 99.0 [97.2–100.0] | 99.5 [98.5–100.0] | 99.2 [97.8–100.0] | 3.5 [1.5–6.0] | 3.0 [1.0–5.5] | 0.4% | 0.8% | 0.0% | 0.0% | — |
| Minimax | minimax-family | 90.4 [87.2–93.4] | 99.4 [98.6–100.0] | 1.2 [-0.0–2.7] | 0.5 [-1.1–1.3] | 92 [83–>99] | 99.8 [99.8–99.9] | 78.2 | 78.5 | 90.2 [87.0–93.4] | 99.9 [99.8–100.0] | 1.2 [0.2–2.6] | 0.1 [-1.1–0.2] | 98 [95–>99] | 99.0 [97.4–100.0] | 98.1 [96.2–100.0] | 99.4 [98.1–100.0] | 99.0 [97.2–100.0] | 99.5 [98.5–100.0] | 99.2 [97.8–100.0] | 3.5 [1.5–6.0] | 3.0 [1.0–5.5] | 0.4% | 0.9% | 0.0% | 0.0% | — |
| IRV-3 | plurality-based | 90.3 [85.7–94.0] | 89.9 [85.3–94.1] | -0.9 [-5.0–3.0] | 1.9 [-1.3–6.6] | 60 [33–>99] | 99.2 [99.0–99.4] | 77.0 | 78.1 | 91.7 [88.4–94.4] | 92.3 [88.5–95.7] | 0.5 [-1.8–3.5] | 0.5 [-1.9–3.7] | 90 [49–>99] | 90.1 [85.9–93.9] | 87.5 [82.9–91.8] | 94.0 [90.1–97.4] | 90.7 [85.8–94.6] | 92.5 [88.5–95.5] | 89.8 [85.5–94.0] | 2.5 [0.5–5.0] | 2.5 [0.5–4.5] | 2.2% | 0.9% | 1.2% | 0.1% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +13 more |
| 321 | hybrid | 90.1 [86.7–93.6] | 92.7 [89.1–96.0] | -0.7 [-3.4–1.9] | 2.0 [-0.7–5.3] | 58 [39–>99] | 100.0 [99.9–100.0] | 85.5 | 86.5 | 91.5 [88.4–94.4] | 94.7 [91.8–97.2] | 0.8 [-1.1–2.7] | 0.6 [-1.6–3.6] | 89 [53–>99] | 88.8 [84.2–92.9] | 86.7 [81.8–91.4] | 95.8 [92.7–98.8] | 94.0 [90.1–97.6] | 95.5 [92.5–98.0] | 94.0 [90.5–97.0] | 3.0 [1.0–5.5] | 3.0 [1.0–5.5] | 0.0% | 0.5% | 0.0% | 4.3% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +11 more |
| MMWV | minimax-family | 90.1 [86.8–93.2] | 99.0 [97.8–99.9] | 0.9 [-0.7–2.5] | 0.9 [-0.9–2.1] | 86 [73–>99] | 99.9 [99.8–99.9] | 78.2 | 78.5 | 90.2 [87.0–93.4] | 99.9 [99.8–100.0] | 1.2 [0.2–2.6] | 0.1 [-1.1–0.2] | 98 [95–>99] | 99.0 [97.4–100.0] | 97.6 [95.4–99.5] | 99.4 [98.1–100.0] | 99.0 [97.2–100.0] | 99.5 [98.5–100.0] | 98.7 [97.0–100.0] | 3.5 [1.5–6.0] | 3.0 [1.0–5.5] | 0.0% | 1.3% | 0.0% | 0.1% | BTR-score (VSE\*) |
| MMPO | minimax-family | 90.1 [86.8–93.2] | 99.0 [97.8–99.9] | 0.9 [-0.7–2.5] | 0.9 [-0.9–2.1] | 86 [73–>99] | 99.9 [99.8–99.9] | 78.2 | 78.5 | 90.2 [87.0–93.4] | 99.9 [99.8–100.0] | 1.2 [0.2–2.6] | 0.1 [-1.1–0.2] | 98 [95–>99] | 99.0 [97.4–100.0] | 97.6 [95.4–99.5] | 99.4 [98.1–100.0] | 99.0 [97.2–100.0] | 99.5 [98.5–100.0] | 98.7 [97.0–100.0] | 3.5 [1.5–6.0] | 3.0 [1.0–5.5] | 0.0% | 1.3% | 0.0% | 0.1% | BTR-score (VSE\*) |
| Ranked pairs | condorcet | 90.1 [86.8–93.3] | 99.8 [99.7–100.0] | 1.1 [0.1–2.4] | 0.2 [-1.0–0.3] | 96 [93–>99] | 99.9 [99.8–99.9] | 78.3 | 78.5 | 90.0 [86.8–93.3] | 99.9 [99.7–100.0] | 1.1 [0.1–2.4] | 0.1 [-1.0–0.3] | 97 [94–>99] | 99.0 [97.4–100.0] | 99.0 [97.4–100.0] | 99.4 [98.1–100.0] | 99.4 [98.1–100.0] | 99.4 [98.3–100.0] | 99.4 [98.3–100.0] | 3.5 [1.5–6.0] | 3.5 [1.5–6.0] | 0.3% | 0.6% | 0.1% | 0.1% | — |
| Copeland | condorcet | 90.0 [86.8–93.2] | 99.5 [98.9–99.9] | 0.9 [-0.0–2.2] | 0.5 [-0.9–1.0] | 91 [83–>99] | 99.9 [99.8–99.9] | 78.2 | 78.4 | 89.9 [86.7–93.2] | 99.7 [99.4–99.9] | 0.9 [0.0–2.2] | 0.3 [-0.8–0.5] | 94 [89–99] | 98.7 [97.2–100.0] | 98.6 [97.0–100.0] | 99.1 [97.5–100.0] | 99.4 [98.1–100.0] | 98.8 [97.0–100.0] | 98.8 [97.0–100.0] | 3.5 [1.5–6.0] | 3.0 [1.0–5.5] | 0.4% | 0.9% | 0.0% | 0.2% | — |
| MJ | median-based | 89.7 [85.7–93.1] | 95.2 [92.8–97.1] | -0.4 [-2.6–1.5] | 2.3 [-0.4–5.1] | 60 [48–93] | 100.0 [100.0–100.0] | 93.3 | 94.4 | 89.5 [85.5–92.9] | 95.8 [93.6–97.6] | -0.4 [-2.6–1.5] | 2.4 [-0.2–4.9] | 61 [50–91] | 89.3 [84.7–93.3] | 88.3 [83.7–92.3] | 95.8 [92.6–98.7] | 95.2 [91.7–98.2] | 96.0 [93.0–98.5] | 95.5 [92.5–98.0] | 3.0 [1.0–6.0] | 3.0 [1.0–6.0] | 0.0% | 0.0% | 0.0% | 5.4% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +9 more |
| Kemeny-Young | condorcet | 89.0 [85.5–92.5] | 99.8 [99.5–99.9] | 0.8 [-0.1–2.1] | 0.2 [-0.5–0.5] | 95 [91–>99] | 99.8 [99.8–99.9] | 78.2 | 78.4 | 90.0 [86.7–93.3] | 99.8 [99.6–100.0] | 1.0 [0.2–2.3] | 0.2 [-0.9–0.4] | 96 [93–>99] | 99.0 [97.4–100.0] | 98.5 [96.5–100.0] | 99.4 [98.1–100.0] | 98.8 [97.0–100.0] | 99.0 [97.5–100.0] | 98.5 [96.5–100.0] | 3.5 [1.5–6.0] | 4.0 [1.5–6.5] | 0.4% | 0.8% | 0.0% | 0.1% | BTR-IRV (VSE\*) |
| IRV | plurality-based | 88.9 [84.1–93.1] | 89.4 [84.4–93.9] | -2.3 [-6.5–1.8] | 3.3 [-0.2–8.1] | 47 [29–>99] | 98.9 [98.7–99.2] | 77.7 | 79.1 | 91.5 [88.3–94.3] | 95.9 [93.3–98.1] | 1.1 [-0.5–3.2] | 0.6 [-1.7–3.2] | 90 [61–>99] | 93.4 [89.7–96.5] | 86.8 [82.2–91.4] | 96.4 [93.1–98.8] | 91.4 [87.0–95.3] | 95.8 [92.8–98.0] | 91.3 [87.4–95.0] | 2.5 [0.5–5.0] | 3.0 [1.0–5.5] | 2.1% | 1.8% | 1.6% | 0.2% | Smith//Score (WME\* sincerity), MJ (WME\* sincerity), MAV (WME\* sincerity) +17 more |
| STAR | hybrid | 88.4 [83.9–92.7] | 95.0 [91.6–97.7] | -1.4 [-5.1–1.7] | 3.5 [-0.3–7.2] | 53 [37–92] | 100.0 [99.9–100.0] | 93.1 | 94.2 | 91.6 [88.5–94.6] | 97.0 [95.4–98.4] | 1.5 [0.1–2.8] | 0.4 [-1.6–3.1] | 94 [72–>99] | 90.8 [86.7–94.8] | 89.8 [85.4–93.9] | 99.4 [98.1–100.0] | 97.0 [94.0–99.4] | 98.5 [96.5–100.0] | 96.5 [94.0–99.0] | 3.0 [1.0–5.5] | 4.0 [1.5–7.0] | 0.0% | 0.7% | 0.0% | 3.1% | Ranked pairs (WME\*), Schulze (WME\*), Kemeny-Young (WME\*) +10 more |
| TTR | two-round | 88.2 [83.3–92.2] | 94.3 [90.9–97.2] | -1.8 [-5.0–1.4] | 3.8 [0.2–7.0] | 50 [37–92] | 98.6 [98.1–99.0] | 64.5 | 65.7 | 90.5 [87.2–93.9] | 94.5 [91.1–97.4] | 0.0 [-2.0–2.3] | 1.6 [-1.1–4.7] | 64 [46–>99] | 92.1 [88.3–95.5] | 90.8 [86.7–94.9] | 95.8 [92.6–98.2] | 94.6 [91.0–97.6] | 94.8 [91.5–97.5] | 94.0 [90.5–97.0] | 3.0 [1.0–5.5] | 4.0 [1.5–7.0] | 2.1% | 0.0% | 1.4% | 0.0% | BTR-score (VSE\* WME\* sincerity), Ranked pairs (WME\* sincerity), Schulze (WME\* sincerity) +19 more |
| Score | rated | 87.4 [83.0–91.9] | 90.7 [87.0–94.2] | -3.2 [-6.6–0.0] | 4.7 [0.9–8.8] | 40 [30–60] | 100.0 [100.0–100.0] | 92.4 | 94.2 | 89.2 [85.4–92.7] | 95.1 [92.6–97.1] | -0.8 [-3.1–1.1] | 2.8 [0.1–5.6] | 57 [47–83] | 86.7 [81.6–91.4] | 81.4 [75.6–86.5] | 95.8 [92.4–98.8] | 90.1 [85.3–94.1] | 96.0 [93.0–98.5] | 91.8 [87.7–95.2] | 4.5 [2.0–7.5] | 5.0 [2.0–8.0] | 0.0% | 0.1% | 0.0% | 8.9% | Smith//Score (VSE\* WME\*), MAV (VSE\* WME\*), Score runoff (VSE\* WME\*) +17 more |
| Borda | positional | 87.4 [83.4–91.3] | 93.8 [90.4–96.3] | -2.4 [-5.5–0.3] | 4.5 [1.2–7.6] | 46 [36–77] | 99.0 [98.8–99.2] | 75.6 | 77.7 | 88.7 [85.2–92.1] | 96.2 [94.4–97.8] | -0.9 [-3.3–1.8] | 3.2 [0.1–5.0] | 59 [49–99] | 89.5 [85.3–93.8] | 87.2 [82.7–91.4] | 90.7 [86.1–94.7] | 90.4 [85.6–94.6] | 91.2 [87.2–95.0] | 91.0 [87.0–94.5] | 2.5 [0.5–5.0] | 3.0 [1.0–5.5] | 3.4% | 5.6% | 0.0% | 4.4% | Smith//Score (VSE\* WME\*), Score runoff (VSE\* WME\*), BTR-score (VSE\* WME\*) +13 more |
| Plurality | plurality-based | 80.0 [74.4–85.1] | 78.7 [73.0–84.2] | -11.2 [-16.2–-6.3] | 12.2 [7.5–17.8] | 18 [14–29] | 99.5 [99.2–99.6] | 64.4 | 65.0 | 77.7 [71.5–83.6] | 78.5 [72.3–84.0] | -13.5 [-19.1–-7.6] | 14.5 [8.8–20.9] | 16 [13–25] | 69.1 [62.9–75.3] | 69.6 [63.3–75.8] | 77.4 [70.8–83.3] | 78.0 [71.6–83.7] | 79.2 [73.5–84.8] | 80.8 [75.0–85.8] | 8.0 [4.5–12.0] | 6.5 [3.5–10.0] | 3.8% | 0.0% | 0.1% | 0.0% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Kemeny-Young (VSE\* WME\*) +25 more |

**Adaptive voters** (a share 1.0 of the electorate, strategic in a round when their best move gains more than their cost of voting insincerely): the share of them voting strategically in the reported round, per method, averaged over elections. A method whose leading candidates are both broadly acceptable gives fewer voters a reason to strategise.

| Method | Adaptive voters strategic |
|---|---|
| Borda | 13.7% |
| Score | 9.8% |
| Plurality | 7.0% |
| IRV | 6.0% |
| 321 | 5.9% |
| Score runoff | 5.9% |
| MJ | 5.7% |
| IRV-3 | 5.5% |
| Approval | 5.4% |
| TTR | 5.2% |
| MAV | 5.0% |
| MARS | 4.9% |
| Approval cond. runoff | 4.8% |
| ER-IRV | 4.6% |
| STAR | 4.1% |
| ARA | 4.0% |
| Approval runoff | 3.8% |
| Supplementary vote | 3.4% |
| Smith//Score | 2.7% |
| BTR-score | 2.6% |
| BTR-IRV | 1.9% |
| Soft Minimax-D | 1.5% |
| Kemeny-Young | 1.5% |
| MMWV | 1.4% |
| MMPO | 1.4% |
| Copeland | 1.4% |
| Minimax | 1.3% |
| Schulze | 1.3% |
| Ranked pairs | 1.2% |

**Nomination** (with sincere ballots; per method, averaged over elections): how often the honest result changed, how many candidates were out of the race and how many clones in it at the end, and how often the candidates reached a field where nobody wanted to move (settled). VSE and WME above are judged on the original field.

| Method | Result changed | Exits | Clones | Settled |
|---|---|---|---|---|
| Plurality | 36.0% | 1.08 | 0.89 | 92% |
| Supplementary vote | 22.5% | 1.07 | 0.64 | 99% |
| TTR | 19.5% | 0.49 | 0.61 | 99% |
| Approval | 19.5% | 0.71 | 0.34 | 100% |
| MAV | 17.5% | 0.51 | 0.07 | 100% |
| 321 | 16.5% | 0.38 | 0.36 | 100% |
| Approval cond. runoff | 16.0% | 0.52 | 0.45 | 100% |
| Approval runoff | 15.0% | 0.40 | 0.38 | 100% |
| MJ | 14.5% | 0.45 | 0.08 | 100% |
| IRV | 14.0% | 0.20 | 0.04 | 100% |
| ER-IRV | 14.0% | 0.20 | 0.04 | 100% |
| IRV-3 | 14.0% | 0.52 | 0.21 | 100% |
| Score runoff | 11.5% | 0.14 | 0.20 | 100% |
| STAR | 11.0% | 0.25 | 0.17 | 100% |
| Score | 10.5% | 0.28 | 0.04 | 100% |
| ARA | 10.0% | 0.24 | 0.07 | 100% |
| Borda | 10.0% | 0.04 | 0.55 | 100% |
| Soft Minimax-D | 8.5% | 0.24 | 0.08 | 100% |
| Smith//Score | 7.5% | 0.22 | 0.04 | 100% |
| BTR-score | 7.0% | 0.23 | 0.04 | 100% |
| MARS | 6.5% | 0.21 | 0.06 | 100% |
| Copeland | 3.5% | 0.01 | 0.07 | 100% |
| BTR-IRV | 2.5% | 0.02 | 0.03 | 100% |
| Kemeny-Young | 2.0% | 0.01 | 0.04 | 100% |
| Ranked pairs | 1.5% | 0.01 | 0.04 | 100% |
| Schulze | 1.5% | 0.01 | 0.03 | 100% |
| Minimax | 1.5% | 0.01 | 0.03 | 100% |
| MMWV | 1.5% | 0.01 | 0.03 | 100% |
| MMPO | 1.5% | 0.01 | 0.03 | 100% |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 196 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 4 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| ARA | 90.3% (91.3%) | 7.7% (6.1%) | 2.0% (2.6%) | 25.0% (25.0%) | 95.9% (96.4%) | 90.3% |
| Supplementary vote | 90.8% (91.3%) | 8.7% (8.2%) | 0.5% (0.5%) | 75.0% (25.0%) | 91.9% (90.3%) | 83.7% |
| MARS | 93.9% (93.4%) | 4.1% (4.1%) | 2.0% (2.6%) | 100.0% (100.0%) | 96.9% (98.5%) | 91.8% |
| BTR-score | 95.4% (95.4%) | 4.1% (3.1%) | 0.5% (1.5%) | 50.0% (75.0%) | 100.0% (100.0%) | 95.9% |
| Soft Minimax-D | 95.4% (95.9%) | 2.6% (2.6%) | 2.0% (1.5%) | 75.0% (75.0%) | 98.5% (99.0%) | 95.2% |
| MAV | 91.3% (90.8%) | 7.1% (8.2%) | 1.5% (1.0%) | 50.0% (50.0%) | 94.4% (94.9%) | 87.8% |
| Smith//Score | 94.4% (95.4%) | 4.1% (3.1%) | 1.5% (1.5%) | 50.0% (75.0%) | 100.0% (100.0%) | 95.4% |
| ER-IRV | 95.9% (99.5%) | 2.0% (0.0%) | 2.0% (0.5%) | 50.0% (50.0%) | 92.8% (93.9%) | 91.8% |
| BTR-IRV | 98.5% (99.5%) | 0.5% (0.5%) | 1.0% (0.0%) | 75.0% (75.0%) | 100.0% (100.0%) | 99.0% |
| Schulze | 98.0% (99.0%) | 1.0% (1.0%) | 1.0% (0.0%) | 25.0% (25.0%) | 100.0% (100.0%) | 98.1% |
| Minimax | 98.0% (99.0%) | 1.0% (1.0%) | 1.0% (0.0%) | 25.0% (25.0%) | 100.0% (100.0%) | 98.1% |
| IRV-3 | 86.7% (89.3%) | 10.7% (8.7%) | 2.6% (2.0%) | 75.0% (75.0%) | 98.7% (99.2%) | 87.5% |
| 321 | 84.2% (81.6%) | 8.7% (8.2%) | 7.1% (10.2%) | 75.0% (75.0%) | 97.3% (98.3%) | 86.7% |
| MMWV | 97.4% (99.0%) | 1.5% (1.0%) | 1.0% (0.0%) | 25.0% (25.0%) | 100.0% (100.0%) | 97.6% |
| MMPO | 97.4% (99.0%) | 1.5% (1.0%) | 1.0% (0.0%) | 25.0% (25.0%) | 100.0% (100.0%) | 97.6% |
| Ranked pairs | 99.0% (99.0%) | 1.0% (1.0%) | 0.0% (0.0%) | 0.0% (0.0%) | 100.0% (100.0%) | 99.0% |
| Copeland | 98.5% (98.5%) | 1.0% (1.0%) | 0.5% (0.5%) | 0.0% (0.0%) | 100.0% (100.0%) | 98.6% |
| MJ | 92.3% (91.3%) | 6.6% (7.7%) | 1.0% (1.0%) | 25.0% (25.0%) | 92.8% (94.9%) | 88.3% |
| Kemeny-Young | 98.5% (99.0%) | 1.0% (1.0%) | 0.5% (0.0%) | 25.0% (25.0%) | 100.0% (100.0%) | 98.5% |
| IRV | 94.9% (99.5%) | 3.6% (0.0%) | 1.5% (0.5%) | 75.0% (50.0%) | 90.2% (93.9%) | 86.8% |
| STAR | 92.9% (94.9%) | 5.6% (3.1%) | 1.5% (2.0%) | 75.0% (75.0%) | 94.9% (93.8%) | 89.8% |
| Score | 89.8% (90.8%) | 6.6% (6.6%) | 3.6% (2.6%) | 75.0% (50.0%) | 87.8% (92.2%) | 81.4% |
| Borda | 92.9% (99.0%) | 1.5% (0.5%) | 5.6% (0.5%) | 50.0% (0.0%) | 89.3% (89.5%) | 87.2% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| ARA | 1.0% (0.0%) | 10.5% (9.5%) | 0.9% (0.8%) |
| Score runoff | 0.0% (0.0%) | — (—) | — (—) |
| Supplementary vote | 0.0% (0.0%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Approval cond. runoff | 2.0% (1.0%) | — (—) | — (—) |
| MARS | 0.0% (0.0%) | 7.0% (9.5%) | 0.5% (0.8%) |
| BTR-score | 0.0% (0.0%) | 10.5% (8.0%) | 0.8% (0.7%) |
| Soft Minimax-D | 0.5% (0.5%) | 9.5% (10.0%) | 0.8% (0.9%) |
| MAV | 0.0% (0.0%) | 9.5% (6.0%) | 0.8% (0.4%) |
| Approval runoff | 0.5% (0.5%) | — (—) | — (—) |
| Smith//Score | 0.0% (0.0%) | 11.0% (8.0%) | 0.8% (0.7%) |
| Approval | 0.0% (0.5%) | — (—) | — (—) |
| ER-IRV | 1.5% (1.5%) | 0.5% (0.0%) | 0.0% (0.0%) |
| BTR-IRV | 0.0% (0.0%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Schulze | 0.5% (0.0%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Minimax | 0.5% (0.0%) | 0.0% (0.0%) | 0.0% (0.0%) |
| IRV-3 | 0.5% (0.5%) | 12.0% (12.5%) | 0.9% (0.9%) |
| 321 | 0.0% (0.0%) | 22.5% (28.5%) | 2.0% (2.3%) |
| MMWV | 0.5% (0.0%) | 1.0% (0.0%) | 0.1% (0.0%) |
| MMPO | 0.5% (0.0%) | 1.5% (0.0%) | 0.1% (0.0%) |
| Ranked pairs | 1.0% (0.5%) | 0.0% (0.0%) | 0.0% (0.0%) |
| Copeland | 1.5% (1.5%) | 0.5% (0.0%) | 0.0% (0.0%) |
| MJ | 0.0% (0.0%) | 8.5% (9.5%) | 0.6% (0.8%) |
| Kemeny-Young | 0.5% (0.5%) | 0.5% (0.0%) | 0.0% (0.0%) |
| IRV | 1.5% (1.5%) | 0.0% (0.0%) | 0.0% (0.0%) |
| STAR | 0.0% (0.0%) | 11.5% (11.5%) | 0.8% (1.1%) |
| TTR | 0.5% (0.5%) | — (—) | — (—) |
| Score | 0.5% (0.0%) | 11.5% (9.0%) | 0.8% (0.7%) |
| Borda | 0.5% (0.5%) | 5.0% (0.0%) | 0.3% (0.0%) |
| Plurality | 1.5% (0.5%) | — (—) | — (—) |

![strategy arrows](arrows-c1.png)

![strategy shifts](shift-c1.png)

![sincerity](sincerity-c1.png)

![fidelity](fidelity-c1.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c1.png)

Scatter and chain plots leave out Condorcet-random-drop, Geometric median, Random ballot, Random pair runoff, Anti-plurality, TRASH, CW else Score loser, CL else Score winner, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
