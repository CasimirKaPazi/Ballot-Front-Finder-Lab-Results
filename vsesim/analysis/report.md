# Ballot Front Finder Lab — analysis

Input: vsesim/01-strategic=0.5, vsesim/02-strategic=1.0 (simulator version 2.1.0; analysis version 2.1.0). Bootstrap: 1000 paired replicates over elections. Brackets are 95% intervals.

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

![sweep over strategic share](chain-strategic.png)

**Rank stability:** rank correlation (Spearman) of the methods' values with those in the base condition (c1), per axis; 1 = same order, 0 = unrelated. Methods hidden in the plots are left out, since their extreme values would inflate the correlation. The first row is a noise reference: the agreement expected from sampling noise alone between two runs of the base condition (its even and odd elections correlated, then scaled up to full size with the Spearman-Brown formula; — where the halves do not agree at all, so the base itself gives no stable order on that axis); for conditions with fewer elections than the base, a further row gives the agreement expected between the base and a run of that size. A condition near or above its row orders the methods as the base does; only values clearly below it show a real change.

| Condition | VSE | WME | VSE\* | WME\* | sincerity |
|---|---|---|---|---|---|
| noise reference: c1 vs itself | 1.00 | 0.99 | 1.00 | 1.00 | 1.00 |
| c2: strategic share 1.0 | 1.00 | 1.00 | 0.95 | 0.90 | 1.00 |

**Electorates** (properties of the generated electorates, independent of the methods): how often a Condorcet winner exists, how often it also has the highest total utility, how much VSE electing it loses (ratio of sums, as VSE), and how steep preferences are (favourite ÷ least liked, median voter). Differences between methods on VSE mostly come from the third column.

| Condition | Condorcet winner | … with highest total utility | VSE lost electing it | favourite ÷ least liked |
|---|---|---|---|---|
| c1: base | 87.2% | 91.2% | 1.1% | — |
| c2: strategic share 1.0 | 87.2% | 91.2% | 1.1% | — |

## c1: base

