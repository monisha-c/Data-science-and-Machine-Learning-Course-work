# Legal, Social, Ethical, Professional, EDI and Sustainability Assessment

**Module:** COM747 — Data Science and Machine Learning CW2  
**Project:** A Machine Learning Framework for Detecting Multidimensional Care-Intensity Discordance and Predicting 30-Day Hospital Readmission in Patients with Diabetes  
**Status:** Working governance assessment and implementation plan; update with actual results, dates, reviewer names and evidence before submission. No clinical deployment or ethics approval is claimed.

## 1. Scope and purpose

The project uses the UCI Diabetes 130-US Hospitals dataset (1999–2008) to examine differences between statistically expected and recorded inpatient care across length of stay, laboratory procedures, non-laboratory procedures and medication counts. RQ1 investigates adjusted associations with recorded 30-day readmission; RQ2 evaluates the incremental predictive value of discordance features. The unit is an encounter, and patients may have repeat encounters. Prediction is at discharge. The statistical benchmark reflects recorded historical practice; it does not establish appropriate treatment, unmet clinical need, over-treatment or causal effects. The project is retrospective coursework and research, not a tool for determining discharge, prescribing or rationing healthcare.

## 2. Legal considerations

- **Data rights and attribution:** UCI lists the dataset under CC BY 4.0 [1, 3]. Cite the creators, dataset DOI and licence, link to the original, identify adaptations (cohort filtering, ICD-9 grouping, binary outcome and derived features), and avoid implying creator endorsement. Cite the original study separately [2].
- **Privacy and data protection:** public availability or removal of direct identifiers does not automatically establish effective anonymisation. Assess identifiability in context and follow the university's data protection and research policies. UK GDPR and the Data Protection Act 2018 may apply if the data constitute personal information; health data would require additional safeguards and an appropriate legal basis if applicable. Consult the university data protection or ethics contact for the project's actual obligations. ICO guidance distinguishes anonymous and pseudonymous information [4, 5]. Do not assert GDPR or HIPAA compliance on the basis of the dataset description alone.
- **Data minimisation:** use patient and encounter identifiers only to deduplicate, group splits, check lineage and support patient-cluster uncertainty estimates. Exclude them from predictors. Keep raw and processed files on approved local or institutional storage; avoid external uploads, public patient-level examples and attempts to re-identify or link records.
- **Governance:** confirm institutional ethics requirements for secondary research before dissemination; no approval/exemption is presumed. Review conference and university requirements regarding authorship, AI assistance and disclosure. The coursework is not a certified medical device; deployment would require separate regulatory, clinical and governance evaluation.

**Implementation evidence:** `cleaning/cleaning.ipynb` retains only specified columns, records provenance/checksums and excludes identifiers from predictor whitelists. `cleaning/validation.ipynb` verifies the whitelists, split isolation and aggregate validation output. The group still needs to implement repository access, retention and approval checks.

Suggested `.gitignore` entries (merge into the existing file):

```gitignore
.venv/
.ipynb_checkpoints/
__pycache__/
data/raw/
data/processed/
*.joblib
*.pkl
```

Before pushing or submitting notebooks, remove row-level previews (including existing EDA `head()` output) and identifiers; inspect all saved outputs. `.gitignore` does not remove previously committed data. Share aggregate results only and follow institutional retention instructions.

## 3. Social considerations

Readmission risk can affect patient experience, caregiver burden and demand for hospital services. A research tool that highlights unusual recorded care patterns may help identify questions for clinical review. However, readmission is not always preventable, nor is it a complete measure of healthcare quality. Patients differ in severity, preferences, access, support, socioeconomic circumstances and care setting; many of these variables are not observed. Hospital or community resource differences may affect both recorded care and readmission. Do not attribute an observed mismatch to clinician error or patient behaviour.

False negatives could miss people who warrant review; false positives could create unnecessary follow-up workload or anxiety. Report both, alongside precision, recall, calibration and sample sizes. Any discussion of future use should retain human clinical oversight, explain uncertainty and allow challenge of results. Do not recommend denying or delaying care on the basis of a score.

## 4. Ethical considerations

