# Honest Disclosures -- Genesis PROV 5b: Critical Mineral Separation

**Purpose:** This document provides a complete and transparent accounting of the limitations, assumptions, and uncertainties in the PROV 5b Janus Ligand computational discovery. We believe honest disclosure strengthens credibility and enables informed technical evaluation.

---

## 1. All Results Are Computational

**No Janus Ligand has been synthesized.** The 730 molecular structures in our library exist as computational designs (SMILES strings, 3D coordinates from geometry optimization) -- not as physical compounds in a vial. No extraction experiment has been performed. No pilot plant has been built. No product purity has been measured.

The single most impactful next step is wet-lab synthesis of the top 3 candidate structures and testing of extraction performance in a laboratory-scale mixer-settler or centrifugal contactor. Estimated cost: approximately $50,000 over 4 weeks at a university chemistry partnership.

Until that synthesis is completed, all performance claims carry the qualifier: "computationally predicted."

---

## 2. DFT Method and Accuracy

### Level of Theory

All DFT calculations use one of two levels of theory:

- **58 verified calculations:** CP2K with PBE functional, DZVP-MOLOPT-PBE-GTH basis set, D3 dispersion correction. These are the forensically-audited anchor points with cloud task IDs.
- **200+ expanded calculations:** B3LYP functional with 6-31G* basis set, D3BJ dispersion correction, CPCM implicit solvation. These are calibrated against the 58 CP2K reference points.

### Known DFT Limitations

- **Absolute accuracy:** B3LYP/6-31G* binding energies for organometallic systems carry typical errors of 5-15 kJ/mol. This is well-documented in the computational chemistry literature.
- **Relative ranking:** Relative comparisons (which ligand binds more strongly to which metal) are more reliable than absolute binding energy values. Our Kremser model uses relative selectivities, partially mitigating this limitation.
- **Basis set superposition error (BSSE):** The 6-31G* basis set is modest by modern standards. Larger basis sets (def2-TZVP, aug-cc-pVTZ) would improve accuracy but at significantly higher computational cost.
- **Implicit solvation:** CPCM models the solvent as a dielectric continuum. It does not capture specific solvent-solute interactions, hydrogen bonding networks, or explicit first-shell solvation effects. Explicit-solvent MD simulations would improve accuracy for binding energy predictions.
- **Static structures:** DFT calculations are performed on optimized static geometries. Thermal fluctuations and conformational dynamics are not captured. Molecular dynamics simulations would provide ensemble-averaged properties.

### What This Means

The DFT results are suitable for identifying promising molecular architectures and ranking candidates. They are not suitable for quantitative prediction of extraction plant performance without experimental calibration.

---

## 3. Kremser Model Is First-Principles Thermodynamic

The Kremser equation (N = ln(x_feed/x_raff) / ln(alpha)) is a rigorous thermodynamic result for countercurrent extraction. It is not an empirical fit. However, the inputs to the equation carry uncertainty:

### Separation Factor Derivation

The separation factor (alpha) is derived from DFT binding energies through a multi-step chain:

1. DFT binding energies for Janus Ligand-Nd and Janus Ligand-Fe complexes
2. Isodesmic reaction analysis relative to TBP reference
3. Boltzmann conversion to ideal separation factor (alpha_ideal = 56.5)
4. Practical correction factor (gamma = 0.3-0.5 from Rydberg 2004)
5. Final practical alpha = 3.3-7.5

Each step introduces uncertainty. The practical correction factor (gamma) accounts for non-ideal mixer-settler behavior but is itself an estimate with uncertainty of +/- 0.1.

### Assumptions in the CapEx Model

- Stage efficiency: 80% (assumed, not measured for Janus Ligands)
- Mixer-settler cost: $5M per stage at 1,000 tpa (MINTEK 2023 reference, may not reflect current market)
- Auxiliary equipment overhead: 40% of mixer-settler cost
- Feed composition: Fe:Nd ratio of 99:1 (typical NdFeB scrap, but varies by feedstock)
- Product purity: 99.9% Nd (defense-grade specification)

### The 70.8% CapEx Claim

The headline figure of 70.8% CapEx reduction assumes the full DFT-predicted thermodynamic selectivity translates to practical performance. This is the **optimistic upper bound**. The defensible range from the Kremser sensitivity analysis is:

| Scenario | Alpha | CapEx Reduction |
|----------|-------|----------------|
| Conservative | 3.35 | 20% |
| Moderate | 5.02 | 30% |
| Optimistic | 7.52 | 40% |

The 70.8% figure requires alpha >> 7.5, which corresponds to the raw thermodynamic selectivity before practical correction. It should be treated as a theoretical limit, not an expected outcome.

---

## 4. ML Surrogates Trained on Simulated Data

The machine learning surrogate model (Ridge regression, R-squared = 0.966) is trained entirely on DFT-computed binding energies. It has never seen an experimental data point. Its accuracy is bounded by the accuracy of the underlying DFT calculations.

### Training Data Limitations

- **58 verified samples** form the anchor dataset. This is a small training set by ML standards.
- **200+ expanded calculations** increase coverage but use calibrated physics models, not raw CP2K output. True model improvement requires HPC cluster runs to generate additional high-fidelity data.
- **Chemical space coverage:** The 730-structure library covers a specific region of chemical space (DPA-class chelating heads with alkyl tails). The model should not be extrapolated to chemically dissimilar extractant families.

### v7 to v8 Fix

The v7 ML model had a critical bug: it used one-hot ligand encoding, causing identical predictions for different ligands complexed with the same metal. This was a fundamental overfitting failure. The v8 model uses molecular fingerprints (ECFP4 + MACCS) and demonstrates proven ligand differentiation. The R-squared = 0.966 figure is from the corrected v8 model.

---

## 5. 730 Structures Are Computationally Designed, Not Synthesized

The 730-structure molecular library was generated by combinatorial assembly of validated building blocks (6 head groups, 10 tails, 4 linkers) followed by computational filtering for:

- Chemical validity (RDKit sanitization)
- Synthetic accessibility score
- LogP within the 3.0-6.0 operating window
- Lipinski-like molecular property constraints

These filters assess computational plausibility, not synthetic feasibility. A computationally valid molecule may be difficult, expensive, or impossible to synthesize in practice. Synthetic accessibility scores are heuristic estimates, not guarantees.

---

## 6. No Pilot Plant Data

The Kremser economic model predicts capital expenditure based on stage count and unit costs. It does not account for:

- Third-phase formation (a common failure mode in solvent extraction)
- Crud and emulsion handling
- Solvent degradation and regeneration costs
- Environmental permitting costs
- Actual kinetics (extraction rate, phase separation time)
- Scale-up non-linearities

A pilot-plant trial (estimated cost: $500K-$2M, 6-12 months) would be required to validate the Kremser predictions against physical reality.

---

## 7. Inter-Lanthanide Separation

The Janus Ligand selectivity data presented in this repository focuses primarily on the **Nd/Fe pair** (the dominant separation challenge from NdFeB scrap feedstock). Inter-lanthanide separations (Nd/Pr, Dy/Tb, Eu/Gd) are inherently harder due to the smaller ionic radius differences between adjacent lanthanides.

The 200+ DFT dataset covers all 15 lanthanides + Fe, but the Kremser analysis and CapEx projections are calibrated for Nd/Fe. Lower separation factors should be expected for adjacent-lanthanide pairs.

---

## 8. Summary of Confidence Levels

| Claim | Confidence | Basis | What Would Change It |
|-------|-----------|-------|---------------------|
| 730 valid molecular structures | HIGH | RDKit validation, 730/730 pass | Synthesis attempts |
| 58/58 DFT converged | HIGH | Forensic audit with task IDs | Independent recomputation |
| Kremser equation correctness | HIGH | Textbook thermodynamics | Nothing (it is exact) |
| Janus SF > 10,000 (thermodynamic) | MODERATE | DFT binding energy difference | Higher-level QM methods |
| Practical alpha = 3.3-7.5 | MODERATE | Kremser + correction factors | Experimental extraction tests |
| CapEx reduction 20-40% | MODERATE | Kremser model | Pilot plant data |
| CapEx reduction 70.8% | LOW | Assumes full DFT selectivity | Almost certainly optimistic |
| ML R-squared = 0.966 | HIGH | Held-out test data | More training data |
| Commercial viability | LOW | Model-based projections | Market conditions, actual costs |

---

*Genesis Platform -- Honest Disclosures -- PROV 5b -- February 2026*
