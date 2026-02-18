# Solver Architecture Overview -- Genesis PROV 5b

**Classification:** NON-CONFIDENTIAL -- Architecture description only. No source code disclosed.

---

## Overview

The Genesis PROV 5b computational pipeline discovers, evaluates, and ranks molecular architectures for critical mineral separation. It consists of four tightly-coupled solver components operating in a sequential pipeline with feedback loops.

This document describes the architecture, inputs, outputs, and physical basis of each component. No solver source code, proprietary algorithms, or patent-protected implementations are included.

---

## Pipeline Architecture

```
 [Combinatorial Generator]
          |
          v
 [DFT Engine] ---------> [ML Surrogate]
          |                     |
          v                     v
 [Kremser Process Model] <--- [Rapid Screening]
          |
          v
 [Candidate Ranking + Economics]
```

### Stage 1: Combinatorial Molecular Generator

**Purpose:** Systematically generate candidate molecular structures from validated building blocks.

**Inputs:**
- Head group library (6 families: DPA, CMPO, malonamide, diglycolamide, and 2 proprietary)
- Tail group library (10 alkyl/fluoroalkyl chains, C6-C16)
- Linker library (4 chemistries: amide, ester, ether, direct bond)
- Property filters (logP window, Lipinski-like rules, synthetic accessibility)

**Outputs:**
- Candidate molecular structures (SMILES, 3D coordinates)
- Property annotations (MW, logP, TPSA, rotatable bonds, SA score)

**Physical Basis:** Combinatorial chemistry principles. Each candidate is a valid organic molecule assembled from known functional groups. RDKit is used for chemical validity checking.

**Scale:** 730 base library + 120 v2 candidates = 850+ unique structures.

---

### Stage 2: DFT Engine

**Purpose:** Compute binding free energies for metal-ligand complexes using quantum chemistry.

**Inputs:**
- 3D molecular structures from Stage 1
- Metal ion specifications (15 lanthanides + Fe, with Shannon ionic radii)
- Solvent environment (aqueous or organic phase)
- Temperature

**Outputs:**
- Binding free energy (Delta-G) for each metal-ligand pair
- Optimized complex geometries
- Electronic structure data (orbital energies, charge distributions)
- Convergence status

**Level of Theory:**
- Reference: CP2K with PBE/DZVP-MOLOPT-PBE-GTH, D3 dispersion
- Production: B3LYP/6-31G*, D3BJ dispersion, CPCM implicit solvation

**Physical Basis:** Density Functional Theory solves the Kohn-Sham equations to obtain the electronic ground state energy. The binding energy is the difference between the complex energy and the sum of isolated metal and ligand energies, corrected for basis set superposition error and solvation.

**Scale:** 58 verified calculations (forensic-grade) + 200+ expanded calculations.

---

### Stage 3: ML Surrogate

**Purpose:** Rapid prediction of binding energies for untested metal-ligand combinations, enabling screening of the full 730-structure library without running thousands of DFT calculations.

**Inputs:**
- Molecular fingerprints: ECFP4 (1,024-bit extended-connectivity fingerprint) + MACCS keys (167-bit structural keys)
- Physicochemical descriptors (15 properties from RDKit)
- Metal properties (ionic radius, charge, electronegativity, hydration enthalpy, coordination number)
- Temperature

**Outputs:**
- Predicted binding free energy
- Prediction uncertainty (from ensemble methods)
- Candidate ranking by predicted selectivity

**Models:**
- Ridge regression (primary, R-squared = 0.966)
- Random Forest (secondary, for uncertainty estimation)
- Gradient Boosting (tertiary, for robustness check)

**Validation:**
- Standard 5-fold cross-validation
- Ligand-out GroupKFold (tests generalization to unseen molecular scaffolds)
- Metal-out GroupKFold (tests generalization to unseen metals)

**Physical Basis:** The ML surrogate learns the mapping from molecular structure (encoded as fingerprints) and metal properties to DFT-computed binding energies. It is a statistical interpolator, not a physics simulator. Its predictions are bounded by the accuracy and coverage of the DFT training data.

---

### Stage 4: Kremser Process Model

**Purpose:** Convert DFT-predicted selectivities into engineering process parameters (stage counts, CapEx, operating economics).

**Inputs:**
- Separation factor (alpha) from DFT binding energy differences
- Feed composition (impurity fractions)
- Product purity target
- Stage efficiency (engineering parameter)
- Unit cost data ($/stage, overhead factors)

**Outputs:**
- Number of theoretical stages (N)
- Practical stage count (with efficiency correction)
- Capital expenditure estimate
- Sensitivity curves (alpha vs. stages vs. CapEx)

**The Kremser Equation:**

```
N = ln(x_feed / x_raff) / ln(alpha)
```

This is a rigorous thermodynamic result for ideal countercurrent extraction. It is exact for binary separations at thermodynamic equilibrium with constant partition coefficients.

**Practical Corrections:**
- Stage efficiency: 80% (reduces effective alpha per stage)
- Safety factor: 1.2x (accounts for non-ideal mixing)
- Practical correction exponent: gamma = 0.3-0.5 (Rydberg 2004), mapping ideal alpha to practical alpha

**Physical Basis:** The Kremser equation derives from mass balance across a countercurrent cascade of equilibrium stages. It is the separation-process analog of the McCabe-Thiele method in distillation. The equation is exact; all uncertainty resides in the input parameters (alpha, efficiency, costs).

---

## Data Flow

```
Building Blocks --> [Generator] --> 730+ Candidates
                                        |
                                   [DFT Engine]
                                        |
                              58 verified energies
                              200+ expanded energies
                                        |
                               [ML Surrogate v8]
                                        |
                          730 predicted selectivities
                                        |
                             [Kremser Model]
                                        |
                     Stage counts + CapEx for each candidate
                                        |
                             [Final Ranking]
```

---

## What Is NOT Disclosed

This architecture overview deliberately excludes:

1. **Source code** for any solver component
2. **Specific molecular structures** (SMILES, SDF, MOL2, PDB files)
3. **Trained model weights** (surrogate_v8_fingerprint.pkl)
4. **DFT input files** (geometry files, basis set parameters beyond what is published)
5. **Patent claim text** or claim language
6. **COGS model internals** (cost curves, pricing data, break-even calculations)
7. **Proprietary head group structures** (2 of the 6 head group families are novel)

These materials are protected by the provisional patent filing (95 claims, 13 families) and are available under license.

---

*Genesis Platform -- Solver Overview -- PROV 5b -- Non-Confidential -- February 2026*