15000 elections (seeds 1), 13075 with a Condorcet winner. 40 voters, 6 candidates, model ks-quinn: Quinn's hierarchical model as in vse-sim's code (one offset per bundle), faction alpha 1.5, cut 0.2, linear utility; 1 poll round(s), 5 polls of 21 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 0.5, scores 0..10. Electorates: Condorcet winner in 87.2%; it also has the highest total utility in 91.2% of those; electing it loses 1.1% of VSE; favourite ÷ least liked: — (utility not positive). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 97.1 to 98.8; with the best VSE (100) WME 96.4 (nothing lower is possible there either); the most balanced point VSE 99.3 / WME 99.7. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 98.6 (at WME 99.5); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–10) at most VSE 98.6 (at WME 99.8); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -61.2 / WME -44.9. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 97.9 / WME 99.9. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 96.8 / 94.1; normalized utility winner 95.6 / 88.1; best worst margin 98.0 / 100.0; Condorcet winner, else random ballot 87.7 / 91.6; Condorcet winner, else utility winner 99.0 / 99.8; random pair, majority 37.1 / 9.2; nearest the median voter 98.6 / 97.4; nearest the mean voter 94.8 / 87.1.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Schulze.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Schulze.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 87.7 / 91.6; its mirror on the utility side 87.6 / 78.9; random pair 37.1 / 9.2. No better than a lottery of them (Lead ≤ 0): with learned ballots IRV, Plurality; with honest ballots IRV, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Schulze | condorcet | 97.6 [97.4–97.7] | 98.9 [98.7–99.0] | 9.9 [9.4–10.4] | 1.0 [0.8–1.1] | 37 [37–38] | 99.5 [99.4–99.5] | 81.8 | 82.7 | 98.0 [97.8–98.1] | 100.0 [100.0–100.0] | 10.3 [9.8–10.8] | 0.0 [0.0–0.0] | 40 [40–40] | 100.0 [100.0–100.0] | 98.0 [97.8–98.2] | 100.0 [100.0–100.0] | 98.4 [98.2–98.7] | 94.5 [94.3–94.8] | 93.1 [92.9–93.4] | 0.3 [0.2–0.3] | 0.4 [0.3–0.5] | 1.3% | 1.7% | 0.1% | 0.3% | — |
| Ranked pairs | condorcet | 97.1 [96.8–97.3] | 98.2 [98.0–98.4] | 9.4 [8.9–9.9] | 1.5 [1.2–1.7] | 36 [35–36] | 99.3 [99.2–99.3] | 81.5 | 82.7 | 98.0 [97.8–98.1] | 100.0 [100.0–100.0] | 10.3 [9.8–10.8] | 0.0 [0.0–0.0] | 40 [40–40] | 100.0 [100.0–100.0] | 97.2 [96.9–97.5] | 100.0 [100.0–100.0] | 97.8 [97.5–98.0] | 94.5 [94.2–94.7] | 92.4 [92.1–92.8] | 0.2 [0.2–0.3] | 0.6 [0.4–0.7] | 1.8% | 2.3% | 0.2% | 0.6% | Schulze (VSE\* WME\*) |
| STAR | hybrid | 95.4 [95.1–95.6] | 88.8 [88.3–89.4] | 7.7 [7.1–8.2] | 3.2 [2.9–3.5] | 27 [26–27] | 99.9 [99.9–99.9] | 90.8 | 97.0 | 97.7 [97.6–97.9] | 92.9 [92.5–93.3] | 10.0 [9.5–10.5] | 0.8 [0.7–1.0] | 35 [35–36] | 88.2 [87.7–88.8] | 83.6 [83.0–84.3] | 96.2 [95.9–96.6] | 89.3 [88.6–89.8] | 93.8 [93.4–94.2] | 86.6 [86.0–87.1] | 0.7 [0.6–0.8] | 1.4 [1.2–1.6] | 0.0% | 1.7% | 0.0% | 12.8% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*) |
| MJ | median-based | 93.4 [93.1–93.6] | 82.0 [81.5–82.6] | 5.7 [5.1–6.2] | 5.2 [4.9–5.5] | 21 [20–22] | 100.0 [100.0–100.0] | 89.8 | 97.0 | 93.1 [92.8–93.4] | 79.0 [78.3–79.7] | 5.4 [4.9–5.9] | 5.4 [5.2–5.7] | 20 [20–21] | 68.8 [68.1–69.6] | 72.5 [71.8–73.3] | 72.3 [71.5–73.1] | 75.9 [75.1–76.7] | 70.0 [69.3–70.7] | 73.0 [72.3–73.7] | 1.7 [1.5–1.9] | 1.7 [1.5–2.0] | 0.0% | 0.7% | 0.0% | 16.9% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), STAR (VSE\* WME\*) |
| MAV | median-based | 92.8 [92.5–93.1] | 81.0 [80.4–81.6] | 5.1 [4.6–5.6] | 5.7 [5.4–6.0] | 20 [19–20] | 100.0 [100.0–100.0] | 89.7 | 97.0 | 92.0 [91.7–92.3] | 76.1 [75.4–76.8] | 4.4 [3.8–4.9] | 6.5 [6.2–6.8] | 18 [17–19] | 66.1 [65.3–66.9] | 71.3 [70.5–72.0] | 70.2 [69.4–71.1] | 75.4 [74.5–76.2] | 67.5 [66.8–68.2] | 72.1 [71.4–72.8] | 2.4 [2.1–2.6] | 2.1 [1.8–2.3] | 0.0% | 0.8% | 0.0% | 17.1% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), MJ (VSE\* WME\*) +1 more |
| Score | rated | 92.1 [91.8–92.4] | 80.5 [79.8–81.2] | 4.4 [3.8–5.0] | 6.4 [6.1–6.8] | 18 [17–19] | 100.0 [100.0–100.0] | 85.6 | 97.0 | 95.4 [95.2–95.6] | 86.0 [85.6–86.5] | 7.7 [7.1–8.2] | 3.2 [2.9–3.4] | 27 [26–27] | 76.6 [75.8–77.3] | 72.8 [72.0–73.6] | 95.1 [94.7–95.4] | 79.9 [79.1–80.7] | 94.6 [94.3–95.0] | 80.0 [79.3–80.6] | 1.9 [1.7–2.1] | 1.9 [1.7–2.2] | 0.0% | 0.5% | 0.0% | 28.7% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), MJ (VSE\* WME\*) +2 more |
| 321 | hybrid | 91.5 [91.1–91.9] | 78.3 [77.6–79.0] | 3.8 [3.2–4.4] | 7.1 [6.7–7.4] | 17 [16–18] | 99.9 [99.9–99.9] | 77.9 | 85.1 | 91.6 [91.3–92.0] | 73.1 [72.4–74.0] | 3.9 [3.4–4.5] | 6.9 [6.6–7.2] | 17 [17–18] | 66.9 [66.1–67.7] | 70.7 [70.0–71.5] | 74.7 [73.9–75.5] | 76.9 [76.1–77.6] | 75.1 [74.5–75.8] | 76.3 [75.7–77.0] | 2.5 [2.3–2.7] | 2.1 [1.9–2.3] | 0.0% | 1.4% | 0.0% | 21.6% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Score (VSE\* WME\*) +3 more |
| Borda | positional | 90.5 [90.1–91.0] | 81.5 [80.9–82.1] | 2.9 [2.2–3.4] | 8.0 [7.6–8.4] | 16 [16–16] | 96.5 [96.4–96.5] | 75.2 | 82.7 | 94.5 [94.2–94.8] | 92.3 [91.9–92.6] | 6.8 [6.2–7.4] | 4.0 [3.7–4.4] | 27 [27–28] | 84.3 [83.7–84.9] | 73.6 [72.9–74.4] | 89.5 [89.0–90.1] | 77.6 [76.9–78.5] | 88.0 [87.5–88.5] | 77.1 [76.4–77.7] | 0.3 [0.2–0.3] | 1.3 [1.2–1.5] | 12.0% | 8.7% | 0.3% | 6.9% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), STAR (VSE\* WME\* sincerity) +2 more |
| Approval | approval-based | 89.6 [89.2–90.0] | 78.1 [77.5–78.7] | 2.0 [1.3–2.6] | 8.9 [8.5–9.3] | 15 [14–15] | 99.9 [99.9–99.9] | 59.5 | 61.2 | 88.8 [88.3–89.2] | 75.9 [75.2–76.5] | 1.1 [0.5–1.7] | 9.8 [9.4–10.2] | 14 [13–14] | 67.2 [66.4–68.0] | 69.2 [68.5–70.0] | 71.0 [70.2–71.8] | 72.5 [71.6–73.3] | 68.5 [67.8–69.2] | 70.4 [69.6–71.0] | 5.9 [5.5–6.3] | 4.6 [4.3–5.0] | 0.0% | 1.4% | 0.0% | 13.5% | Ranked pairs (VSE\* WME\*), Schulze (VSE\* WME\*), Score (VSE\* WME\*) +4 more |
| IRV | plurality-based | 84.4 [83.8–85.0] | 72.2 [71.4–73.0] | -3.3 [-4.1–-2.6] | 14.2 [13.6–14.8] | 10 [9.6–10] | 97.5 [97.4–97.5] | 78.9 | 82.7 | 86.3 [85.8–86.7] | 73.3 [72.6–73.9] | -1.4 [-2.0–-0.8] | 12.3 [11.8–12.7] | 12 [11–12] | 67.4 [66.5–68.2] | 67.8 [67.1–68.6] | 71.9 [71.0–72.7] | 71.5 [70.6–72.2] | 68.3 [67.6–69.1] | 68.8 [68.1–69.5] | 7.2 [6.8–7.6] | 7.5 [7.0–7.8] | 9.2% | 3.3% | 2.0% | 0.3% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity) +6 more |
| Plurality | plurality-based | 65.8 [64.8–66.7] | 56.6 [55.7–57.4] | -21.9 [-23.0–-20.9] | 32.7 [31.9–33.7] | 5.1 [4.9–5.3] | 98.7 [98.6–98.7] | 51.3 | 52.8 | 58.3 [57.1–59.3] | 52.3 [51.3–53.2] | -29.4 [-30.7–-28.3] | 40.3 [39.3–41.4] | 4.5 [4.3–4.6] | 50.0 [49.1–50.8] | 53.5 [52.7–54.4] | 53.1 [52.1–54.1] | 56.6 [55.7–57.5] | 50.0 [49.2–50.8] | 53.9 [53.2–54.7] | 22.5 [21.9–23.2] | 18.6 [18.0–19.2] | 7.6% | 0.0% | 0.6% | 0.0% | Approval (VSE\* WME\* sincerity), Score (VSE\* WME\* sincerity), MJ (VSE\* WME\* sincerity) +5 more |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 13075 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 1925 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Schulze | 97.4% (100.0%) | 0.9% (0.0%) | 1.7% (0.0%) | 6.5% (0.0%) | 100.0% (100.0%) | 98.0% |
| Ranked pairs | 96.0% (100.0%) | 1.3% (0.0%) | 2.8% (0.0%) | 7.7% (0.0%) | 100.0% (100.0%) | 97.2% |
| STAR | 86.7% (90.4%) | 8.4% (5.4%) | 4.8% (4.2%) | 65.7% (61.2%) | 93.8% (95.1%) | 83.6% |
| MJ | 86.2% (90.4%) | 9.1% (5.4%) | 4.7% (4.2%) | 68.4% (61.2%) | 78.3% (70.8%) | 72.5% |
| MAV | 86.0% (90.4%) | 9.3% (5.4%) | 4.7% (4.2%) | 69.8% (61.2%) | 77.2% (67.7%) | 71.3% |
| Score | 87.7% (90.4%) | 7.6% (5.4%) | 4.7% (4.2%) | 67.3% (61.2%) | 79.0% (80.6%) | 72.8% |
| 321 | 71.2% (64.8%) | 21.7% (21.2%) | 7.1% (14.0%) | 86.4% (77.9%) | 94.7% (94.0%) | 70.7% |
| Borda | 85.7% (100.0%) | 5.8% (0.0%) | 8.5% (0.0%) | 27.2% (0.0%) | 81.3% (84.3%) | 73.6% |
| IRV | 85.4% (100.0%) | 8.8% (0.0%) | 5.8% (0.0%) | 31.2% (0.0%) | 78.5% (67.4%) | 67.8% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| Schulze | 10.4% (10.9%) | 35.4% (34.5%) | 3.4% (3.3%) |
| Ranked pairs | 10.6% (10.9%) | 35.6% (34.5%) | 3.4% (3.3%) |
| STAR | 1.0% (2.6%) | 24.8% (25.1%) | 2.1% (2.2%) |
| MJ | 0.2% (1.1%) | 25.4% (25.1%) | 2.1% (2.2%) |
| MAV | 1.5% (4.5%) | 25.1% (25.1%) | 2.1% (2.2%) |
| Score | 1.0% (3.6%) | 26.0% (25.1%) | 2.2% (2.2%) |
| 321 | 3.2% (12.6%) | 35.9% (41.9%) | 3.0% (3.8%) |
| Borda | 1.9% (3.5%) | 37.2% (34.5%) | 3.5% (3.3%) |
| Approval | 7.1% (14.1%) | — (—) | — (—) |
| IRV | 10.8% (14.6%) | 35.4% (34.5%) | 3.4% (3.3%) |
| Plurality | 5.5% (8.4%) | — (—) | — (—) |

