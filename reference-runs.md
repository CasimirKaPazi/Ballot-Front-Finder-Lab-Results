# Reference Runs

A fresh set of runs with the released version, for comparison and as the base for new tests: the defaults, a large base, the main robustness tests and a vse-sim-like setting. This file says how to make them again. It can also serve as the README of a repository of the results.

The set is chosen to show how robust the findings are, not to tune the model: the tests that only served to set the model's defaults and the strategy engine's parameters (noise strength, dimensions, poll samples, poll error, search, aggressive share and the like; see [journal.md](journal.md)) are not repeated.

## Before Starting

- **One commit of the lab for everything.** Run all commands from the lab's folder at one commit and record it with the results:

  ```
  R=../bffl-runs                            # where the results go (a folder of its own)
  mkdir -p $R && git rev-parse HEAD > $R/lab-commit.txt
  python test_methods.py && python regression.py
  ```

  Every results row also records the simulator version and a fingerprint of each method's results ([running.md](running.md), Safety), so `compare.py` can tell later whether runs belong to the same code.
- **Add-ons.** Move the test add-ons out of `addons/` (keep `ids.py` and `_template.py`), so the runs contain only the lab's methods.
- **Methods.** The tests use the set `extended`: the standard set and the strongest newer methods (ESR, DMS, Geometric median, Soft Minimax, Soft Minimax-40, MARS-T; 35 methods), so every test shows them in all conditions. The default run uses the plain standard set, as a user gets it.
- **The same elections everywhere.** All runs use seed 1, so a test that changes only strategy or ballots faces exactly the electorates of the base, and its first 1000 elections are the base's first 1000.
- **Time it first.** Run the base with `--elections 20`, and scale: time grows with the elections, the methods, the candidates and the poll rounds. As a guide, the cost of each item is given below in **units**: one unit is 1000 elections of the extended set at the defaults (about the time of `--elections 20` × 50).

## The Runs

```
# 1. Reference                                                            units
python run.py --out $R/defaults                                          # 0.2   200 elections, standard set
python run.py --elections 2000 --methods extended --manipulation --out $R/base           # 2.5
python run.py --elections 2000 --methods all --exclude extended references inverse \
    --out $R/base-rest                                                   # 3-4   every other method, same elections
python sweep.py --model ks-quinn --utility linear --strategic 0.5 1 --adaptive 0 --rounds 1 \
    --voters 40 --elections 15000 --max-score 10 --methods 321 STAR Score Approval "Ranked Pairs" \
    Schulze IRV MJ MAV Borda Plurality --out $R/vsesim                   # ~5

# 2. Strategy
python sweep.py --strategic 0.5 1 --adaptive 0 --elections 1000 --methods extended --out $R/strategic-share  # 2
python sweep.py --aggressive 0 1 --elections 1000 --methods extended --out $R/aggressive-kind                # 2
python sweep.py --rounds 1 2 4 5 6 --elections 1000 --methods extended --out $R/rounds                       # 2 (one run)
python run.py --start previous --elections 1000 --methods extended --out $R/lockin                           # 1

# 3. Ballots and voters
python sweep.py --lazy 0.1 0.5 --elections 1000 --methods extended --out $R/laziness                         # 2
python sweep.py --max-score 9 99 --elections 2000 --methods rated hybrid median-based "Smith//Score" "Score runoff" --out $R/max-score                      # 4
python run.py --exits 1 --clones 2 --elections 1000 --methods extended --out $R/nomination                   # 1.5

# 4. Electorates
python sweep.py --model clusters field ks --elections 1000 --methods extended --out $R/models                # 3
python sweep.py --width 1.4 2.8 --elections 1000 --methods extended --out $R/width                           # 2
python sweep.py --utility linear quadratic --elections 1000 --methods extended --out $R/utility              # 2
python sweep.py --cand-dispersion 0.5 2 --elections 1000 --methods extended --out $R/cand-dispersion         # 2
python sweep.py --cands 4 5 7 8 9 10 --elections 700 --methods extended --out $R/candidates                  # 5.5
python run.py --samples 5 --elections 1000 --methods extended --out $R/samples                               # 0.5
python run.py --samples 5 --poll-size 9 --elections 1000 --methods extended --out $R/poll-size               # 0.5
python run.py --voters 49 --elections 1000 --methods extended --out $R/voters                                # 0.5

# 5. Combined
python run.py --exits 1 --clones 2 --lazy 0.1 --poll-size 9 --cand-dispersion 0.5 --elections 1000 \
    --methods extended --out $R/hard                                     # 1.5
```

