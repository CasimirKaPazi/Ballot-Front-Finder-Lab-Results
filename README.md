# Ballot Front Finder Lab: Reference Runs

Results of the reference runs of the [Ballot Front Finder Lab](https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab) 1.0.0, a simulator of single-winner voting methods under poll-based strategy.

- **[findings.md](findings.md)**: what the runs show. Start here.
- **[reference-runs.md](reference-runs.md)**: the commands that made the runs and their analyses.
- **lab-commit.txt**: the commit of the lab used for all runs.
- **One folder per run**: its `command.txt` and its analysis folder (`report.md`, `summary.csv`, plots). The `defaults` run is complete; the other runs' `elections.csv` files are left out for size (~1.9 Gib in total).
- **comparison/**: all methods over all conditions (`compare.py`).
- **overview-\*/**: combined analyses.

Every election depends only on its parameters, seed and number, so the commands in reference-runs.md, run with the lab at the recorded commit, recreate the full results.

The election data is not included in this repo due to the amount of data (~1.9 GiB). I can be made available if needed.

## License

The results and texts in this repository are licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). The lab itself is free software under the GNU GPL v3 (see its repository).
