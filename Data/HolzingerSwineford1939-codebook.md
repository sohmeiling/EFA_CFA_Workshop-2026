# Codebook: `HolzingerSwineford1939.csv`

## 1. Dataset overview

| Characteristic                  | Description                                                                 |
| -------------------------------- | ---------------------------------------------------------------------------- |
| Dataset name                     | `HolzingerSwineford1939`                                                     |
| Original source                  | Holzinger, K. J., & Swineford, F. (1939). *A study in factor analysis: The stability of a bi-factor solution.* Supplementary Educational Monograph, no. 48. Chicago: University of Chicago Press. |
| R package                        | `lavaan` (`data(HolzingerSwineford1939, package = "lavaan")`) — this CSV is a direct export of that subset |
| Sample                           | 301 seventh- and eighth-grade pupils from two Chicago schools (Pasteur and Grant-White) |
| Number of variables              | 15 (6 identifier/demographic variables + 9 mental-ability test scores)      |
| Intended constructs              | Three correlated cognitive-ability factors: visual perception, verbal/textual ability, processing speed |
| Missing data                     | None in this classic teaching subset                                        |
| Primary teaching uses            | The "hello world" example for **confirmatory factor analysis** in `lavaan`; also usable for EFA/PCA demonstrations |

```r
library(readr)
library(dplyr)

hs <- read_csv("HolzingerSwineford1939.csv")

glimpse(hs)
```

Note that the full original Holzinger–Swineford (1939) study administered 26 tests; the version distributed with `lavaan` (and reproduced here) retains only 9 of those tests, selected because they load cleanly onto three factors. This is the version almost universally used in CFA/SEM teaching materials, including Rosseel's `lavaan` tutorial.

## 2. Identifier and demographic variables

| Variable | Description                                                              |
| -------- | -------------------------------------------------------------------------- |
| `id`     | Pupil identification number (not sequential; not a substantive variable)   |
| `sex`    | Pupil sex: `1` = male, `2` = female                                        |
| `ageyr`  | Age in completed years at testing                                          |
| `agemo`  | Additional age in completed months (use with `ageyr`, e.g. age in years ≈ `ageyr + agemo/12`) |
| `school` | School attended: `Pasteur` or `Grant-White`                                |
| `grade`  | School grade level (7 or 8)                                                |

`id`, `sex`, `school` should be read as identifier/grouping variables rather than analysis items. `school` is commonly used to demonstrate measurement invariance / multi-group CFA, since the two schools differ somewhat in ability level.

## 3. Cognitive-ability test scores (`x1`–`x9`)

All nine variables are continuous test scores (not raw item responses), already scaled by the original authors. They are conventionally grouped into three latent factors for CFA:

### Visual perception factor

| Variable | Test name                    |
| -------- | ------------------------------ |
| `x1`     | Visual perception               |
| `x2`     | Cubes                            |
| `x3`     | Lozenges                         |

### Textual / verbal factor

| Variable | Test name                    |
| -------- | ------------------------------ |
| `x4`     | Paragraph comprehension          |
| `x5`     | Sentence completion              |
| `x6`     | Word meaning                     |

### Speed factor

| Variable | Test name                                                    |
| -------- | --------------------------------------------------------------- |
| `x7`     | Speeded addition                                                  |
| `x8`     | Speeded counting of dots                                          |
| `x9`     | Speeded discrimination of straight and curved capitals            |

## 4. The classic three-factor CFA model

This is the standard model fitted to this dataset in `lavaan` (and reproduced in most CFA teaching materials, including Rosseel, 2012):

```r
library(lavaan)

HS.model <- '
  visual  =~ x1 + x2 + x3
  textual =~ x4 + x5 + x6
  speed   =~ x7 + x8 + x9
'

fit <- cfa(HS.model, data = hs)
summary(fit, fit.measures = TRUE, standardized = TRUE)
```

For an EFA/PCA demonstration prior to CFA, students can use `x1`–`x9` only (dropping the six identifier/demographic columns) with `psych::fa()` or `stats::princomp()`, and then compare the extracted structure to the confirmatory three-factor model above.

## 5. Practical notes for the workshop

- Because this dataset is small (*N* = 301) and the three-factor structure is very clean, it is a useful low-noise example to introduce CFA syntax and fit indices before moving to the messier `bfi` and DASS datasets.
- `school` provides a ready-made grouping variable for a follow-up demonstration of configural/metric/scalar invariance testing, if time in the tutorial allows.
- Because `x1`–`x9` are already composite scores rather than raw items, do not treat them as ordinal (i.e. do not fit them with `ordered()`/WLSMV); treat them as continuous and use maximum likelihood estimation.