![strategy arrows](arrows-c1.png)

![strategy shifts](shift-c1.png)

![sincerity](sincerity-c1.png)

![fidelity](fidelity-c1.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c1.png)

## c2: strategic share 1.0

15000 elections (seeds 1), 13075 with a Condorcet winner. 40 voters, 6 candidates, model ks-quinn: Quinn's hierarchical model as in vse-sim's code (one offset per bundle), faction alpha 1.5, cut 0.2, linear utility; 1 poll round(s), 5 polls of 21 voters, poll error 2.0x, individual voters with pooled discovery (the moves that pay for 0.25 of each type of voter, by their order of the poll's top three, counted on the polls; a voter makes one if it gains more than a cost up to 0.3 of their utility range), strategic share 1.0, scores 0..10. Electorates: Condorcet winner in 87.2%; it also has the highest total utility in 91.2% of those; electing it loses 1.1% of VSE; favourite ÷ least liked: — (utility not positive). Reachable region (any rule, knowing the sincere utilities): with the best WME (100) VSE 97.1 to 98.8; with the best VSE (100) WME 96.4 (nothing lower is possible there either); the most balanced point VSE 99.3 / WME 99.7. With normalized ballots (each voter's utilities stretched between their least and most liked candidate, taken at face value) at most VSE 98.6 (at WME 99.5); the dotted line in the plots. With the normalized ballots rounded to the score range (the honest score ballots, 0–10) at most VSE 98.6 (at WME 99.8); the dash-dotted line. A random candidate (the zero of classical VSE) gives VSE -61.2 / WME -44.9. The maximal lottery of the sincere preferences (the Condorcet winner if there is one, else a lottery no candidate beats in expectation) gives VSE 97.9 / WME 99.9. Further reference points (VSE / WME): a coin between the best worst margin and the best normalized total 96.8 / 94.1; normalized utility winner 95.6 / 88.1; best worst margin 98.0 / 100.0; Condorcet winner, else random ballot 87.7 / 91.6; Condorcet winner, else utility winner 99.0 / 99.8; random pair, majority 37.1 / 9.2; nearest the median voter 98.6 / 97.4; nearest the mean voter 94.8 / 87.1.

