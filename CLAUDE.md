# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A teaching repository for a "Python for Data Science and AI" course. The deliverable is the **notebooks**, not a library. There is no test suite, no linter config, and no build step — `main.py` and `src/python_for_ds_and_ai/` are leftover `uv init` scaffolding and are not imported by anything.

## Environment

Managed by [uv](https://docs.astral.sh/uv/), Python 3.12 (pinned in `.python-version`).

```bash
uv sync                      # create/refresh .venv from pyproject.toml + uv.lock
uv add <pkg>                 # runtime dep (goes in [project.dependencies])
uv add --dev <pkg>           # dev dep (goes in [dependency-groups].dev)
uv run jupyter lab           # run notebooks (launch from the repo root, see below)
uv run python main.py        # the placeholder entry point
```

Notebooks select the `.venv` kernel via `ipykernel`. New third-party imports must be added with `uv add`, not `!pip install` — existing `#!pip install opencv-python` lines in the notebooks are commented-out teaching hints, not the real install path.

## Layout and conventions

- `notebooks/NN_topic.ipynb` — numbered lessons, read in order (`01_python_fundamentals` → `05_scikit_learn`). `04_pandas.ipynb` is a stub and `05_scikit_learn.ipynb` is an empty 0-byte file; both are placeholders for upcoming lessons.
- `exercises/NN_topic.ipynb` — mirrors a lesson by the same number, with empty code cells for the student to fill in.
- `data/` — shared assets referenced by the lessons (`penguins.csv`, `The Odyssey.txt`, `mandril.png`, `Deep Learning - Nature.pdf`). Add new assets here rather than beside a notebook.

**Data paths are relative to the repo root** (`'data/penguins.csv'`, not `'../data/penguins.csv'`). This is a recent migration — committed notebooks up to `HEAD` still use `../data/` with a `notebooks/` working directory. Follow the root-relative form in new and edited cells, and launch Jupyter from the repo root (in VS Code that means setting `jupyter.notebookFileRoot` to `${workspaceFolder}`).

Some lesson cells deliberately mutate the working tree (`os.mkdir('models')`, writing then deleting `data/*.txt`). Running a notebook top-to-bottom is expected to leave the tree clean; if it doesn't, the notebook's own cleanup cell is missing.

## Notebook style

Match the existing lessons when adding cells:

- A markdown cell introduces each concept — `#` for a major section, `##` for a sub-topic — followed by a short bullet list or one-sentence explanation, then several small code cells that each demonstrate one thing.
- Code-cell comments use a double hash and lowercase: `## read csv`. Single `#` is reserved for commented-out code.
- Lesson prose is in **English**; exercise prompts in `exercises/` are in **Spanish**. Keep each side in its own language.
- Cell outputs are committed (they are the rendered lesson). Re-run a notebook before committing rather than stripping outputs.

## Commits

Single-line, lowercase, `feat: <lesson topic>` (e.g. `feat: file handling`). Every commit so far uses `feat:`.
