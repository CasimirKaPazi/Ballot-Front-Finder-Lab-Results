---
title: "Voting Methods under Poll-Based Strategy"
subtitle: "Findings of the Ballot Front Finder Lab Reference Runs, Version 2.1.0"
author:
  - Hiveism
  - Claude (Anthropic)
date: "2026-11-08"
lang: en
documentclass: article
papersize: a4
geometry: margin=2.2cm
fontsize: 10pt
toc: true
toc-depth: 2
numbersections: false
colorlinks: true
abstract: |
  The Ballot Front Finder Lab simulates single-winner elections in which voters respond to polls. It measures each voting method on two goals: the total satisfaction of the voters with the winner (VSE) and the winner's support against every rival head to head (WME). It also measures how honest the voters can afford to be. Every voter weighs the value of a strategic move, found on the published polls, against a personal cost of voting insincerely. Candidates are drawn from the voters and may withdraw or field clones when that pays for them. The reference runs of version 2.1.0 compare 38 methods in 44 conditions, each changing one assumption of the model or several at once, and 71 methods in the base condition.

  No method is best everywhere, and the leading methods are close. About a dozen are within one point of the best results in nearly every condition, and in the base they are only about one point ahead of a lottery of trivial ideal rules.

  - The Condorcet methods and Soft Minimax-D are undominated most often, in about two thirds of their conditions. The Condorcet methods reach the corner of what any rule can reach on majority support; few voters find a strategy that pays against them, and strategic nomination hardly touches them.
  - Score runoff, the most robust method of version 1.0.0, is still within one point of the best results everywhere, but Soft Minimax-D dominates it in more than half of the conditions.
  - Strategic nomination costs the rated methods their lead in total satisfaction and gives them majority support instead; they are no longer among the best.
  - When every voter votes strategically, the Condorcet methods hold up if voters test their moves by counting them on the polls with the method itself, as in the default model. They lose more if voters move in large groups or misjudge the pairwise counts.

  How strongly the two goals conflict, which depends on the shape of utility and on who stands, remains the model's most consequential assumption, together with the score range of the ballots. Simplicity, which the lab does not measure, has to be weighed separately.
---

The code of the lab is at <https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab>, the results and reports at <https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab-Results>; the results and findings of version 1.0.0 are in its release [v1.0.0](https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab-Results/releases/tag/v1.0.0). The commands, the lab commit and how to make the results again are in [reference-runs.md](reference-runs.md). The reports cited are in the run folders (`*/analysis*/report.md`), `overview-*/` and `comparison/`; the plots mentioned are in the same folders. The methods are defined in the lab's [methods.md](https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab/blob/main/docs/methods.md).

## 1 Introduction

Every voting method turns ballots into a winner, and every method can be gamed to some degree. Comparisons with honest ballots alone flatter methods that are easy to manipulate. Comparisons under fully rational strategy depend on assumptions nobody believes. The lab places voters in between: they read polls and change their ballots when that pays enough to them, the way people vote tactically in real elections, and it measures what each method makes of that.

Two goals are measured, because they pull apart. A winner can satisfy the electorate as a whole (high total utility) and still lose a head-to-head vote against another candidate, and the other way round. No method reaches the best on both, so the result is a front of methods, each the best at some balance of the two, rather than a single ranking. The question shifts from "which method is best?" to "which of the good methods do we want, given what we value?".

This document reports a fixed set of runs made with version 2.1.0 of the lab, chosen to test how robust the findings are. It follows the findings of version 1.0.0 and says where the results changed. It can be read on its own; the lab's documentation ([model.md](https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab/blob/main/docs/model.md), [analysis.md](https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab/blob/main/docs/analysis.md), [methods.md](https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab/blob/main/docs/methods.md)) has the full details.

## 2 The Lab in Brief

**Electorates.** In each election, 99 voters are points in a six-dimensional issue space, gathered in clumps within clumps (a fractal-noise density). The 6 candidates are voters too: they are drawn from the electorate, vote like everyone else, and their preferences are their own rows as voters. A voter's utility for a candidate falls exponentially with distance (width 0.7), so far-away candidates are nearly equally bad. Every election draws a new electorate.

**Ballots and strategy.**

- Each voter first fills in the method's ballot honestly. A ranking follows the utilities, approval goes to candidates above a personal threshold, and scores 0–5 are stretched to the voter's own range.
- Then follow three poll rounds. A poll shows the method's own counts (the totals a real poll could publish) from a random sample of 49 voters, 5 polls per round, each spread by twice its sampling error, as real polls miss by about that much.
- **Individual voters with pooled discovery** (new in version 2.x). Each voter decides alone, but finds possible moves together with the voters who see the race the same way: those who order the poll's top three candidates alike (a type). For each type the lab tries the obvious moves (compromise, burial, push-over): it changes the ballots of a quarter of the type accordingly and counts every poll again with the method. Each voter then values each move by their own utilities, as the gain in expected utility relative to their utility range.
- **Value against cost.** Each voter has a personal cost of voting insincerely, drawn uniformly from 0 to 0.3 of their utility range. A voter makes the best move if its value exceeds that cost, and votes sincerely otherwise. A move that disappointed in the last poll is trusted less. Under Plurality about 4% of all voters move per poll, as in surveys of plurality elections.
- **Strategic nomination** (on by default since version 2.0.0). Before the vote, a candidate who would gain by withdrawing does so, and a candidate who would gain by fielding a clone (a near-identical second candidate) does.

**Measures.**

- **VSE** (voter satisfaction efficiency) and **WME** (worst-margin efficiency) are both 0 for a random ballot and 100 for the best candidate of each election. VSE measures the winner's total utility. WME measures the winner's smallest head-to-head margin against any rival, by sincere preferences, so it is a refined Condorcet criterion.
- Starred values (VSE\*, WME\*) are the results after the poll rounds; unstarred ones are with honest ballots.
- **Sincerity** measures how far the strategic ballots go against the voters' own preferences, within each ballot format: 100 = no preference reversed. A rated ballot can be strategic by exaggerating (ties), which reverses no preference; a ranking can only be strategic by reversing one. Differences under 1 point count as ties.

**Frontiers.** The **reachable frontier** is the best combination of VSE and WME that any rule could reach knowing every voter's utilities. The **normalized-ballot line** is what is within reach when every voter's utilities are stretched between their least and most liked candidate, as score ballots do.

