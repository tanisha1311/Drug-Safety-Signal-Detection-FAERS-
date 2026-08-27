# Drug-Safety-Signal-Detection-FAERS

# Business Problem
Post-market drug safety monitoring relies on identifying drug-adverse event combinations that occur more frequently than expected — these "signals" guide regulatory action, label changes, and further investigation. Manual review can't scale to millions of reports; this project builds a statistical signal-detection pipeline to flag disproportionate drug-event associations, the same core technique used in pharmacovigilance.

# Dataset
FDA Adverse Event Reporting System (FAERS) Quarterly Data Extract Files

- Landing page: https://www.fda.gov/drugs/fda-adverse-event-monitoring-system-aems/fda-adverse-event-monitoring-system-aems-latest-quarterly-data-files
- API alternative (openFDA, easier to query than raw XML): https://open.fda.gov/apis/drug/event/

# Approach

- Pull 3–4 recent quarters of FAERS data (XML or via openFDA API).
- Parse into structured tables (report, drug, reaction) using ElementTree or the API's JSON — merge on case/report ID.
- Clean duplicate case IDs (a known FAERS data quality issue).
- Build 2x2 contingency tables per drug-event pair.
- Compute PRR and ROR with Fisher's exact test for significance.
- Apply Benjamini-Hochberg FDR correction across all tested pairs (this is the step that separates it from a basic ratio calculation).
- Validate: confirm your pipeline recovers 1–2 known FDA-flagged safety signals.
- Output: ranked signal table (drug, event, PRR, CI, corrected p-value).
