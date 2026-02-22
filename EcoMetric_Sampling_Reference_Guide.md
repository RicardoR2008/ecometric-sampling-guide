# EcoMetric Sampling Advisor — Reference Guide
# Upload this file to the Knowledge section of the Copilot Agent Builder

---

## SAMPLING CHECKLIST (Sections 0–8)

### SECTION 0 — PROJECT CONTEXT (3 items)
- [ ] Evaluation type identified (Impact / Process / Hybrid)
- [ ] Sample unit identified (Measure / Project / Participant / Account / Premise)
- [ ] Target precision confirmed with PM

### SECTION 1 — DATA (4 items)
- [ ] Population data sourced and raw file preserved separately
- [ ] Unique record identified (primary key, business key, or composite)
- [ ] Eligibility criteria defined and records-dropped documented at each step
- [ ] Final eligible population count confirmed

### SECTION 2 — RELATIONSHIPS (2 items)
- [ ] Data relationships mapped (one-to-many, many-to-many)
- [ ] Multiples identified and handling decision documented

### SECTION 3 — CLEANING (3 items)
- [ ] Contact info standardized and validated
- [ ] Category/measure names standardized
- [ ] Computed fields created and documented (if needed)

### SECTION 4 — STRATIFICATION (2 items)
- [ ] Strata defined with population counts
- [ ] Stratification approach reviewed with PM

### SECTION 5 — SAMPLE DESIGN (4 items)
- [ ] Sample sizes calculated per stratum
- [ ] Allocation method documented (Proportional / Neyman / Equal / Census)
- [ ] Random seed recorded in 3 places (file name, memo, code)
- [ ] Unique Case IDs assigned

### SECTION 6 — FINALIZATION (4 items)
- [ ] Weights calculated (N ÷ n per stratum)
- [ ] Weight check passes: sum(n × weight) = N per stratum
- [ ] No duplicate contacts in final sample
- [ ] Replacements drawn and labeled

### SECTION 7 — SURVEY PREP (3 items)
- [ ] Fill fields reviewed for human-friendly wording
- [ ] Analysis variables carried through as embedded data
- [ ] Contact info validated for chosen collection method

### SECTION 8 — DOCUMENTATION (2 items)
- [ ] Sampling memo completed (context, eligibility, stratification, methodology, deviations, limitations)
- [ ] Project manager reviewed and approved

---

## COMMON MISTAKES

1. WRONG SAMPLE UNIT: Sampling measures for a phone survey, then calling the same person 3 times. Ask: am I contacting PEOPLE or verifying RECORDS?
2. NOT FILTERING BEFORE SAMPLING: Mixing record types that need different instruments. Filter the type column first.
3. WRONG DEDUP KEY: Using email when vendor emails contaminate the data. Cross-reference with other columns.
4. MISSING CROSS-FILE MULTIPLES: Same person gets surveys from multiple files. Dedup ACROSS all files.
5. NO RANDOM SEED: Can't reproduce the sample. Record in file name + memo + code.
6. FORGETTING WEIGHTS: Unweighted results with different sampling rates distort estimates. Always apply weights.
7. NOT OVERSAMPLING: Drawing exact target, reaching only 30%. Always draw replacements.
8. CONFUSING RECORD vs CONTACT COUNTS: Population of 1,000 records may have only 850 unique contacts. Weights use RECORD-level population ÷ completes.
9. SKIPPING ELIGIBILITY DOCUMENTATION: Not tracking how many records dropped at each step. PM can't verify your decisions.
10. NOT STANDARDIZING CATEGORY NAMES: "LED" vs "Led Lighting" vs "LED Lamps" splits one stratum into three.

---

## GLOSSARY

### Universal Terms
- 90/10: 90% confidence, ±10% relative precision.
- Account: Billing account number. One customer may have multiple.
- Case ID: Unique tracking number YOU assign. Not from program data.
- Census: Include everyone — no sampling.
- Certainty Stratum: Subgroup sampled at 100%.
- Composite Key: Combination of fields that together create uniqueness.
- Cooperation Rate: % of reached contacts who complete an interview.
- CV: Coefficient of Variation (SD ÷ mean). Drives sample sizes for impact evaluations.
- Dedup/Multiples: Records sharing the same contact info. "Multiples" is the EcoMetric term for grouped records with the same contact — we contact the person once on behalf of multiple records.
- FPC: Finite Population Correction. Reduces required n when sampling a large fraction of N.
- Measure: Individual equipment or action (one LED bulb, one HVAC tune-up).
- Neyman Allocation: Sample size per stratum proportional to both population AND variance.
- Oversampling: Drawing more contacts than needed to account for non-response.
- Participant: Entity enrolled in a program. The person you contact.
- Population: The entire eligible group.
- PPS: Probability Proportional to Size. Larger units have higher selection probability.
- Precision: How close your estimate is to truth. ±10% = could be off by up to 10%.
- Premise: Physical location where service is consumed.
- Primary Key: Unique identifier where each value appears exactly once.
- Project/Application: Specific instance of participation. One participant may have multiple.
- Random Seed: Number that makes random selection reproducible.
- Response Rate: % of invited people who complete a self-administered survey.
- Sampling Frame: Complete list of all eligible units.
- Sampling Rate: n ÷ N.
- Sampling Weight: N ÷ n. How many records each sample point represents.
- Stratification: Dividing population into subgroups before sampling.

