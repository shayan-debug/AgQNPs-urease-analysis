# Ag@QNPs Urease Inhibition — In Silico Analysis

[![DOI](https://img.shields.io/badge/DOI-10.1038%2Fs41598--025--96684--2-blue)](https://doi.org/10.1038/s41598-025-96684-2)
[![Python](https://img.shields.io/badge/Python-3.10-green)](https://www.python.org/)
[![RDKit](https://img.shields.io/badge/RDKit-2024-orange)](https://www.rdkit.org/)

Computational analysis supporting the **Scientific Reports 2025** paper on silver-quercetin nanoparticles (Ag@QNPs) as urease inhibitors.

> **Key finding:** Ag@QNPs show **250× greater potency** than free quercetin (IC₅₀: 22.4 vs 5610 µg/mL), justified by both DFT interaction energies and ADMET profiling.

---

## Repository Structure

```
AgQNPs-urease-analysis/
├── notebooks/
│   ├── lipinski_analysis.ipynb     # Rule of 5 + RDKit
│   ├── dft_interaction_plot.ipynb  # Singlet/Doublet energy plot
│   └── ic50_comparison.ipynb       # Potency visualization
├── data/
│   └── swissadme_quercetin.csv     # ADMET results
└── README.md
```

---

## 1. Lipinski Rule of 5 — Quercetin

Calculated using **RDKit** in Google Colab:

| Parameter | Value | Threshold | Status |
|-----------|-------|-----------|--------|
| Molecular Weight | 302.24 g/mol | ≤ 500 | ✅ |
| LogP | 0.79 | ≤ 5 | ✅ |
| H-Bond Donors (HBD) | 5 | ≤ 5 | ⚠️ borderline |
| H-Bond Acceptors (HBA) | 7 | ≤ 10 | ✅ |

> Quercetin passes Lipinski but sits at the **boundary of HBD**, indicating limited oral drug-likeness as a free molecule.

---

## 2. DFT Interaction Energies

Spin-state analysis of Ag@QNPs complex:

| Spin State | Interaction Energy |
|------------|-------------------|
| Singlet | −53.79 kcal/mol |
| Doublet | −50.77 kcal/mol |

Singlet state shows stronger binding, consistent with experimental urease inhibition data.

---

## 3. IC₅₀ Comparison

| Compound | IC₅₀ (µg/mL) | Relative Potency |
|----------|--------------|-----------------|
| Quercetin (free) | 5610 | 1× |
| **Ag@QNPs** | **22.4** | **250×** |

---

## 4. ADMET Profiling — SwissADME

Quercetin profiled at [swissadme.ch](http://www.swissadme.ch):

### Physicochemical Properties

| Parameter | Value |
|-----------|-------|
| Formula | C₁₅H₁₀O₇ |
| Molecular Weight | 302.24 g/mol |
| TPSA | 131.36 Ų |
| Rotatable Bonds | 1 |
| H-Bond Donors | 5 |
| H-Bond Acceptors | 7 |
| Consensus LogP | 0.79 |

### Druglikeness

| Rule | Result |
|------|--------|
| Lipinski | ✅ Yes — 0 violations |
| Ghose | ✅ Yes |
| Veber | ✅ Yes |
| Egan | ✅ Yes |
| Muegge | ✅ Yes |
| **Bioavailability Score** | **0.55** |

### Pharmacokinetics

| Parameter | Value |
|-----------|-------|
| GI Absorption | High |
| BBB Permeant | No |
| P-gp Substrate | No |
| Log Kp (skin) | −7.01 cm/s |
| Water Solubility (ESOL) | Soluble (Log S = −3.19) |

### Medicinal Chemistry Alerts

| Alert | Detail |
|-------|--------|
| PAINS | 1 alert: catechol_A |
| Brenk | 1 alert: catechol |
| Leadlikeness | Yes |
| Synthetic Accessibility | 3.23 / 10 |

> **Interpretation:** Quercetin passes all druglikeness filters but consistently sits at the boundary (TPSA = 131.36 Ų vs Egan cutoff 131.6 Ų; HBD = 5 at Lipinski limit). The catechol PAINS alert explains potential assay interference in free form. **Ag@QNPs bypasses these limitations**, delivering 250× potency improvement through nanoparticle encapsulation.

---

## SMILES

```
Quercetin: O=c1c(O)c(-c2ccc(O)c(O)c2)oc2cc(O)cc(O)c12
```

---

## Citation

If you use this analysis, please cite:

> Asadi, S. et al. *Silver-quercetin nanoparticles as urease inhibitors*. Scientific Reports (2025). https://doi.org/10.1038/s41598-025-96684-2

---

## Author

**Shayan Asadi** — Medicinal Chemistry  
GitHub: [@shayan-debug](https://github.com/shayan-debug)