| Risk | Safeguard and evidence |
|---|---|
| Historical practice encoded as a norm | Describe predicted care as a conditional statistical benchmark, not an ideal quantity. Assess residual distributions and unmeasured confounding; avoid labelling patients as under- or over-treated. |
| Outcome leakage or inflated performance | Freeze patient-grouped splits; use training-only fitted transforms and cross-fitted expected-care predictions. Perform nested group-aware feature generation if tuning via CV. Verify split audit before modelling. |
| Ineligible or competing outcomes | Exclude death/hospice discharges and missing disposition IDs under documented cohort rules. Report cohort attrition; consider unknown disposition and transfer sensitivity analyses. Remaining patients may die or be readmitted elsewhere; absence of recorded readmission is not proof of recovery. |
| Misinterpretation of missingness | Preserve `None` as test not performed, distinct from missing entries. Do not equate lack of testing with normal results. Keep missing demographic values visible in aggregate audits. |
| Overclaiming efficacy | Use paired held-out comparisons, confidence intervals and calibration. Associations are not causal; an improvement may be small, absent or negative. No observed intervention outcomes exist. |
| Privacy and stigma | Avoid record-level publication, use neutral language, report aggregate results and suppress small groups. |

For RQ1, use patient-cluster robust standard errors (or another justified repeated-measures approach) and report adjusted odds ratios with confidence intervals. The residual features are estimated quantities; acknowledge generated-regressor uncertainty and, if feasible, re-estimate them within patient bootstrap samples for inference. Avoid causal language and examine multicollinearity when including residuals and their raw components. Predefine the association model and the direction of signed residuals; lower-than-expected care is not inadequate care.

## 5. Professional considerations

Follow CRISP-DM and principles of professional conduct such as IEEE's emphasis on safety, honest claims, privacy and proper attribution [6].

| CRISP-DM phase | Professional practice / evidence |
|---|---|
| Business understanding | Document the two RQs, intended discharge-time use, scope, assumptions and exclusions. |
| Data understanding | Preserve original dataset; review EDA with encounter rather than patient counts, and record missingness and data limitations. |
| Data preparation | Reproducible code, feature contract, cohort flow, checksums, patient partitions and validation report. |
| Modelling | Reuse shared environment; version dependencies; fixed seeds; comparable feature-set budgets; track model configurations and runtime. |
| Evaluation | Predefine AP, recall, F1, FNR, calibration and thresholds; report uncertainty, subgroup support and failed/negative experiments. |
| Reporting | Reproducible figures and results, author contribution log, citations and limitations; no clinical deployment. |

Use Git for code and documentation, with peer review of transformations and metric calculations. Agree the final dependency versions after testing; record the Python environment and random seeds. Every contributor should be able to explain their own code and methods. Do not fabricate results, citations, originality or contribution claims. Verify novelty separately through literature review and acknowledge related work. Record any permitted AI assistance according to institutional policy. Use the same splits and evaluation protocol for all models. Select thresholds using validation, never test labels.

## 6. Equality, diversity and inclusion (EDI)

The historical US cohort may not represent contemporary US or UK populations. Recorded race and gender are limited categories and cannot capture the full diversity of identity or lived experience. Small groups and missing demographic labels may result in unreliable estimates. Do not infer missing identities. The observed treatment distribution may encode historical inequality, and using protected attributes in a statistical benchmark may normalise those patterns. Removing those attributes does not guarantee fairness, because other features may act as proxies.

**Implemented at preparation:** training-only demographic coverage audit (age, recorded gender and race, including missing categories); proposed predictor specification with age, race and gender and planned sensitivity excluding race and gender. All demographics are retained for permitted aggregate audits, not patient profiling for treatment eligibility. The validation notebook suppresses small groups in its displayed training summary using a proposed threshold of 20 for encounters, unique patients and each outcome class. This threshold is a reporting rule, not proof of statistical adequacy or anonymity.

**To implement after modelling:** evaluate each subgroup using the same held-out predictions and a threshold selected globally on validation data. Report encounter and unique patient counts, prevalence, AP where estimable, recall/sensitivity, FNR, precision and calibration where supported. Report gap and uncertainty (patient-cluster bootstrap), not just an unqualified fairness score. Compare the expected-care residual distributions and prediction errors across subgroups, and compare primary and demographic-excluded specifications. Keep a minimum-support policy, suppress small or identifying cells and consider disclosure through totals. Avoid interpreting noisy small-group estimates as discrimination or equal rates as proof of equity. Do not optimise a separate threshold for the test groups. No claims of legal or clinical fairness are made.