**Pareto front** (point estimates: no other method at least as good on all of VSE\*, WME\*, sincerity and better on one): Schulze, MJ.

**Undominated** (no other method clearly better, i.e. beyond the paired 95% interval, on one of these axes and clearly worse on none): Schulze, MJ.

**Natural baseline:** trivial rules (VSE / WME) random ballot 0.0 / 0.0; CW//random ballot 87.7 / 91.6; its mirror on the utility side 87.6 / 78.9; random pair 37.1 / 9.2. No better than a lottery of them (Lead ≤ 0): with learned ballots Approval, 321, Borda, IRV, Plurality; with honest ballots IRV, Plurality.

| Method | Group | VSE\* | WME\* | Lead\* | Gap\* | Jury\* | Sincerity | Fidelity | (honest) | VSE | WME | Lead | Gap | Jury | CE | CE\* | NUCW | NUCW\* | Span | Span\* | Low | Low\* | Compromise | Burial | Push-over | Exaggeration | Dominated by |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Schulze | condorcet | 97.1 [96.9–97.2] | 97.5 [97.3–97.8] | 9.4 [8.9–9.9] | 1.5 [1.3–1.7] | 34 [33–35] | 98.9 [98.9–99.0] | 81.0 | 82.7 | 98.0 [97.8–98.1] | 100.0 [100.0–100.0] | 10.3 [9.8–10.8] | 0.0 [0.0–0.0] | 40 [40–40] | 100.0 [100.0–100.0] | 96.4 [96.1–96.7] | 100.0 [100.0–100.0] | 96.8 [96.5–97.2] | 94.5 [94.3–94.8] | 91.8 [91.5–92.2] | 0.3 [0.2–0.3] | 0.5 [0.4–0.7] | 2.6% | 3.4% | 0.1% | 0.5% | — |
| Ranked pairs | condorcet | 95.3 [94.9–95.7] | 95.2 [94.7–95.6] | 7.6 [7.0–8.3] | 3.2 [2.8–3.6] | 30 [29–30] | 98.5 [98.5–98.6] | 80.3 | 82.7 | 98.0 [97.8–98.1] | 100.0 [100.0–100.0] | 10.3 [9.8–10.8] | 0.0 [0.0–0.0] | 40 [40–40] | 100.0 [100.0–100.0] | 94.3 [93.9–94.7] | 100.0 [100.0–100.0] | 94.9 [94.4–95.3] | 94.5 [94.2–94.7] | 90.0 [89.6–90.4] | 0.2 [0.2–0.3] | 1.2 [1.0–1.4] | 3.5% | 4.6% | 0.4% | 1.2% | Schulze (VSE\* WME\*) |
| MJ | median-based | 92.1 [91.7–92.4] | 80.4 [79.8–81.1] | 4.4 [3.8–4.9] | 6.5 [6.1–6.8] | 18 [17–19] | 99.9 [99.9–99.9] | 82.6 | 97.0 | 93.1 [92.8–93.4] | 79.0 [78.3–79.7] | 5.4 [4.9–5.9] | 5.4 [5.2–5.7] | 20 [20–21] | 68.8 [68.1–69.6] | 71.8 [71.0–72.6] | 72.3 [71.5–73.1] | 74.9 [74.1–75.8] | 70.0 [69.3–70.7] | 72.8 [72.1–73.5] | 1.7 [1.5–1.9] | 2.0 [1.8–2.3] | 0.0% | 1.4% | 0.0% | 33.8% | — |
| MAV | median-based | 91.3 [91.0–91.7] | 79.2 [78.6–79.8] | 3.6 [3.1–4.2] | 7.2 [6.9–7.6] | 17 [16–17] | 99.9 [99.9–99.9] | 82.4 | 97.0 | 92.0 [91.7–92.3] | 76.1 [75.4–76.8] | 4.4 [3.8–4.9] | 6.5 [6.2–6.8] | 18 [17–19] | 66.1 [65.3–66.9] | 70.8 [70.0–71.5] | 70.2 [69.4–71.1] | 74.1 [73.2–74.9] | 67.5 [66.8–68.2] | 71.8 [71.1–72.5] | 2.4 [2.1–2.6] | 2.3 [2.1–2.6] | 0.0% | 1.7% | 0.0% | 34.2% | Schulze (VSE\* WME\*), MJ (VSE\* WME\*) |
| STAR | hybrid | 89.4 [88.7–90.0] | 81.0 [80.3–81.8] | 1.7 [0.9–2.4] | 9.2 [8.6–9.8] | 15 [15–16] | 99.8 [99.7–99.8] | 84.6 | 97.0 | 97.7 [97.6–97.9] | 92.9 [92.5–93.3] | 10.0 [9.5–10.5] | 0.8 [0.7–1.0] | 35 [35–36] | 88.2 [87.7–88.8] | 78.3 [77.6–79.0] | 96.2 [95.9–96.6] | 82.9 [82.2–83.6] | 93.8 [93.4–94.2] | 80.8 [80.2–81.4] | 0.7 [0.6–0.8] | 3.5 [3.2–3.8] | 0.0% | 3.4% | 0.0% | 25.6% | Schulze (VSE\* WME\*), MJ (VSE\*) |
| Score | rated | 88.7 [88.3–89.2] | 76.4 [75.6–77.2] | 1.0 [0.3–1.7] | 9.9 [9.4–10.3] | 14 [13–14] | 100.0 [100.0–100.0] | 74.3 | 97.0 | 95.4 [95.2–95.6] | 86.0 [85.6–86.5] | 7.7 [7.1–8.2] | 3.2 [2.9–3.4] | 27 [26–27] | 76.6 [75.8–77.3] | 71.1 [70.3–72.0] | 95.1 [94.7–95.4] | 75.4 [74.6–76.3] | 94.6 [94.3–95.0] | 75.9 [75.3–76.6] | 1.9 [1.7–2.1] | 2.7 [2.4–3.0] | 0.0% | 0.9% | 0.0% | 57.2% | MJ (VSE\* WME\*), MAV (VSE\* WME\*), STAR (WME\*) |
| Approval | approval-based | 87.6 [87.1–88.1] | 75.0 [74.2–75.7] | -0.1 [-0.8–0.5] | 10.9 [10.5–11.4] | 13 [12–13] | 99.8 [99.8–99.8] | 57.8 | 61.2 | 88.8 [88.3–89.2] | 75.9 [75.2–76.5] | 1.1 [0.5–1.7] | 9.8 [9.4–10.2] | 14 [13–14] | 67.2 [66.4–68.0] | 67.1 [66.3–67.9] | 71.0 [70.2–71.8] | 70.0 [69.2–70.9] | 68.5 [67.8–69.2] | 68.7 [68.0–69.5] | 5.9 [5.5–6.3] | 5.1 [4.8–5.5] | 0.0% | 2.8% | 0.0% | 26.8% | Schulze (VSE\* WME\*), Score (VSE\* WME\*), MJ (VSE\* WME\*) +2 more |
| 321 | hybrid | 87.2 [86.7–87.9] | 74.0 [73.2–74.9] | -0.4 [-1.2–0.3] | 11.3 [10.7–11.9] | 12 [12–13] | 99.8 [99.8–99.8] | 70.6 | 85.1 | 91.6 [91.3–92.0] | 73.1 [72.4–74.0] | 3.9 [3.4–4.5] | 6.9 [6.6–7.2] | 17 [17–18] | 66.9 [66.1–67.7] | 68.6 [67.9–69.5] | 74.7 [73.9–75.5] | 73.3 [72.4–74.1] | 75.1 [74.5–75.8] | 73.1 [72.4–73.9] | 2.5 [2.3–2.7] | 3.3 [3.0–3.5] | 0.0% | 2.8% | 0.0% | 43.2% | Schulze (VSE\* WME\*), Score (VSE\* WME\*), MJ (VSE\* WME\*) +2 more |
| Borda | positional | 80.2 [79.5–80.9] | 64.9 [64.0–65.9] | -7.5 [-8.4–-6.7] | 18.4 [17.6–19.1] | 7.9 [7.6–8.2] | 92.9 [92.8–93.0] | 67.8 | 82.7 | 94.5 [94.2–94.8] | 92.3 [91.9–92.6] | 6.8 [6.2–7.4] | 4.0 [3.7–4.4] | 27 [27–28] | 84.3 [83.7–84.9] | 63.3 [62.5–64.1] | 89.5 [89.0–90.1] | 66.3 [65.4–67.3] | 88.0 [87.5–88.5] | 66.6 [65.9–67.4] | 0.3 [0.2–0.3] | 4.2 [3.9–4.5] | 23.8% | 17.4% | 0.6% | 13.9% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity) +5 more |
| IRV | plurality-based | 77.0 [76.0–77.9] | 64.9 [63.9–65.8] | -10.7 [-11.8–-9.6] | 21.5 [20.6–22.5] | 7.1 [6.9–7.4] | 94.9 [94.8–95.0] | 75.2 | 82.7 | 86.3 [85.8–86.7] | 73.3 [72.6–73.9] | -1.4 [-2.0–-0.8] | 12.3 [11.8–12.7] | 12 [11–12] | 67.4 [66.5–68.2] | 64.3 [63.5–65.1] | 71.9 [71.0–72.7] | 67.5 [66.6–68.4] | 68.3 [67.6–69.1] | 65.4 [64.7–66.1] | 7.2 [6.8–7.6] | 10.3 [9.8–10.7] | 18.3% | 6.7% | 4.0% | 0.6% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity) +5 more |
| Plurality | plurality-based | 64.9 [63.8–65.8] | 54.5 [53.6–55.5] | -22.8 [-23.9–-21.7] | 33.6 [32.7–34.7] | 4.8 [4.6–4.9] | 97.3 [97.2–97.4] | 49.8 | 52.8 | 58.3 [57.1–59.3] | 52.3 [51.3–53.2] | -29.4 [-30.7–-28.3] | 40.3 [39.3–41.4] | 4.5 [4.3–4.6] | 50.0 [49.1–50.8] | 53.3 [52.5–54.2] | 53.1 [52.1–54.1] | 56.2 [55.2–57.1] | 50.0 [49.2–50.8] | 54.2 [53.5–55.0] | 22.5 [21.9–23.2] | 18.4 [17.8–19.0] | 15.2% | 0.0% | 1.3% | 0.0% | Ranked pairs (VSE\* WME\* sincerity), Schulze (VSE\* WME\* sincerity), Approval (VSE\* WME\* sincerity) +5 more |

