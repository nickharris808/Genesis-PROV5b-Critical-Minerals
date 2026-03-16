# Honest Disclosures -- Genesis PROV 5b: Critical Mineral Separation

**Purpose:** This document provides a complete and transparent accounting of the limitations, assumptions, and uncertainties in the PROV 5b Janus Ligand computational discovery. We believe honest disclosure strengthens credibility and enables informed technical evaluation.

---

## 1. All Results Are Computational

**No Janus Ligand has been synthesized.** The 405 molecular structures in our expanded library exist as computational designs (SMILES strings, 3D coordinates from geometry optimization) -- not as physical compounds in a vial. No extraction experiment has been performed. No pilot plant has been built. No product purity has been measured.

The single most impactful next step is wet-lab synthesis of the top 3 candidate structures and testing of extraction performance in a laboratory-scale mixer-settler or centrifugal contactor. Estimated cost: approximately $50,000 over 4 weeks at a university chemistry partnership.

Until that synthesis is completed, all performance claims carry the qualifier: "computationally predicted."

---

## 2. DFT Method and Accuracy

### Level of Theory

All DFT calculations use one of two levels of theory:

- **58 verified calculations:** CP2K with PBE functional, DZVP-MOLOPT-SR-GTH basis set. No dispersion correction was applied to the 58 verified runs (D3 exists in template_publication.inp but was never used). These are the forensically-audited anchor points with cloud task IDs.
- **166 expanded estimates:** These are SYNTHETIC FORMULA-GENERATED values using a calibrated analytical Coulomb+LJ+Born model, NOT quantum DFT calculations. No electronic structure calculation is performed for these entries. The original label of "B3LYP/6-31G*" was misleading and has been corrected. These should be understood as parametric extrapolations from the 58 CP2K anchors, not independent quantum mechanical results.

### La Used as Nd Proxy -- Systematic Error Unknown

**All 58 verified CP2K calculations use Lanthanum (La), not Neodymium (Nd).** The Nd CP2K runs all aborted and were never completed. La was chosen as a proxy because La and Nd have similar ionic radii (La3+: 1.032 A, Nd3+: 0.983 A).

However, La and Nd have fundamentally different electronic structures:
- La has no f-electrons (4f0); Nd has three (4f3)
- f-electron contributions to bonding are not captured by the La proxy
- The systematic error introduced by this substitution is **unknown** and has not been quantified
- Claims about "Nd selectivity" are actually "La selectivity" measurements projected onto Nd

### Known DFT Limitations

- **Absolute accuracy:** The 58 verified CP2K PBE/DZVP calculations carry typical DFT errors of 5-15 kJ/mol for organometallic systems. The 166 expanded estimates use calibrated physics models (not quantum DFT) and may have larger systematic errors.
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

1. DFT binding energies for Janus Ligand-La (not Nd) and Janus Ligand-Fe complexes
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

## 4. ML Surrogate Has No Predictive Power for Unseen Scaffolds

The machine learning surrogate model (Ridge regression, R-squared = 0.966) is trained entirely on DFT-computed binding energies. It has never seen an experimental data point. Its accuracy is bounded by the accuracy of the underlying DFT calculations.

### Critical Limitation: One-Hot Encoding = Lookup Table

The ML model uses a combination of one-hot encoding and molecular fingerprints (ECFP4, MACCS). The one-hot component means the model is partially a lookup table -- it memorizes ligand-metal associations rather than learning transferable chemical patterns. For any ligand not in the training set, the one-hot features are all zero, and the model relies entirely on molecular fingerprints, which have not been validated for out-of-distribution prediction.

**The model has no demonstrated predictive power for unseen scaffold families.** The R-squared = 0.966 reflects interpolation within the training distribution, not generalization.

### Training Data Limitations

- **58 verified samples** form the anchor dataset. This is a small training set by ML standards.
- **166 expanded estimates** increase coverage but are SYNTHETIC FORMULA-GENERATED values (analytical Coulomb+LJ+Born), not raw CP2K output. Training an ML model on synthetic data generated by a simple formula, then claiming ML "predictions," is circular.
- **Chemical space coverage:** The 405-structure expanded library covers a specific region of chemical space (DPA-class chelating heads with alkyl tails). The model should not be extrapolated to chemically dissimilar extractant families.

### v7 to v8 Fix

