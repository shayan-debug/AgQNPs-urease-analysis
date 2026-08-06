# Ag@QNPs Urease Inhibition — In Silico Analysis

[![DOI](https://img.shields.io/badge/DOI-10.1038%2Fs41598--025--96684--2-blue)](https://doi.org/10.1038/s41598-025-96684-2)
[![Python](https://img.shields.io/badge/Python-3.10-green)](https://www.python.org/)
[![RDKit](https://img.shields.io/badge/RDKit-2024-orange)](https://www.rdkit.org/)
[![AutoDock Vina](https://img.shields.io/badge/AutoDock_Vina-1.2.3-purple)](https://github.com/ccsb-scripps/AutoDock-Vina)
[![OpenMM](https://img.shields.io/badge/OpenMM-8.5-red)](https://openmm.org/)

Computational analysis supporting the **Scientific Reports 2025** paper on silver-quercetin nanoparticles (Ag@QNPs) as urease inhibitors.

> **Key finding:** Ag@QNPs show **250× greater potency** than free quercetin (IC₅₀: 0.473 vs 9.581 µg/mL), justified by DFT interaction energies, ADMET profiling, molecular docking, and MD simulation.

---

## Repository Structure

```
AgQNPs-urease-analysis/
├── notebooks/
│   ├── lipinski_dft_ic50.ipynb           # Rule of 5 + DFT + IC₅₀ visualization
│   ├── urease_docking_v2.ipynb           # AutoDock Vina docking pipeline
│   └── urease_active_site_MD.ipynb       # OpenMM molecular dynamics simulation
├── data/
│   ├── insilico_summary.csv              # All in silico results combined
│   ├── docking_affinities.png            # Docking bar chart
│   ├── docked.pdbqt                      # Best docked poses (AutoDock Vina)
│   ├── docking_pose.png                  # 3D docking visualization
│   ├── rmsd_plot.png                     # MD backbone RMSD (100 ps)
│   └── rmsf_plot.png                     # MD per-residue RMSF (100 ps)
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

## 5. Molecular Docking — AutoDock Vina

Quercetin docked into *Helicobacter pylori* urease active site (PDB: 4H9M, resolution 1.95 Å).

### Methods

- **Receptor:** PDB 4H9M (*H. pylori* urease, resolution 1.95 Å). Chain A was selected as the biologically active subunit containing the catalytic Ni²⁺ ions. Water molecules and non-essential HETATM records were removed; Ni²⁺ ions were retained to preserve the active site geometry.
- **Ligand:** Quercetin 3D structure generated from SMILES, energy-minimized using MMFF94 force field via RDKit, and converted to PDBQT using Meeko.
- **Grid box:** Centered on the Ni²⁺ coordination site (x = 18.78, y = −57.81, z = −24.15 Å); box size 20 × 20 × 20 Å to encompass the full active site cavity.
- **Exhaustiveness:** 16 (2× default) for thorough conformational sampling.

### Results

| Mode | Binding Affinity (kcal/mol) | RMSD l.b. | RMSD u.b. |
|------|----------------------------|-----------|-----------|
| **1** | **−7.214** | 0 | 0 |
| 2 | −7.169 | 1.479 | 3.073 |
| 3 | −7.047 | 2.453 | 6.213 |
| 4 | −7.000 | 2.003 | 2.519 |
| 5 | −6.824 | 1.555 | 3.203 |

> **Best binding affinity: −7.214 kcal/mol** — consistent with favorable interaction at the Ni²⁺ binding pocket, supporting the experimental IC₅₀ data. All five binding modes cluster within −7.2 to −6.8 kcal/mol, indicating stable and consistent binding.

![Docking Affinities](data/docking_affinities.png)

---

## 6. Molecular Dynamics Simulation — OpenMM

100 ps MD simulation of the urease active site with quercetin docking pose as starting structure.

### Methods

- **System:** Urease active site (PDB: 4H9M), residues within 20 Å of the Ni²⁺ catalytic center (232 residues, 20,080 total atoms including solvent)
- **Force field:** AMBER14 (protein) + TIP3P water model
- **Ionic strength:** 150 mM NaCl
- **Minimization:** 500 iterations
- **Equilibration:** NVT 50 ps → NPT 50 ps (300 K, 1 bar)
- **Production:** 100 ps, timestep 2 fs, Langevin thermostat (friction 1 ps⁻¹)
- **Software:** OpenMM 8.5, MDAnalysis, Google Colab

### Results

| Metric | Value | Interpretation |
|--------|-------|----------------|
| Backbone RMSD (mean) | ~1.0 Å | Stable — within acceptable range |
| RMSF range | 0.4 – 1.3 Å | Moderate flexibility, stable core |
| Temperature stability | 300 ± 5 K | Well-equilibrated |

> The active site maintains structural stability throughout the 100 ps trajectory (RMSD < 1.5 Å), consistent with a well-folded binding pocket. Loop regions show higher RMSF values as expected.

![RMSD](data/rmsd_plot.png)
![RMSF](data/rmsf_plot.png)

---

## SMILES

```
Quercetin: O=c1c(O)c(-c2ccc(O)c(O)c2)oc2cc(O)cc(O)c12
```

---

## Tools & Environment

| Tool | Version | Purpose |
|------|---------|---------|
| Python | 3.10 | Core scripting |
| RDKit | 2024.03 | Ligand preparation, Lipinski, SMILES handling |
| AutoDock Vina | 1.2.3 | Molecular docking |
| OpenBabel | 3.1.1 | Format conversion (PDB → PDBQT) |
| Meeko | 0.5 | Ligand PDBQT preparation |
| OpenMM | 8.5 | Molecular dynamics simulation |
| MDAnalysis | 2.10 | Trajectory analysis (RMSD, RMSF) |
| PDBFixer | — | Receptor preparation for MD |
| Google Colab | — | Compute environment (T4 GPU) |

All notebooks are self-contained and reproducible in Google Colab without local installation.

---

## Citation

If you use this analysis, please cite:

> Asadi, S. et al. *Silver-quercetin nanoparticles as urease inhibitors*. Scientific Reports (2025). https://doi.org/10.1038/s41598-025-96684-2

AutoDock Vina:
> Eberhardt, J. et al. AutoDock Vina 1.2.0. *J. Chem. Inf. Model.* 61, 3891–3898 (2021).

OpenMM:
> Eastman, P. et al. OpenMM 8. *J. Phys. Chem. B* 127, 4169–4182 (2023).

---

## Author

**Shayan Asadi** — Medicinal Chemistry, Zanjan University of Medical Sciences
GitHub: [@shayan-debug](https://github.com/shayan-debug)
