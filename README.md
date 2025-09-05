**Ancillary repository for dissertation**
>
> **Countdown to Alzheimer’s – A comparative survival analysis of clinical vs miRNA models for predicting MCI→AD conversion**  
> **Aditi Pethia**  
> **Stephenson College, Durham University**  
>  
> A report presented for the degree of **Master of Data Science – Health**  
> **September 2025**

# MCI→AD Prediction (clinical-first) — GSE150693

Leakage-safe survival pipeline for predicting **time-to-conversion** from Mild Cognitive Impairment (MCI) to Alzheimer’s disease (AD), re-analysing the public **GSE150693** dataset.  
**Thesis focus:** a **clinical-first** Cox model (Age, Sex, APOE ε4) vs. miRNA-augmented elastic-net Cox, with strict leakage control, calibration checks, and decision-curve analysis (DCA).

---

## ⭐ Highlights
- **Independent re-analysis**: only the dataset reused; all modelling choices are new.
- **Leakage-safe**: train-only imputation, screening, pruning, and inner CV.
- **One-SE selection**: parsimonious, stable elastic-net models.
- **Full evaluation**: C-index, time-AUC(12/24/36 m), Brier/IBS, calibration slope + 24 m calibration curve, DCA @24 m, bootstrap CIs, PH checks.
- **Clinical-first**: cost-conscious, NHS-style triage framing.

---

## 🧠 Methods (pipeline)
1. **Split**: single stratified train/test (75/25), fixed seed.
2. **Impute**: train-fitted median imputers for clinical & miRNA blocks; apply to both sets.
3. **Clinical CoxPH** baseline (Age, Sex, APOE ε4).
4. **miRNA prep (train-only)**: univariate Cox screen (BH-FDR ≤ 0.20, cap 100) → correlation prune (|r| > 0.85).
5. **Coxnet (elastic-net)**: inner 5-fold CV, `l1_ratio ∈ {1.0, 0.5, 0.2}`, log-spaced alphas → **one-SE** model.
6. **Combined**: stack **clinical linear predictor** + selected miRNAs; refit CoxPH for calibration.
7. **Metrics (test-only)**: C-index; AUC(t) at **safe** horizons (12/24/36 m); Brier/IBS; calibration slope; **24 m** calibration curve; **DCA @24 m**; bootstrap CIs; subgroups; PH checks.

> Rationale: prevents leakage, reduces overfitting, improves calibration/transportability.

---

## 📦 Requirements
Python 3.9–3.11. Minimal deps:
