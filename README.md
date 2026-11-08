# Ballot Front Finder Lab: Reference Runs

Results of the reference runs of the [Ballot Front Finder Lab](https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab) 2.1.0, a simulator of single-winner voting methods under poll-based strategy.

- **[findings.md](findings.md)**: what the runs show. Start here.
- **[reference-runs.md](reference-runs.md)**: the commands that made the runs and their analyses.
- **lab-commit.txt**: the commit of the lab used for all runs.
- **One folder per run**: its `command.txt` and its analysis folder (`report.md`, `summary.csv`, plots). The `defaults` run is complete; the other runs' `elections.csv` files are left out for size (about 6.5 GB in total).
- **comparison/**: all methods over all conditions (`compare.py`), with the method map and the pairs discussed in the findings.
- **overview-\*/**: combined analyses.
- **electorates/**: the electorates of the models without any method (`show_electorates.py`): example electorates, their dimensionality and how costly electing the Condorcet winner is.

Every election depends only on its parameters, seed and number, so the commands in reference-runs.md, run with the lab at the recorded commit, recreate the full results.

The results and findings of version 1.0.0 are in the release [v1.0.0](https://github.com/CasimirKaPazi/Ballot-Front-Finder-Lab-Results/releases/tag/v1.0.0).

findings.md can be turned into a PDF with pandoc, from this folder so that the figures are found:

```
pandoc findings.md -o findings.pdf --pdf-engine=xelatex
```

## Authors and AI Use

The lab and these findings were made by Hiveism together with Claude, an AI model by Anthropic, in conversation. Hiveism set the questions and the direction, decided the model's assumptions and the tests, ran the simulations and reviewed the results and the text. Claude wrote most of the code, the documentation and the drafts of the findings, and checked their statements against the analysis reports. The numbers come from those reports, which the commands in reference-runs.md recreate; the interpretations are the authors' and may contain errors.

## License

The results and texts in this repository are licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). The lab itself is free software under the GNU GPL v3 (see its repository).