### Domain-Specific (use when relevant)
- Ex-ante/Ex-post: Claimed vs verified values.
- Free-ridership: Those who'd have acted the same without the program.
- NTG: Net-to-Gross ratio.
- Realization Rate: Verified ÷ Claimed outcome.
- Spillover: Actions taken due to program influence but not directly counted.
- TRM: Technical Reference Manual.

---

## SAMPLE SIZE QUICK REFERENCE (proportion-based, p=0.5, with FPC)

| Pop   | 90/10 | 80/20 | 90/15 | 95/10 |
|-------|-------|-------|-------|-------|
| ≤30   | Census| Census| Census| Census|
| 50    | 38    | 14    | 22    | 44    |
| 100   | 40    | 16    | 25    | 49    |
| 250   | 55    | 18    | 28    | 70    |
| 500   | 60    | 19    | 29    | 81    |
| 1000  | 65    | 19    | 30    | 88    |
| 2500  | 67    | 20    | 30    | 93    |
| 5000+ | 68    | 20    | 30    | 96    |

---

## COOPERATION / RESPONSE RATES & OVERSAMPLING

| Method      | Typical Rate | Oversampling Multiplier |
|-------------|-------------|------------------------|
| Phone       | 20-40%      | 2-3×                   |
| Web survey  | 10-15%      | Census if feasible     |
| On-site     | 70-90%      | 1.5×                   |
| Mail        | 5-15%       | 3-4×                   |
| Desk review | 100%        | No replacements needed |

---

## DATA HIERARCHY (Checklist Section 1A)

### Billing Hierarchy
Customer → Account(s) → Premise(s) → Meter(s)
- Customer: Entity receiving service. May have multiple accounts and premises.
- Account: Billing account number. One customer may have multiple.
- Premise: Physical location where energy/service is consumed.
- Meter: Measurement device at a premise.

### Program Hierarchy
Participant → Project(s)/Application(s) → Measure(s)
- Participant: Entity enrolled in a program.
- Project/Application: Specific instance of participation.
- Measure: Individual equipment installed or action taken.

### Why the Same Person Appears Multiple Times
- Multiple premises (owns several buildings)
- Multiple measures per project (lighting + HVAC)
- Multiple program years (2023 and 2024)
- Multiple programs (lighting and HVAC programs)

### Sample Unit Decision
- Impact Evaluation → usually MEASURE or PROJECT level
- Process/Survey → usually PARTICIPANT or ACCOUNT level
- Hybrid → need BOTH levels

For non-utility data, replace with whatever hierarchy exists: Organization → Department → Employee → Activity, etc.

---

## SAMPLING 101 WORKBOOK AUTO-FILL — CELL MAP

When the user uploads the EcoMetric Sampling 101 Blank Template along with their data, auto-fill these yellow cells using openpyxl after completing the data analysis workflow.

### TAB: Dashboard
- B26 → "SCENARIO A: [scenario name]"
- C27 → total record count (number)
- C28 → evaluation goal (text)
- C29 → study type: "Impact" or "Process" or "NTG" or "Hybrid"
- B31 → "SCENARIO B: [second scenario if applicable]"
- C32, C33, C34 → same as above for scenario B

### TAB: Our Data
- B5 → "SCENARIO A: [scenario name]"
- C6 → source file name
- C7 → total record count
- C8 → program year(s)
- Rows 11-14 (subgroup table, one row per stratum):
  - B11 → stratum 1 name, C11 → count, D11 → % of total, E11 → mean outcome, F11 → min, G11 → max, H11 → count with valid phone/email
  - B12-B14 → same for strata 2-4
- Duplicate count table (rows 19-22):
  - C19 → total phones, D19 → unique phones, E19 → # with dupes, F19 → interpretation
  - C20-C22 → same for emails, names, accounts
- Data quality flags (rows 26-29):
  - B26 → issue description, C26 → record count, D26 → recommended action

### TAB: Pick Your Sample
- C8 → study type answer
- C11 → number of data files
- C14 → total records
- C17 → collection method
- C20 → precision level
- C23 → stratification approach
- C26 → duplicates answer
- C29 → outliers answer
- C32 → data quality issues answer
- C35 → random seed

### TAB: Sample Size & Weights (rows 35-38)
- B35 → stratum 1 name, C35 → pop (N), D35 → sample (n), F35 → "Yes"/"No" census, H35 → replacement count
- B36-B38 → same for strata 2-4
- Columns E and G: Rate = n/N and Weight = N/n

### TAB: Checklist (column B = status, column D = notes)
- B8/D8 → study type identified / which type
- B9/D9 → sample unit / "Measures" or "Participants"
- B10/D10 → data files ready / file count and names
- B12/D12 → population counted / "N = [count]"
- B13/D13 → ineligible removed / what and why
- B14/D14 → duplicates handled / dedup key and method
- B15/D15 → strata created / list of strata
- B17/D17 → sample sizes / "n per stratum: [list]"
- B18/D18 → random selection / "SRS within strata"
- B19/D19 → random seed / the seed value
- B20/D20 → Case IDs / the format used
- B22/D22 → weights / "Weight per stratum: [list]"
- B23/D23 → sizes match plan / "Yes, verified"
- B24/D24 → no dupes in sample / dedup check method
- Items B25, B27-B29, B31-B32 → leave as "Not Done" (require manual human action)

### After Filling
- Save as: [Client]_Sampling_101_Filled_[Date].xlsx
- Warn user: "Some dropdown menus and conditional formatting may need to be re-applied. The data values are correct."


