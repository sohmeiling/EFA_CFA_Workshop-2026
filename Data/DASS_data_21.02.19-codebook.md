# Codebook: `DASS_data_21.02.19` (`data.csv`)

## 1. Dataset overview

| Characteristic                  | Description                                                                 |
| -------------------------------- | ---------------------------------------------------------------------------- |
| Instrument                       | Depression Anxiety Stress Scales (DASS-42) — [www2.psy.unsw.edu.au/dass](http://www2.psy.unsw.edu.au/dass/) |
| Source                           | Open Psychometrics online administration of the DASS, publicly released as `DASS_data_21.02.19.zip` |
| Collection period                | 2017–2019                                                                    |
| Sample                           | Anonymous online respondents who completed the DASS-42 and consented to research use ("Have you given accurate answers and may they be used for research?" = yes) |
| File                              | `data.csv` (tab-separated, despite the `.csv` extension — see §2)           |
| Number of columns                | 172                                                                          |
| Missing data                     | Not formally coded as `NA`; check for blank/placeholder values before analysis |
| Primary teaching uses            | EFA/CFA on a real, "messy" self-report clinical screening instrument with a well-established three-factor structure (Depression, Anxiety, Stress) |

The archive also contains the original data collector's raw notes (`codebook.txt`) and a screenshot of the survey item format (`demo1.png`); this file reorganises that same information into the workshop's standard codebook format.

```r
library(readr)
library(dplyr)

# data.csv is tab-delimited, not comma-delimited
dass <- read_tsv("DASS_data_21.02.19/data.csv")

glimpse(dass)
```

## 2. Important data-cleaning note

Despite living in a file conventionally named `data.csv`, the columns are **tab-separated**, not comma-separated. Use `read_tsv()` (or `read.delim()` / `read.table(sep = "\t")`), not `read_csv()`. Loading it with `read_csv()` will silently return a single malformed column.

## 3. DASS-42 items and response scale

Each of the 42 DASS items was presented **in random order** for every respondent, together with a 4-point frequency-of-occurrence scale asking how much each statement applied to them **over the past week**:

| Value | Response                                                     |
| ----: | -------------------------------------------------------------- |
|     1 | Did not apply to me at all                                      |
|     2 | Applied to me to some degree, or some of the time                |
|     3 | Applied to me to a considerable degree, or a good part of the time |
|     4 | Applied to me very much, or most of the time                      |

Because items were randomised, each item generates **three** raw columns:

| Suffix | Meaning                                                                 |
| ------ | -------------------------------------------------------------------------- |
| `A`    | The response given (1–4) — this is the substantive score to use            |
| `I`    | The item's position (order) in which it was presented to that respondent   |
| `E`    | Time taken to answer that item, in milliseconds                            |

For example `Q1A`, `Q1I`, `Q1E` are the answer, presentation position, and response time for item 1. For scale scoring, only the `A` columns (`Q1A`…`Q42A`) are needed.

### 3.1 Item wording and subscale key

The DASS-42 yields three subscales (Depression, Anxiety, Stress) using the standard Lovibond & Lovibond (1995) scoring key. Each subscale score is the **sum of its 14 items**; unlike the DASS-21, DASS-42 raw subscale sums do **not** need to be multiplied by 2.

**Depression subscale** — items 3, 5, 10, 13, 16, 17, 21, 24, 26, 31, 34, 37, 38, 42

| Item | Wording |
| ---- | ------- |
| Q3   | I couldn't seem to experience any positive feeling at all. |
| Q5   | I just couldn't seem to get going. |
| Q10  | I felt that I had nothing to look forward to. |
| Q13  | I felt sad and depressed. |
| Q16  | I felt that I had lost interest in just about everything. |
| Q17  | I felt I wasn't worth much as a person. |
| Q21  | I felt that life wasn't worthwhile. |
| Q24  | I couldn't seem to get any enjoyment out of the things I did. |
| Q26  | I felt down-hearted and blue. |
| Q31  | I was unable to become enthusiastic about anything. |
| Q34  | I felt I was pretty worthless. |
| Q37  | I could see nothing in the future to be hopeful about. |
| Q38  | I felt that life was meaningless. |
| Q42  | I found it difficult to work up the initiative to do things. |

**Anxiety subscale** — items 2, 4, 7, 9, 15, 19, 20, 23, 25, 28, 30, 36, 40, 41

| Item | Wording |
| ---- | ------- |
| Q2   | I was aware of dryness of my mouth. |
| Q4   | I experienced breathing difficulty (e.g. excessively rapid breathing, breathlessness in the absence of physical exertion). |
| Q7   | I had a feeling of shakiness (e.g. legs going to give way). |
| Q9   | I found myself in situations that made me so anxious I was most relieved when they ended. |
| Q15  | I had a feeling of faintness. |
| Q19  | I perspired noticeably (e.g. hands sweaty) in the absence of high temperatures or physical exertion. |
| Q20  | I felt scared without any good reason. |
| Q23  | I had difficulty in swallowing. |
| Q25  | I was aware of the action of my heart in the absence of physical exertion (e.g. sense of heart rate increase, heart missing a beat). |
| Q28  | I felt I was close to panic. |
| Q30  | I feared that I would be "thrown" by some trivial but unfamiliar task. |
| Q36  | I felt terrified. |
| Q40  | I was worried about situations in which I might panic and make a fool of myself. |
| Q41  | I experienced trembling (e.g. in the hands). |

**Stress subscale** — items 1, 6, 8, 11, 12, 14, 18, 22, 27, 29, 32, 33, 35, 39

| Item | Wording |
| ---- | ------- |
| Q1   | I found myself getting upset by quite trivial things. |
| Q6   | I tended to over-react to situations. |
| Q8   | I found it difficult to relax. |
| Q11  | I found myself getting upset rather easily. |
| Q12  | I felt that I was using a lot of nervous energy. |
| Q14  | I found myself getting impatient when I was delayed in any way (e.g. elevators, traffic lights, being kept waiting). |
| Q18  | I felt that I was rather touchy. |
| Q22  | I found it hard to wind down. |
| Q27  | I found that I was very irritable. |
| Q29  | I found it hard to calm down after something upset me. |
| Q32  | I found it difficult to tolerate interruptions to what I was doing. |
| Q33  | I was in a state of nervous tension. |
| Q35  | I was intolerant of anything that kept me from getting on with what I was doing. |
| Q39  | I found myself getting agitated. |

```r
library(dplyr)

dass <- dass %>%
  mutate(
    depression = Q3A + Q5A + Q10A + Q13A + Q16A + Q17A + Q21A + Q24A +
                 Q26A + Q31A + Q34A + Q37A + Q38A + Q42A,
    anxiety    = Q2A + Q4A + Q7A + Q9A + Q15A + Q19A + Q20A + Q23A +
                 Q25A + Q28A + Q30A + Q36A + Q40A + Q41A,
    stress     = Q1A + Q6A + Q8A + Q11A + Q12A + Q14A + Q18A + Q22A +
                 Q27A + Q29A + Q32A + Q33A + Q35A + Q39A
  )
```

## 4. Timing variables

| Variable       | Description                                                                 |
| --------------- | ----------------------------------------------------------------------------- |
| `introelapse`   | Time spent on the introduction/landing page (seconds)                        |
| `testelapse`    | Time spent on all 42 DASS questions (seconds)                                |
| `surveyelapse`  | Time spent on the demographic/survey questions that followed (seconds)       |

## 5. Ten Item Personality Inventory (TIPI)

Administered after the DASS items (Gosling, Rentfrow & Swann, 2003). Respondents rated "I see myself as: ___" on a 7-point agreement scale (1 = Disagree strongly … 7 = Agree strongly).

| Variable | Trait pair                       |
| -------- | ---------------------------------- |
| `TIPI1`  | Extraverted, enthusiastic          |
| `TIPI2`  | Critical, quarrelsome              |
| `TIPI3`  | Dependable, self-disciplined       |
| `TIPI4`  | Anxious, easily upset              |
| `TIPI5`  | Open to new experiences, complex   |
| `TIPI6`  | Reserved, quiet                    |
| `TIPI7`  | Sympathetic, warm                  |
| `TIPI8`  | Disorganized, careless             |
| `TIPI9`  | Calm, emotionally stable           |
| `TIPI10` | Conventional, uncreative           |

## 6. Vocabulary checklist (VCL1–VCL16)

Respondents were shown a list of words and instructed to check all words whose meaning they were sure they knew (`1` = checked, `0` = unchecked).

| Variable | Word          | Notes                          |
| -------- | -------------- | --------------------------------- |
| `VCL1`   | boat           |                                    |
| `VCL2`   | incoherent     |                                    |
| `VCL3`   | pallid         |                                    |
| `VCL4`   | robot          |                                    |
| `VCL5`   | audible        |                                    |
| `VCL6`   | cuivocal       | **Not a real word** — validity check |
| `VCL7`   | paucity        |                                    |
| `VCL8`   | epistemology   |                                    |
| `VCL9`   | florted        | **Not a real word** — validity check |
| `VCL10`  | decide         |                                    |
| `VCL11`  | pastiche       |                                    |
| `VCL12`  | verdid         | **Not a real word** — validity check |
| `VCL13`  | abysmal        |                                    |
| `VCL14`  | lucid          |                                    |
| `VCL15`  | betray         |                                    |
| `VCL16`  | funny          |                                    |

Respondents who checked `VCL6`, `VCL9`, or `VCL12` as "known" words may be inattentive or careless responders; consider flagging or excluding them.

## 7. Demographic and derived variables

| Variable                  | Description                                                                                                                              |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `country`                  | ISO country code inferred from the respondent's connection                                                                                |
| `source`                    | How the respondent found the survey: `1` = front page of hosting site, `2` = Google, `0` = other/unknown                                  |
| `education`                 | Highest education completed: `1` = less than high school, `2` = high school, `3` = university degree, `4` = graduate degree               |
| `urban`                     | Childhood area type: `1` = rural, `2` = suburban, `3` = urban                                                                              |
| `gender`                    | `1` = male, `2` = female, `3` = other                                                                                                      |
| `engnat`                    | Is English the respondent's native language? `1` = yes, `2` = no                                                                           |
| `age`                       | Age in years                                                                                                                               |
| `hand`                      | Writing hand: `1` = right, `2` = left, `3` = both                                                                                          |
| `religion`                  | `1` Agnostic, `2` Atheist, `3` Buddhist, `4` Christian (Catholic), `5` Christian (Mormon), `6` Christian (Protestant), `7` Christian (Other), `8` Hindu, `9` Jewish, `10` Muslim, `11` Sikh, `12` Other |
| `orientation`                | `1` Heterosexual, `2` Bisexual, `3` Homosexual, `4` Asexual, `5` Other                                                                     |
| `race`                       | `10` Asian, `20` Arab, `30` Black, `40` Indigenous Australian, `50` Native American, `60` White, `70` Other                                |
| `voted`                      | Voted in a national election in the past year? `1` = yes, `2` = no                                                                         |
| `married`                    | `1` Never married, `2` Currently married, `3` Previously married                                                                           |
| `familysize`                 | Number of children the respondent's mother had, including the respondent                                                                  |
| `major`                      | Free-text university major (only present if the respondent attended university)                                                            |
| `screensize`                 | `1` = small screen (phone etc.), `2` = large screen (laptop/desktop etc.)                                                                  |
| `uniquenetworklocation`      | `1` = only one survey submitted from this respondent's network in the dataset; `2` = multiple submissions from that network (does not necessarily indicate duplicate individuals — see note below) |

Note on `uniquenetworklocation`: a value of `2` does not automatically mean duplicate records for one person — it could reflect several students on the same school network, or several household members. Even a value of `1` does not guarantee a single individual submitted only once (e.g. once on wifi, once on mobile data). Treat as an imperfect duplicate-detection flag rather than a definitive one.

## 8. Suggested EFA/CFA use in the workshop

- Run **EFA** on the 42 `A` items (`Q1A`…`Q42A`) first, without imposing structure, to let students discover that a three-factor solution (Depression / Anxiety / Stress) emerges empirically, mirroring the original DASS validation work.
- Follow with a **CFA** specifying the three a-priori subscales listed in §3.1, and compare fit to alternative models (e.g. a single general-distress factor, or a bifactor model), which is a common point of discussion in the DASS validity literature.
- Given the large, uncurated online sample, discuss data-cleaning decisions with students before modelling: extreme `testelapse` values (very fast or very long completion), the `VCL6`/`VCL9`/`VCL12` validity checks, and duplicate submissions flagged by `uniquenetworklocation`.
