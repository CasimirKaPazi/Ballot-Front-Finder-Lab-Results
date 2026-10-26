# Voting Methods under Poll-Based Strategy: Findings of the Ballot Front Finder Lab Reference Runs

**Abstract.** The Ballot Front Finder Lab simulates single-winner elections in which some voters respond to polls. It measures each voting method on two goals, the total satisfaction of the voters with the winner (VSE) and the winner's support against every rival head to head (WME), together with how honest the voters can afford to be. The reference runs compare 35 methods in 37 conditions, each changing one assumption of the model or several at once, and 66 methods in the base condition.

No method is best everywhere. About twenty methods are within one point of the front in nearly every condition: the choice among them is mainly a choice of position on the trade-off between the two goals.
- Score runoff is undominated most often (80% of its conditions), but it attracts clones under strategic nomination.
- The Condorcet methods sit at the corner of what any rule can reach, but lose ground with truncated rankings.
- The rated methods' lead in total satisfaction depends on a steep fall-off of utility with distance and on moderate strategy.

How strongly the two goals conflict is the model's most consequential assumption. Good poll information lets weak methods compensate through strategy. Simplicity, which the lab does not measure, has to be weighed separately.

The commands, the lab commit and how to make the results again are in [reference-runs.md](reference-runs.md). The reports cited are in the run folders (`*/analysis*/report.md`), `overview-*/` and `comparison/`.

## 1 Introduction

Every voting method turns ballots into a winner, and every method can be gamed to some degree. Comparisons with honest ballots alone flatter methods that are easy to manipulate. Comparisons under fully rational strategy depend on assumptions nobody believes. The lab places voters in between: some of them read polls and change their ballots when that pays, the way people vote tactically in real elections, and it measures what each method makes of that.

Two goals are measured, because they pull apart. A winner can satisfy the electorate as a whole (high total utility) and still lose a head-to-head vote against another candidate, and the other way round. No method reaches the best on both, so the result is a front of methods, each the best at some balance of the two, rather than a single ranking. The question shifts from "which method is best?" to "which of the good methods do we want, given what we value?".

This document reports a fixed set of runs made with version 1.0.0 of the lab, chosen to test how robust the findings are, not to tune the model. It can be read on its own; the lab's documentation (`docs/model.md`, `docs/analysis.md`) has the full details.

## 2 The Lab in Brief

**Electorates.** In each election, 99 voters and 6 candidates are points in a six-dimensional issue space, the voters gathered in clumps within clumps (a fractal-noise density). A voter's utility for a candidate falls exponentially with distance (width 0.7), so far-away candidates are nearly equally bad. Every election draws a new electorate.

**Ballots and strategy.**
- Each voter first fills in the method's ballot honestly. A ranking follows the utilities, approval goes to candidates above a personal threshold, and scores 0–5 are stretched to the voter's own range.
- Then follow three poll rounds. A poll shows the method's own counts from a random sample of 49 voters (10 polls per round), with twice the sampling error, as real polls have.
- Voters are adaptive: each votes strategically in a round when the race between the two leaders matters enough to them. Groups of like-minded voters try ballot changes and keep a change only if it improves the polled result for them.
- Half of the strategic voters are aggressive (they exaggerate whenever it cannot hurt them), half kind (only for a clear gain).

**Measures.**
- **VSE** (voter satisfaction efficiency) and **WME** (worst-margin efficiency) are both 0 for a random ballot and 100 for the best candidate of each election. VSE measures the winner's total utility. WME measures the winner's smallest head-to-head margin against any rival, by sincere preferences, so it is a refined Condorcet criterion.
- Starred values (VSE\*, WME\*) are the results after the poll rounds; unstarred ones are with honest ballots.
- **Sincerity** measures how far the strategic ballots go against the voters' own preferences, within each ballot format: 100 = no preference reversed. A rated ballot can be strategic by exaggerating (ties), which reverses no preference; a ranking can only be strategic by reversing one. So ranked methods sit near 98 and rated ones near 99.9, and differences under 1 point count as ties. Fidelity, which also counts ties, drops by about the same amount under strategy in both formats (base: rated ballots 94.1 → about 90, rankings 79.7 → about 76.4).

**Frontiers.** The **reachable frontier** is the best combination of VSE and WME that any rule could reach knowing every voter's utilities. The **normalized-ballot line** is what is within reach when every voter's utilities are stretched between their least and most liked candidate, as score ballots do.

**The front.** Methods are judged on VSE\*, WME\* and sincerity together:
- **Undominated**: no other method is clearly better on one of the three (the paired 95% bootstrap interval of the difference excludes 0) and clearly worse on none. All methods face the same elections, so differences are measured precisely.
- **Pareto front**: the same with point estimates.
- **Near the front (1 / 2)**: no method is better by 1 (2) points on VSE\* and WME\* at once.

**Across runs** (`comparison/`, made with `compare.py --boot 1000`), every condition counts once, whatever the number of elections behind it. Shares are over the conditions a method was run in, and these sets differ:
- the vse-sim setting has only 11 methods;
- the max-score sweep has 28 (the methods that read scores; the others were not run again);
- the methods outside the extended set are only in the base.

So Score runoff's shares are over 35 conditions without the vse-sim setting, Schulze's over 35 without the max-score sweep.

## 3 What Was Tested