About 45 units in all. If that is too long, these come first (about 20 units): the defaults, the base, strategic share, the utility fall-off (width and utility), models and the vse-sim setting; the rest can follow. Every run can be interrupted and continued with the same command plus `--resume`; the same way, a finished run can be extended with a larger `--elections`.

What each part answers:

| Part            | Question                                                                                                                                                                     |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| defaults        | what a user gets with `python run.py`: the example to compare one's own runs with                                                                                            |
| base, base-rest | every method on the same 2000 elections at the defaults, with the manipulation test; the reference for all tests                                                             |
| vsesim          | close to vse-sim: Quinn's hierarchical model as in its code, linear utility, 40 voters, scores 0–10, a fixed share of strategic voters answering one poll; vse-sim's methods |
| strategic-share | a fixed share of always-strategic voters (half, all) instead of adaptive voters                                                                                              |
| aggressive-kind | all strategic voters kind (exaggerate only for a clear gain) or all aggressive (whenever it cannot hurt), instead of half and half                                           |
| rounds          | one, two, four, five and six poll rounds (three in the base), all from one run: does the result settle or drift?                                                             |
| lockin          | the first poll from a previous election's strategic result: can the electorate leave a strategic state?                                                                      |
| laziness        | truncated rankings, fewer approvals and scores                                                                                                                               |
| max-score       | the score range 0–5 against 0–9 and 0–99: how much the coarse scale costs                                                                                                    |
| nomination      | candidates may withdraw or field clones                                                                                                                                      |
| models          | three other electorate models: clusters, field and ks (Quinn's model as in vse-sim's code is in the vsesim setting)                                                          |
| width, utility  | how quickly utility falls with distance: the default (mode-like), wider (towards linear), linear (median) and quadratic (mean)                                               |
| cand-dispersion | candidates clustered in the centre or spread out, unlike the voters                                                                                                          |
| candidates      | 4, 5, 7, 8, 9 and 10 candidates (6 in the base), 700 elections each                                                                                                          |
| samples         | fewer polls per round (5 instead of 10): less poll information                                                                                                               |
| poll-size       | polls of 9 voters instead of 49 (5 per round): much less poll information; compared with the samples run, which differs from it only in poll size                            |
| voters          | 49 voters instead of 99                                                                                                                                                      |
| hard            | several hard assumptions at once: candidates clustered in the centre, some laziness, strategic nomination and small polls                                                    |

## Analysis

```
for t in strategic-share aggressive-kind rounds lockin laziness nomination models width utility cand-dispersion candidates samples voters hard; do
    python analyze.py $R/base $R/$t                   # -> $R/$t/analysis-vs-base/
done
python analyze.py $R/samples $R/poll-size              # -> $R/poll-size/analysis-vs-samples/
python analyze.py $R/defaults                         # -> $R/defaults/analysis/
python analyze.py $R/base $R/base-rest --out $R/overview-all-methods
python analyze.py $R/base $R/base-rest $R/max-score --show rated hybrid median-based "Smith//Score" "Score runoff" --out $R/max-score
python analyze.py $R/vsesim                           # -> $R/vsesim/analysis/
python analyze.py $R/base $R/width $R/utility --out $R/overview-utility
python compare.py $R                                  # -> $R/comparison/: every method over all conditions
```

`compare.py` counts every condition once and refuses runs of different code (fingerprints), so it should run cleanly on this folder. It uses the same bootstrap draws as analyze.py, so its dominance calls agree with the single reports. To read the vse-sim numbers against vse-sim's own, note the different zero point: the lab's VSE is 0 for a random ballot, vse-sim's for a random candidate ([analysis.md](analysis.md), Axes; the report gives a random candidate's value on the lab's scale).

## What to Publish

The results files are large: about 40 MB per 1000 elections of the extended set, more for sweeps over poll rounds (one set of rows per number of rounds) and in all one to two gigabytes. A results repository can hold this file, `lab-commit.txt`, the `defaults` run complete (its `elections.csv` is about 7 MB), and of every other run its `command.txt` and its analysis folders (`report.md`, `summary.csv`, plots), plus `comparison/` and the `overview-*` folders. With the commit and the commands here, anyone can make the full results again: every election depends only on its parameters, seed and number, so the same commands give the same numbers.

A `.gitignore` for that:

```
*/elections.csv
*/runs/
!defaults/elections.csv
```