The v7 ML model had a critical bug: it used one-hot ligand encoding, causing identical predictions for different ligands complexed with the same metal. This was a fundamental overfitting failure. The v8 model uses molecular fingerprints (ECFP4 + MACCS) and demonstrates proven ligand differentiation. The R-squared = 0.966 figure is from the corrected v8 model.

---

## 5. 405 Structures Are Computationally Designed, Not Synthesized

The 405-structure expanded molecular library (`expanded_candidates.sdf`) was generated through combinatorial assembly of validated building blocks followed by computational filtering for:

- Chemical validity (RDKit sanitization)
- Synthetic accessibility score
- LogP within the 3.0-6.0 operating window
- Lipinski-like molecular property constraints

These filters assess computational plausibility, not synthetic feasibility. A computationally valid molecule may be difficult, expensive, or impossible to synthesize in practice. Synthetic accessibility scores are heuristic estimates, not guarantees.

---

## 6. GROMACS: Only Energy Minimization, Not Production MD

The GROMACS simulation results in this repository consist of **10 ps of steepest-descent energy minimization only**. This is NOT production molecular dynamics. Specifically:

- No equilibration (NVT or NPT) was performed
- No production trajectory was generated
- No statistically converged thermodynamic properties were obtained
- No free energy calculations from GROMACS are available (PMF values attributed to GROMACS are from umbrella sampling at 0.5 ns/window for Li+ and K+ only; Na+ used the Born analytical model because GROMACS returned NaN)

Claims based on "GROMACS molecular dynamics" should be understood as energy minimization geometry optimization, not time-evolved molecular simulation.

---

## 7. No Pilot Plant Data

The Kremser economic model predicts capital expenditure based on stage count and unit costs. It does not account for:

- Third-phase formation (a common failure mode in solvent extraction)
- Crud and emulsion handling
- Solvent degradation and regeneration costs
- Environmental permitting costs
- Actual kinetics (extraction rate, phase separation time)
- Scale-up non-linearities

A pilot-plant trial (estimated cost: $500K-$2M, 6-12 months) would be required to validate the Kremser predictions against physical reality.

---

## 8. Inter-Lanthanide Separation

The Janus Ligand selectivity data presented in this repository focuses primarily on the **Nd/Fe pair** (actually La/Fe -- see Section 2), the dominant separation challenge from NdFeB scrap feedstock. Inter-lanthanide separations (Nd/Pr, Dy/Tb, Eu/Gd) are inherently harder due to the smaller ionic radius differences between adjacent lanthanides.

The 166 formula-generated dataset covers all 15 lanthanides + Fe, but these are analytical estimates, not CP2K results. The Kremser analysis and CapEx projections are calibrated for La/Fe (used as proxy for Nd/Fe). Lower separation factors should be expected for adjacent-lanthanide pairs.

---

## 9. Summary of Confidence Levels

| Claim | Confidence | Basis | What Would Change It |
|-------|-----------|-------|---------------------|
| 405 valid expanded molecular structures | HIGH | RDKit validation, 405/405 pass | Synthesis attempts |
| 58/58 DFT converged (La proxy) | HIGH | Forensic audit with task IDs | Independent recomputation with actual Nd |
| 166 "expanded DFT" are formula-generated | FACT | Code inspection | Nothing (this is what the code does) |
| Kremser equation correctness | HIGH | Textbook thermodynamics | Nothing (it is exact) |
| Janus SF > 10,000 (thermodynamic, La not Nd) | MODERATE | DFT binding energy difference (La proxy) | Higher-level QM with actual Nd |
| Practical alpha = 3.3-7.5 | MODERATE | Kremser + correction factors | Experimental extraction tests |
| CapEx reduction 20-40% | MODERATE | Kremser model | Pilot plant data |
| CapEx reduction 70.8% | LOW | Assumes full DFT selectivity | Almost certainly optimistic |
| ML R-squared = 0.966 | HIGH (within training set) | Held-out test data | Out-of-distribution testing |
| ML predicts unseen scaffolds | LOW | No evidence | Novel scaffold DFT validation |
| GROMACS production MD completed | FALSE | Only 10ps energy minimization exists | Actual production MD runs |
| Commercial viability | LOW | Model-based projections | Market conditions, actual costs |

---

*Genesis Platform -- Honest Disclosures -- PROV 5b -- February 2026*
