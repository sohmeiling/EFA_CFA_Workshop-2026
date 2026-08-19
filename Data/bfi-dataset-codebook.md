# Codebook: `bfi.csv`

A naming caution is worth flagging before anything else. This dataset is often called "the BFI" because that is the object name used in the R `psych`/`psychTools` packages, but it is **not** the standard 44-item Big Five Inventory (John & Srivastava, 1999). It contains 25 public-domain items from the International Personality Item Pool (IPIP), collected through the Synthetic Aperture Personality Assessment (SAPA) project. For the workshop, refer to it as **the 25-item IPIP/SAPA personality dataset distributed as `bfi`**, rather than as a shortened official BFI instrument. Full documentation: [personality-project.org/r/html/bfi.html](https://www.personality-project.org/r/html/bfi.html).

The file `bfi.csv` in this project folder is a direct CSV export of that dataset and is the file students should load in R.

## 1. Dataset overview

| Characteristic                  | Description                                                               |
| -------------------------------- | -------------------------------------------------------------------------- |
| Dataset name                     | `bfi`                                                                       |
| Source package (R)               | Originally distributed in `psych`; now maintained in `psychTools` (`library(psychTools); data(bfi)`) |
| Data-collection project          | Synthetic Aperture Personality Assessment (SAPA), Spring 2010              |
| Item source                      | International Personality Item Pool                                        |
| Sample size                      | 2,800 respondents                                                          |
| Number of substantive variables  | 28 (25 items + 3 demographic variables)                                    |
| Personality items                | 25                                                                          |
| Demographic variables            | 3 (`gender`, `education`, `age`)                                           |
| Intended constructs              | Agreeableness, conscientiousness, extraversion, neuroticism and openness   |
| Response format                  | Six-point ordered response scale                                           |
| Missing data                     | Blank cells, read into R as `NA` by default                                |
| Primary teaching uses            | PCA, EFA, CFA, reliability analysis and scale construction                 |

`bfi.csv` also contains a first column called `rownames`. This is the original SAPA respondent identifier (not a sequential row index — the values run from roughly 61,000 upward and are not consecutive). It is an ID variable, not a substantive item, and should be excluded from any analysis.

```r
library(readr)
library(dplyr)

bfi <- read_csv("bfi.csv")

glimpse(bfi)
```

## 2. Response coding

All 25 personality items use the same six-point scale:

| Value | Response              |
| ----: | ---------------------- |
|     1 | Very inaccurate         |
|     2 | Moderately inaccurate   |
|     3 | Slightly inaccurate     |
|     4 | Slightly accurate       |
|     5 | Moderately accurate     |
|     6 | Very accurate           |
| Blank | Missing response (`NA` in R) |

In R, `read_csv()` already reads blank cells as `NA`, so no explicit recoding of missingness is required. Keep the items as numeric (`dbl`) for PCA/EFA using Pearson or polychoric correlations. If an ordinal-specific CFA estimator (e.g. `WLSMV` in `lavaan`) is used later, the items would need to be explicitly declared ordered factors with `ordered()` at that point — this is a modelling decision to make at the CFA stage, not at import.

## 3. Personality-item codebook

The "Reverse" column indicates whether an item must be reverse-scored before it is combined into a scale where higher values consistently mean "more of the named trait".

### Agreeableness

Agreeableness reflects interpersonal concern, compassion, warmth and accommodation.

| Variable | SAPA item | Item wording                              | Reverse |
| -------- | --------- | ------------------------------------------ | :-----: |
| `A1`     | q_146     | Am indifferent to the feelings of others.   |   Yes   |
| `A2`     | q_1162    | Inquire about others' well-being.           |    No   |
| `A3`     | q_1206    | Know how to comfort others.                 |    No   |
| `A4`     | q_1364    | Love children.                              |    No   |
| `A5`     | q_1419    | Make people feel at ease.                   |    No   |

A higher correctly scored agreeableness score indicates greater interpersonal concern and warmth.

### Conscientiousness

Conscientiousness reflects organisation, persistence, planning and task discipline.

| Variable | SAPA item | Item wording                           | Reverse |
| -------- | --------- | ---------------------------------------- | :-----: |
| `C1`     | q_124     | Am exacting in my work.                  |    No   |
| `C2`     | q_530     | Continue until everything is perfect.    |    No   |
| `C3`     | q_619     | Do things according to a plan.           |    No   |
| `C4`     | q_626     | Do things in a half-way manner.          |   Yes   |
| `C5`     | q_1949    | Waste my time.                           |   Yes   |

A higher correctly scored conscientiousness score indicates greater organisation and persistence.

### Extraversion

Extraversion reflects sociability, interpersonal engagement, assertiveness and social confidence.

| Variable | SAPA item | Item wording                           | Reverse |
| -------- | --------- | ---------------------------------------- | :-----: |
| `E1`     | q_712     | Don't talk a lot.                        |   Yes   |
| `E2`     | q_901     | Find it difficult to approach others.    |   Yes   |
| `E3`     | q_1205    | Know how to captivate people.            |    No   |
| `E4`     | q_1410    | Make friends easily.                     |    No   |
| `E5`     | q_1768    | Take charge.                             |    No   |

A higher correctly scored extraversion score indicates greater sociability and assertiveness.

### Neuroticism

Neuroticism reflects emotional reactivity and proneness to anger, distress, mood instability and anxiety.

| Variable | SAPA item | Item wording               | Reverse |
| -------- | --------- | ---------------------------- | :-----: |
| `N1`     | q_952     | Get angry easily.            |    No   |
| `N2`     | q_974     | Get irritated easily.        |    No   |
| `N3`     | q_1099    | Have frequent mood swings.   |    No   |
| `N4`     | q_1479    | Often feel blue.             |    No   |
| `N5`     | q_1505    | Panic easily.                |    No   |

A higher neuroticism score indicates greater emotional reactivity. None of these five items is reverse-scored when the construct is labelled **neuroticism**. All five would need reversal only if the scale were instead defined as **emotional stability**.

### Openness

Openness reflects intellectual curiosity, engagement with ideas, reflection and cognitive exploration.

| Variable | SAPA item | Item wording                               | Reverse |
| -------- | --------- | -------------------------------------------- | :-----: |
| `O1`     | q_128     | Am full of ideas.                            |    No   |
| `O2`     | q_316     | Avoid difficult reading material.            |   Yes   |
| `O3`     | q_492     | Carry the conversation to a higher level.    |    No   |
| `O4`     | q_1738    | Spend time reflecting on things.             |    No   |
| `O5`     | q_1964    | Will not probe deeply into a subject.        |   Yes   |

A higher correctly scored openness score indicates greater intellectual curiosity and engagement.

## 4. Reverse scoring

Because the response scale runs from 1 to 6, the reverse-scoring formula is:

Reversed score = 7 − original score

| Original | Reversed |
| -------: | -------: |
|        1 |        6 |
|        2 |        5 |
|        3 |        4 |
|        4 |        3 |
|        5 |        2 |
|        6 |        1 |

The reverse-keyed items are:

```text
A1, C4, C5, E1, E2, O2, O5
```

Create new variables rather than overwriting the originals, so the raw responses stay available for inspection:

```r
bfi <- bfi %>%
  mutate(
    A1_R = 7 - A1,
    C4_R = 7 - C4,
    C5_R = 7 - C5,
    E1_R = 7 - E1,
    E2_R = 7 - E2,
    O2_R = 7 - O2,
    O5_R = 7 - O5
  )
```

## 5. Scale composition

After reverse scoring, the five scale scores are calculated as follows:

| Scale             | Items used                        |
| ----------------- | ---------------------------------- |
| Agreeableness      | `A1_R`, `A2`, `A3`, `A4`, `A5`     |
| Conscientiousness  | `C1`, `C2`, `C3`, `C4_R`, `C5_R`   |
| Extraversion       | `E1_R`, `E2_R`, `E3`, `E4`, `E5`   |
| Neuroticism        | `N1`, `N2`, `N3`, `N4`, `N5`       |
| Openness           | `O1`, `O2_R`, `O3`, `O4`, `O5_R`   |

### Sum scores

Each five-item scale ranges from 5 to 30 (sum of the five items).

### Mean scores

Each scale mean stays on the original 1–6 metric (sum divided by 5) and is usually easier for students to interpret.

For teaching purposes, a defensible missing-data rule is to calculate a scale mean only when at least four of the five items have a valid response, and otherwise treat the scale score as missing. This four-item rule is not part of the original `psych`/`psychTools` documentation — it is an analytic decision that should be identified as such when reported.

```r
bfi <- bfi %>%
  rowwise() %>%
  mutate(
    agree_mean = if (sum(!is.na(c_across(c(A1_R, A2, A3, A4, A5)))) >= 4)
      mean(c_across(c(A1_R, A2, A3, A4, A5)), na.rm = TRUE) else NA_real_,
    consc_mean = if (sum(!is.na(c_across(c(C1, C2, C3, C4_R, C5_R)))) >= 4)
      mean(c_across(c(C1, C2, C3, C4_R, C5_R)), na.rm = TRUE) else NA_real_,
    extra_mean = if (sum(!is.na(c_across(c(E1_R, E2_R, E3, E4, E5)))) >= 4)
      mean(c_across(c(E1_R, E2_R, E3, E4, E5)), na.rm = TRUE) else NA_real_,
    neuro_mean = if (sum(!is.na(c_across(c(N1, N2, N3, N4, N5)))) >= 4)
      mean(c_across(c(N1, N2, N3, N4, N5)), na.rm = TRUE) else NA_real_,
    open_mean  = if (sum(!is.na(c_across(c(O1, O2_R, O3, O4, O5_R)))) >= 4)
      mean(c_across(c(O1, O2_R, O3, O4, O5_R)), na.rm = TRUE) else NA_real_
  ) %>%
  ungroup()
```

Alternatively, the `psych` package's own `scoreItems()` function will apply reverse keying and compute scale scores in one step, and is worth demonstrating as the more idiomatic route once students understand the manual logic above:

```r
library(psych)

keys_list <- list(
  agree = c("-A1", "A2", "A3", "A4", "A5"),
  consc = c("C1", "C2", "C3", "-C4", "-C5"),
  extra = c("-E1", "-E2", "E3", "E4", "E5"),
  neuro = c("N1", "N2", "N3", "N4", "N5"),
  open  = c("O1", "-O2", "O3", "O4", "-O5")
)

scores <- scoreItems(keys_list, bfi, min = 1, max = 6)
summary(scores)
```

## 6. Demographic variables

| Variable    | Description                      | Coding                | Notes                                             |
| ----------- | ---------------------------------- | ----------------------- | -------------------------------------------------- |
| `gender`    | Recorded gender category           | 1 = male; 2 = female     | No missing values in this file (n = 919 male, 1,881 female) |
| `education` | Self-reported educational level    | 1–5; see below           | 223 respondents (8.0%) missing                     |
| `age`       | Age in years                       | Numeric                  | Ranges from 3 to 86 in this file — the minimum is implausible and should be screened before use, not treated as a valid data point without checking |
| `rownames`  | Original SAPA respondent identifier | Non-sequential integer  | Exclude from analysis; keep only as an ID/reference column |

### Education coding

| Value | Description            |
| ----: | ------------------------ |
|     1 | High school               |
|     2 | Finished high school       |
|     3 | Some college                |
|     4 | College graduate             |
|     5 | Graduate degree                |
| Blank | Missing or unreported            |

The distinction between categories 1 and 2 is not explained further in the package documentation. It would be unsafe to impose a more specific interpretation, such as "did not complete high school", without consulting the original data-collection materials.

### Gender coding limitation

The documentation labels the variable `gender` with only male and female categories. This reflects the coding used when the data were collected in 2010 and should not be presented as a comprehensive contemporary measure of gender identity. The category labels describe the data as collected rather than a theoretical claim that gender is binary.

### A note on `age`

A quick check of this file (`bfi.csv`) shows `age` ranging from 3 to 86. An age of 3 is not plausible for a self-report personality survey and is worth using as a live data-screening example: ask students to flag and discuss (rather than silently drop) implausible values before any analysis, consistent with the data-screening habits emphasised elsewhere in the workshop.

```r
bfi %>%
  count(age) %>%
  arrange(age) %>%
  head(10)
```

## 7. Variables for each workshop analysis

### PCA or EFA

Select the 25 personality items only (reverse-scored versions recommended — see the note below):

```r
pca_efa_vars <- bfi %>%
  select(A1_R, A2, A3, A4, A5,
         C1, C2, C3, C4_R, C5_R,
         E1_R, E2_R, E3, E4, E5,
         N1, N2, N3, N4, N5,
         O1, O2_R, O3, O4, O5_R)
```

Exclude `rownames`, `gender`, `education` and `age` from the item set used in PCA/EFA — these are auxiliary variables, not indicators of the latent traits.

For a workshop, using the reverse-scored versions in PCA, EFA and CFA is recommended because all indicators then point in the same conceptual direction, which simplifies interpretation without changing the underlying strength of the associations. Reverse scoring changes the sign of a correlation, not its absolute magnitude. If the original (non-reversed) items are retained instead, negatively worded items will generally load in the opposite direction on their factor — this is a scoring-direction artefact, not necessarily evidence that those items perform poorly.

### CFA

Use the same 25 items (reverse-scored) and specify the proposed five-factor structure with `lavaan`:

```r
library(lavaan)

cfa_model <- '
  Agreeableness     =~ A1_R + A2 + A3 + A4 + A5
  Conscientiousness =~ C1 + C2 + C3 + C4_R + C5_R
  Extraversion      =~ E1_R + E2_R + E3 + E4 + E5
  Neuroticism       =~ N1 + N2 + N3 + N4 + N5
  Openness          =~ O1 + O2_R + O3 + O4 + O5_R
'

fit <- cfa(cfa_model, data = bfi, missing = "fiml", std.lv = TRUE)

summary(fit, fit.measures = TRUE, standardized = TRUE)
```

`missing = "fiml"` uses full-information maximum likelihood, which is generally preferable to listwise deletion given the modest amount of missing data described in Section 9.

## 8. Proposed CFA model

The five latent factors should ordinarily be allowed to correlate (the default in the `lavaan` syntax above, since `=~` factors are correlated unless set orthogonal). An orthogonal model would impose the strong — and for personality traits, generally implausible — assumption that the five traits are unrelated to one another. This is a useful discussion point when comparing model fit against a constrained orthogonal alternative:

```r
cfa_model_orthogonal <- '
  Agreeableness     =~ A1_R + A2 + A3 + A4 + A5
  Conscientiousness =~ C1 + C2 + C3 + C4_R + C5_R
  Extraversion      =~ E1_R + E2_R + E3 + E4 + E5
  Neuroticism       =~ N1 + N2 + N3 + N4 + N5
  Openness          =~ O1 + O2_R + O3 + O4 + O5_R

  Agreeableness ~~ 0*Conscientiousness
  Agreeableness ~~ 0*Extraversion
  Agreeableness ~~ 0*Neuroticism
  Agreeableness ~~ 0*Openness
  Conscientiousness ~~ 0*Extraversion
  Conscientiousness ~~ 0*Neuroticism
  Conscientiousness ~~ 0*Openness
  Extraversion ~~ 0*Neuroticism
  Extraversion ~~ 0*Openness
  Neuroticism ~~ 0*Openness
'
```

## 9. Missing-data considerations

Item-level missingness in `bfi.csv` is modest but not negligible: individual items range from 0 to 36 missing responses (0.0–1.3% of the sample), and `education` has 223 missing responses (8.0%). Missing values should never be recoded as 0, since 0 falls outside the 1–6 response scale and would distort means, correlations and factor loadings.

For each workshop demonstration, ask students to report:

- The number and percentage of missing responses per item (`colSums(is.na(bfi))`).
- Which missing-data method was used — pairwise deletion, listwise deletion, full-information maximum likelihood (FIML), or another approach — and why.
- Whether the chosen estimator supports that missing-data method (for example, FIML is available with maximum-likelihood estimators in `lavaan`, but not directly with `WLSMV`).
- The final sample size actually included in each analysis, since this can differ from n = 2,800 once a missing-data method is applied.

```r
colSums(is.na(bfi)) %>%
  sort(decreasing = TRUE) %>%
  head(10)
```

The SAPA data-collection design deliberately gives respondents a subset of the full item pool rather than every item, so missing responses in data of this kind should not automatically be interpreted as refusals or respondent error.

## 10. Citation and source note

A suitable dataset note for student write-ups:

> The dataset comprised 25 personality items drawn from the International Personality Item Pool and collected through the Synthetic Aperture Personality Assessment (SAPA) project. The data were distributed as the `bfi` dataset in the `psych`/`psychTools` R packages and included 2,800 respondents, 25 personality items and three demographic variables (Revelle, ongoing).

Item wording, scoring information and demographic codes are documented at [personality-project.org/r/html/bfi.html](https://www.personality-project.org/r/html/bfi.html).
