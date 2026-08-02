# ml-05-ensembles

[![Workflow Guide](https://img.shields.io/badge/Pro--Guide-pro--analytics--02-green)](https://denisecase.github.io/pro-analytics-02/workflow-b-apply-example-project/)
[![Python 3.14](https://img.shields.io/badge/python-3.14%2B-blue?logo=python)](./pyproject.toml)
[![MIT](https://img.shields.io/badge/license-see%20LICENSE-yellow.svg)](./LICENSE)

> Professional Python project: combining models with ensemble methods.

## Project Description

This project compares a single decision tree against two ensemble models on a
medical insurance dataset of 1,338 individuals.

The data holds age, sex, bmi, children, smoker, region, and charges. Charges was
binned into four quartile classes labeled low, moderate, high, and very high,
turning a regression problem into a four-class classification problem. The models
predict which cost class an individual falls into from the remaining features.

Three models were fit: a decision tree limited to depth 3 as a baseline, a random
forest of 200 trees, and a soft-count voting classifier combining a decision tree,
a support vector machine, and a neural network. Each was fit twice, once on eight
base features and once with an added smoker_bmi interaction feature, using the
same train and test split both times.

Links:

- [ml_05_ensembles.ipynb](notebooks/ml_05_ensembles.ipynb) - example notebook
- [ml_05_ensembles_gracecode42_modification.ipynb](notebooks/ml_05_ensembles_gracecode42_modification.ipynb) - Phase 4 notebook, predicting `sex` instead of `species`
- [ml_05_ensembles_gracecode42_project.ipynb](notebooks/ml_05_ensembles_gracecode42_project.ipynb) - Phase 5 custom project
- [docs/index.md](docs/index.md) - full write-up

## Command Reference

<details>
<summary>Show command reference</summary>

### In a machine terminal (open in your `Repos` folder)

After you get a copy of this repo in your own GitHub account,
open a machine terminal in your `Repos` folder:

```shell
# Replace username with YOUR GitHub username.
git clone https://github.com/gracecode42/ml-05-ensembles

cd ml-05-ensembles
code .
```

### In a VS Code terminal

These are listed for convenience.
For best results, follow the detailed instructions in
[pro-analytics-02 guide](https://denisecase.github.io/pro-analytics-02/).

```shell
uv self update
uv python pin 3.14
uv lock --upgrade
uv sync --extra dev --extra docs --upgrade

uvx pre-commit install
uvx pre-commit autoupdate

git add -A
uvx pre-commit run --all-files
# repeat if changes were made
uvx pre-commit run --all-files

# run the example module to verify the environment (.venv/)
uv run python -m mlstudio.app_case

# run common chores
uv run ruff format .
uv run ruff check . --fix
uv run python -m pyright
uv run python -m pytest
uv run python -m zensical build

# save progress
git add -A
git commit -m "update"
git push -u origin main
```

</details>

## Findings and Visuals

All models were scored on a stratified test set of 268 instances. Random guessing
on four balanced classes scores 0.25.

| Model | Features | Test Accuracy | Test F1 | Accuracy Gap | Errors |
|---|---|---|---|---|---|
| single_tree | base | 0.8396 | 0.8397 | 0.0128 | 43 |
| random_forest | base | 0.8619 | 0.8599 | 0.1016 | 37 |
| voting | base | 0.8694 | 0.8677 | 0.0801 | 35 |
| single_tree | +smoker_bmi | 0.8358 | 0.8358 | 0.0156 | 44 |
| random_forest | +smoker_bmi | 0.8769 | 0.8748 | 0.0829 | 33 |
| voting | +smoker_bmi | 0.8731 | 0.8713 | 0.0717 | 34 |

The random forest with the smoker_bmi interaction performed best at 0.8769 test
accuracy, against 0.8396 for the single decision tree on the base features. That
is 33 misclassified individuals instead of 43. Both ensembles beat the single tree
on both feature sets, though by modest margins.

![Test accuracy by model, with and without the smoker_bmi interaction](./docs/images/model_comparison.png)

Feature importances rank how much the random forest relied on each feature. Age
leads at 0.505. Adding smoker_bmi drew weight almost entirely from smoker_yes,
which fell from 0.256 to 0.118.

![Random forest feature importances, with and without smoker_bmi](./docs/images/feature_importances.png)

## Project Documentation

Full project write-up:

[docs/index.md](docs/index.md)

## Citation

[CITATION.cff](./CITATION.cff)

## License

[MIT](./LICENSE)
