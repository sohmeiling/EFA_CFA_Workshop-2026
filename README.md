# EFA & CFA Workshop 2026

Guest lecture for MCP5143 Psychometric Testing (UoC students).

## Readings

- Main reading: Learning Statistics with jamovi — Chapter 15: Factor Analysis
- Supplementary reading: Learning Statistics with R

## Data

Data taken from: davidfoxcroft/lsj-data

Data for this lecture is inside the `Data` folder.

## Slides

https://sohmeiling.github.io/EFA_CFA_Workshop-2026/Slides/01-pca-efa-lecture-slides.html

## Software

Download jamovi here: https://www.jamovi.org/download.html

## Overview

This repository contains teaching materials for a workshop on exploratory factor analysis (EFA), confirmatory factor analysis (CFA), and related dimensionality-reduction concepts such as principal component analysis (PCA).

## Repository structure

- `Data/` — CSV and codebook files used in examples and exercises
- `Notes/` — written lecture notes and background explanations
- `Slides/` — Quarto/RevealJS slide decks for the workshop
- `Textbooks/` — reference materials and supporting reading

## Topics covered

- PCA vs. factor analysis
- exploratory factor analysis (EFA)
- factor rotation and interpretation
- latent variables and common variance
- confirmatory factor analysis (CFA)
- applied data analysis workflows in psychometrics

## How to use this repository

### View notes

Open the Markdown files in `Notes/` directly in VS Code or another editor.

### Render slides

If you have Quarto installed, you can render the slide files locally. For example:

```bash
quarto render "Slides/01-pca-efa-lecture-slides.qmd"
```

You can also open the `.qmd` files directly in a Quarto-enabled editor.

## Notes on project workflow

This project is primarily a source-materials repository. Generated outputs such as HTML render files and `_files/` directories are intentionally ignored by Git to keep the repo clean and focused on the source content.

## Suggested workflow

1. Edit the source `.qmd` and `.md` files.
2. Render locally to preview results.
3. Check `git status` before committing.
4. Commit only intended source files.
5. Push changes to the remote repository.

## Licensing and usage

Use these materials for teaching and learning within the workshop context. If you plan to reuse the materials beyond the workshop, review any local institutional or third-party licensing expectations before distributing them.