**Condorcet winner in the cast ballots** (methods with ordered ballots: rankings, scores, grades): what a study of the ballot data would see. Of the 13075 elections with a real (sincere) Condorcet winner: the ballots show the same one, another one (false), or none (a cycle or ties). Of the 1925 without one: the ballots show one anyway (spurious). CE\* (ballots): how often the method elects the ballots' Condorcet winner, where they have one; compare CE\* (real) in the main table. Honest ballots in brackets (rounding and ties of the format alone).

| Method | Same | False | None | Spurious | CE\* (ballots) | CE\* (real) |
|---|---|---|---|---|---|---|
| Schulze | 95.6% (100.0%) | 1.5% (0.0%) | 2.9% (0.0%) | 8.2% (0.0%) | 100.0% (100.0%) | 96.4% |
| Ranked pairs | 93.0% (100.0%) | 2.6% (0.0%) | 4.3% (0.0%) | 10.0% (0.0%) | 100.0% (100.0%) | 94.3% |
| MJ | 81.7% (90.4%) | 11.9% (5.4%) | 6.4% (4.2%) | 65.9% (61.2%) | 83.4% (70.8%) | 71.8% |
| MAV | 81.4% (90.4%) | 12.0% (5.4%) | 6.6% (4.2%) | 66.9% (61.2%) | 82.5% (67.7%) | 70.8% |
| STAR | 80.6% (90.4%) | 13.4% (5.4%) | 6.0% (4.2%) | 66.5% (61.2%) | 94.8% (95.1%) | 78.3% |
| Score | 83.5% (90.4%) | 9.9% (5.4%) | 6.6% (4.2%) | 58.2% (61.2%) | 82.0% (80.6%) | 71.1% |
| 321 | 69.5% (64.8%) | 24.7% (21.2%) | 5.8% (14.0%) | 82.8% (77.9%) | 95.9% (94.0%) | 68.6% |
| Borda | 73.0% (100.0%) | 8.9% (0.0%) | 18.2% (0.0%) | 29.2% (0.0%) | 78.0% (84.3%) | 63.3% |
| IRV | 75.9% (100.0%) | 16.7% (0.0%) | 7.4% (0.0%) | 37.2% (0.0%) | 82.7% (67.4%) | 64.3% |