The tests use the set `extended`, 35 methods: the standard set of the lab and the strongest newer methods. They cover plurality-based methods (Plurality, IRV and variants, Supplementary vote), two-round methods with a real runoff (TTR, Approval runoff, Score runoff), Condorcet and minimax methods, score–pairwise hybrids (STAR, Smith//Score, BTR-score, the Soft Minimax family, MARS, ARA, 3-2-1), rated methods (Score, Geometric median, DMS, ESR), median-based methods (MJ, MAV) and Borda. The base condition has all 66 methods of the lab.

| Test                 | What changes from the base                                                                                                               | Values              | Elections   |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ----------- |
| Base                 | nothing (the defaults above), with a manipulation test                                                                                   | —                   | 2000        |
| Defaults             | the run a user gets with `python run.py` (standard set)                                                                                  | —                   | 200         |
| Strategic share      | a fixed share of always-strategic voters instead of adaptive voters                                                                      | half, all           | 1000        |
| Kind or aggressive   | all strategic voters kind, or all aggressive                                                                                             |                     | 1000        |
| Poll rounds          | number of poll rounds (3)                                                                                                                | 1, 2, 4, 5, 6       | 1000        |
| First poll           | the first poll shows a previous election's strategic result                                                                              |                     | 1000        |
| Poll samples         | polls per round (10)                                                                                                                     | 5                   | 1000        |
| Poll size            | voters per poll (49), with 5 polls per round                                                                                             | 9                   | 1000        |
| Laziness             | truncated honest ballots                                                                                                                 | 0.1, 0.5            | 1000        |
| Score range          | top score (5), for the methods with score ballots                                                                                        | 9, 99               | 2000        |
| Nomination           | candidates may withdraw or field clones                                                                                                  | exits 1, clones 2   | 1000        |
| Electorate model     | clumps (noise)                                                                                                                           | clusters, field, ks | 1000        |
| Utility width        | 0.7                                                                                                                                      | 1.4, 2.8            | 1000        |
| Utility shape        | exponential                                                                                                                              | linear, quadratic   | 1000        |
| Candidate dispersion | candidates spread like the voters (1)                                                                                                    | 0.5, 2              | 1000        |
| Candidates           | 6                                                                                                                                        | 4, 5, 7, 8, 9, 10   | 700 each    |
| Voters               | 99                                                                                                                                       | 49                  | 1000        |
| Hard                 | candidate dispersion 0.5, laziness 0.1, nomination and polls of 9 at once                                                                |                     | 1000        |
| vse-sim setting      | Quinn's model as in vse-sim's code, linear utility, 40 voters, scores 0–10, one poll, half or all voters strategic; vse-sim's 11 methods |                     | 15 000 each |

That makes 37 conditions. All runs use seed 1, so the tests face the base's first elections, and a test that changes only strategy or ballots faces exactly the same electorates.

## 4 Results across All Conditions

![Base run with 35 methods](base/analysis/arrows-c1.png)

### 4.1 The Leading Methods Are Close; They Differ in where They Sit on the Trade-off

Over the 37 conditions, about twenty methods are within 1 point of the front (on VSE\* and WME\* at once) in nearly every condition they were run in: Score runoff, the Condorcet and minimax methods, the Soft Minimax family, BTR-score, Smith//Score, STAR, ARA, MARS, Geometric median, ESR. What separates them is less how good they are than which mix of VSE and WME they deliver.

| Method | Group | Conditions | Undominated | Pareto front | Near front (1 / 2) | Mean VSE\* | Mean WME\* | Mean sincerity |
|---|---|---|---|---|---|---|---|---|
| Score runoff | two-round | 35 | 80% | 89% | 100% / 100% | 87.0 | 97.5 | 99.8 |
| Ranked pairs | condorcet | 35 | 73% | 66% | 100% / 100% | 86.2 | 98.8 | 97.6 |
| Kemeny-Young | condorcet | 33 | 68% | 32% | 97% / 100% | 85.5 | 98.9 | 97.8 |
| Schulze | condorcet | 35 | 67% | 40% | 100% / 100% | 86.2 | 98.7 | 97.6 |
| Minimax | minimax-family | 33 | 58% | 30% | 100% / 100% | 85.6 | 98.8 | 97.8 |
| MMWV | minimax-family | 33 | 55% | 27% | 97% / 100% | 85.4 | 98.8 | 98.2 |
| Soft Minimax-D | hybrid | 35 | 53% | 66% | 100% / 100% | 88.4 | 95.9 | 99.9 |
| Soft Minimax | hybrid | 35 | 46% | 80% | 100% / 100% | 88.0 | 96.3 | 99.9 |
| BTR-score | hybrid | 35 | 44% | 33% | 100% / 100% | 88.0 | 96.0 | 99.9 |
| Smith//Score | condorcet-based | 35 | 33% | 29% | 100% / 100% | 88.2 | 95.8 | 99.9 |
| Geometric median | rated | 35 | 17% | 74% | 94% / 97% | 90.8 | 90.3 | 99.9 |
| ESR | rated | 35 | 3% | 43% | 100% / 100% | 89.8 | 93.1 | 99.9 |
| Score | rated | 37 | 1% | 34% | 92% / 100% | 90.2 | 91.4 | 99.9 |

(Selection from `comparison/report.md`, which lists all methods.)

Three groups emerge:

- **Condorcet and minimax methods** (WME\* about 99, VSE\* about 85–86): undominated in 44–73% of the conditions (MMPO 44%). Their sincerity is about 2 points lower than that of rated methods, since strategy on a ranking has to reverse preferences.
- **Score–pairwise hybrids** (Soft Minimax family, BTR-score, Smith//Score, MARS, ARA, STAR; WME\* 94–96, VSE\* 88–90): undominated in 11–53%, but often on the Pareto front (up to 80%).
- **Rated methods** (Score, Geometric median, DMS, ESR; highest VSE\*, about 90–91, WME\* 90–93): on the Pareto front in a third to three quarters of the conditions, but rarely undominated (1–17%). Their lead on VSE\* over the hybrids is mostly within the noise, while the hybrids' lead on WME\* is clear: that combination leaves a method on the Pareto front but dominated. Their VSE lead also depends on the base's steep utility and on moderate strategy (sections 5 and 7).

Of the other methods, only Approval runoff is undominated anywhere (in one condition). They fall into two kinds:

- **Clearly off the front:** every plurality-based method including IRV, and TTR and Approval. They are within 2 points of the front in at most 10% of the conditions.
- **Close behind:** 321, MAV, MJ, Borda, Approval runoff and Approval cond. runoff. They are within 2 points in 50–94% of the conditions, but within 1 point in only 6–48%.

### 4.2 Who Clearly Dominates Whom

The figure orders the methods by clear dominance over all conditions. An arrow A → B means that A dominates B in at least half of the conditions both were run in, and more often than B dominates A. Arrows implied by a chain of others are left out, so every method is dominated, at this level, by everything above it along the arrows. The shares are in `comparison/report.md` and `comparison/beatpath.csv`.

![Clear dominance across conditions](comparison/beatpath.png)

- **The top layer** holds 14 methods that no method dominates in half of their conditions: Score runoff, the Condorcet and minimax methods (Ranked pairs, Kemeny-Young, Schulze, Minimax, MMWV, MMPO) and seven score–pairwise hybrids (Soft Minimax, Soft Minimax-D, Soft Minimax-40, BTR-score, Smith//Score, ARA, MARS-T). This is the group of good methods of section 4.1, seen from another side.
- **Just below** are methods dominated by one or two of them:
  - Copeland and BTR-IRV by Score runoff and every Condorcet and minimax method (65–91%);
  - MARS and STAR by Soft Minimax (57% and 63%);
  - Approval runoff by Score runoff (91%) and several hybrids;
  - ESR by ARA, MARS, MARS-T and Soft Minimax-40 (54–71%).
- **The rated methods sit in the middle.** Geometric median is dominated by ESR (71%). Score is dominated by STAR, ESR, Smith//Score and Soft Minimax-D (50–59%), and DMS by Score (57%). Their high VSE does not protect them from methods that are about as good on VSE and clearly better on WME.
- **Further down**, in this order: Borda, the median-based methods (MAV, MJ), then the IRV variants and Approval, then TTR and Supplementary vote, and Plurality at the bottom, dominated by Supplementary vote in 88% of the conditions.

### 4.3 Score Runoff is the Most Robust Single Method

Score runoff (scores in round one, a real runoff between the two highest-scoring candidates) is undominated in 28 of its 35 conditions and within 1 point of the front in all. It combines a WME\* close to the Condorcet methods' (mean 97.5 against about 99) with the sincerity of score ballots, and its Condorcet efficiency with strategic ballots is 96.0% in the base (Condorcet methods: 95.5–98.0%).

Against the Condorcet methods it is clearly better on VSE\* in about half of the conditions or more (15.5–19 of 33) and clearly worse on WME\* in about four fifths (25.5–26.5 of 33); the higher sincerity then decides. Against the hybrids it is the other way round: they are better on VSE\*, it is better on WME\*. It dominates Smith//Score in 15 of 35 conditions, Soft Minimax in 14, BTR-score in 13.5.

It is dominated in seven conditions:

- **All strategic voters kind** (exaggerating only for a clear gain): the Condorcet methods dominate it.
- **Strategic nomination** (exits and clones): Soft Minimax, Soft Minimax-D and BTR-score dominate it, and are better on WME\*, the axis where it normally leads. It attracts more clones than the other methods near the front.
- **A fixed share of always-strategic voters (half or all); wider utility (width 1.4)**: Soft Minimax dominates it.
- **Scores 0–9 and 0–99**: Soft Minimax-D dominates it. On finer scales the score–pairwise hybrids approach the Condorcet methods, while Score runoff stays where it was.

Each of these is examined in the section of its test.

### 4.4 Condorcet Methods Sit at the Corner of what Any Rule Can Reach

In the base (2000 elections), the reachable region is bounded by VSE 83.1–83.4 at the best WME (electing the candidate with the best worst margin) and by the balanced point VSE 94.4 / WME 95.9. Ranked pairs, Schulze and Kemeny-Young reach VSE\* 83.2–83.5 at WME\* 99.3–99.4: practically that corner, even with strategic voters.

At the other end, Geometric median reaches VSE\* 90.3, against at most 90.5 for any rule reading normalized ballots (each voter's utilities stretched between their least and most liked candidate). Score ballots cannot carry more, so the best rated methods are at the limit of their ballot, not held back by strategy (Geometric median honest 91.2, strategic 90.3).

In between, the hybrids fall short of the reachable frontier: Smith//Score has VSE\* 87.3 at WME\* 96.1, while a rule knowing the utilities reaches VSE 94.4 at about the same WME. Most of this gap is what score ballots cannot carry: in the arrow plot of `overview-all-methods`, the line of normalized ballots runs at about VSE 89.5 at WME 96 and about 88.5 at WME 98 (read off the plot). The hybrids are 1–2.5 points short of it, Score runoff about 3. The methods of the front thus lie close to what their ballots allow, all along the front: the Condorcet methods at the corner of the reachable region, the rated methods at the end of the normalized-ballot line, the hybrids and Score runoff a little inside that line.

### 4.5 With Adaptive Voters, Strategy Changes Little for the Good Methods and Helps the Weak Ones

From honest to strategic ballots in the base:

| Method | VSE → VSE\* | WME → WME\* |
|---|---|---|
| Geometric median | 91.2 → 90.3 | 90.1 → 90.4 |
| Score | 90.2 → 88.4 | 92.7 → 92.2 |
| STAR | 87.9 → 86.8 | 95.7 → 95.5 |
| Score runoff | 85.3 → 85.2 | 98.3 → 98.0 |
| Ranked pairs | 83.3 → 83.5 | 100.0 → 99.3 |
| IRV | 85.4 → 84.9 | 87.7 → 90.8 |
| TTR | 83.8 → 83.3 | 84.8 → 90.4 |
| Borda | 80.9 → 85.5 | 95.4 → 95.7 |
| Plurality | 69.8 → 79.8 | 69.0 → 76.9 |

The good methods move by at most 2 points. Plurality-based methods gain: compromising on a viable, broadly acceptable candidate is what strategy does there. The share of adaptive voters who vote strategically varies little between methods (40.6–51.1%), highest under Plurality.

How much a different strategy model changes this is the subject of the strategy tests.

### 4.6 Manipulation from Honest Ballots: Condorcet Methods Hold, but Counter-strategy is Needed

The manipulation test (base, extended set) asks how often an informed coalition can change the result in its favour from honest ballots, and what that does to VSE:

- **One-sided** (only the attackers act): Score 87.8% of elections, DMS 86.2%, Borda 93.0%; the Condorcet methods 57–60%; Score runoff 51.0%; Soft Minimax 40.8%; TTR 27.7%.
- **Two-sided** (the honest winner's supporters answer): Condorcet methods 2.4%, Score runoff 5.7%, Soft Minimax 12.5%, Score 19.4%, Plurality 40.5%. The VSE with the worst successful manipulation is then 81–83 for the Condorcet methods, Score runoff and Geometric median, against 53.8 for Plurality.

So the methods resist manipulation mainly because the attacked side can answer, which is what the poll rounds simulate.

### 4.7 Rated Ballots Hide the Condorcet Winner More Often than Strategy Does

In the base, rated ballots show the real Condorcet winner in about 88% of the elections with one, with strategic and with honest ballots alike: the 0–5 scale's ties and rounding alone hide or falsify it in about 12%. Rankings show it in 100% honestly and 95–97% after strategy. A study of real score or grade ballots would see a false Condorcet winner in about 9% of elections from the format alone.

### 4.8 Sample Size: the Default Run as an Example

The default run (200 elections, standard set) finds Score, Score runoff and MMPO undominated. In the base (2000 elections, same first 200), Score is dominated by Smith//Score and others on WME\*. With 200 elections the intervals are about ±4 points on VSE\*, so fewer differences are clear: the default run is an example to compare one's own runs with, not a result.

### 4.9 Steps up from Plurality

Pairs asked for in the comparison show how the simple methods relate, which matters for practical reforms (section 10.3):

| Pair | First dominates second | Second dominates first |
|---|---|---|
| Approval — Plurality | 35 of 35 conditions | 0 |
| Supplementary vote — Plurality | 29 of 33 | 0 |
| TTR — Supplementary vote | 15.5 of 33 | 0 |
| IRV — TTR | 25.5 of 33 | 1 |
| IRV-3 — TTR | 29 of 33 | 0 |
| Approval cond. runoff — TTR | 33 of 33 | 0 |
| Approval cond. runoff — Approval | 29 of 33 | 0 |
| Approval runoff — Approval cond. runoff | 24.5 of 33 | 1 |
| BTR-score — IRV | 33 of 33 | 0 |

- **Approval dominates Plurality in every condition**, with a ballot almost as simple.
- **The plurality-based methods form a ladder:** Supplementary vote over Plurality, TTR over Supplementary vote (in about half the conditions), IRV and IRV-3 over TTR. An approval first round makes a two-round system much better: Approval cond. runoff dominates TTR in every condition.
- **Adding a runoff to Approval helps** (Approval cond. runoff over Approval in 29 of 33 conditions), and a runoff between the two most-approved candidates helps more (Approval runoff over Approval cond. runoff in 24.5 of 33).
- **A score–pairwise method beats IRV in every condition** (BTR-score shown here).

## 5 Strategy

The tests have 1000 elections, the first 1000 of the base's 2000. Their honest values therefore differ slightly from the base's (Score: honest VSE 89.9 against 90.2), so changes are read as the cost of strategy within each condition (honest minus strategic) or between the tests. The noise reference for rank stability in the reports is that of the 2000-election base; a test with 1000 elections can fall a little below it from noise alone.

### Who Strategises

The base has adaptive voters: each is strategic in a round when the race between the two leaders matters enough to them, which is the case for about 43% of them. The test replaces them with a fixed share of always-strategic voters, half or all.

| Method | Base (adaptive) | Half strategic | All strategic |
|---|---|---|---|
| Geometric median | 90.3 / 90.4 / 99.9 | 90.4 / 92.1 / 99.9 | 87.3 / 92.4 / 99.7 |
| Score | 88.4 / 92.2 / 100.0 | 86.9 / 94.2 / 100.0 | 84.7 / 92.0 / 99.9 |
| ESR | 89.1 / 93.2 / 99.9 | 87.5 / 94.8 / 99.9 | 86.3 / 95.3 / 99.9 |
| Soft Minimax | 86.5 / 96.1 / 99.9 | 86.6 / 97.6 / 99.9 | 84.9 / 97.8 / 99.8 |
| BTR-score | 86.7 / 96.3 / 99.9 | 86.3 / 97.2 / 99.9 | 84.6 / 97.5 / 99.8 |
| Score runoff | 85.2 / 98.0 / 99.9 | 84.4 / 98.3 / 99.8 | 83.4 / 97.6 / 99.6 |
| Ranked pairs | 83.5 / 99.3 / 97.9 | 84.3 / 99.4 / 96.4 | 82.9 / 97.2 / 92.9 |
| MMWV | 83.1 / 99.0 / 98.3 | 84.0 / 99.1 / 97.4 | 82.8 / 96.5 / 94.9 |
| IRV | 84.9 / 90.8 / 97.6 | 84.7 / 93.4 / 96.2 | 83.0 / 92.8 / 92.9 |
| Plurality | 79.8 / 76.9 / 98.3 | 78.2 / 79.6 / 96.3 | 81.1 / 81.9 / 92.2 |

VSE\* / WME\* / sincerity. Honest VSE on the tests' elections: Geometric median 90.7, Score 89.9, ESR 89.2, Soft Minimax 87.1, BTR-score 87.1, Score runoff 86.0, Ranked pairs 84.5, MMWV 84.6, IRV 85.5, Plurality 67.4.

- **Half strategic.** Only a little more strategy than in the base, but regardless of the stakes. Score methods lose more VSE than with adaptive voters (Score 3.0 points instead of 1.8), and WME\* rises for most rated and hybrid methods. The Condorcet methods lose 1.5 points of sincerity (burial). Undominated: Geometric median, Soft Minimax, Smith//Score, BTR-score, MMPO, MMWV. Here MMWV and MMPO dominate Schulze, Ranked pairs and Minimax on sincerity: they bury, but do not compromise.
- **All strategic.** This is the one strategy test that clearly reorders the methods (rank correlation with the base: VSE\* 0.72, WME\* 0.77, against 0.97 and 0.99 from noise). Every method loses:
  - Rated methods lose most of their VSE lead: Score 5.2 points, DMS 5.8, Geometric median 3.4.
  - Condorcet methods lose WME (100 → 96.5–97.5) and sincerity (about 93; 26% of the voters bury, 14% compromise; MMWV and MMPO: 43% bury).
  - Undominated: only **Soft Minimax and BTR-score**, two score–pairwise hybrids. Soft Minimax dominates Score runoff here, and no rated method is left undominated.
- Plurality gains with more strategy: compromise is what makes it work at all.

### Kind or Aggressive

In the base, half the strategic voters are aggressive (they exaggerate whenever it cannot hurt them in the polls), half kind (only for a clear gain). The test makes them all kind or all aggressive.

- **Rated and hybrid methods hardly change.** Score has VSE\* 89.9 with kind voters and 89.0 with aggressive ones (honest 89.9); ARA, STAR and the Soft Minimax family move by about 1 point or less.
- **Ranked methods change in sincerity, much less in result.** With kind voters, almost nobody buries: the Condorcet methods' sincerity is 99.8 (burial 1%), VSE\* 83.9–84.8, WME\* 99.4–99.8. With aggressive voters it is 96.0–96.7 (burial 24–33%), VSE\* 83.4–84.1, WME\* 98.2–98.7.
- **The base's sincerity gap between ranked and rated methods thus comes from aggressive burial,** which costs the Condorcet methods little in the result. Since that gap is what keeps Score runoff undominated against them, the order between the two depends on this assumption:
  - With kind voters, the Condorcet methods dominate Score runoff (equal sincerity, better WME\*), and only STAR, Smith//Score, BTR-score, Kemeny-Young and Ranked pairs are undominated.
  - With aggressive voters, the front is wide (12 undominated methods, Condorcet, minimax, hybrid and Score runoff alike).
- Plurality does worse with kind voters (VSE\* 73.8, against 82.3 with aggressive ones): fewer voters compromise.

### Poll Rounds

The rounds test reports one run after 1, 2, 4, 5 and 6 rounds (the base has 3), so the rounds are successive states of the same elections, not separate samples.

- **The result settles after the first round.** The good methods stay within about 1 point from round 1 to round 6, and the order of the methods stays close to the base's (rank correlation 0.94–0.96 on VSE\*, 0.97–0.98 on WME\*).
- Score runoff and the Condorcet methods are undominated after every number of rounds; the other undominated methods change from round to round (Soft Minimax, BTR-score, Soft Minimax-D, DMS, Geometric median, ARA, Smith//Score each in some).
- Plurality improves over the first rounds (VSE\* 76.8 after one, 79.6 after four) and then stays.
- DMS rises steadily (VSE\* 89.6 after one round, 91.0 after six, above its honest 90.1). Earlier runs also found DMS's position unsteady; it is the one method that drifts.

### First Poll from a Previous Election

Here the first poll shows the strategic result of a previous election instead of honest ballots: can the electorate leave a strategic state?

- **The good methods do.** Their results are within about 1 point of a start from honest polls (Score 88.9, Geometric median 90.0, Score runoff 85.8 / 97.5). The Condorcet methods lose a little WME\* (98.8–98.9) but bury less (sincerity 98.1).
- **Plurality, TTR and Supplementary vote end lower:** Plurality VSE\* 75.2, against about 79.5 after the same number of rounds from honest polls (rounds test), TTR 82.2, Supplementary vote 81.4. The previous strategic state persists there.
- Undominated: MARS-T, Soft Minimax, Score runoff and the Condorcet and minimax methods.

### Fewer Poll Samples

The samples run used 5 polls per round instead of 10, with the default 3 rounds (not 6 rounds as in reference-runs.md): less poll information than in the base.

- **The results hardly change.** The cost of strategy is the same within a few tenths (Score 1.6 points instead of 1.8, Geometric median 0.9 as in the base), and the Condorcet methods' sincerity is 97.7 instead of 97.9.
- The order of the methods is close to the base's (rank correlation 0.95 on VSE\*, 0.97 on WME\*). The undominated methods: ESR, Soft Minimax, Score runoff and the Condorcet and minimax methods.
- Since the polls take a large part of the running time, 5 polls per round (or a value between 5 and 10) would serve as well and save time. That would change the default, and every result with it, so it is for a later version of the lab.

### Smaller Polls

The poll-size run uses polls of 9 voters instead of 49, with 5 polls per round; it is compared with the samples run, which differs from it only in poll size. VSE\* / WME\* (honest VSE in the first column):

| Method | Honest VSE | Polls of 49 | Polls of 9 |
|---|---|---|---|
| Geometric median | 90.7 | 89.8 / 90.7 | 89.5 / 89.5 |
| Score | 89.9 | 88.3 / 91.6 | 88.6 / 90.7 |
| Soft Minimax | 87.1 | 86.6 / 96.7 | 86.8 / 95.7 |
| Score runoff | 86.0 | 85.3 / 98.0 | 85.8 / 98.2 |
| Ranked pairs | 84.5 | 83.3 / 99.1 | 83.2 / 97.9 |
| Approval | 85.1 | 86.5 / 86.5 | 85.2 / 82.8 |
| Borda | 81.4 | 84.4 / 93.2 | 80.9 / 91.9 |
| IRV | 85.5 | 85.6 / 90.8 | 80.8 / 84.6 |
| TTR | 83.9 | 82.9 / 89.6 | 81.1 / 83.9 |
| Supplementary vote | 79.1 | 82.9 / 83.7 | 78.1 / 78.1 |
| Plurality | 67.4 | 77.5 / 74.9 | 67.3 / 67.1 |

- **Most methods do not care.** The rated, hybrid and score-runoff methods move by about 1 point of VSE\* or less.
- **The plurality-based methods, Borda and to a lesser extent Approval lose what strategy gave them.** Plurality falls back to its honest value (VSE\* 77.5 → 67.3). IRV and TTR end below their honest values (IRV VSE\* 80.8, honest 85.5). With good polls, their voters compromise on a viable candidate and so repair the method's faults; with poor polls they cannot, and part of their strategy misfires.
- **The Condorcet and minimax methods lose 0.6–1.8 points of WME\*** (Ranked pairs 99.1 → 97.9) and are then dominated by Score runoff (VSE\* and sincerity). Undominated: ARA, MARS, STAR, Score runoff.
- **How much poll information is realistic** is an open question, discussed in section 10.2.

### What the Strategy Tests Show together

- **Score runoff or the Condorcet methods, usually both, are undominated under every strategy model tested except full strategy.** Kind voters tip the balance to the Condorcet methods; with all voters strategic, only the score–pairwise hybrids Soft Minimax and BTR-score remain undominated.
- **For the good methods, the amount of strategy matters more than its timing or information.** Rounds, poll samples, poll size and the starting poll change little. A fixed share of always-strategic voters changes a lot, and full strategy reorders the methods.
- **The weak methods depend on good information.** With small polls, Plurality, IRV, TTR and Borda lose most of what strategy gave them; in the base's generous polls, strategy hides part of their faults.
- **Rated methods' VSE lead holds only while strategy is moderate.** With all voters strategic, Score falls level with Soft Minimax (84.7 and 84.9), Geometric median keeps a lead of 2.4 points but is dominated on WME\*, and no rated method is left undominated.

## 6 Ballots

### Laziness

With laziness λ, honest ballots are truncated: each voter ranks, approves or scores only down to a personal cut, with the cuts spread from complete ballots to the middle of the voter's range (λ = 0.5) or less (0.1). Real ranked ballots suggest about 0.1 for an engaged electorate with few candidates and 0.2–0.5 for public elections with many (the lab's docs/model.md, Laziness). Plurality and TTR name a single candidate anyway and do not change.

- **At 0.1, little changes.** Undominated: Score runoff and the Condorcet methods (Schulze, Minimax, Ranked pairs, Kemeny-Young). The Condorcet methods lose half a point of honest WME (100 → 99.5).
- **At 0.5, truncated rankings move the Condorcet methods into the middle of the front.** Unranked candidates are tied last, so the pairwise counts miss the preferences among them. The Condorcet methods' honest WME falls to about 95, their honest VSE rises (84.6 → 87.7), and after strategy they stand at about VSE\* 87.4 / WME\* 95.6, among the hybrids, but with the lower sincerity of rankings. Borda gains even more (honest VSE 81.4 → 88.4): truncation turns it into something closer to approval.
- **Score ballots hardly change**, since a blank counts as 0, close to what a lazy voter would give anyway: Score 89.7 / 91.2, Geometric median 89.8 / 89.2.
- **At 0.5, Score runoff keeps the highest WME\* of all methods** (VSE\* 86.3 / WME\* 97.4), about 2 points above the Condorcet methods. Undominated: Score runoff, ARA, Smith//Score and Soft Minimax; none of the Condorcet methods. The order of the methods changes clearly (rank correlation with the base on VSE\* 0.72).

### Score Range

Scores 0–9 and 0–99 instead of 0–5, for the methods with score ballots (rated, hybrid, median-based, Smith//Score, Score runoff). The other methods do not read scores, so they were not run again; their values are those of the base. VSE\* / WME\*:

| Method | 0–5 (base) | 0–9 | 0–99 |
|---|---|---|---|
| Score | 88.4 / 92.2 | 89.2 / 92.0 | 89.7 / 91.3 |
| Geometric median | 90.3 / 90.4 | 91.1 / 90.3 | 90.5 / 90.1 |
| DMS | 89.5 / 90.9 | 90.2 / 91.0 | 89.8 / 91.1 |
| ESR | 89.1 / 93.2 | 89.0 / 93.5 | 87.8 / 94.5 |
| ARA | 88.7 / 94.6 | 88.6 / 95.6 | 88.0 / 97.2 |
| Soft Minimax | 86.5 / 96.1 | 86.7 / 96.8 | 87.3 / 96.8 |
| MARS | 87.3 / 95.4 | 87.2 / 96.4 | 86.3 / 98.3 |
| MARS-T | 87.9 / 95.5 | 87.0 / 96.5 | 86.5 / 98.1 |
| Soft Minimax-D | 87.2 / 95.9 | 86.4 / 97.2 | 84.5 / 99.0 |
| Smith//Score | 87.3 / 96.1 | 86.3 / 97.2 | 83.7 / 99.0 |
| BTR-score | 86.7 / 96.3 | 85.6 / 97.6 | 83.7 / 99.1 |
| MMWV+Score | 86.3 / 96.3 | 85.7 / 97.4 | 83.7 / 99.4 |
| Score runoff | 85.2 / 98.0 | 85.3 / 97.9 | 85.3 / 97.8 |

(The base and the two tests have the same 2000 elections.)

- **The middle position of the score–pairwise hybrids is a product of the coarse scale.** On a 0–5 ballot, the rankings that scores imply have many ties, so the pairwise step sees only part of the preferences. On 0–99 there are few ties, and Smith//Score, BTR-score, MMWV+Score and Soft Minimax-D move to the Condorcet corner (WME\* about 99, VSE\* 83.7–84.5). This already holds for honest ballots (Smith//Score honest: VSE 87.1 / WME 96.2 on 0–5, 84.0 / 99.8 on 0–99), so it is the ballot, not strategy. The Condorcet methods do not depend on the score range, and these are the same elections, so the base's values hold for them (Ranked pairs 83.5 / 99.3 / sincerity 97.9). On 0–99, these hybrids match the Condorcet methods on VSE\* and WME\* and are 2 points more sincere.
- **ARA, MARS, MARS-T and Soft Minimax stay in the middle,** since their pairwise part only partly overrides the scores. At 0–99, ARA (88.0 / 97.2), MARS, MARS-T, IC-MARS, Soft Minimax-D, MMWV+Score and MMWVxScore are the undominated methods; at 0–9, Geometric median, ARA, MARS and Soft Minimax-D.
- **Score runoff does not depend on the scale** (85.2–85.3 / 97.8–98.0): its runoff reads a separate ranking. On finer scales, the hybrids catch up with it on WME\* while keeping more VSE\*, and Soft Minimax-D dominates it at both 0–9 and 0–99.
- **Pure rated methods change little.** Score's cost of strategy falls (honest 90.2 on 0–5 and 90.5 on 0–99; strategic 88.4 and 89.7), and ESR loses a little VSE on the fine scale (89.1 → 87.8).
- The order of the methods changes clearly at 0–99 (rank correlation with the base: VSE\* 0.83, WME\* 0.91, against 0.94 and 0.98 from noise for these methods), not at 0–9 (0.94 and 0.99).

### Strategic Nomination

Before the vote, candidates may withdraw (exits 1) or field up to two clones (clones 2) when that helps them (after Green-Armytage 2014). VSE and WME are judged on the original field.

| Method | Honest result changed | Exits | Clones | VSE\* / WME\* (base) | VSE\* / WME\* (nomination) |
|---|---|---|---|---|---|
| Plurality | 32.8% | 0.95 | 0.88 | 79.8 / 76.9 | 79.0 / 79.9 |
| Geometric median | 12.1% | 0.36 | 0.15 | 90.3 / 90.4 | 84.3 / 93.9 |
| Score runoff | 10.7% | 0.15 | 0.19 | 85.2 / 98.0 | 86.3 / 96.1 |
| Score | 10.3% | 0.23 | 0.06 | 88.4 / 92.2 | 86.2 / 93.5 |
| Borda | 10.3% | 0.05 | 0.55 | 85.5 / 95.7 | 85.4 / 94.8 |
| ARA | 9.2% | 0.21 | 0.08 | 88.7 / 94.6 | 87.1 / 96.4 |
| Soft Minimax | 8.9% | 0.20 | 0.09 | 86.5 / 96.1 | 85.9 / 97.8 |
| Smith//Score | 7.8% | 0.16 | 0.06 | 87.3 / 96.1 | 85.6 / 96.8 |
| Ranked pairs | 2.4% | 0.02 | 0.04 | 83.5 / 99.3 | 84.5 / 99.2 |
| Schulze | 2.0% | 0.01 | 0.04 | 83.2 / 99.4 | 84.1 / 99.3 |

Exits and clones: average number per election at the end.

- **Condorcet methods are almost immune.** The honest result changes in 2–4% of the elections, and their values stay as they are.
- **Rated methods lose their VSE lead.** Exits change the result in 10–14% of the elections and cost VSE: Geometric median falls from 90.3 to 84.3 and is dominated, Score and DMS fall to about 86–87.
- **Score runoff attracts clones.** With 0.19 clones per election, it has more than any other method near the front (the hybrids: 0.04–0.09). Its WME falls by 2 points, honest and strategic alike (98.3 → 96.2 honest), and BTR-score, MARS, Soft Minimax and Soft Minimax-D dominate it. This fits clones taking the second runoff place; the report does not show the mechanism.
- **Plurality and Borda attract the most clones** (0.88 and 0.55 per election), followed by Supplementary vote and TTR (0.54 and 0.51, not in the table).
- **Plurality profits from exits:** its honest VSE rises from 67.4 to 72.6 as spoilers withdraw; IRV's honest WME rises from 88.2 to 95.3.
- Undominated: ARA, Soft Minimax, BTR-score and the Condorcet and minimax methods (Ranked pairs, Schulze, Kemeny-Young, MMWV, MMPO). The order of the methods changes clearly (rank correlation with the base on VSE\* 0.72).

### What the Ballot Tests Show together

- **The front's middle depends on the score scale.** On 0–5, the score–pairwise hybrids lie between the Condorcet and the rated methods. On 0–99, most of them become Condorcet methods in effect, and only those whose pairwise part merely corrects the scores (ARA, Soft Minimax, MARS) stay in between.
- **Laziness weakens the Condorcet methods; strategic nomination weakens the rated methods and Score runoff.** The Condorcet methods' strength rests on complete rankings, the rated methods' on a fixed field.
- **Methods that stay undominated in all three ballot tests:** none. Score runoff is undominated under laziness, but not on the finer score scales or under nomination. ARA comes closest: undominated at 0–9, at 0–99, at laziness 0.5 and under nomination, but not at laziness 0.1.

## 7 Electorates

The electorate decides how strongly VSE and WME pull apart, and that sets the stage for everything else. The reports' measure of it is how much VSE electing the Condorcet winner loses:

| Condition | Electing the Condorcet winner loses | Best WME reachable at VSE |
|---|---|---|
| Base (fractal-noise clumps, exponential utility, width 0.7) | 16.2% | 83.1 |
| Width 1.4 / 2.8 | 6.7% / 3.9% | 92.9 / 95.8 |
| Linear / quadratic utility | 2.6% / 4.1% | 97.1 / 95.5 |
| Clusters / field / ks model | 12.2% / 7.3% / 7.6% | 86.3 / 91.8 / 92.0 |
| Candidates clustered in the centre / spread out (dispersion 0.5 / 2) | 17.5% / 6.3% | 80.5 / 93.4 |
| 4 to 10 candidates | 15.6–19.6% | 78.3–83.3 |

The base thus has one of the sharpest conflicts between the two goals; only candidates clustered in the centre and other numbers of candidates (in the same model) have as sharp or sharper ones. In most other electorates, the candidate with the best worst margin is also nearly the best for total utility.

### Utility Width and Shape

Utility falls exponentially with distance in the base (mode-like: the best candidate is near a dense group of voters); a wider width makes the fall-off flatter, linear utility (as in vse-sim) puts the best candidate near the median, quadratic near the mean. VSE\* / WME\*:

| Method | Base | Width 1.4 | Width 2.8 | Linear | Quadratic |
|---|---|---|---|---|---|
| Geometric median | 90.3 / 90.4 | 95.7 / 91.7 | 97.0 / 93.3 | 97.3 / 94.8 | 95.3 / 93.3 |
| Score | 88.4 / 92.2 | 94.5 / 93.0 | 96.5 / 93.7 | 97.0 / 92.7 | 96.7 / 88.6 |
| ESR | 89.1 / 93.2 | 95.2 / 95.0 | 96.8 / 95.8 | 97.5 / 96.7 | 96.7 / 93.8 |
| ARA | 88.7 / 94.6 | 94.7 / 96.3 | 96.3 / 96.6 | 97.4 / 95.9 | 97.1 / 90.8 |
| Soft Minimax-D | 87.2 / 95.9 | 94.2 / 97.4 | 96.9 / 97.6 | 97.7 / 97.6 | 97.1 / 94.1 |
| Soft Minimax | 86.5 / 96.1 | 93.8 / 97.7 | 96.4 / 97.9 | 97.3 / 97.9 | 96.8 / 94.6 |
| BTR-score | 86.7 / 96.3 | 94.2 / 97.1 | 96.8 / 97.5 | 97.5 / 98.1 | 96.7 / 94.4 |
| Smith//Score | 87.3 / 96.1 | 94.1 / 97.2 | 96.3 / 97.5 | 97.4 / 97.9 | 96.8 / 94.1 |
| Score runoff | 85.2 / 98.0 | 92.9 / 98.4 | 95.5 / 99.0 | 97.0 / 98.9 | 96.1 / 98.1 |
| Ranked pairs | 83.5 / 99.3 | 92.8 / 99.6 | 95.6 / 99.7 | 97.1 / 99.9 | 95.4 / 99.7 |
| IRV | 84.9 / 90.8 | 91.5 / 91.8 | 93.2 / 92.0 | 93.7 / 92.0 | 91.2 / 93.0 |
| Plurality | 79.8 / 76.9 | 83.1 / 78.1 | 84.7 / 77.8 | 83.8 / 78.0 | 80.5 / 79.1 |

- **With a flatter utility, the trade-off nearly disappears.** Under linear utility, all good methods reach VSE\* 97–98, and they differ by less than 1 point; the Condorcet methods reach 97.1 at WME\* 99.9. WME then decides, and the order of the methods on VSE is reshuffled within that narrow range (rank correlation with the base on VSE\* 0.41–0.58).
- **The rated methods' VSE lead belongs to the steep utility of the base.** Geometric median, Score, DMS and ESR are dominated in all four conditions; under quadratic utility, Score and ARA also lose WME\* (88.6 and 90.8).
- **Undominated in all four:** Soft Minimax-D and the Condorcet methods. Score runoff in three (not at width 1.4, where Soft Minimax is better on VSE\*), BTR-score at width 2.8, linear and quadratic, Soft Minimax at width 1.4 and quadratic.
- **IRV and Plurality stay behind whatever the utility:** IRV 4–6 points of VSE\* behind the best method and 5–7 points of WME\* behind Score runoff, Plurality more than 12 and about 20.

### Electorate Models

![An example visualization showing the distributions that the different models create.](electorates/examples.png)

- **Clusters** (Zipf dimensions, per-group salience; the Condorcet winner costs 12.2% of VSE): close to the base. Undominated: MARS, MARS-T, Score runoff, and the Condorcet methods Ranked pairs, Minimax, Schulze.
- **Field** (scale-free opinion fields; 7.3%): Score runoff dominates the Condorcet methods (on sincerity, for some also on VSE\*; its WME\* is 98.7 against their 98.1–98.4), and only MARS, MARS-T, Soft Minimax-D and Score runoff are undominated.
- **ks** (Quinn's hierarchical model, lab version; 7.6%): the score–pairwise hybrids lead on VSE\* (STAR, MARS, Soft Minimax-D 95.1–95.2), the rated methods fall back on WME\* (Geometric median 86.3, Score 89.6), and IRV and Plurality do badly (WME\* 83.5 and 63.0). Undominated: STAR, MARS, Soft Minimax-D, Score runoff and the Condorcet methods.
- In none of the three is a rated method undominated.

### Candidate Dispersion

- **Candidates clustered in the centre (0.5)** is the hardest electorate: preferences are flat (favourite ÷ least liked 3.2, against 8.0), the Condorcet winner costs 17.5% of VSE and has the highest total utility in only 55% of the elections. It gives the lowest WME\* of the whole collection for most methods: Geometric median 79.5, DMS 77.7, Score 83.7, Approval 65.6, IRV 73.0, Plurality 48.4. The good methods also elect a below-median candidate more often (Low\* about 6–7%, against about 3% in the base). Undominated: Geometric median, Soft Minimax-D, Smith//Score, BTR-score, Score runoff and the Condorcet methods; the full width of the front is used.
- **Candidates spread out (2.0)** is the easiest: the Condorcet winner costs 6.3%, and even Plurality reaches VSE\* 93.0 / WME\* 93.2. The Condorcet methods bury more here (21% of the voters, sincerity 97.2), and Score runoff dominates them on sincerity alone. Undominated: MARS, Soft Minimax-D, ARA, Score runoff.

### Number of Candidates

![Performance across different number of candidates. Methods grouped into types for visibility.](candidates/analysis-vs-base/chain-cands.png)

4, 5, 7, 8, 9 and 10 candidates (6 in the base), 700 elections each, so the values are noisier than elsewhere and not always monotone. VSE\* / WME\*:

| Method | 4 | 5 | 6 (base) | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|
| Geometric median | 86.9 / 92.8 | 88.1 / 91.7 | 90.3 / 90.4 | 89.4 / 88.9 | 92.0 / 87.5 | 92.4 / 86.3 | 91.6 / 85.5 |
| ARA | 83.8 / 95.5 | 86.8 / 94.5 | 88.7 / 94.6 | 89.0 / 94.4 | 90.5 / 91.3 | 91.6 / 93.0 | 91.1 / 91.1 |
| Soft Minimax-D | 81.0 / 97.4 | 84.7 / 96.0 | 87.2 / 95.9 | 86.4 / 95.8 | 87.8 / 94.5 | 89.4 / 94.1 | 88.8 / 93.0 |
| Smith//Score | 83.2 / 97.4 | 84.9 / 96.2 | 87.3 / 96.1 | 86.7 / 95.8 | 87.9 / 94.3 | 88.9 / 94.1 | 87.8 / 93.4 |
| Score runoff | 80.4 / 98.9 | 82.4 / 98.0 | 85.2 / 98.0 | 85.5 / 97.3 | 86.1 / 97.4 | 87.1 / 96.9 | 87.0 / 96.0 |
| Ranked pairs | 80.2 / 99.0 | 82.0 / 98.9 | 83.5 / 99.3 | 82.5 / 99.6 | 80.2 / 99.6 | 83.5 / 99.3 | 82.3 / 99.1 |
| Approval | 82.6 / 90.0 | 85.4 / 88.7 | 87.0 / 87.5 | 85.4 / 84.9 | 88.6 / 82.7 | 89.9 / 80.4 | 89.0 / 81.6 |
| IRV | 80.9 / 94.2 | 83.5 / 92.7 | 84.9 / 90.8 | 84.2 / 90.1 | 86.3 / 88.2 | 85.5 / 86.3 | 85.0 / 85.1 |
| TTR | 80.3 / 94.0 | 81.8 / 91.8 | 83.3 / 90.4 | 81.2 / 89.7 | 84.1 / 85.2 | 83.3 / 82.1 | 83.7 / 82.7 |
| Plurality | 78.6 / 85.9 | 78.3 / 79.8 | 79.8 / 76.9 | 75.5 / 72.7 | 76.9 / 66.9 | 76.8 / 64.7 | 75.0 / 64.2 |

- **Score runoff is undominated at every number of candidates**, the only method that is. With 10 candidates the front is widest (13 undominated methods, among them Approval runoff, its only undominated condition).
- **More candidates cost the single-mark and approval methods WME:** Plurality 85.9 → 64.2, TTR 94.0 → 82.7, IRV 94.2 → 85.1, Approval 90.0 → 81.6. Spoilers and split votes grow with the field.
- **The Condorcet methods hold WME\* at about 99** at any number of candidates, with VSE\* of 80–83.5. They are undominated at every number except 4, where only Smith//Score and Score runoff are.
- **Rated and hybrid methods gain VSE\* and lose WME\* as the field grows** (Geometric median 92.8 → 85.5 WME\*, Smith//Score 97.4 → 93.4). For the hybrids this fits the score-range result: with 10 candidates on a 0–5 ballot, more candidates share a score, and the pairwise step sees less.
- With 4 candidates the Condorcet winner costs the most VSE (19.6%), and most methods have their lowest VSE\* of the collection there.

### Number of Voters

With 49 voters instead of 99, the Condorcet winner costs a little more (18.0% of VSE), and VSE\* is up to 2.3 points lower, most for the Condorcet methods (Ranked pairs VSE\* 81.2 against 83.5, WME\* 99.0), least for the rated methods (Score 88.2 against 88.4). The order of the methods is the base's (rank correlation 0.97 on VSE\* and 0.98 on WME\*, as from noise). Undominated: MARS, MARS-T, Smith//Score, Soft Minimax-D, Score runoff and the Condorcet methods (Ranked pairs, Schulze, Kemeny-Young, Minimax).

### What the Electorate Tests Show together

- **The size of the conflict between VSE and WME is the model's most consequential assumption.** The base's steep utility puts it at 16%; a flatter or differently shaped utility, other electorate models or spread-out candidates put it at 3–12%.
- **Where the conflict is small, the rated methods' lead vanishes, and the front belongs to the Condorcet methods, Score runoff and a few hybrids** (Soft Minimax-D, MARS, BTR-score). Over the 16 electorate conditions, Score runoff is undominated in 15, the Condorcet methods (at least one of them) in 13, Soft Minimax-D in 11, and the rated methods (Geometric median, Score, DMS, ESR) in one (Geometric median, with candidates clustered in the centre).
- **Plurality, IRV, TTR and Approval are off the front in every electorate.** Their gap grows with the number of candidates and with candidates clustered in the centre.

## 8 Everything Hard at Once

The hard condition combines candidates clustered in the centre, laziness 0.1, strategic nomination and polls of 9 voters. VSE\* / WME\* / sincerity:

| Method | Hard | Result changed by nomination | Clones |
|---|---|---|---|
| Geometric median | 87.0 / 82.8 / 99.9 | 20.0% | 0.21 |
| STAR | 85.0 / 90.6 / 99.8 | 18.9% | 0.34 |
| Soft Minimax-D | 84.8 / 92.1 / 99.8 | 12.7% | 0.16 |
| Score runoff | 84.6 / 91.7 / 99.7 | 19.9% | 0.38 |
| Score | 84.2 / 85.2 / 99.8 | 19.6% | 0.18 |
| Soft Minimax | 84.2 / 92.9 / 99.7 | 14.8% | 0.21 |
| BTR-score | 83.6 / 92.3 / 99.7 | 16.0% | 0.23 |
| Ranked pairs | 81.9 / 94.7 / 98.2 | 8.8% | 0.14 |
| MMWV | 81.1 / 95.8 / 98.8 | 9.3% | 0.14 |
| IRV | 80.7 / 81.2 / 97.5 | 34.2% | 0.16 |
| Approval | 79.7 / 73.5 / 99.6 | 34.2% | 0.46 |
| Plurality | 62.3 / 50.7 / 97.0 | 45.1% | 1.13 |

- **Every method loses**, and the differences grow. The good methods elect a below-median candidate in 6–8% of the elections (base: about 3%), Plurality in 21%. The order of the methods changes clearly (rank correlation with the base on VSE\* 0.76).
- **The front stays wide:** Geometric median, Soft Minimax-D, Soft Minimax, BTR-score, Score runoff and the Condorcet and minimax methods (Ranked pairs, Kemeny-Young, MMWV, MMPO) are undominated.
- **Score runoff keeps its place on the front but loses its WME lead.** Its WME\* of 91.7 is its lowest in the collection, below Soft Minimax, Soft Minimax-D and BTR-score (92.1–92.9) and the Condorcet methods (94–96). It again attracts the most clones of the good methods (0.38).
- **The Condorcet methods hold best:** nomination changes their result least (8–9%), and they keep the highest WME\*.
- **The single-mark methods break down:** Plurality reaches VSE\* 62.3 and WME\* 50.7; nomination changes its result in 45% of the elections.

## 9 The vse-sim Setting

![vse-sim like results with 11 methods](vsesim/analysis/arrows-c1.png)

A setting close to Jameson Quinn's vse-sim: Quinn's hierarchical model as in vse-sim's code, linear utility, 40 voters, 6 candidates, scores 0–10, one poll, a fixed share of strategic voters (half or all), vse-sim's methods; 15 000 elections each.

In these electorates the conflict between VSE and WME practically vanishes: electing the Condorcet winner costs 1.1% of VSE, and some rule reaches VSE 97.0–98.8 at the best WME.

vse-sim's VSE is 0 for a random candidate, the lab's for a random ballot. With a random candidate at −60.7 on the lab's scale in this setting, classic VSE = (VSE + 60.7) / 160.7. Lab VSE (classic VSE in brackets):

| Method | Honest | Half strategic | All strategic |
|---|---|---|---|
| Ranked pairs | 97.9 (98.7) | 97.2 (98.3) | 95.3 (97.1) |
| Schulze | 97.9 (98.7) | 97.2 (98.3) | 95.2 (97.0) |
| STAR | 97.7 (98.6) | 97.4 (98.4) | 96.0 (97.5) |
| Score | 95.2 (97.0) | 95.7 (97.3) | 95.0 (96.9) |
| Borda | 94.6 (96.6) | 93.8 (96.1) | 85.8 (91.2) |
| MJ | 93.1 (95.7) | 94.7 (96.7) | 94.8 (96.8) |
| MAV | 92.0 (95.0) | 94.2 (96.4) | 94.4 (96.5) |
| 321 | 91.9 (95.0) | 94.1 (96.3) | 92.6 (95.4) |
| Approval | 88.7 (93.0) | 90.7 (94.2) | 90.6 (94.2) |
| IRV | 86.1 (91.4) | 90.1 (93.8) | 89.6 (93.5) |
| Plurality | 59.0 (74.5) | 70.0 (81.3) | 74.3 (84.0) |

- **The front:** STAR and Ranked pairs are undominated with half and with all voters strategic (Schulze too with half). STAR has the higher VSE\* (by 0.2 and 0.7 points), Ranked pairs the higher WME\* (97.8 against 94.0 with half strategic; 93.8 against 91.7 with all) but the lower sincerity (96.5 and 93.2 against 99.8 and 99.5). Score is dominated by STAR.
- **Against vse-sim's own figures.** vse-sim's summary page ([Voter Satisfaction Efficiency (VSE) summary](https://electionscience.github.io/vse-sim/VSEbasic)) gives ranges without stating the settings behind them: Plurality around 75%, Approval 89–95%, 3-2-1 92–95%, STAR up to 98%, Score 96% down to 84% under strategy, Condorcet methods 98% at the top and Ranked pairs as low as 86% under strategy, IRV about 87% with honest voters down to about 60% with all strategic, median-based methods up to 92%, Borda below 0% under strategy.
  - **The honest values agree** in order and roughly in level: Plurality 74.5, Approval 93.0, STAR 98.6, Score 97.0, Condorcet methods 98.7. The zero-point conversion thus works. The lab gives IRV (91.4) and MJ (95.7) somewhat more than vse-sim.
  - **The strategic values do not.** In vse-sim, strategy costs Borda, IRV, Ranked pairs and Score a lot. Here it costs at most a few points (Borda about 9 points with all voters strategic), and it improves IRV and Plurality. In vse-sim's code, a chooser decides which ballot a voter casts: honest or strategic with fixed probabilities, or strategic for the voters who prefer the strategic target (one-sided strategy). The strategic ballot is each method's own, worked out from one poll (the true standings or a noisy, biased or skewed version of them). Nothing checks whether that ballot gains the voter anything. The lab's voters adopt a strategic ballot only if it improves the poll result for them, so self-defeating strategies are not used, and compromise, which helps IRV and Plurality, is.
- So the two simulators agree on the electorates and honest voting, and differ in the model of strategy, which is the point of the lab's strategy engine.

## 10 Discussion

### 10.1 Strategic Nomination is the Normal case

In the comparison, strategic nomination is one condition of 37. In real elections it is not optional: parties and candidates always decide who runs, knowing the voting method. Its weight in the counts therefore understates its importance.

This matters most for Score runoff, the method that does best overall:
- Under nomination, it attracts more clones than any other method near the front (0.19 per election; 0.38 in the hard condition), and its WME\* falls from 98.0 to 96.1, part of the way towards plain Score (93.5).
- A runoff between a front-runner and their own clone protects nothing, costs a second election and suggests a majority check that did not take place.

The lab's clones cost their party nothing, while a real clone needs a second credible candidate and splits a campaign, so the test may overstate the danger. Still, a method whose main strength can be undone by running a teammate is weaker in practice than its overall share suggests. The Condorcet methods, at the other extreme, are almost unaffected by nomination.

### 10.2 Good Information Helps the Weak Methods

The base's polls sample half the electorate (49 of 99 voters). With smaller polls, Plurality, IRV, TTR, Borda and to a lesser extent Approval lose much of what strategy gave them, while the good methods hardly change. Good information lets voters repair a method's faults by compromising; the base thus favours the weak methods, and the comparison under-states their real weakness where voters know less.

How much poll information is realistic is not settled by the share of the electorate polled:
- A poll's precision depends on its absolute size. With the doubled error the lab assumes, the base's polls of 49 are already less precise than a typical national poll of 1000, and polls of 9 much less.
- On the other hand, real voters do not all read polls, the methods' poll counts can say more than real polls would (all of IRV's elimination orders, the full pairwise matrix), and polls of 9 voters show many ties.

The poll-size test is therefore best read as voters with poor information, a plausible part of reality, rather than as more realistic polls.

### 10.3 What the front Does not Measure

The front weighs total satisfaction, majority support and sincerity, and nothing else. Several properties that matter for a real choice are not measured:
- **Simplicity** for voters (filling in the ballot), for officials (counting, recounts, adding up results from polling stations) and for the public (understanding why the winner won).
- **The cost of a second round:** money, time, and turnout that may differ between rounds; the lab's runoffs assume the same voters return and rank sincerely.
- **Resistance to strategic nomination** beyond the one test (10.1).

Simplicity in particular changes the picture. Approval is never on the front, but with a ballot almost as simple as Plurality's it improves on it by about 10 points of VSE\* and 12 of WME\* (means over the conditions 87.7 / 85.7 against 77.6 / 73.4). Several methods near the front, in contrast, are hard to explain: Soft Minimax, Kemeny-Young or ESR. A method one step behind the front but much simpler may be the better practical choice. A measure of simplicity would be needed to put this on the front; the lab has none yet.

### 10.4 Limits of the Model

- **Electorates.** The electorates are spatial. Candidates differ only in their positions, not in competence or charisma, and the electorate sizes are small (40–99 voters). How strongly total satisfaction and majority support conflict depends on the utility shape (section 7), for which the evidence is thin.
- **Strategy.** Groups of like-minded voters improve their ballots against polls; there are no campaigns, no coordination across groups and no mistakes beyond poll error.
- **Ballots.** Score ballots run 0–5 in most conditions, which shapes the hybrids' results (section 6).
- **Statistics.** Dominance calls are statistical. Where a difference is borderline, a different bootstrap draw can turn a dominance into a tie (Appendix), so single dominance counts are uncertain by about one condition.

## 11 Conclusion

- **The good methods are close.** About twenty methods are within 1 point of the front (on VSE\* and WME\* at once) in nearly every condition. Choosing among them is mostly choosing a position on the trade-off between total satisfaction (VSE) and majority support (WME), not choosing a better or worse method.
- **No method is undominated everywhere.** Each family has a weak spot:
  - the Condorcet methods with truncated rankings, small polls and all voters strategic;
  - the rated methods wherever the conflict between VSE and WME is small, and with strategic nomination;
  - Score runoff with clones, with only kind strategic voters and with all voters strategic;
  - the score–pairwise hybrids with the score scale itself.
- **Score runoff is the most robust single method in the simulations.** It is undominated in 28 of its 35 conditions, in 15 of the 16 electorate conditions and at every number of candidates, and within 1 point of the front everywhere. Its weakness to clones matters more than its single condition suggests, since nomination is always strategic in practice (10.1).
- **The Condorcet methods sit at the corner of what any rule can reach** (best WME, VSE about as high as possible there). They are undominated in 55–73% of the conditions and almost immune to strategic nomination. Their lower sincerity comes from aggressive voters burying, which costs little in the result.
- **The rated methods' VSE lead belongs to the base's steep utility and to moderate strategy.** Under a flatter or differently shaped utility, other electorate models or full strategy, they are dominated.
- **The score–pairwise hybrids are in the middle of the front because of the coarse 0–5 scale.** On 0–99 most of them become Condorcet methods in effect; ARA, Soft Minimax and MARS stay in the middle. Soft Minimax-D is undominated in all four utility conditions and in 11 of the 16 electorate conditions.
- **For the good methods, the amount of strategy matters, not its timing or information.** With all voters strategic, only Soft Minimax and BTR-score are left undominated. **The weak methods, in contrast, depend on good poll information**, which lets their voters compensate through strategy.
- **The most consequential assumption is how strongly VSE and WME conflict.** In the base, electing the Condorcet winner costs 16% of VSE; under linear utility 2.6%, in the vse-sim setting 1.1%. Where the conflict is small, WME decides.
- **Plurality, IRV, TTR and Approval are off the front in every condition.** Approval, though, gains the most over Plurality for the least added complexity.
- **In the vse-sim setting, the honest results agree with vse-sim's published figures; the strategic ones do not**, because the lab's voters adopt only strategies that pay off in the polls.

The simulations narrow the choice to a group of good methods and show what each trades away. The choice within that group depends on values the front does not contain: how to weigh total satisfaction against majority support, and how much complexity and cost a method may have.

## Appendix: Notes on the Collection

- The default run's 200 elections are the first 200 of the base (same seed). `compare.py` treats the two as replicates of one condition and gives each half its weight, so those 200 elections weigh a little more in the defaults condition. The effect is small, and it touches only the defaults condition.
- Pair counts with halves (7.5 of 33) come from that condition.
- **Bootstrap draws.** Each condition's bootstrap replicates (1000) depend only on its elections and the seed, in `compare.py` and `analyze.py` alike (since 2026-10-25). The comparison and each run's own report therefore agree on every dominance, and adding runs to the collection does not change the other conditions' results. In earlier versions one random stream ran through all conditions, and borderline dominances could change with the set of runs analysed together.