## 7. Sustainability

**Healthcare resource perspective:** the four recorded care dimensions are proxies for resource utilisation, not actual financial cost, energy use, staffing burden or environmental impact. Analyse care patterns and their association with readmission as exploratory evidence. A statistically unusual encounter may be medically justified. Do not infer cost savings, shorter safe stays, reduced emissions or preventable readmissions from these data.

**Computational perspective:** use a shared environment, lightweight baselines, bounded hyperparameter search, deterministic cached features where valid, CPU-first experiments and reusable code. Avoid unnecessary reruns of expensive models. Compare incremental predictive value with computational effort. Report elapsed training/feature-generation time, hardware, tuning trial counts and, where available, measured energy. Do not convert runtime to carbon without defensible energy measurement and emissions factors. Discuss links to SDG 3 (health and well-being) and SDG 12 (responsible consumption and production) [7] as possible motivations, not measured contributions.

Proposed experiment log (fill from actual runs):

| Model/feature set | Hardware | Training + feature generation seconds | Tuning trials | Test AP | Notes |
|---|---|---:|---:|---:|---|
| Baseline A | Pending | Pending | Pending | Pending | |
| Baseline B | Pending | Pending | Pending | Pending | |
| Proposed C | Pending | Pending | Pending | Pending | |

## 8. Risk register and evidence checklist

Initial qualitative priorities are planning judgements, not assessed clinical risk levels. Assign an owner and date for each item during group review.

| Risk / priority | Required evidence or action | Status |
|---|---|---|
| Data leakage / high | Actual `validation_report.json`, cross-fitting and tuning code review | Data audit code supplied; actual run pending |
| Clinical misuse / high | Scope and limitations in notebook and IEEE paper | Scope documented; review pending |
| Privacy / high | Approved storage, no row-level public outputs, `.gitignore`, ethics check | Pending group implementation |
| Historical inequity / high | Subgroup metrics, coverage, residual analysis and sensitivity | Coverage audit supplied; actual results pending |
| Outcome/cohort bias / high | Mapping verification, attrition and sensitivity analysis | Cleaning code supplied; actual run pending |
| Reproducibility / medium | Environment version record, seeds, hashes, peer code review | Partial implementation; group testing pending |
| Sustainability overclaim / medium | Runtime log and accurate limits on care proxies | Planned |
| Unsupported novelty / medium | Verified literature search and fair comparison | Pending verification |

At the end of the project, replace planning statements with what was actually done and cite artefacts/results. Retain unresolved risks and limitations rather than marking all items complete.

## 9. References

[1] J. Clore, K. Cios, J. DeShazo, and B. Strack, “Diabetes 130-US Hospitals for Years 1999-2008,” UCI Machine Learning Repository, 2014, doi: [10.24432/C5230J](https://doi.org/10.24432/C5230J). Dataset page: https://archive.ics.uci.edu/dataset/296/diabetes%2B130-us%2Bhospitals%2Bfor%2Byears%2B1999-2008.

[2] B. Strack et al., “Impact of HbA1c Measurement on Hospital Readmission Rates: Analysis of 70,000 Clinical Database Patient Records,” *BioMed Research International*, 2014, Art. no. 781670, doi: [10.1155/2014/781670](https://doi.org/10.1155/2014/781670).

[3] Creative Commons, “Attribution 4.0 International.” https://creativecommons.org/licenses/by/4.0/.

[4] Information Commissioner's Office, “Introduction to anonymisation.” https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/data-sharing/anonymisation/introduction-to-anonymisation/.

[5] Information Commissioner's Office, “What requirements and safeguards must we have in place?” https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/the-research-provisions/what-requirements-and-safeguards-must-we-have-in-place/.

[6] IEEE Circuits and Systems Society, “IEEE Code of Ethics.” https://ieee-cas.org/about/ieee-code-ethics.

[7] United Nations, “The 17 Goals.” https://sdgs.un.org/goals.

UCI, original study, ICO guidance and IEEE CASS code consulted on 27 November 2026. Check institutional policies and source status before final submission; this document is coursework guidance, not a legal determination.