**Ties.** Split: the share of elections whose winner is drawn by lot among several candidates (a tie the method breaks at random, or a method that draws). Tied pairs, for ordered ballots (rankings, scores, grades): the share of elections in which the cast ballots leave at least one pair of candidates tied (as many voters put one above the other as the other way round), and the share of all pairs that are tied. Learned ballots, honest in brackets.

| Method | Split | Elections with a tied pair | Tied pairs |
|---|---|---|---|
| Schulze | 10.0% (10.9%) | 34.4% (34.5%) | 3.3% (3.3%) |
| Ranked pairs | 10.2% (10.9%) | 34.5% (34.5%) | 3.4% (3.3%) |
| MJ | 0.6% (1.1%) | 28.9% (25.1%) | 2.5% (2.2%) |
| MAV | 2.0% (4.5%) | 28.8% (25.1%) | 2.5% (2.2%) |
| STAR | 1.1% (2.6%) | 26.7% (25.1%) | 2.4% (2.2%) |
| Score | 1.9% (3.6%) | 32.3% (25.1%) | 2.9% (2.2%) |
| Approval | 5.7% (14.1%) | — (—) | — (—) |
| 321 | 3.6% (12.6%) | 39.9% (41.9%) | 3.6% (3.8%) |
| Borda | 1.4% (3.5%) | 33.8% (34.5%) | 3.9% (3.3%) |
| IRV | 9.4% (14.6%) | 32.3% (34.5%) | 3.5% (3.3%) |
| Plurality | 4.1% (8.4%) | — (—) | — (—) |

![strategy arrows](arrows-c2.png)

![strategy shifts](shift-c2.png)

![sincerity](sincerity-c2.png)

![fidelity](fidelity-c2.png)

**Trade-off on the Pareto front:** for each weighting of VSE\*, WME\* and sincerity (a point of the triangle; corners: all weight on one axis), the Pareto method with the highest weighted sum. Each axis is stretched to the range the Pareto methods span on it, so the picture shows the trade-offs within the front, not their absolute size. Point estimates; the methods hidden in the scatter plots are left out, so it is the Pareto front of the methods shown.

![trade-off](tradeoff-c2.png)

Scatter and chain plots leave out Condorcet-random-drop, Geometric median, Random ballot, Random pair runoff, Anti-plurality, TRASH, CW else Score loser, CL else Score winner, Inverse-Plurality, Inverse-Approval, Inverse-Minimax, Inverse-IRV, Inverse-ER-IRV, Inverse-TTR, Inverse-Borda, Inverse-Score; they are in all tables and heatmaps.