**A natural baseline** (new in version 2.1.0). Four trivial rules: the random ballot; the Condorcet winner, else a random ballot (CW//random ballot); its mirror image on the utility side (the winner by normalized utilities, mixed with a random ballot so that it falls as far short of the best VSE as CW//random ballot falls short of the best WME); and a random pair decided by majority. **Lead** is by how many points a method beats every lottery (random mixture) of these rules on at least one of the two axes; a negative Lead means some lottery of trivial rules is better on both axes. **Gap** is how far a method stays below the normalized-ballot line, on both axes at once. Neither needs a weight between VSE and WME.

**Equivalent jury size** (new in version 2.1.0). A random jury of k voters decides ideally with its honest normalized ballots and is judged on the whole electorate. A jury of one is the random ballot; larger juries come closer to the normalized-ballot line. A method's jury size is the smallest jury that reaches it: how many voters' honest judgment the method turns into its result.

**The front.** Methods are judged on VSE\*, WME\* and sincerity together:

- **Undominated**: no other method is clearly better on one of the three (the paired 95% bootstrap interval of the difference excludes 0; for sincerity, also by at least 1 point) and clearly worse on none. All methods face the same elections, so differences are measured precisely.
- **Pareto front**: the same with point estimates.
- **Near the front (1 / 2)**: no method is better by 1 (2) points on VSE\* and WME\* at once (point estimates).

**Across runs** (`comparison/`, made with `compare.py`), every condition counts once, whatever the number of elections behind it. Shares are over the conditions a method was run in, and these sets differ:

- the vse-sim setting (two conditions) has only 11 methods;
- the max-score test has 29 (the methods that read scores);
- the methods outside the extended set are only in the base and the max-score test.

So the Condorcet and minimax methods' shares are over 41 conditions, Schulze's and Ranked pairs' over 43 (they are in the vse-sim setting), and the score methods' over 42 (they are in the max-score test).

## 3 What Was Tested

The tests use the set `extended`, 38 methods: the standard set of the lab, the strongest newer methods and the delegated methods ([methods.md](https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab/blob/main/docs/methods.md) has the definitions):

- **Plurality-based methods**: Plurality; IRV (instant runoff); IRV-3 (IRV with at most three candidates ranked); ER-IRV (IRV where equal first ranks each count a whole vote); Supplementary vote (top two by first preferences, then second preferences count).
- **Two-round methods** with a real second election between the top two: TTR (two-round runoff, plurality first round), Approval runoff, Approval cond. runoff (no runoff if one is approved by a majority), Score runoff.
- **Condorcet and minimax methods**: Schulze, Ranked pairs, Kemeny-Young, Copeland, Minimax (the candidate whose worst pairwise defeat is smallest), MMWV (Minimax by winning votes), MMPO (minimax pairwise opposition), BTR-IRV (bottom-two runoff: the two with the fewest first choices meet pairwise, the loser is out).
- **Score–pairwise hybrids**: STAR (score then automatic runoff), Smith//Score (the highest score total among the Smith set), BTR-score (bottom-two pairwise runoffs climbing up the score order); the Soft Minimax family (Minimax on margins in which each ballot's score difference counts in proportion, up to a clip: Soft Minimax, Soft Minimax-40, Soft Minimax-D; section 10.3); MARS and MARS-T (the score order corrected by pairwise matches that add the share of voters preferring one candidate to their score), ESR-MARS (MARS among the candidates ESR finds), ARA (the same with approvals, the scores above the middle of the range), 321 (3-2-1 voting with three grades).
- **Rated methods**: Score; DMS (depth maximin); ESR (expanding support runoff); IMDS (iterated maxed-and-dropped score, a score elimination that can be counted by hand).
- **Median-based methods**: MJ (majority judgment), MAV (majority approval voting, a graded Bucklin).
- **Others**: Borda, Approval, and the delegated methods Asset (asset voting: each candidate passes the votes they hold to their own favourite when eliminated) and Delegated 321 (321 with what a lazy voter leaves blank filled from their favourite's grades).

The base condition has all 71 methods of the lab.

| Test                 | What changes from the base                                                                                                                         | Values                            | Elections   |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- | ----------- |
| Base                 | nothing (the defaults above), with a manipulation test                                                                                             | —                                 | 4000        |
| Defaults             | the run a user gets with `python run.py` (standard set)                                                                                            | —                                 | 200         |
| Strategic share      | a fixed share of always-strategic voters (no cost) instead of adaptive voters                                                                      | half, all                         | 2000        |
| Coordination         | share of their type a voter expects to act alike (0.25)                                                                                            | 0, 0.1, 0.5                       | 2000        |
| Honesty cost         | upper end of the personal cost of voting insincerely (0.3)                                                                                         | 0.1, 1                            | 2000        |
| Poll error           | how much less precise than their sampling error polls are taken to be (2)                                                                          | 1, 4                              | 2000        |
| Poll samples         | polls per round (5)                                                                                                                                | 10, 20                            | 2000        |
| Poll rounds          | number of poll rounds (3)                                                                                                                          | 1, 2, 4, 5                        | 2000        |
| First poll           | the first poll shows a previous election's strategic result                                                                                        | —                                 | 2000        |
| Campaigns            | candidates recommending moves to their audience; needs the arithmetic response, so with its own baseline, with adaptive and with all voters strategic | campaigns 0, 1 × adaptive, all    | 2000        |
| Groups               | the ranking groups (the strategy engine of version 1.0.0) instead of individual voters                                                             | adaptive, all strategic           | 2000        |
| Laziness             | truncated honest ballots                                                                                                                           | 0.25, 0.5                         | 2000        |
| Score range          | top score (5), for the methods that read scores                                                                                                    | 99                                | 2000        |
| No nomination        | the field as drawn: no candidate withdraws or fields a clone                                                                                       | —                                 | 2000        |
| Electorate model     | clumps (noise)                                                                                                                                     | clusters, field, ks               | 2000        |
| Utility width        | 0.7                                                                                                                                                | 1.4                               | 2000        |
| Utility shape        | exponential                                                                                                                                        | linear, quadratic                 | 2000        |
| Candidate dispersion | who stands: candidates spread like the voters (1)                                                                                                  | 0.5 (central), 2 (spread out)     | 2000        |
| Candidate shift      | who stands: candidates from one side, by this many standard deviations of the voters                                                               | 0.5, 1                            | 2000        |
| Candidates           | 6                                                                                                                                                  | 4, 8, 10                          | 2000        |
| Voters               | 99                                                                                                                                                 | 199                               | 2000        |
| Hard                 | candidate dispersion 0.5, laziness 0.25 and poll error 4 at once                                                                                   | —                                 | 2000        |
| vse-sim setting      | Quinn's model as in vse-sim's code, linear utility, 40 voters, scores 0–10, one poll, half or all voters strategic, fixed field apart from the voters; vse-sim's 11 methods | half, all | 15 000 each |

That makes 44 conditions. All runs use seed 1, so every test faces the base's first 2000 electorates, and a test that changes only strategy or ballots faces exactly the same electorates. The electorates are nested: with 8 or 10 candidates, the base's 6 are the first of them, and with 199 voters the base's 99 are the first.

## 4 Results across All Conditions

The plot below shows the base with all methods of the lab, each as an arrow from its honest result (open circle) to its result after the poll rounds (filled, coloured by sincerity), zoomed to the front. Left out are the reference rules, the Inverse-\* methods and the methods far below the front (Plurality, Asset, Asset-foresight, Borda, Borda-ER, Supplementary vote; all in `overview-all-methods/arrows-c1.png`). The grey area at the top is beyond what any rule can reach; the thin diagonal line runs from CW//random ballot to its mirror, the trivial rules of section 4.6: methods above and to the right of it are ahead of them.

The methods form groups. The Condorcet and minimax methods sit together in the upper left, at VSE about 84 and WME about 99, and barely move under strategy. Below and to the right, at VSE 86–87 and WME 97–98, lie the score–pairwise methods that check the pairwise result (BTR-score, Smith//Score, Smith//Approval, Soft Minimax, the MMWV score hybrids, IMDS and INS). Further right, at VSE 87–88 and WME 95–97, are the score methods with a runoff or a pairwise correction (Score runoff, STAR, ARA, MARS, the other Soft Minimax variants, ESR-MARS); their arrows point down and a little left, a loss of 0.4–1.7 points of WME. Score and DMS come down further, by 2.5–3 points of WME to about 92.5; MJ, MAV and 321 lose less and end at WME 93–94. IRV, IRV-3, Dowdall, DSC and DAC have the longest arrows, 2–3 points down and to the left, while TTR and ER-IRV move little; these ranked and plurality-based methods are the least sincere (red). The approval methods (Approval, MDDA, MAMPO) lie at the bottom, around WME 90.5, and hardly move.

![All methods in the base after strategy, zoomed to the front](overview-all-methods/arrows-c1-zoomed.png)

### 4.1 The Condorcet Methods and Soft Minimax-D Lead; the Leading Methods Are Close

Over the 44 conditions, about a dozen methods are within 1 point of the front (on VSE\* and WME\* at once) in more than nine tenths of the conditions they were run in: the Condorcet and minimax methods, the Soft Minimax family, BTR-score, Smith//Score and Score runoff. Within 2 points, ARA, MARS, MARS-T, IMDS, ESR-MARS, STAR and BTR-IRV join them. What separates them is less how good they are than which mix of VSE and WME they deliver.

| Method          | Group           | Conditions | Undominated | Pareto front | Near front (1 / 2) | Mean VSE\* | Mean WME\* | Mean sincerity | Worst WME\* |
| --------------- | --------------- | ---------- | ----------- | ------------ | ------------------ | ---------- | ---------- | -------------- | ----------- |
| Schulze         | condorcet       | 43         | 72%         | 49%          | 95% / 95%          | 84.3       | 98.5       | 99.5           | 91.1        |
| Soft Minimax-D  | hybrid          | 42         | 69%         | 86%          | 98% / 100%         | 85.6       | 97.3       | 100.0          | 94.5        |
| Kemeny-Young    | condorcet       | 41         | 68%         | 44%          | 93% / 95%          | 83.6       | 98.5       | 99.6           | 89.8        |
| MMWV            | minimax-family  | 41         | 68%         | 29%          | 93% / 95%          | 83.5       | 98.4       | 99.6           | 89.2        |
| Minimax         | minimax-family  | 41         | 66%         | 34%          | 93% / 95%          | 83.7       | 98.5       | 99.5           | 90.8        |
| Ranked pairs    | condorcet       | 43         | 65%         | 44%          | 91% / 95%          | 84.3       | 98.4       | 99.5           | 90.8        |
| Copeland        | condorcet       | 41         | 54%         | 34%          | 95% / 98%          | 83.9       | 98.7       | 99.6           | 94.0        |
| MMPO            | minimax-family  | 41         | 51%         | 20%          | 93% / 95%          | 83.5       | 98.4       | 99.6           | 88.5        |
| BTR-score       | hybrid          | 42         | 50%         | 55%          | 93% / 95%          | 85.2       | 97.2       | 100.0          | 93.4        |
| Soft Minimax    | hybrid          | 42         | 36%         | 74%          | 98% / 100%         | 85.1       | 97.5       | 100.0          | 94.7        |
| Smith//Score    | condorcet-based | 42         | 33%         | 29%          | 98% / 100%         | 85.2       | 97.2       | 100.0          | 93.8        |
| Soft Minimax-40 | hybrid          | 42         | 31%         | 83%          | 98% / 100%         | 86.1       | 96.2       | 100.0          | 92.2        |
| Score runoff    | two-round       | 42         | 26%         | 62%          | 100% / 100%        | 86.4       | 95.7       | 99.9           | 92.0        |
| ARA             | hybrid          | 42         | 10%         | 29%          | 95% / 98%          | 86.0       | 95.6       | 100.0          | 90.1        |
| MARS            | hybrid          | 42         | 7%          | 43%          | 90% / 98%          | 85.7       | 96.3       | 100.0          | 91.6        |
| STAR            | hybrid          | 44         | 2%          | 32%          | 86% / 93%          | 86.4       | 94.6       | 99.9           | 81.0        |
| IMDS            | rated           | 42         | 0%          | 7%           | 90% / 100%         | 84.9       | 96.9       | 100.0          | 92.6        |
| ESR             | rated           | 42         | 0%          | 0%           | 67% / 93%          | 85.5       | 94.5       | 100.0          | 89.1        |
| Score           | rated           | 44         | 0%          | 2%           | 41% / 86%          | 85.6       | 91.7       | 100.0          | 76.4        |

(Selection from `comparison/report.md`, which lists all methods.)

Three groups emerge:

- **Condorcet and minimax methods** (WME\* about 98.5, VSE\* 83.5–84.3): undominated in 51–72% of their conditions, Copeland and MMPO least. Their sincerity is now as high as that of the rated methods (99.5–99.6), so the higher sincerity of score ballots no longer decides against them (sections 4.5 and 4.13).
- **Score–pairwise hybrids** (Soft Minimax family, BTR-score, Smith//Score; VSE\* 85.1–86.1, WME\* 96.2–97.5): undominated in 31–69%, Soft Minimax-D most, and on the Pareto front most often of all (up to 86%). Their worst WME\* over all conditions is higher than that of every Condorcet method (92.2–94.7 against 88.5–94.0; section 4.3).
- **Score runoff and the methods with more VSE** (Score runoff, ARA, MARS, STAR; VSE\* 85.7–86.4, WME\* 94.6–96.3): close to the front nearly everywhere, but rarely undominated (2–26%).

**A note on the scale.** WME measures the winner's smallest head-to-head margin, which is the quantity Minimax makes as large as it can on the ballots. With honest rankings Minimax reaches WME 100 by construction, and the other Condorcet and minimax methods elect the same candidate whenever there is a Condorcet winner (97.5% of the base's elections). Their place at the top of the WME axis is therefore partly definitional, and a method can only dominate them by matching it. What is not definitional is how little they lose to strategy and to nomination, and how much VSE they keep. Soft Minimax-D, the leading hybrid, is discussed in section 10.3: why it does better than its relatives with a fixed clip.

The rated methods (Score, DMS, ESR, IMDS) are undominated nowhere. Their mean VSE\* (84.9–85.6) is no longer above the hybrids': strategic nomination costs them about 3 points of honest VSE, which it does not cost the Condorcet methods (journal, 2026-11-06), and it is on by default now. IMDS, new in the set, is the rated method with the highest WME\* (96.9) and within 2 points of the front everywhere.

Of the other methods, Approval runoff, BTR-IRV, ESR-MARS and MJ are undominated in one or two conditions. They fall into two kinds:

- **Close behind:** ESR-MARS, Delegated 321, 321, ESR, DMS, MAV, MJ, Approval runoff, Approval cond. runoff and Score are within 2 points of the front in 80–98% of the conditions, but within 1 point in 18–86%.
- **Clearly off the front:** the plurality-based methods (Plurality, IRV, IRV-3, Supplementary vote; ER-IRV less so), TTR, Borda and Asset are within 2 points in at most 20% of the conditions (ER-IRV 61%); Approval in 58%, but within 1 point in only 5%.

### 4.2 Who Clearly Dominates Whom

The figure orders the methods by clear dominance over all conditions. An arrow A → B means that A dominates B in at least half of the conditions both were run in, and more often than B dominates A. Arrows implied by a chain of others are left out, so every method is dominated, at this level, by everything above it along the arrows. The shares are in `comparison/report.md` and `comparison/beatpath.csv`.

![Clear dominance across all conditions: an arrow A → B means A dominates B in at least half of the conditions both were run in](comparison/beatpath.png){height=85%}

- **The top layer** holds 11 methods that no method dominates in half of their conditions: the Condorcet and minimax methods (Schulze, Kemeny-Young, Ranked pairs, Copeland, Minimax, MMWV, MMPO), Soft Minimax-D, Soft Minimax, BTR-score and Smith//Score. Copeland has joined it since version 1.0.0; Score runoff, Soft Minimax-40, ARA and MARS-T have left it.
- **Within the top layer, dominance is rare.** No method of it dominates another in more than 16 of their shared conditions; most pairs in fewer than 7. The largest shares are among the hybrids: Soft Minimax-D over Soft Minimax (16 of 42) and over BTR-score (13), Soft Minimax over Smith//Score (14) and over BTR-score (11); and Schulze over Copeland (12 of 41). Soft Minimax-40, just below, is dominated by Soft Minimax-D in 23 of 42.
- **Just below**, dominated by one or two of them:
  - Soft Minimax-40, Score runoff, ARA, MARS, MARS-T and IMDS by Soft Minimax-D (55–88%), ARA, MARS, MARS-T and IMDS also by BTR-score, Smith//Score or Soft Minimax (50–69%);
  - BTR-IRV by every Condorcet and minimax method (78–90%).
- **The middle**: ESR-MARS and STAR, dominated by Soft Minimax-40 (81% each), ESR-MARS also by Score runoff, ARA, MARS and MARS-T, STAR also by MARS. ESR is dominated by Score runoff, ARA, MARS-T, STAR and IMDS (52–79%).
- **Further down**, dominated by ESR-MARS and ESR (61–86%): Approval runoff, MJ, Delegated 321, 321, MAV, Score and ER-IRV. Then DMS, Approval cond. runoff, TTR and Approval, then Borda and IRV, IRV-3, Supplementary vote, Asset, and Plurality at the bottom.

So Score, the plain rated method, is now in the lower middle: methods about as good on VSE\* and clearly better on WME\* dominate it in most conditions.

**Which methods elect the same winners.** Dominance says who is better; the map of the methods says who is alike. The distance between two methods is the share of elections in which they elect different winners, averaged over the conditions, laid out in two dimensions (which show 38% of the spread, so closeness on the map is a hint, not a measure). Open circles mark the honest ballots, filled ones the ballots after the poll rounds, with an arrow between them (coordinates in `comparison/method-map.csv`).

![Map of the methods: close together means the same winner in most elections](comparison/method-map.png)

- **The Condorcet and minimax methods form one tight cluster** (with Black, Nanson, Baldwin, Benham, BTR-IRV and the -ER variants): whichever of them is chosen, the winner is nearly always the same.
- **The score–pairwise hybrids that check the pairwise result** (BTR-score, Smith//Score, Smith//Approval, MMWV+Score, MMWVxScore, Soft Minimax, Soft Minimax-D, IMDS, INS) lie between that cluster and a second one of score methods: Score runoff, STAR, MARS, ARA, Soft Minimax-40, ESR-MARS, 321, Score and DMS. The median-based methods (MJ, MAV) lie beside the score cluster.
- **The plurality-based methods form groups of their own** further away: IRV, ER-IRV and TTR together, IRV-3 near them, Supplementary vote, DSC and DAC (Woodall's descending solid and acquiescing coalitions) further out, the approval methods (Approval, MDDA and MAMPO, which add a majority test to approval) on the other side, Plurality and the asset methods far off.
- **Strategy moves few methods far.** The longest arrows belong to Plurality, Borda and Borda-ER (whose strategic winners move towards those of the Condorcet methods), Dowdall, IRV and IRV-3, and the approval runoffs. The methods of the front move little, as section 4.5 shows.

### 4.3 Where the Condorcet Methods Lose: All Voters Strategic, with the Older Engines

The Condorcet methods' worst WME\* values (88.5–91.1; Copeland 94.0) all come from three conditions. In each of them every voter is always strategic, and the voters find their moves in one of the two older ways the lab still offers: by arithmetic on the poll totals (the *arithmetic response*, with and without campaigns), or as groups of voters with the same sincere ranking (the *ranking groups*, the engine of version 1.0.0). There the hybrids beat every Condorcet method except Copeland on WME\*, the very axis on which the Condorcet methods normally lead. BTR-score and Smith//Score then dominate Schulze, Kemeny-Young, MMWV, Minimax and Ranked pairs, and Soft Minimax dominates most of them.

Copeland is no real exception. It holds up under the arithmetic only because the arithmetic almost never finds a move against it: 0.2% of the voters move under Copeland, against 3.6% under the other Condorcet methods (an earlier check of the arithmetic had found that under Copeland it rarely sees a third candidate in the race). Under the ranking groups, Copeland loses the most of all Condorcet methods (section 5.9).

With the default engine, pooled discovery, a fully strategic electorate does not hurt the Condorcet methods this way. In that condition Schulze, Kemeny-Young, Ranked pairs and the minimax methods dominate Soft Minimax-D and Soft Minimax-40, not the other way round; only Copeland loses, to Soft Minimax-D, Soft Minimax-40, BTR-score and Smith//Score. How much a fully strategic electorate costs the Condorcet methods therefore depends on how the voters find their moves:

- the arithmetic finds compromise and push-over against them (3.6% and 2% of the voters; WME\* about 91);
- the ranking groups find burial and compromise (25% and 21%; WME\* about 96);
- pooled discovery, which tests each move by counting it on the polls with the method itself, finds less of both (WME\* about 97.4).

Section 5.10 compares the engines.

The other exceptions are fewer and scattered:

- **Laziness** (truncated ballots) favours the Condorcet methods over Soft Minimax-D: Schulze, Minimax and Ranked pairs dominate it at laziness 0.25 and 0.5. In version 1.0.0 truncation cost the Condorcet methods ground.
- **Linear utility and more voters** favour BTR-score and Smith//Score over Soft Minimax-D (linear utility, 199 voters; Smith//Score also with candidates shifted by 1).
- **Score range 0–99** reverses how Soft Minimax compares with Soft Minimax-D, BTR-score and Smith//Score: in every other condition Soft Minimax has more WME\* and less VSE\* than they do, on 0–99 it has more VSE\* and less WME\*.

### 4.4 Condorcet Methods Sit at the Corner of what Any Rule Can Reach; the Rest Stay about 1.5 Points inside the Ballot Line

In the base (4000 elections), the reachable region is bounded by VSE 83.9–84.5 at the best WME (electing the candidate with the best worst margin) and by the balanced point VSE 95.1 / WME 96.2. The Condorcet and minimax methods reach VSE\* 84.0–84.5 at WME\* 99.1–99.4: practically that corner, even with strategic voters. Their Gap\* to the normalized-ballot line is 0.6–0.9 points.

The normalized-ballot line ends at VSE 91.0 (WME 92.5), and rounding to the 0–5 scale costs only 0.1 of it. No method comes near that end now: the highest VSE\* is 87.9 (ESR-MARS), Score has 87.1, Geometric median 87.7. In version 1.0.0 the best rated methods sat at the end of the line. Now they are about 4 points short, and already with honest ballots (Score 88.2): the result after strategic nomination is judged on the original field, and nomination costs the rated methods about 3 points of honest VSE, the Condorcet methods nothing (journal, 2026-11-06; section 4.7).

In between, the hybrids, Score runoff and IMDS stay 1.4–1.9 points inside the line on both axes (Gap\*), the rated and median-based methods 2.6–3.2, the plurality-based methods 3.3–5.9, Plurality 14.4. So the front still follows the line of what ballots carry, about 1.5 points inside it everywhere but at the Condorcet corner.

### 4.5 Strategy in the Base: Few Voters Move, and It Costs the Methods They Move under

From honest to strategic ballots in the base, with the share of voters voting strategically in the last round:

| Method | VSE → VSE\* | WME → WME\* | Voters strategic |
|---|---|---|---|
| Score | 88.2 → 87.1 | 95.7 → 92.8 | 9.2% |
| DMS | 88.4 → 87.3 | 94.9 → 92.3 | 8.0% |
| ESR | 87.2 → 86.8 | 96.5 → 94.7 | 9.1% |
| IMDS | 85.9 → 85.9 | 97.9 → 97.3 | 9.0% |
| STAR | 87.9 → 87.4 | 96.3 → 95.6 | 3.8% |
| Score runoff | 88.2 → 87.7 | 96.4 → 95.9 | 4.8% |
| Soft Minimax-D | 86.9 → 86.8 | 97.7 → 97.3 | 1.6% |
| Schulze | 84.3 → 84.4 | 99.7 → 99.4 | 1.4% |
| IRV | 86.0 → 83.6 | 95.4 → 92.5 | 6.3% |
| TTR | 85.1 → 84.5 | 94.1 → 93.9 | 4.6% |
| Borda | 82.7 → 81.7 | 94.2 → 91.1 | 13.1% |
| Plurality | 74.3 → 76.7 | 74.9 → 75.8 | 8.6% |

- **Few voters move.** Every voter is adaptive and weighs a move's value against a personal cost. Under the Condorcet methods and the Soft Minimax family only 1.3–1.6% of the voters vote strategically; under the score–pairwise hybrids 2.4–6%; under the rated methods 8–9%; under Borda 13%. Strategy types follow: under Condorcet methods 0.1–0.4% compromise and 0.7–1.2% bury; under rated methods 7–9% exaggerate and almost nobody reverses a preference.
- **Where voters move, it costs.** The rated methods lose 2.5–3 points of WME (Score, DMS), Borda 3, IRV and IRV-3 about 3 on both axes. IRV is the clearest change since version 1.0.0, where strategy raised its WME by 3 points: individual voters taking the best of a few noisy poll counts overreact under IRV's hard-to-predict count (the samples test, section 5, shows how much).
- **Only Plurality gains**, by 2.4 points of VSE and 0.9 of WME, from voters deserting candidates who cannot win. The gain is smaller than in version 1.0.0 (10 points), since the cost of voting insincerely keeps most voters sincere.
- The methods of the front move by at most 0.7 points on either axis.

### 4.6 The Natural Baseline: the Front Is about One Point ahead of Trivial Rules

The trivial rules sit at VSE / WME 0 / 0 (random ballot), 83.0 / 98.8 (CW//random ballot), 89.9 / 91.3 (its mirror) and 15.4 / 2.6 (random pair). The line between CW//random ballot and its mirror runs close to the front: the best methods are only about 1 point ahead of every lottery of trivial rules (Lead\*: Soft Minimax-D 1.3; ARA, Soft Minimax-40, MARS, BTR-score 1.2; Score runoff, Smith//Score, Soft Minimax 1.1; the Condorcet methods 0.7–1.0). These rules need the sincere preferences and are no proposal; they show how little room there is between a coin among simple ideal rules and the best ballots can do.

**No better than a lottery of trivial rules** (Lead\* at most 0): MAV, 321, Delegated 321, DMS, MJ, Score, Approval runoff, Approval cond. runoff, Approval, ER-IRV and every plurality-based method, TTR, Asset and Borda; ESR is at 0.0. With honest ballots Score, DMS, 321 and MJ are still ahead (Lead 0.1–1.2): strategy costs them their lead.

The **equivalent jury size** says the same in another unit. The Condorcet and minimax methods do as well as an ideal jury of 84–89 of the 99 voters (honest ballots 89–93); the Soft Minimax family, BTR-score and Smith//Score as 70–74; Score runoff 69; ESR-MARS and STAR 64; Score 52 (honest 70), Approval 44, IRV 41, Borda 34, Asset 28 and Plurality 17. A jury size near the electorate means the method uses nearly all the honest information the ballots carry.

### 4.7 Strategic Nomination Changes Few Results under Condorcet Methods

With honest ballots, strategic nomination (candidates withdrawing or fielding clones when that pays) changes the winner in:

- 2.6–4.0% of the elections under the Condorcet and minimax methods (almost no exits);
- 7.5–10.7% under the score–pairwise hybrids, IMDS and ESR-MARS;
- 11–16% under Borda, Score, ESR, DMS, Score runoff, 321, MJ, IRV and ER-IRV;
- 16–24% under MAV, the approval methods, IRV-3, Asset, TTR and Supplementary vote; 34.6% under Plurality (1.1 exits and 0.9 clones per election).

Score runoff and Borda attract the most clones among the methods with few exits (0.22 and 0.56 per election). The random ballot invites clones in every election (2 per election), which is why the reference rule Random ballot scores VSE −11 on the original field rather than 0. Nomination settles in nearly every election (Plurality 94%).

### 4.8 Manipulation from Honest Ballots: Condorcet Methods Hold, Counter-strategy Is Needed

The manipulation test (base, extended set) asks how often an informed coalition can change the result in its favour from honest ballots, and what that does to VSE:

- **One-sided** (only the attackers act): TTR 17% of elections, IRV-3 29%, Approval runoff 32%; Soft Minimax 42%, IMDS 45%; Score runoff 55%; the Condorcet methods 60–63% (Copeland 78%); Score 90%, DMS 88%, Borda 95%.
- **Two-sided** (the honest winner's supporters answer): BTR-IRV 1.1%, the Condorcet methods 1.8–2.1%; Soft Minimax, IMDS, BTR-score and Smith//Score 7.3–7.4%; Score runoff 10.3%; Score 13.2%; Asset 22.9%, Plurality 29.6%. The VSE with the worst successful manipulation is then 83.1–83.4 for the Condorcet methods, 82–83 for the hybrids, against 64.3 for Plurality.

So the methods resist manipulation mainly because the attacked side can answer, which is what the poll rounds simulate. Compared with version 1.0.0, the Soft Minimax family has become harder to manipulate two-sided (7.3% against 12.5%) and Score runoff easier (10.3% against 5.7%).

### 4.9 Rated Ballots Hide the Condorcet Winner More Often than Strategy Does

Of the 3900 elections with a sincere Condorcet winner, most rated ballots show the same one in 89–92% (321's grades 82.6%) (honest 89.5–92.8%) and a false one in 6–8%: the 0–5 scale's ties and rounding, not strategy. Rankings show it in 97.4–97.6% after strategy and 98.4% honestly; the missing 1.6% honestly are presumably elections in which nomination changed the field, since an honest ranking always shows the sincere Condorcet winner of the field it is cast on. A study of real score or grade ballots would see a false Condorcet winner in about 7% of elections from the format alone.

### 4.10 The Methods Only in the Base

The base-rest run adds the other 33 methods, references included, on the same 4000 elections (`overview-all-methods/`). Undominated among all 71: ARA, Soft Minimax-40, Soft Minimax-D, BTR-score, Soft Minimax, MMWV, Ranked pairs, Schulze, Minimax, and of the methods outside the extended set MMWVxScore (score share times one minus the worst defeat in winning votes), Schulze-ER and Minimax-ER (Schulze and Minimax on rankings with ties) and the reference Condorcet-random-drop (the Condorcet winner, else voters dropped at random until the rest have one). Black (the Condorcet winner, else the Borda winner), MMPOWV (a product of minimax pairwise opposition and the worst winning-votes defeat) and MMWV+Score (score share minus the worst winning-votes defeat) are on the Pareto front.

- **The two new methods on the approval cutoff ballot.** Smith//Approval (VSE\* 86.2 / WME\* 97.6) is as good as Smith//Score (86.2 / 97.5) and dominated only by Soft Minimax-D (on VSE\*): the voter's own approval cutoff is as good a tie-break among the Smith set as the score total. ARA-C (86.5 / 95.6) is clearly worse than ARA (87.6 / 96.4) on both axes: with each voter's own cutoff instead of the middle of the range, ARA loses about 1 point of each, and nomination changes its results more often (12.3% against 10.5%).
- **Others of note:** Soft Minimax-L (87.7 / 95.8) has the highest VSE\* of the Soft Minimax family, but is dominated on WME\* by MARS, MARS-T and ARA. INS (iterated normalized score: each round every ballot is stretched to the full range over the remaining candidates, and the lowest total is out; 85.8 / 97.3) gives the same results as IMDS, which only maxes the top and strikes the bottom and can be counted by hand. Geometric median (the geometric median of the score ballots; 87.7 / 93.1) is no better than a lottery of trivial rules. Coombs keeps a high WME with honest ballots (98.7) but loses 2 points of it under strategy (96.5). IDS (iterated dropped score), STLR (score then levelled runoff), MCA (majority choice approval), ARA-Top (ARA with only the top score as approval) and Reciprocal score are dominated by many.

The plot below shows which method of the base's Pareto front, among all methods, a given weighting of VSE\*, WME\* and sincerity leads to. Each point of the triangle is a weighting (corners: all weight on one axis), coloured by the method with the highest weighted sum, with each axis stretched to the range the front spans on it. With most weight on VSE\* it is ESR-MARS; the large middle belongs to ARA, then Soft Minimax-D and Soft Minimax as the weight moves towards WME\*; near the WME\* corner Schulze, Ranked pairs, Kemeny-Young, Schulze-ER and MMWV. Soft Minimax-L takes a strip towards sincerity, Score runoff only a sliver. The sincerity corner (MAV, MJ, Score+, INS) should be read with care: the front's sincerity spans only 99.7–100, so stretching that range turns differences no voter would notice into whole regions of the triangle. IMDS, ESR, MARS, MARS-T, Soft Minimax-40 and several MMWV variants are on the Pareto front but best for no weighting. The picture shows which method a set of values leads to, not how large the differences are: most of them are below a point.

![The best method of the base's Pareto front for each weighting of VSE\*, WME\* and sincerity](overview-all-methods/tradeoff-c1.png){height=60%}

### 4.11 Sample Size: the Default Run as an Example

The default run (`python run.py`: 200 elections, standard set) finds BTR-score, BTR-IRV, Schulze, Minimax, Ranked pairs and Copeland undominated. Its 200 electorates are the base's first and happen to be easier: electing the Condorcet winner loses 9.9% of VSE there, against 14.9% over the base's 4000, so every method has more VSE. With 200 elections the intervals are about ±3 points on VSE\* and dominance is decided mostly on WME\*. Some values are far off the base's: STAR's Lead\* is −1.4 (base 0.8), Score's −3.2 (base −0.8). The default run is an example to compare one's own runs with, not a result.

### 4.12 Steps up from Plurality

Pairs asked for in the comparison show how the simple methods relate, which matters for practical reforms:

| Pair | First dominates second | Second dominates first |
|---|---|---|
| Approval — Plurality | 42 of 43 conditions | 0 |
| Supplementary vote — Plurality | 39 of 41 | 0 |
| TTR — Supplementary vote | 34 of 41 | 0 |
| TTR — IRV | 25 of 41 | 4 |
| TTR — IRV-3 | 32 of 41 | 1 |
| Approval cond. runoff — TTR | 24 of 41 | 1 |
| Approval cond. runoff — Approval | 37 of 41 | 2 |
| Approval runoff — Approval cond. runoff | 25 of 41 | 0 |
| BTR-score — IRV | 41 of 41 | 0 |

- **Approval dominates Plurality nearly everywhere**, with a ballot almost as simple. The one exception is the arithmetic with all voters strategic and campaigns, where Plurality has the higher VSE\*.
- **The plurality-based ladder now runs Plurality → Supplementary vote → TTR → IRV.** TTR, a real runoff between the two leaders, dominates IRV in 25 of 41 conditions; in version 1.0.0 IRV dominated TTR in 25.5 of 33. IRV is ahead only where little strategy reaches it (coordination 0, honesty cost 1, 20 polls) and with 10 candidates. With honest ballots IRV is the better of the two (base: 86.0 / 95.4 against 85.1 / 94.1); the voters' overreaction to noisy polls (sections 4.5 and 5.5) turns that round. Strategic nomination adds a little: it changes the honest result under TTR more often than under IRV (20.8% against 15.5% of the elections, 0.53 exits per election against 0.22), since IRV's eliminations already do much of what a withdrawal would do, while under TTR a spoiler who withdraws changes who reaches the runoff. Nomination gives TTR 0.5 points of honest VSE and 9.5 of WME, IRV −0.9 and 9.0 (section 6.3). Without nomination TTR is still ahead of IRV under strategy (84.7 / 88.0 against 84.1 / 85.9), so nomination widens TTR's lead but does not create it.
- **An approval first round helps a runoff** (Approval cond. runoff over TTR in 24 of 41), though TTR keeps more WME\* in 13 conditions. Adding a runoff to Approval helps clearly (37 of 41), and a runoff between the two most-approved candidates helps more (Approval runoff over Approval cond. runoff in 25 of 41).
- **A score–pairwise method beats IRV in every condition.** BTR-score is the natural comparison: like IRV it eliminates candidates one by one, but each elimination is a pairwise runoff between the two candidates lowest in the score order rather than dropping the one with the fewest first preferences. It needs no recount of the ballots between rounds (the pairwise counts and score totals are tallied once), so it is arguably simpler to count than IRV, and it dominates IRV in all 41 conditions.

Two pairs show methods that are practically the same. **IMDS and INS** differ clearly on no axis in either condition both were run in: maxing at the top and striking at the bottom, which can be counted by hand, give the same results as INS's full normalization. **BTR-score and Smith//Score** dominate each other in only 6 and 3 of 42 conditions and differ clearly on VSE\* in 6 and on WME\* in 4: the bottom-two runoff and the Smith set lead to the same winners nearly always.

### 4.13 What Changed since Version 1.0.0

The model changed in several places at once between the two sets of runs: the strategy engine (individual voters with value against cost instead of ranking groups), strategic nomination on by default, candidates drawn from the voters, 5 polls per round instead of 10. The tests of the groups engine and of no nomination separate some of these effects (sections 5 and 6). Across all conditions:

| | Version 1.0.0 | Version 2.1.0 |
|---|---|---|
| Score runoff undominated | 80% of 35 conditions | 26% of 42 |
| Condorcet and minimax methods undominated | 44–73% | 51–72% |
| Soft Minimax-D undominated | 53% | 69% |
| Mean sincerity, Condorcet methods | 97.6–98.2 | 99.5–99.6 |
| Mean VSE\*, Score | 90.2 | 85.6 |
| Mean VSE\*, Schulze | 86.2 | 84.3 |

- **Score runoff lost its place** at the top. It used to win on sincerity against the Condorcet methods and on WME\* against the hybrids. Now the Condorcet methods are as sincere (section 4.5), and Soft Minimax-D dominates it in 57% of the conditions. It still has the highest worst VSE\* of all methods (59.8, with candidates spread out) and is within 1 point of the front everywhere.
- **Ranked ballots are now nearly as sincere as rated ones.** Few voters reverse a preference on a ranking. The reason is that few voters find a move that pays: under the Condorcet methods 1.3–1.4% of the voters vote strategically in the base, against 9% under Score (section 4.5). Pooled discovery counts each move on the polls with the method itself, and against a Condorcet method compromise and burial rarely pay there. The groups test confirms it is the engine, not the ballots: with the ranking groups of version 1.0.0 the Condorcet methods' sincerity is 97.9 again (section 5.9).
- **VSE is lower for everyone**, the rated methods most. Candidates drawn from the voters and strategic nomination change the electorates and the fields. The rated methods' lead in VSE was the part of the 1.0.0 findings that depended most on the conditions (steep utility, moderate strategy); nomination costs them about 3 points, and the no-nomination test (section 6) shows how much of the lost lead that accounts for.

Both versions together. Pooling the two collections (81 conditions, compare.py --force-versions) tests whether the leading group depends on the model version: the strategy engine, strategic nomination and how candidates are drawn all differ between them. It hardly does. The methods that no other method dominates in half of their conditions are the top layer of version 2.1.0 without Copeland (weak in version 1.0.0), plus Score runoff (strong there): Schulze, Ranked pairs, Kemeny-Young, Minimax, MMWV, MMPO, Soft Minimax-D, Soft Minimax, BTR-score, Smith//Score and Score runoff. Schulze and Ranked pairs are undominated most often (71% and 69%), Soft Minimax-D most often among the hybrids (62%), and Score runoff is the only method within 1 point of the front in every condition. The pooled means and shares mix two different models and are not meant as results.


## 5 Strategy

The tests have 2000 elections, the first 2000 of the base's 4000. These are somewhat easier than the whole base (honest VSE of Schulze 84.9 against 84.3, of Score 87.5 against 88.2), so the tables below compare each test with the honest values of the same 2000 elections, which are the same in every condition of a strategy test. The base's own strategic values on these elections lie between those of 2 and 4 poll rounds (section 5.6). The rank stability tables use the noise reference for runs of 2000 elections (section 4).

### 5.1 Who Strategises

The base has adaptive voters: each weighs the best move against a personal cost. The test replaces them with a fixed share of always-strategic voters, who make every move that pays at all: half, or all of the electorate.

| Method | Honest | Half strategic | All strategic |
|---|---|---|---|
| Schulze | 84.9 / 99.7 | 84.8 / 98.7 | 84.1 / 97.4 |
| Copeland | 84.9 / 99.6 | 84.4 / 98.3 | 82.6 / 94.3 |
| Soft Minimax | 86.1 / 98.1 | 85.6 / 97.7 | 84.1 / 95.9 |
| Soft Minimax-D | 86.9 / 97.9 | 86.0 / 97.0 | 84.4 / 95.2 |
| BTR-score | 86.5 / 98.1 | 86.0 / 97.3 | 83.7 / 95.1 |
| Score runoff | 87.9 / 96.5 | 86.2 / 95.1 | 85.1 / 93.0 |
| Approval runoff | 87.4 / 93.2 | 86.6 / 94.0 | 86.0 / 93.0 |
| STAR | 87.9 / 96.5 | 86.1 / 94.7 | 82.1 / 89.7 |
| Score | 87.5 / 95.7 | 84.6 / 91.4 | 79.5 / 85.4 |
| IRV | 86.2 / 95.9 | 81.9 / 90.6 | 73.1 / 81.3 |
| Borda | 83.2 / 94.1 | 79.3 / 89.4 | 71.2 / 75.2 |
| Plurality | 74.8 / 74.6 | 75.4 / 76.3 | 75.2 / 74.4 |

(VSE\* / WME\*; honest: VSE / WME.)

- **The Condorcet methods hold best.** With every voter strategic, Schulze, Kemeny-Young, Ranked pairs and the minimax methods keep WME\* 97.1–97.6 and lose about 1 point of VSE (0.8–1.4); they and Approval runoff are the undominated methods. Copeland loses more (WME\* 94.3).
- **The Soft Minimax family, BTR-score and Smith//Score lose 2–4 points on each axis**, Score runoff 2.8 / 3.5; STAR, ARA, MARS and ESR-MARS lose 6–8 points of WME.
- **Rated methods, IRV and Borda collapse**: Score loses 8 points of VSE and 10 of WME, IRV 13 and 15, Borda 12 and 19. Score ballots become approval ballots (27% of the voters exaggerate), and IRV and Borda reward compromise and burial (Borda 14% and 11%).
- **Approval runoff barely changes** (86.0 / 93.0 against 87.4 / 93.2): approval ballots leave little to exaggerate, and the real runoff decides between two candidates honestly. **Plurality does not change** either: deserting a hopeless candidate is what its voters do anyway.
- With all voters strategic **no method is better than a lottery of trivial rules** (Lead\* at most 0 everywhere; Schulze −0.7, Soft Minimax-D −1.5). Rank stability on VSE\* drops to 0.24 (noise reference 0.92): which methods do well on total utility depends on how much strategy there is. On WME\* the order mostly holds (0.95 against 0.99).

### 5.2 Coordination

A voter values a move as if a share of the voters who see the race as they do (their type) made it too: 0.25 in the base. The test uses 0, 0.1 and 0.5.

- **Without coordination nobody votes strategically.** At 0, a single voter's move never changes the polled chances enough to be worth its cost, and the results are the honest ones exactly. Strategic voting in this model is a group phenomenon, as with the deserting voters of plurality elections, who leave as a camp.
- **More coordination, more strategy**: at 0.1, 0.25 and 0.5, 0.4%, 1.4% and 3.5% of the voters vote strategically under the Condorcet methods, 4.6%, 9.2% and 14.5% under Score, 6%, 13% and 21% under Borda.

| Method | Coordination 0.1 | 0.5 |
|---|---|---|
| Schulze | 85.0 / 99.6 | 84.9 / 99.2 |
| Soft Minimax-D | 86.9 / 97.8 | 87.1 / 97.7 |
| Score runoff | 87.7 / 96.4 | 87.2 / 95.1 |
| Score | 86.6 / 93.8 | 86.4 / 91.3 |
| DMS | 87.5 / 92.6 | 86.7 / 90.7 |
| IRV | 81.6 / 90.6 | 85.6 / 94.7 |
| Borda | 83.0 / 92.9 | 81.9 / 89.0 |
| Plurality | 76.0 / 74.5 | 78.1 / 76.8 |

- The good methods change by less than 1 point; the rated methods lose WME as coordination grows (Score 1.9 points of honest WME at 0.1, 4.4 at 0.5), Plurality gains.
- **IRV is worst with little coordination** (81.6 / 90.6 at 0.1, 85.6 / 94.7 at 0.5). With few voters acting alike, the moves that reach its count are small and their effect on the eliminations hard to foresee; with many, a camp's compromise works as intended.
- Rank stability on VSE\* is 0.94 at 0.1 and 0.88 at 0.5 (noise reference 0.92); at 0 the order is the honest one.

### 5.3 Honesty Cost

The upper end of the personal cost of voting insincerely: 0.3 of a voter's utility range in the base, here 0.1 (about twice as many voters strategic) and 1 (about a third). Under Plurality 16% and 3% of the voters vote strategically, against 9% in the base.

| Method | Cost 0.1 | Cost 1 |
|---|---|---|
| Schulze | 84.5 / 98.3 | 84.9 / 99.7 |
| Soft Minimax-D | 86.0 / 97.1 | 86.9 / 97.9 |
| BTR-score | 86.5 / 97.2 | 86.6 / 98.1 |
| Score runoff | 86.7 / 94.4 | 87.8 / 96.4 |
| Score | 85.2 / 89.0 | 87.3 / 94.9 |
| IRV | 78.6 / 87.2 | 86.0 / 95.4 |
| Borda | 76.8 / 83.6 | 84.4 / 94.8 |
| Plurality | 76.9 / 75.3 | 74.6 / 74.4 |

- With the low cost the picture is that of half the voters strategic (section 5.1): undominated are Smith//Score, BTR-score, Ranked pairs, Kemeny-Young and Schulze, and only the Soft Minimax family, BTR-score, Smith//Score, Ranked pairs and Kemeny-Young keep a Lead\* above 0. Score loses 6.7 points of honest WME, IRV 8.7, Borda 10.5.
- With the high cost nearly everyone votes sincerely and the results are within about 1 point of the honest ones; Plurality loses the little its deserting voters gained.
- Rank stability on VSE\* is 0.81 at the low cost (noise reference 0.92): how much strategy there is changes the order on VSE\*, not on WME\* (0.98).

### 5.4 Poll Error

How much less precise than their sampling error voters take the polls to be: 2 in the base, here 1 (polls taken at face value) and 4.

| Method | Poll error 1 | 4 |
|---|---|---|
| Schulze | 84.9 / 99.5 | 84.7 / 99.2 |
| Soft Minimax-D | 86.9 / 97.6 | 86.7 / 97.7 |
| Score runoff | 87.2 / 95.3 | 87.9 / 96.3 |
| STAR | 87.6 / 95.2 | 88.0 / 95.6 |
| Score | 86.1 / 90.5 | 87.6 / 93.5 |
| DMS | 86.3 / 90.7 | 87.3 / 92.7 |
| IRV | 84.2 / 93.2 | 84.3 / 90.8 |
| Plurality | 77.3 / 77.1 | 73.8 / 73.6 |

- **Precise polls cost the rated methods**: Score's WME\* falls 3 points from poll error 4 to 1. When voters trust the polls, more of them exaggerate (Score 9.9% against 7.1%).
- **Vague polls cost Plurality** 3.5 points on both axes: the voters of a hopeless candidate are less sure to leave. They also cost IRV 2.4 points of WME.
- Under the Condorcet methods more voters move with vague polls (2.1% against 1.0%), but WME\* changes by 0.3.
- The order of the methods stays within the noise reference on every axis. Poll error matters for single methods, not for the comparison.

### 5.5 Poll Samples

Polls per round: 5 in the base, here 10 and 20, each with the same spread. With few polls each move's value is a coarse estimate, and voters taking the best of several such values overreact to lucky ones. The base keeps 5 on purpose: overreacting to noisy information is part of what a method must withstand.

| Method | 5 polls (rounds 2–4) | 10 polls | 20 polls |
|---|---|---|---|
| Schulze | 84.6–85.2 / 99.3–99.5 | 85.2 / 99.6 | 85.0 / 99.6 |
| Soft Minimax-D | 86.9–87.1 / 97.7–97.8 | 87.0 / 97.8 | 87.0 / 97.9 |
| Score runoff | 87.5 / 95.8–96.0 | 87.7 / 96.1 | 87.8 / 96.0 |
| Score | 87.2–87.3 / 92.6–93.0 | 87.2 / 93.6 | 87.0 / 93.9 |
| DMS | 86.7–87.4 / 92.6–92.7 | 87.4 / 93.3 | 87.6 / 94.0 |
| TTR | 84.5–84.9 / 93.9–94.0 | 85.1 / 94.0 | 85.5 / 94.5 |
| IRV | 83.9–84.5 / 92.3–93.5 | 83.8 / 94.1 | 85.3 / 95.5 |
| Borda | 82.2–83.0 / 91.2–92.4 | 84.6 / 93.9 | 85.5 / 93.9 |

(5 polls: the range of 2 and 4 poll rounds on the same elections, between which the base's 3 rounds lie.)

- **Fewer voters move with more polls** (Score 9.2%, 8.0%, 6.7%; Condorcet methods 1.4%, 1.2%, 1.0%).
- **IRV and Borda gain most.** With 20 polls IRV is within half a point of its honest WME (95.9) and Borda's VSE\* (85.5) is above its honest VSE (83.2): its voters' compromises help once they are based on good information. The rated methods gain about 1 point of WME, the good methods little.
- Rank stability on VSE\* is 0.89 with 20 polls (noise reference 0.92): the order changes little, mostly through IRV and Borda moving up.

### 5.6 Poll Rounds

One, two, four and five poll rounds instead of three, all from one run (the results of every round of a five-round run).

| Method | 1 round | 2 rounds | 4 rounds | 5 rounds |
|---|---|---|---|---|
| Schulze | 84.9 / 99.4 | 85.2 / 99.5 | 84.6 / 99.3 | 84.8 / 99.5 |
| Soft Minimax-D | 86.9 / 97.8 | 86.9 / 97.7 | 87.1 / 97.8 | 87.0 / 97.7 |
| Score runoff | 87.5 / 95.4 | 87.5 / 96.0 | 87.5 / 95.8 | 87.6 / 96.0 |
| Score | 85.6 / 91.3 | 87.2 / 92.6 | 87.3 / 93.0 | 87.1 / 92.9 |
| IRV | 82.6 / 91.4 | 83.9 / 92.3 | 84.5 / 93.5 | 83.6 / 93.5 |
| Borda | 81.3 / 89.4 | 82.2 / 91.2 | 83.0 / 92.4 | 82.4 / 91.3 |

- **The result settles after the second round.** The first round, voters answering a poll of sincere ballots, is the worst for the methods that strategy hurts: more voters move (Score 11.4% against 9.1% in round two), and the next poll shows them which moves disappointed, which they then trust less.
- The good methods do not change beyond noise in any round, and the order of the methods stays at the noise reference.

### 5.7 First Poll from a Previous Election

The first poll shows the strategic result of a previous election (its ballots carried over to a field that can differ), instead of sincere ballots: can the electorate leave a strategic state?

- **It can.** The results are those of an honest start within noise for nearly every method: Schulze 85.0 / 99.4, Soft Minimax-D 86.7 / 97.6, Score runoff 87.6 / 96.0 (honest start on the same elections, 2–4 rounds: 84.6–85.2 / 99.3–99.5, 86.9–87.1 / 97.7–97.8, 87.5 / 95.8–96.0). The order of the methods agrees with the base's at the noise reference (0.95 and 0.99).
- **IRV, IRV-3 and Score keep a little of the old state**: IRV 82.8 / 91.6 against 83.9–84.5 / 92.3–93.5 from an honest start, IRV-3 82.6 / 90.3, Score 86.4 / 92.6 against 87.2–87.3 / 92.6–93.0. Under them a strategic first poll leaves voters with moves that still seem to pay.
- Undominated, as in the base: Soft Minimax-40, Soft Minimax-D, BTR-score and the Condorcet and minimax methods.

### 5.8 Campaigns (with the Arithmetic Response)

In a campaign, a candidate recommends a move to their supporters, planned on the assumption that a share of them follows. Campaigns are only available with the arithmetic response, in which each voter values a move by simple arithmetic on the poll totals instead of counting the polls again. So the test has its own baseline: the arithmetic without campaigns, with adaptive voters and with all voters strategic.

| Method | Arithmetic | + campaigns | Arithmetic, all strategic | + campaigns |
|---|---|---|---|---|
| Schulze | 84.6 / 99.4 | 84.4 / 99.4 | 79.5 / 91.1 | 80.8 / 91.7 |
| Copeland | 84.9 / 99.6 | 84.8 / 99.5 | 84.8 / 99.4 | 84.6 / 98.7 |
| Soft Minimax-D | 86.7 / 97.7 | 87.0 / 97.4 | 83.9 / 97.0 | 83.5 / 96.9 |
| Score runoff | 87.7 / 96.5 | 87.8 / 96.5 | 84.7 / 98.2 | 85.1 / 98.4 |
| Approval runoff | 87.3 / 93.1 | 87.1 / 93.1 | 84.2 / 97.0 | 84.2 / 97.0 |
| Score | 87.4 / 94.7 | 87.3 / 95.3 | 83.7 / 96.3 | 83.8 / 96.3 |
| IRV | 83.4 / 92.9 | 83.2 / 92.6 | 75.4 / 84.9 | 76.4 / 86.7 |
| Borda | 82.7 / 90.2 | 81.6 / 89.2 | 79.6 / 90.6 | 78.4 / 89.3 |
| Plurality | 76.7 / 76.8 | 78.3 / 79.4 | 84.4 / 84.4 | 85.3 / 86.0 |

- **The arithmetic itself** (the engine of 2026-10-31 to 11-06) differs from pooled discovery with adaptive voters mainly under the rated methods, which lose less WME (Score 94.7 against about 92.8), and under the Condorcet methods, where fewer voters move (0.3% against 1.4%).
- **With all voters strategic the arithmetic turns the picture round.** Score ballots become approval-like (90% exaggerate), which costs 4 points of VSE but raises WME above the honest value; Score runoff reaches WME\* 98.2. Under Schulze, Ranked pairs, Kemeny-Young and Minimax, 3.6% compromise and 2% push the count over, and WME\* falls to about 91. Copeland stays unchanged because the arithmetic hardly finds a move against it (section 4.3). The order on WME\* is unrelated to the base's (rank stability 0.18).
- **Campaigns change little.** With adaptive voters they add strategic voters (Score 16.7% against 12%), but the results move by less than 1 point, except for Plurality, which gains 1.6 and 2.6 points: campaigns coordinate the desertion of hopeless candidates. With all voters strategic they add compromise under the Condorcet methods (13% of the voters), with no clear net effect.

### 5.9 The Ranking Groups (the Engine of Version 1.0.0)

In the engine of version 1.0.0, all voters with the same sincere ranking act as one group. The group changes its ballots if that raises its expected utility, judged by counting the polls again with the changed ballots. The test runs it with adaptive voters and with all voters strategic.

| Method | Groups, adaptive | Groups, all strategic |
|---|---|---|
| Schulze | 84.6 / 98.8 | 83.9 / 95.6 |
| Copeland | 84.6 / 98.2 | 82.9 / 94.0 |
| Soft Minimax-D | 86.6 / 97.4 | 84.7 / 97.2 |
| Soft Minimax-40 | 87.1 / 96.6 | 86.3 / 96.4 |
| Score runoff | 87.6 / 96.1 | 85.8 / 96.5 |
| Approval runoff | 87.4 / 94.1 | 86.3 / 95.8 |
| STAR | 86.0 / 95.2 | 86.0 / 95.6 |
| Score | 86.7 / 93.4 | 84.9 / 92.2 |
| IRV | 79.1 / 86.9 | 65.2 / 72.9 |
| Borda | 85.2 / 94.3 | 78.8 / 83.7 |
| Plurality | 78.0 / 76.6 | 79.7 / 81.3 |

- **Under the groups many more voters strategise**: 39–56% of the adaptive voters, against 1.3–13% for individual voters. Under the Condorcet methods 13% bury and 6% compromise, so their sincerity falls to 97.9, as in version 1.0.0. Their WME\* still stays at 98.8.
- **With all voters strategic the groups bury a quarter of the electorate under the Condorcet methods** (25% burial, 21% compromise), and WME\* falls to 95.4–96.0. Then Soft Minimax-D, Soft Minimax-40, Score runoff and Approval runoff are the undominated methods, with WME\* 95.8–97.2.
- **IRV fails under the groups**: 79.1 / 86.9 with adaptive voters, 65.2 / 72.9 with all strategic. Borda, by contrast, gains VSE under adaptive groups (85.2 against honest 83.2).
- The order on VSE\* changes somewhat (rank stability 0.84 with adaptive groups, 0.88 with all strategic; noise reference 0.92), and with all voters strategic the order on WME\* too (0.75).

### 5.10 What the Strategy Tests Show together

- **How much strategy there is matters more than its details.** The shares of strategic voters (always strategic voters, coordination, honesty cost) change the order of the methods on VSE\*; poll error, poll samples and rounds change single methods but not the order.
- **The Condorcet and minimax methods and the Soft Minimax family hold in every strategy setting** with individual voters: none loses more than 1.5 points of WME\* except with all voters always strategic, where the Condorcet methods lose 2.0–2.6 and the Soft Minimax family 2.2–3.9.
- **How voters find their moves decides what a fully strategic electorate can do to the Condorcet methods.**

| Engine, all voters strategic | Condorcet methods WME\* | Soft Minimax family WME\* | How they lose |
|---|---|---|---|
| Individual voters, pooled discovery (default) | 97.1–97.6 | 93.1–95.9 | few compromise or bury (up to 5% each) |
| Ranking groups (version 1.0.0) | 95.4–96.0 | 96.4–97.2 | burial 25%, compromise 21% |
| Individual voters, arithmetic | 90.6–91.1 | 96.8–97.1 | compromise 3.6%, push-over 2% |

The arithmetic misjudges how pairwise counts respond to a move (the response check), and the ranking groups move whole groups of identical ballots at once. Pooled discovery counts the moves with the method on the published polls, which anyone knowing the method can do. It is the most plausible of the three as a picture of what real voters can find, and against it the Condorcet methods hold best.

- **IRV and Borda depend most on the strategy model**: IRV from 65 to 86 points of VSE\*, Borda from 71 to 86, depending on the engine, the share of strategic voters and the number of polls.
- **Plurality is the one method strategy helps**, more so with coordination and campaigns: strategy under Plurality means deserting hopeless candidates.

## 6 Ballots and Candidates

As in section 5, the tests have the base's first 2000 elections, and changes are read against the honest values of the same elections.

### 6.1 Laziness

With laziness, honest ballots are truncated: each voter ranks, approves or scores only down to a personal cut, the cuts spread from complete ballots down to the middle of the voter's range (0.5) or a quarter of the way (0.25). Plurality, Supplementary vote, TTR and Asset name one or two candidates anyway and do not change.

| Method | Complete (honest) | Laziness 0.25 | Laziness 0.5 |
|---|---|---|---|
| Schulze | 84.9 / 99.7 | 86.3 / 98.3 | 86.6 / 96.8 |
| Copeland | 84.9 / 99.6 | 86.2 / 98.2 | 86.8 / 97.0 |
| Soft Minimax-D | 86.9 / 97.9 | 86.8 / 97.2 | 87.0 / 96.2 |
| Soft Minimax-40 | 87.4 / 97.0 | 87.4 / 96.4 | 88.0 / 95.8 |
| BTR-score | 86.5 / 98.1 | 86.8 / 97.2 | 87.1 / 96.1 |
| Score runoff | 87.9 / 96.5 | 87.6 / 96.0 | 88.1 / 94.8 |
| STAR | 87.9 / 96.5 | 88.0 / 95.3 | 88.2 / 94.7 |
| Score | 87.5 / 95.7 | 86.4 / 92.1 | 86.6 / 92.9 |
| IRV | 86.2 / 95.9 | 84.3 / 90.9 | 84.9 / 90.1 |
| Borda | 83.2 / 94.1 | 85.5 / 93.1 | 86.4 / 93.9 |

(VSE\* / WME\*; the first column is honest with complete ballots, for reference.)

- **Truncated rankings move the Condorcet methods off the corner.** Unranked candidates are tied last, so the pairwise counts miss the preferences among them: the Condorcet methods' honest WME falls from 99.7 to 98.6 and 97.3, their honest VSE rises from 84.9 to 86.3 and 86.8. After strategy they stand at about 86.6 / 96.9 at laziness 0.5, in the middle of the front.
- **The hybrids and Score runoff move the same way, a little less** (WME\* 1.7–2 points lower at laziness 0.5); Score runoff ends with the lowest WME\* of the front's methods (94.8). Score hardly changes, since a blank counts as the lowest score, close to what a lazy voter would give anyway.
- **The front narrows.** At laziness 0.5 the methods of the front lie between VSE\* 86.5 and 88.2 and WME\* 94.7 and 97.0. Undominated: Soft Minimax-40, Soft Minimax and the Condorcet and minimax methods (Copeland, Minimax, Ranked pairs, Kemeny-Young, MMWV, BTR-IRV); at laziness 0.25 ARA, Score runoff and the Condorcet methods.
- **This reverses version 1.0.0**, where laziness 0.5 left none of the Condorcet methods undominated and Score runoff kept the highest WME\*. Then the Condorcet methods' honest WME fell to about 95 and the ranking groups' strategy added to it; now it falls to 97.3, and strategy costs them only 0.5 points more.
- **Borda gains from truncation** (honest VSE 83.2 → 87.3 at laziness 0.5): truncation turns it into something closer to approval. **IRV loses** more under strategy as rankings get shorter (WME\* 90.1 against honest 94.5).
- **Delegation fills truncated rankings.** Delegated 321 differs from 321 only with lazy ballots, and it is better: 87.6 / 93.8 against 87.2 / 93.2 at laziness 0.25, 88.1 / 93.6 against 87.4 / 92.6 at laziness 0.5.
- The order of the methods changes on honest VSE (rank stability 0.83 at laziness 0.5) and on VSE\* (0.86), less on WME\* (0.96).

### 6.2 Score Range

Scores 0–99 instead of 0–5, for the 29 methods that read scores (the score–pairwise hybrids, rated and median-based methods, Score runoff, Smith//Score, Smith//Approval, ARA-C; analysed together with base-rest). The other methods do not read scores; their values are those of the base. VSE\* / WME\*:

| Method | 0–5 (honest) | 0–5 | 0–99 |
|---|---|---|---|
| Score | 87.5 / 95.7 | 87.2–87.3 / 92.6–93.0 | 87.1 / 92.1 |
| Score runoff | 87.9 / 96.5 | 87.5 / 95.8–96.0 | 87.5 / 96.6 |
| Soft Minimax | 86.1 / 98.1 | 86.2 / 98.0 | 86.6 / 98.2 |
| Soft Minimax-40 | 87.4 / 97.0 | 87.4 / 96.6–96.8 | 87.4 / 96.9 |
| ARA | 87.1 / 96.9 | 87.0–87.4 / 96.3–96.4 | 85.9 / 98.1 |
| MARS | 87.2 / 97.6 | 86.8–87.0 / 96.6–97.0 | 86.0 / 98.4 |
| Soft Minimax-D | 86.9 / 97.9 | 86.9–87.1 / 97.7–97.8 | 85.6 / 99.2 |
| BTR-score | 86.5 / 98.1 | 86.4–86.6 / 97.8–98.1 | 84.8 / 99.4 |
| Smith//Score | 86.5 / 98.1 | 86.4 / 97.8–97.9 | 85.0 / 99.4 |
| IMDS | 86.1 / 98.0 | 86.1 / 97.3–97.4 | 84.8 / 98.6 |

(0–5: honest values and the range of 2 and 4 poll rounds on the same 2000 elections, as in section 5.5; the base has 3.)

- **On a fine scale the score–pairwise hybrids become Condorcet methods in effect**, as in version 1.0.0. On a 0–5 ballot the rankings that scores imply have many ties, so the pairwise step sees only part of the preferences (section 4.9). On 0–99 there are few ties: BTR-score, Smith//Score, Smith//Approval, MMWV+Score and MMWVxScore reach WME\* 99.4–99.5 at VSE\* 84.8–85.2, Soft Minimax-D 85.6 / 99.2. This holds for honest ballots too (BTR-score 85.0 / 99.7), so it is the ballot, not strategy.
- **Soft Minimax, Soft Minimax-40, Soft Minimax-L and Score runoff stay where they are**: their pairwise part only corrects the scores (Soft Minimax) or the runoff reads a separate ranking (Score runoff). ARA, MARS, MARS-T and IC-MARS move halfway (WME\* 98.1–98.4).
- **Pure rated methods do not change** (Score 87.1 / 92.1, DMS, ESR, MJ, MAV within about 1 point).
- **Undominated on 0–99**: Score runoff, Soft Minimax-40, Soft Minimax, Soft Minimax-D, MMWVxScore, MMWV+Score, Smith//Score and Smith//Approval. This is the one condition where Soft Minimax has the higher VSE\* and Soft Minimax-D the higher WME\*, the reverse of all others (section 4.3).
- The order of the methods changes clearly (rank correlation with the base 0.70 on VSE\* and 0.94 on WME\*, against 0.82 and 0.99 from noise for these methods).

**The score range is a design choice that shifts the results across much of the field.** On the default 0–5 scale the score–pairwise hybrids lose part of the voters' pairwise information to ties, which makes the Condorcet methods look better relative to them. On 0–99, BTR-score, Smith//Score, Smith//Approval and the MMWV score hybrids match the Condorcet methods' WME\* at the same VSE\* (Schulze on the same elections: 84.6–85.2 / 99.3–99.5), and Soft Minimax-D keeps 0.4–1 point more VSE\* at WME\* 99.2: with the extra information they become the better part of the Condorcet methods. A 0–99 ballot is impractical in real elections, while 0–9 or 0–10 is realistic; such a range was not part of these runs (section 11).

### 6.3 Strategic Nomination

Strategic nomination is on by default since version 2.0.0: candidates withdraw (exits) or field up to two clones when that pays for them. The test turns it off: the field as drawn. VSE and WME are judged on the original field in both.

| Method | Honest, with nomination | Honest, field as drawn | Strategic, field as drawn |
|---|---|---|---|
| Schulze | 84.9 / 99.7 | 84.8 / 100.0 | 85.0 / 99.7 |
| Copeland | 84.9 / 99.6 | 84.6 / 99.8 | 84.5 / 99.4 |
| Soft Minimax-D | 86.9 / 97.9 | 89.0 / 95.9 | 89.2 / 95.5 |
| Soft Minimax-40 | 87.4 / 97.0 | 90.3 / 93.9 | 89.9 / 93.9 |
| BTR-score | 86.5 / 98.1 | 88.7 / 95.9 | 88.8 / 95.7 |
| ARA | 87.1 / 96.9 | 90.5 / 94.1 | 90.0 / 93.9 |
| ESR-MARS | 87.6 / 96.5 | 90.9 / 93.3 | 90.2 / 92.6 |
| Score runoff | 87.9 / 96.5 | 87.3 / 98.3 | 87.6 / 97.5 |
| STAR | 87.9 / 96.5 | 89.6 / 95.4 | 89.0 / 94.3 |
| Score | 87.5 / 95.7 | 90.9 / 92.3 | 89.1 / 90.7 |
| MAV | 87.3 / 93.6 | 90.0 / 87.0 | 89.7 / 88.3 |
| Approval | 86.2 / 90.2 | 86.2 / 81.9 | 87.9 / 84.0 |
| IRV | 86.2 / 95.9 | 87.1 / 86.9 | 84.1 / 85.9 |
| TTR | 85.5 / 94.5 | 85.0 / 85.0 | 84.7 / 88.0 |
| Plurality | 74.8 / 74.6 | 70.9 / 68.6 | 76.4 / 71.7 |

(VSE / WME honest, VSE\* / WME\* strategic.)

- **Condorcet methods are almost immune.** Nomination changes their honest result in about 3% of the elections (section 4.7), and their values stay within half a point.
- **Under the rated methods and the hybrids, nomination trades VSE for WME.** With the field as drawn, Score has honest VSE 90.9 at the end of the normalized-ballot line (section 4.4) and WME 92.3; with nomination 87.5 and 95.7. The hybrids, STAR and ESR-MARS move the same way by 1.7–3.4 points on each axis: the winner moves towards the Condorcet corner, at a cost in total utility. The report does not show the mechanism.
- **Score runoff goes the other way**: nomination costs it 1.8 points of honest WME and gives it 0.6 of VSE. It attracts clones (0.22 per election, section 4.7), which can take the second runoff place.
- **The plurality-based methods, TTR and Approval gain WME from nomination**, by 8–10 points honestly (IRV 86.9 → 95.9, TTR 85.0 → 94.5, Approval 81.9 → 90.2): spoilers withdraw, so vote splitting does less harm. Plurality gains on both axes (honest 70.9 / 68.6 → 74.8 / 74.6).
- **Undominated with the field as drawn**: Soft Minimax-D, Smith//Score, BTR-score, Score runoff and the Condorcet methods Minimax, Schulze and Ranked pairs. The order changes on both axes (rank stability on honest VSE 0.90 and WME 0.91, against 0.95 and 0.99 from noise).
- The rated methods' lead in VSE, which version 1.0.0 found with a fixed field, is there again without nomination (Score 89.1, ARA 90.0, ESR-MARS 90.2 against the Condorcet methods' 85.0), but at a lower WME\* (90.7–93.9). Strategic nomination is the main reason the rated methods lost their lead (section 4.13).

### 6.4 What the Ballot and Candidate Tests Show together

- **The front's middle depends on the score scale.** On 0–5 the score–pairwise hybrids lie between the Condorcet and the rated methods; on 0–99 BTR-score, Smith//Score and Soft Minimax-D become Condorcet methods in effect, and only those whose pairwise part merely corrects the scores (Soft Minimax, Soft Minimax-40, Score runoff) stay in between.
- **Laziness and strategic nomination both move methods along the front, in opposite directions**: laziness moves the Condorcet methods towards more VSE and less WME, nomination moves the rated methods and the hybrids towards more WME and less VSE. Both narrow the front.
- **The Condorcet methods' standing no longer rests on complete rankings.** With few strategic voters they stay undominated with lazy ballots, unlike in version 1.0.0.
- **Undominated in every ballot test they were run in**: Minimax and Ranked pairs (laziness 0.25 and 0.5, field as drawn; they do not read scores, so the 0–99 test does not concern them). Of the score methods, Soft Minimax-40 and Soft Minimax are undominated at laziness 0.5 and on 0–99, but not at laziness 0.25 or with the field as drawn; Score runoff at laziness 0.25, on 0–99 and with the field as drawn, but not at laziness 0.5.

## 7 Electorates

These tests change the electorates, so the honest values differ from condition to condition, and each condition has its own frontiers. The tables give VSE\* / WME\* with honest VSE / WME of the same condition in brackets where they matter.

| Condition | Condorcet winner | Electing it loses VSE | Reachable at the best WME | Normalized-ballot line ends at |
|---|---|---|---|---|
| base (4000) | 97.5% | 14.9% | VSE 83.9–84.5 | VSE 91.0 (WME 92.5) |
| model clusters | 94.7% | 12.9% | 85.4–85.8 | 91.7 (93.7) |
| model field | 95.5% | 6.7% | 92.3–92.8 | 94.4 (98.7) |
| model ks | 98.8% | 6.5% | 93.2–93.3 | 96.3 (96.5) |
| width 1.4 | 97.9% | 6.5% | 92.6–93.0 | 95.3 (94.7) |
| linear utility | 97.9% | 2.5% | 97.1–97.2 | 98.3 (98.2) |
| quadratic utility | 97.9% | 3.7% | 95.7–95.9 | 98.4 (95.7) |
| candidates central (0.5) | 85.8% | 37.2% | 57.9–60.1 | 63.2 (91.2) |
| candidates spread out (2) | 97.5% | 44.8% | 53.6–54.5 | 61.6 (93.9) |
| candidates shifted 0.5 | 98.1% | 15.4% | 83.3–83.8 | 89.2 (93.0) |
| candidates shifted 1 | 97.6% | 16.9% | 81.8–82.6 | 88.2 (94.5) |
| 4 candidates | 98.8% | 15.2% | 84.3–84.5 | 89.2 (93.9) |
| 8 candidates | 96.4% | 14.0% | 84.4–85.3 | 92.5 (90.6) |
| 10 candidates | 95.3% | 13.3% | 85.0–85.9 | 93.4 (89.5) |
| 199 voters | 97.9% | 15.6% | 83.6–83.7 | 90.8 (92.8) |

How much the two goals conflict (the VSE lost by electing the Condorcet winner) is the electorate property that matters most: where it is small, all good methods are close together on VSE and the Condorcet methods lose little; where it is large, the methods spread out.

### 7.1 Electorate Models

Three other models: clusters (Gaussian clusters, close to the base), field (a smooth density) and ks (Quinn's hierarchical model; as in vse-sim's code it is the vse-sim setting, section 9). What the models' electorates look like, without any method, is in the folder `electorates/` (made with `show_electorates.py`): example electorates, how many dimensions they effectively have, and how costly electing the Condorcet winner is.

| Method | clusters | field | ks |
|---|---|---|---|
| Schulze | 85.5 / 99.0 | 92.1 / 98.8 | 93.2 / 99.4 |
| Copeland | 85.3 / 98.7 | 92.3 / 98.9 | 93.2 / 99.2 |
| Soft Minimax-D | 87.0 / 98.2 | 93.6 / 98.4 | 94.5 / 95.9 |
| Soft Minimax | 87.1 / 98.3 | 93.4 / 98.6 | 94.1 / 96.3 |
| BTR-score | 87.2 / 97.7 | 93.5 / 98.0 | 94.2 / 95.8 |
| Score runoff | 89.6 / 96.2 | 93.6 / 97.1 | 95.0 / 93.6 |
| STAR | 88.8 / 95.6 | 93.1 / 96.6 | 94.9 / 93.5 |
| Score | 88.0 / 94.0 | 92.6 / 94.8 | 93.6 / 88.8 |
| IRV | 81.7 / 92.4 | 86.0 / 90.9 | 90.5 / 86.4 |
| Plurality | 74.3 / 76.8 | 72.4 / 72.8 | 76.4 / 65.7 |

- **Clusters** order the methods as the base does (rank stability at the noise reference). Score runoff gains: it is undominated, with the highest VSE\* (89.6) at WME\* 96.2.
- **Field and ks are easier**: the Condorcet winner costs only 6.5–6.7% of VSE, so every good method reaches VSE\* 92–95 and the differences shrink. In the field model only Soft Minimax-D and Soft Minimax are undominated; in ks MARS, MARS-T, Soft Minimax-40, Soft Minimax and the Condorcet methods Ranked pairs, MMWV and MMPO.
- **In ks the front is close to the trivial rules**: Lead\* is 0.4 at most (Condorcet methods, Soft Minimax-D), and Score runoff, ARA, STAR and Score are no better than a lottery of them. The Condorcet methods are only 1.8 points of VSE\* behind the best method there and keep WME\* 99.4; the Soft Minimax family and BTR-score are 3.1–3.6 points below them on WME\*.
- **IRV loses most under strategy where honest IRV is good**: in the field model its honest result is 92.1 / 98.1, its strategic one 86.0 / 90.9.
- The order of the methods changes in field and ks (rank stability on VSE\* 0.73 and 0.63, against 0.92 from noise); on WME\* less (0.95 and 0.96).

### 7.2 Utility Width and Shape

How quickly utility falls with distance: the base's exponential fall-off with width 0.7 (mode-like: nearby candidates matter most), width 1.4 (towards linear), linear (median-like) and quadratic (mean-like).

| Method | width 1.4 | linear | quadratic |
|---|---|---|---|
| Schulze | 92.8 / 99.4 | 97.0 / 99.4 | 95.7 / 99.4 |
| Soft Minimax-D | 93.7 / 98.3 | 97.7 / 97.4 | 97.8 / 94.5 |
| BTR-score | 93.6 / 98.3 | 97.5 / 97.9 | 97.1 / 95.0 |
| Soft Minimax-40 | 94.1 / 97.6 | 97.8 / 96.4 | 97.6 / 92.2 |
| Score runoff | 94.1 / 96.6 | 97.6 / 95.3 | 97.7 / 93.0 (honest 97.8 / 89.8) |
| STAR | 93.9 / 95.8 | 97.3 / 94.5 | 97.6 / 91.3 (honest 97.8 / 89.5) |
| Score | 92.7 / 92.6 | 95.8 / 91.1 | 95.4 / 86.7 |
| IRV | 90.7 / 92.8 | 93.6 / 92.9 | 92.5 / 94.1 |
| Plurality | 80.9 / 75.6 | 81.8 / 75.0 | 80.1 / 75.6 |

The sweep plot `utility/analysis-vs-base/chain-utility.png` shows each method's path from exponential to linear and quadratic utility.

- **The flatter the utility, the less the goals conflict.** With width 1.4 the Condorcet winner costs 6.5% of VSE, with linear utility 2.5%: the Condorcet methods reach VSE\* 97.0 there, within 0.8 of the best method, at WME\* 99.4. Undominated with linear utility: BTR-score and the Condorcet methods (Minimax, Ranked pairs, Schulze, MMWV, Kemeny-Young, Copeland); with width 1.4: Soft Minimax-40, Soft Minimax-D, Smith//Score, BTR-score and the Condorcet methods.
- **Quadratic utility favours the methods with more VSE**: Soft Minimax-D, Soft Minimax-40, ARA, Score runoff, STAR and ESR-MARS reach VSE\* 97.6–97.8, the Condorcet methods 95.7. Twelve methods are undominated, from MARS and Soft Minimax-D to the Condorcet methods: the trade-off is real again, and steep.
- **Under quadratic utility strategy raises the WME of the score methods with a runoff**: Score runoff from honest 89.8 to 93.0, STAR from 89.5 to 91.3, Soft Minimax-40 from 91.3 to 92.2.
- **Score is never undominated**: in every shape, a hybrid is as good on VSE\* and clearly better on WME\*.
- Utility shape changes the order of the methods more than any other test but central candidates (section 7.3): rank stability on honest VSE 0.40 with linear utility, on honest WME 0.65 and WME\* 0.79 with quadratic utility. How strongly the two goals conflict remains the model's most consequential assumption, as in version 1.0.0.

### 7.3 Candidate Dispersion

Who stands: candidates spread like the voters (1, the base), or drawn mostly from the centre (0.5) or from the edges (2) of the electorate (by who stands among the voters: an exponential tilt, docs/model.md).

| Method | central (0.5) | spread out (2) |
|---|---|---|
| Schulze | 58.6 / 96.1 | 54.4 / 99.5 |
| Copeland | 59.3 / 95.6 | 53.4 / 99.2 |
| Soft Minimax-D | 61.1 / 94.7 | 55.5 / 97.8 |
| Soft Minimax-40 | 61.3 / 93.3 | 55.6 / 97.1 |
| Score runoff | 61.4 / 92.3 | 59.8 / 96.2 |
| STAR | 61.2 / 91.3 | 59.6 / 96.1 |
| Score | 61.0 / 87.1 | 59.1 / 94.1 |
| Borda | 63.0 / 87.8 | 54.7 / 89.0 |
| IRV | 52.2 / 73.3 | 52.5 / 91.7 |
| TTR | 54.0 / 78.0 | 55.1 / 95.8 |
| Approval | 44.9 / 70.0 | 54.6 / 93.3 |
| Plurality | 18.2 / 40.8 | 48.8 / 85.8 |

- **Both are hard conditions for VSE.** The field is unrepresentative, so electing the Condorcet winner costs 37–45% of VSE and the good methods' VSE\* is between 53 and 63. These are the worst VSE\* values of nearly every method in the comparison (column *Worst VSE\** in `comparison/report.md`).
- **Central candidates break the plurality-based methods.** With many similar candidates near the centre, first preferences split: Plurality falls to 18.2 / 40.8 (honest 17.8 / 40.1), IRV to 52.2 / 73.3, TTR to 54.0 / 78.0. Approval falls even further (44.9 / 70.0): approving several central candidates gives no way to rank among them. Only 85.8% of these elections have a Condorcet winner.
- **There Borda has the highest VSE\* (63.0)**, with the jury size of nearly the whole electorate (98), but low WME (87.8); undominated are Soft Minimax-D and the Condorcet and minimax methods (Schulze, Minimax, MMPO, MMWV). The good methods are 3–6 points ahead of a lottery of trivial rules, which are poor here (CW//random ballot 54.3 / 91.9).
- **Spread-out candidates favour the methods with more VSE**: Score runoff (59.8 / 96.2) and STAR (59.6 / 96.1) are undominated, as are MARS, MARS-T and the Condorcet methods (54.4 / 99.5). The two goals conflict most here.
- The order of the methods changes completely with central candidates (rank stability on honest VSE 0.34, VSE\* 0.37) and stays with spread-out ones (0.89).

### 7.4 Candidate Shift

Candidates drawn from one side: half or one standard deviation of the voters further along the main dimension.

- **Little changes.** The Condorcet winner costs a little more VSE (15.4% and 16.9%), the methods with more VSE gain a little relative to the Condorcet methods (STAR 86.2 / 96.3, Score runoff 85.9 / 96.4 against Schulze 83.6 / 99.6 at shift 0.5), and the order stays within the noise reference (0.89–0.93 on VSE\*, 0.99 on WME\*).
- Undominated at 0.5: Soft Minimax and the Condorcet and minimax methods; at 1: Smith//Score, BTR-score and the Condorcet and minimax methods (Copeland, Minimax, Ranked pairs, Kemeny-Young, MMWV, MMPO).

### 7.5 Number of Candidates

4, 8 and 10 candidates instead of 6. The electorates are nested: the base's 6 candidates are the first of the 8 and 10, with the same voters.

| Method | 4 | 8 | 10 |
|---|---|---|---|
| Schulze | 84.3 / 99.5 | 84.7 / 99.3 | 84.9 / 99.2 |
| Soft Minimax-D | 85.2 / 99.2 | 88.6 / 96.5 | 89.2 / 96.2 |
| Soft Minimax-40 | 86.0 / 98.6 | 89.6 / 95.0 | 90.4 / 94.2 |
| ARA | 85.6 / 97.9 | 88.8 / 95.2 | 90.5 / 94.1 |
| Score runoff | 85.9 / 97.6 | 89.1 / 94.5 | 89.9 / 93.5 |
| ESR-MARS | 86.1 / 97.4 | 89.6 / 93.4 | 91.0 / 91.8 |
| Score | 86.1 / 95.9 | 88.1 / 90.1 | 89.6 / 89.5 |
| Approval | 85.0 / 95.2 | 87.6 / 87.6 | 88.2 / 85.0 |
| IRV | 83.3 / 96.2 | 84.9 / 90.3 | 86.2 / 88.0 |
| TTR | 83.7 / 97.4 | 85.1 / 90.4 | 84.5 / 86.7 |
| Plurality | 76.4 / 83.6 | 75.1 / 69.1 | 73.0 / 63.6 |

The sweep plot `candidates/analysis-vs-base/chain-cands.png` shows each method's path from 4 to 10 candidates.

- **The trade-off opens with more candidates.** The normalized-ballot line ends at VSE 89.2 with 4 candidates and 93.4 with 10, while the best WME stays at VSE about 85. The methods with more VSE gain (ESR-MARS 86.1 → 91.0), the Condorcet methods stay at about 84.5–85 / 99.3.
- **The methods that read only first preferences or approvals lose WME as the field grows**: Plurality 83.6 → 63.6, TTR 97.4 → 86.7, Approval 95.2 → 85.0, IRV 96.2 → 88.0. Score's cost of strategy grows too (WME\* 95.9 → 89.5).
- **Soft Minimax-D and Soft Minimax-40 are undominated at every size**, Soft Minimax at 4 and 8, the Condorcet methods at 8 and 10 (and Copeland and MMWV at 4), ARA at 10.
- With more candidates there is more room above the trivial rules: Lead\* of the front's methods grows from about 0.5 with 4 candidates to about 3 with 10.
- The order of the methods stays within the noise reference.

### 7.6 Number of Voters

199 voters instead of 99 (polls of half the electorate); the base's 99 voters are the first 99 of these.

- **Little changes.** Values move by less than 1 point for the good methods; the order stays within the noise reference. Undominated: ARA, Smith//Score, BTR-score, Soft Minimax, Copeland, Minimax, Schulze, MMWV.
- **Score loses more to strategy** (honest 87.5 / 96.0, strategic 86.0 / 92.2; with 99 voters 87.5 / 95.7 and about 87.2 / 92.8).
- The equivalent jury sizes scale with the electorate: the Condorcet methods do as well as an ideal jury of 170–176 of the 199 voters (base: 84–89 of 99), the Soft Minimax family and BTR-score as 114–127, Score as 57.

### 7.7 What the Electorate Tests Show together

- **How strongly the two goals conflict decides how far apart the methods are.** Where electing the Condorcet winner costs little VSE (linear utility, wide utility, the field and ks models), all good methods are within a few points of each other and the Condorcet methods lose little. Where it costs much (quadratic utility, many candidates, spread-out candidates), the methods with more VSE gain and the trade-off is real.
- **Who stands matters as much as how voters think.** Central candidates break every method that reads only first preferences or approvals; unrepresentative fields lower everyone's VSE by 25–30 points.
- **The Condorcet corner is robust**: the Condorcet methods keep WME\* 98.7–99.6 in every electorate test except central candidates (95.6–96.3), and some of them are undominated in every electorate test but the field model.
- **Soft Minimax-D is the method undominated in most electorate tests** (models clusters and field, width 1.4, quadratic utility, central candidates, 4, 8 and 10 candidates).
- **Score is undominated in none**: as in the base, methods about as good on VSE\* and clearly better on WME\* dominate it.

## 8 Everything Hard at Once

The hard condition combines candidates clustered in the centre (dispersion 0.5), laziness 0.25 and poor poll information (poll error 4), with strategic nomination as everywhere. Its electorates are those of the central-candidates test (section 7.3), so the comparison with that test shows what laziness and vague polls add. VSE\* / WME\*:

| Method | Central candidates | Hard |
|---|---|---|
| Soft Minimax | 60.0 / 95.4 | 60.9 / 94.7 |
| Soft Minimax-D | 61.1 / 94.7 | 60.7 / 94.7 |
| Soft Minimax-40 | 61.3 / 93.3 | 61.2 / 93.3 |
| Schulze | 58.6 / 96.1 | 59.2 / 95.3 |
| Copeland | 59.3 / 95.6 | 60.0 / 94.5 |
| Smith//Score | 59.6 / 94.4 | 59.9 / 93.8 |
| Score runoff | 61.4 / 92.3 | 60.7 / 92.0 |
| STAR | 61.2 / 91.3 | 60.7 / 91.1 |
| Score | 61.0 / 87.1 | 59.8 / 87.8 |
| Borda | 63.0 / 87.8 | 59.9 / 89.9 |
| IRV | 52.2 / 73.3 | 54.4 / 73.4 |
| TTR | 54.0 / 78.0 | 52.9 / 75.4 |
| Approval | 44.9 / 70.0 | 43.3 / 69.6 |
| Plurality | 18.2 / 40.8 | 18.9 / 40.5 |

- **The candidates make the condition hard; laziness and vague polls add little.** Most methods move by less than 1 point from the central-candidates test. Laziness moves the Condorcet methods slightly towards VSE (Schulze +0.6 / −0.8), as in section 6.1, and costs Borda its lead in VSE\*.
- **Only Soft Minimax and Soft Minimax-D are undominated.** The Condorcet methods keep the highest WME\* (94.4–95.3), but at 1–2 points less VSE\*.
- **The methods that read only first preferences or approvals break down.** Approval runoff, Approval cond. runoff, TTR, Supplementary vote, IRV-3 and Approval have their worst VSE\* of the whole comparison here, Approval also its worst WME\* (69.6). Supplementary vote falls to 40.3 / 58.7, Plurality to 18.9 / 40.5.
- **The good methods are 4–6 points ahead of the trivial rules** (Lead\*: Soft Minimax 5.8, Soft Minimax-40 and Soft Minimax-D 5.7, Score runoff 5.0, Copeland 5.0, Schulze 4.6). Where the trivial rules are poor, the methods' differences show.
- The order of the methods on VSE\* is unrelated to the base's (rank stability 0.25), on WME\* it largely holds (0.93).

Compared with version 1.0.0, whose hard condition had laziness 0.1 and polls of 9 instead of laziness 0.25 and poll error 4: then Geometric median, the Soft Minimax family, BTR-score, Score runoff and the Condorcet methods were undominated, and every method but Approval and Plurality was at VSE\* 80 or above. Central candidates are far harder now, probably because the field is drawn from the most central voters, many of them close together (since 2026-11-03, candidates are voters); electing the Condorcet winner costs 37% of VSE.

## 9 The vse-sim Setting

A setting close to Jameson Quinn's vse-sim: Quinn's hierarchical model as in vse-sim's code, linear utility, 40 voters, 6 candidates apart from the voters, a fixed field (no nomination), scores 0–10, one poll round, a fixed share of always-strategic voters (half or all), vse-sim's methods; 15 000 elections each.

In these electorates the conflict between VSE and WME practically vanishes: electing the Condorcet winner costs 1.1% of VSE, and some rule reaches VSE 97.1–98.8 at the best WME. A Condorcet winner exists in only 87.2% of the elections (with 40 voters, pairs are often tied), and the trivial rules are weak (CW//random ballot 87.7 / 91.6), so every method but IRV and Plurality is far ahead of them.

vse-sim's VSE is 0 for a random candidate, the lab's for a random ballot. With a random candidate at −61.2 on the lab's scale in this setting, classic VSE = (VSE + 61.2) / 161.2. Lab VSE (classic VSE in brackets):

| Method | Honest | Half strategic | All strategic |
|---|---|---|---|
| Schulze | 98.0 (98.8) | 97.6 (98.5) | 97.1 (98.2) |
| Ranked pairs | 98.0 (98.8) | 97.1 (98.2) | 95.3 (97.1) |
| STAR | 97.7 (98.6) | 95.4 (97.1) | 89.4 (93.4) |
| Score | 95.4 (97.1) | 92.1 (95.1) | 88.7 (93.0) |
| Borda | 94.5 (96.6) | 90.5 (94.1) | 80.2 (87.7) |
| MJ | 93.1 (95.7) | 93.4 (95.9) | 92.1 (95.1) |
| MAV | 92.0 (95.0) | 92.8 (95.5) | 91.3 (94.6) |
| 321 | 91.6 (94.8) | 91.5 (94.7) | 87.2 (92.1) |
| Approval | 88.8 (93.1) | 89.6 (93.5) | 87.6 (92.3) |
| IRV | 86.3 (91.5) | 84.4 (90.3) | 77.0 (85.7) |
| Plurality | 58.3 (74.1) | 65.8 (78.8) | 64.9 (78.2) |

- **The front: Schulze alone** with half the voters strategic (VSE\* 97.6 / WME\* 98.9), Schulze and MJ with all strategic. Ranked pairs, equal to Schulze with honest ballots, loses more to strategy (95.3 / 95.2 with all strategic, against 97.1 / 97.5); with 40 voters, a tenth of the elections end in a tie the method breaks at random. STAR, undominated here in version 1.0.0, is now dominated: by Schulze and Ranked pairs with half the voters strategic, by Schulze and MJ with all.
- **Strategy now costs the methods vse-sim finds vulnerable.** With all voters strategic, STAR loses 8.3 points of VSE, Score 6.7, Borda 14.3 and IRV 9.3, and STAR's and Score's WME fall by 12 and 10 points (on Score ballots 57% of the voters exaggerate). In version 1.0.0 strategy cost these methods at most a few points and improved IRV.
- **The median-based methods hold best among the rated ones**: MJ and MAV lose about 1 point of VSE with all voters strategic, and MJ is undominated there.
- **Plurality gains** from strategy (honest 58.3, strategic about 65), as everywhere: its voters desert hopeless candidates.
- **Against vse-sim's own figures.** vse-sim's summary page ([Voter Satisfaction Efficiency (VSE) summary](https://electionscience.github.io/vse-sim/VSEbasic)) gives ranges without stating the settings behind them: Plurality around 75%, Approval 89–95%, 3-2-1 92–95%, STAR up to 98%, Score 96% down to 84% under strategy, Condorcet methods 98% at the top and Ranked pairs as low as 86% under strategy, IRV about 87% with honest voters down to about 60% with all strategic, median-based methods up to 92%, Borda below 0% under strategy.
  - **The honest values agree** in order and roughly in level, as in version 1.0.0: Plurality 74.1, Approval 93.1, STAR 98.6, Score 97.1, Condorcet methods 98.8. The lab gives IRV (91.5) and MJ (95.7) somewhat more than vse-sim.
  - **The strategic values now point the same way**: strategy costs Score, STAR, Borda and IRV, as in vse-sim, though less (Score 93.0 against vse-sim's 84, IRV 85.7 against about 60, Borda 87.7 against below 0). The Condorcet methods lose least here and much less than in vse-sim (Ranked pairs 97.1 against 86). In vse-sim each method has its own strategic ballot, cast whether or not it gains the voter anything; the lab's voters make a move only if it pays on the polls, with the method's own count.
- So the two simulators agree on the electorates and honest voting, and on which methods strategy hurts; they differ in how much, most of all for the Condorcet methods.

## 10 Discussion

### 10.1 Strategic Nomination Is the Normal Case

In the comparison, strategic nomination is on in 41 of the 44 conditions, and the no-nomination test shows what it does. In real elections it is not optional: parties and candidates always decide who runs, knowing the voting method.

- **It moves the rated methods and the score–pairwise hybrids along the front**, trading 2–3.5 points of VSE for as much WME (section 6.3). The rated methods' lead in VSE, the most visible finding of version 1.0.0, belonged to a fixed field.
- **It costs Score runoff WME** through clones (0.22 per election in the base), as in version 1.0.0. A runoff between a front-runner and their own clone protects nothing, costs a second election and suggests a majority check that did not take place.
- **It helps the plurality-based methods, TTR and Approval** by 8–10 points of WME: spoilers withdraw, which is what parties do under Plurality.
- **The Condorcet methods are almost immune**: their result changes in about 3% of the elections.

The lab's clones cost their party nothing, while a real clone needs a second credible candidate and splits a campaign, so the test may overstate the effect of clones. Exits, in contrast, are cheap in reality too.

### 10.2 The Strategy Model Decides What Full Strategy Can Do

With adaptive voters, the three strategy engines agree on the order of the good methods. With all voters strategic they do not (section 5.10): under pooled discovery the Condorcet methods hold best (WME\* about 97.4), under the ranking groups the Soft Minimax family does (burial costs the Condorcet methods 4 points), and under the arithmetic the Condorcet methods fall to WME\* 91 through compromise and push-over, moves whose effect on the pairwise counts the arithmetic misjudges.

Pooled discovery is the most defensible of the three. It tests each move by counting the published polls again with the method, which anyone who knows the method can do, and it needs no special rule for any method. But it is not the only possible picture of real voters:

- Real voters may follow simpler heuristics, closer to the arithmetic, or the advice of campaigns, which the lab models only with the arithmetic (section 5.8).
- Pooled discovery groups voters by their order of the poll's top three and tries only the obvious moves; organised campaigns could find others, as the ranking groups' burial shows.
- With 5 polls per round voters overreact to lucky estimates (section 5.5). That is deliberate, since methods must withstand noisy information, but it is what hurts IRV and Borda most, and it reverses IRV's place against TTR (section 4.12).

The safest reading: the Condorcet methods are robust against strategy that voters find by checking it on the polls. They are vulnerable to burial coordinated in large groups, and to voters who misjudge the pairwise counts in a way that happens to harm them. Soft Minimax and Soft Minimax-D have the best worst case across the three engines (WME\* at least 95.2).

### 10.3 Why Soft Minimax-D Does Well

Soft Minimax-D was proposed in this lab, so its standing needs a reason that does not come from the simulations alone.

The Soft Minimax family is Minimax on soft margins. For a pair of candidates, each ballot's score difference counts in proportion up to a clip and as a full vote beyond it; the candidate whose worst soft margin is best wins. With a clip of one point on the 0–5 scale every difference is a full vote and the method is Minimax on the score ballots; with a clip as large as the whole range it is Score. Soft Minimax and Soft Minimax-40 use a fixed clip (20% and 40% of the range) and lie between the two. Soft Minimax-D sets the clip per pair: one point plus the difference of the two candidates' average scores.

That rule allocates the conflict between the two goals pair by pair. Electing the winner of a head-to-head majority instead of the candidate with the higher score total costs total utility in proportion to the difference of their average scores. Where two candidates have about the same average score, the clip is about one point, the pair is decided by majority as in Minimax, and following the majority costs almost nothing. Where one is clearly better liked on average, the clip is wider, the pair is decided more by how much voters prefer one, as in Score, and the majority is overruled only where overruling it saves much total utility. A fixed clip weighs every pair the same, so it gives up majority support where it gains little and total utility where it costs much; that is why Soft Minimax-D does better than an interpolation between Minimax and Score (it dominates Soft Minimax-40 in 23 of 42 conditions, Soft Minimax-40 dominates it in one, with laziness 0.5; Soft Minimax-40 has the higher VSE\* in 16).

This argument does not depend on the electorate model: it only uses that total utility and the average score difference go together, which holds as far as scores reflect utilities. Still, Soft Minimax-D was chosen among four clip rules (Soft Minimax, Soft Minimax-40, Soft Minimax-L, Soft Minimax-D) with these simulations, so part of its lead may be fitted to the model. Its lead holds in most of the electorate tests (8 of 14 conditions, at every number of candidates), which speaks against a narrow fit; other simulators and real ballot data would be the stronger test.

### 10.4 The Front Is Close to Trivial Rules

In the base, a lottery between two trivial ideal rules, the Condorcet winner else a random ballot and its mirror on the utility side, comes within about one point of the best methods on both axes (section 4.6). Several respected methods (Score, MJ, MAV, 321, Approval runoff) are no better than such a lottery once voters are strategic.

This does not make the methods pointless. The trivial rules need the sincere preferences, which no ballot delivers, and they are not proposals. What the measure shows is how little room there is between the best results ballots allow and a coin among simple ideals, in the base's electorates. The room grows where the trivial rules are poor: with central candidates the good methods are 3–6 points ahead (section 7.3), in the vse-sim setting 8–10 (section 9), with 10 candidates about 3 (section 7.5). The value of a good method thus lies less in its average than in how it holds up where the simple answers fail.

The equivalent jury size says the same in another unit: the Condorcet methods do as well as an ideal jury of 84–89 of the 99 voters, the Soft Minimax family as about 70–74, Plurality as 17 (section 4.6).

### 10.5 What the Front Does Not Measure

The front weighs total satisfaction, majority support and sincerity, and nothing else. Several properties that matter for a real choice are not measured:

- **Simplicity** for voters (filling in the ballot), for officials (counting, recounts, adding up results from polling stations) and for the public (understanding why the winner won).
- **The cost of a second round**: money, time, and turnout that may differ between rounds; the lab's runoffs assume the same voters return and rank sincerely.
- **Strategy in elections that repeat**, beyond the one test of a first poll from a previous election.

Simplicity in particular changes the picture. Approval is never near the front, but with a ballot almost as simple as Plurality's it dominates Plurality in 42 of 43 conditions (means over the conditions 84.0 / 88.8 against 73.1 / 73.6). Several methods of the front are hard to explain: Soft Minimax-D, Kemeny-Young, ESR-MARS. Among the simpler ones, a few pairs show that a simple version loses nothing: IMDS (scores maxed at the top and struck at the bottom, countable by hand) gives the same results as INS, Smith//Approval (the voter's own approval cutoff as tie-break) as Smith//Score, and BTR-score (a bottom-two runoff) nearly the same as Smith//Score (section 4.12). A method one step behind the front but much simpler may be the better practical choice; a measure of simplicity would be needed to put this on the front.

### 10.6 Limits of the Model

- **Electorates.** The electorates are spatial. Candidates differ only in their positions, not in competence or charisma, and the electorates are small (40–199 voters). How strongly total satisfaction and majority support conflict depends on the utility shape and on who stands (section 7), for which the evidence is thin.
- **Candidates.** Candidates are voters chosen among the electorate; who stands is modelled only by a tilt towards the centre, the edges or one side. Nomination is strategic, but clones cost nothing (10.1).
- **Strategy.** Voters find their moves by pooled discovery and decide by value against a personal cost (calibrated with the arithmetic on surveys of plurality elections); they coordinate within their type by an assumed share (0.25), and without coordination nobody votes strategically at all (section 5.2). Campaigns exist only with the arithmetic. There is no learning across elections beyond the first-poll test.
- **Ballots.** Score ballots run 0–5 in most conditions, which shapes the hybrids' results (section 6.2).
- **Statistics.** Dominance calls are statistical. Where a difference is borderline, a different bootstrap draw can turn a dominance into a tie, so single dominance counts are uncertain by about one condition. The tests have 2000 elections against the base's 4000, so their own orders agree with the base's a little less from noise alone (the second noise row of the rank stability tables).

## 11 Outlook

Several questions are left for later versions of the lab:

- **The strategy model needs another look.** Under the default conditions Approval hardly moves, while one would expect strategic approval voting to bring it closer to the Condorcet winner, even with only part of the voters strategic.
- **Strategy without coordination.** Without coordination nobody votes strategically in the model (section 5.2). Real voters do it anyway, even when they cannot expect their vote to matter: some naive burial on ranked ballots, for instance, with no reason to expect a gain. As in the beauty contest game, voters with limited time for thinking have to be modelled as acting partly irrationally; doing that well needs care.
- **Campaigns with pooled discovery.** Campaigns now need the arithmetic response (section 5.8). With pooled discovery the picture would be clearer, and campaigns that plan better would have more effect.
- **The reference rules without nomination.** The reference rules go through strategic nomination like every method, which moves the random ballot to VSE −11 (section 4.7). Exempting them would keep the zero of the scale exact.
- **A brute-force reference method.** Take the winners of all methods as a new field and hold a new election among them under all methods; repeat until the set no longer changes, then drop the candidates with the fewest wins (or break the tie otherwise). Where the methods agree, this finds what they agree on.
- **More winning sets in the composition pipeline**, such as the Schwartz set, CDTT and the Banks set, and intermediate steps such as MJ's median winners, so that the methods built on them can be compared independently of their tie-breakers.
- **A realistic finer score range.** The score range changes the results across much of the field (section 6.2). The next runs should include 0–9 again, a range that real ballots can use, and the findings should state the score range's role with every comparison of score methods.

## 12 Conclusion

- **The good methods are close.** About a dozen methods are within 1 point of the front (on VSE\* and WME\* at once) in nearly every condition, and in the base the front is only about one point ahead of a lottery of trivial ideal rules. Choosing among them is mostly choosing a position on the trade-off between total satisfaction (VSE) and majority support (WME), and choosing how a method holds up where simple answers fail.
- **No method is undominated everywhere.** Each family has a weak spot:
  - the Condorcet methods on total satisfaction where the goals conflict strongly (quadratic utility, many or spread-out candidates), and under full strategy with voters who bury in groups or misjudge pairwise counts;
  - the Soft Minimax family under full strategy (2–4 points of WME) and where the goals hardly conflict;
  - Score runoff with clones, with lazy ballots and against Soft Minimax-D;
  - the rated methods with strategic nomination and with strategy; Score is undominated in none of the 44 conditions.
- **The Condorcet methods sit at the corner of what any rule can reach** and are undominated in about two thirds of their conditions. Their top place on WME holds partly by construction of the measure (section 4.1); what the runs add is how well they keep it. Few voters find a strategy that pays against them (1.3–1.4% in the base, against 9% under Score), they are almost immune to strategic nomination, and with all voters strategic they hold best as long as voters check their moves on the polls.
- **Soft Minimax-D is the most robust score–pairwise method**: undominated in 69% of its conditions, at every number of candidates and in 8 of the 14 electorate conditions, with the highest worst-case WME\* of the front's methods together with Soft Minimax. Its rule, deciding a pair by majority where the two candidates are about equally liked and more by scores where they are not, gives a reason for this beyond the simulations (section 10.3), though it was chosen here among four variants.
- **Score runoff has lost its place at the top** but is within 1 point of the front everywhere and has the highest worst-case VSE\* of all methods.
- **The rated methods' VSE lead belonged to a fixed field.** Strategic nomination costs them VSE and gives them WME instead, and strategy costs them most of what is left of their lead.
- **The score range shapes the middle of the front.** On the default 0–5 scale the score–pairwise hybrids lie between the Condorcet and the rated methods, because ties on the coarse scale hide part of the voters' pairwise preferences. On 0–99, BTR-score, Smith//Score and Soft Minimax-D match the Condorcet methods' WME\* at the same or more VSE\*; Soft Minimax and Score runoff stay in between. A simple design choice of the ballot thus changes the comparison over much of the field; a realistic finer range (0–9) is the next thing to test.
- **For the good methods, the amount of strategy matters, not its timing or information.** The weak methods, IRV and Borda above all, depend on how good the voters' information is and on the strategy model.
- **The most consequential assumption is how strongly VSE and WME conflict**, set by the shape of utility and by who stands. In the base, electing the Condorcet winner costs 15% of VSE; under linear utility 2.5%, in the vse-sim setting 1.1%, with unrepresentative fields 37–45%.
- **Plurality and the methods that read only first preferences or approvals are off the front in every condition**, and break down with central candidates. Approval dominates Plurality in 42 of 43 conditions for the least added complexity; the plurality-based ladder now runs Plurality → Supplementary vote → TTR → IRV, and BTR-score, which eliminates by pairwise runoffs and is no harder to count than IRV, dominates IRV everywhere.
- **In the vse-sim setting, the honest results agree with vse-sim's published figures, and the strategic ones now point the same way**, though strategy costs much less here, the Condorcet methods least of all.

The simulations narrow the choice to a group of good methods and show what each trades away. The choice within that group depends on values the front does not contain: how to weigh total satisfaction against majority support, and how much complexity and cost a method may have.

## Note on AI Use

The lab and this report were made by Hiveism together with Claude, an AI model by Anthropic, in conversation. Hiveism set the questions and the direction, decided the model's assumptions and the tests, ran the simulations and reviewed the results and the text. Claude wrote most of the code, the documentation and the drafts of this report, and checked its statements against the analysis reports. The numbers come from those reports, which anyone can make again with the commands in [reference-runs.md](reference-runs.md); the interpretations are the authors' and may contain errors.

## Appendix: Notes on the Collection

- **The collection** (`comparison/`): 24 run or sweep folders with 43 results files, simulator and analysis version 2.1.0, 44 conditions and 71 methods; 1000 bootstrap replicates per condition.
- **The default run** has the same parameters and seed as the base, and its 200 elections are the base's first 200. `compare.py` therefore counts it as part of the base condition (c1, 4000 elections); section 4.11 reads its own report.
- **Tests against the base.** The tests have 2000 elections, the base's first 2000; the base has 4000. Their own reports compare them with the base by rank stability, with a second noise row for runs of 2000 elections (new in version 2.1.0); the findings compare values with the honest values of the same 2000 elections.
- **The zero of VSE and strategic nomination.** VSE and WME are judged on the original field, and the reference rules go through strategic nomination like every method. Under the random ballot every candidate gains by fielding clones, so the reference rule Random ballot scores VSE −11 rather than 0 (section 4.7). The zero point of the scale is the random ballot on the original field and is not affected.
- **Bootstrap draws.** Each condition's bootstrap replicates depend only on its elections and the seed, in `compare.py` and `analyze.py` alike, so the comparison and each run's own report agree on every dominance, and adding runs to the collection does not change the other conditions' results.
