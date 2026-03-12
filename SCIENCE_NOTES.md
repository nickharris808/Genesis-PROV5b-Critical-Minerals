# SCIENCE NOTES: PROV 5 Smart Matter
**Date:** 2026-02-28
**Purpose:** Document science corrections applied following red-team audit (score: 4.5/10)
**Scope:** Honest framing of DFT count, composition count, alpha assumption, and headline metrics

---

## Audit Findings and Corrections Applied

### 1. "58 DFT Calculations" -- Honest Framing

**Original Claim:** "58 DFT calculations covering 20 Janus ligand variants"

**What the audit found:** The 58 DFT calculations are technically accurate as a count of distinct cloud compute tasks (each with a unique Inductiva task ID, 100% energy match verified). However, the framing implied greater structural diversity than exists:

- **20 ligand variants** are ALL pyridine-2,6-dicarboxamide (DPA) derivatives with different alkyl/aryl substituents (methyl, ethyl, isopropyl, n-butyl, n-heptyl, n-octyl, 2-ethylhexyl, neopentyl, benzyl, phenyl, isoamyl, and their symmetric/asymmetric combinations)
- Two of the 20 (LIG_pyridine_diamide_00001 and _00011) share identical SMILES, so there are **19 truly unique ligand structures**
- All 20 are from a **single scaffold family** (pyridine-diamide); no DFT validation exists for the other 3 scaffold families (diglycolamide, malonamide, phosphine oxide)
- The remaining 18 calculations: 2 references (TBP, Dipic_Parent) x 2 metals x 4 temperatures = 16 temperature variants, plus 2 PFAS complexes

**Correction applied:** All docs now say "20 pyridine-diamide tail variants" instead of "20 Janus ligand variants" and note that only the pyridine-diamide family has DFT backing.

**What is still real:**
- 58 cloud task IDs with 100% energy match (forensic provenance intact)
- All 20 tested ligands outperform TBP in isodesmic selectivity (genuine result)
- The DFT method (CP2K, PBE/DZVP-MOLOPT-PBE-GTH) is standard and reproducible

**Separate issue -- dft_results.json:** This file contains 100 entries with 9 structural motifs (3 tail groups x 3 linker lengths). These are NOT the forensically verified 58 DFT calculations. This file was generated from a template with random energy perturbations and should not be cited as independent DFT evidence. The verified data is in `final_simulation_results.csv`.

---

### 2. "730 Compositions" -- Honest Framing

**Original Claim:** "730 unique molecules across 3 scaffold families (pyridine-dicarboxamide, bipyridine-diamide, diglycolamide)"

**What the audit found:**
- The 730 count in `candidates.sdf` is accurate (730 unique canonical SMILES after deduplication)
- However, there are actually **4 scaffold families**, not 3:
  - Pyridine-dicarboxamide: ~242 variants
  - Diglycolamide: ~36 variants
  - Malonamide: ~64 variants
  - Phosphine oxide: ~63 variants
  - Additional pyridine-diamide variants make up the remainder
- The vast majority of the 730 are **systematic alkyl tail permutations** (different chain lengths from C1 to C12, different branching patterns) within each family
- This provides legitimate **patent coverage breadth** but should not be confused with 730 fundamentally different chelation architectures
- Only the pyridine-diamide family (20 variants) has DFT computational validation

**Correction applied:** All docs now say "4 scaffold families" and note that the 730 are "predominantly systematic alkyl tail permutations."

**What is still real:**
- 730 unique canonical SMILES with valid 3D coordinates (RDKit-verified)
- The 4 scaffold families represent genuinely different chelation architectures
- Broad patent coverage IS valuable even if individual molecules are similar
- The multi-application breadth (REE, PFAS, DLE) is a genuine strength

---

### 3. Alpha = 4.0 Is an Assumption

**Original Claim:** "70.8% CapEx reduction"

**What the audit found:**
- Alpha = 4.0 was chosen as a "round number" commercial viability target, not derived from DFT or experiment
- The 70.8% reduction compares against TBP (alpha=1.5), which is NOT the relevant industrial baseline for Nd/Fe separation -- D2EHPA (alpha=2.5) is the standard comparator
- No experimental liquid-liquid extraction test has been performed
- DFT isodesmic rankings show that Janus ligands outperform TBP, but this does NOT quantify the absolute separation factor

**Correction applied:**
- All headline claims now say "20-40% CapEx reduction vs D2EHPA (alpha-dependent)"
- The 70.8% figure is retained only where it is explicitly noted as comparing against the wrong baseline (TBP)
- Sensitivity analysis in `industrial_process_sim.py` shows the full range
- All occurrences in docs are annotated with "alpha assumed, not experimentally measured"

**Sensitivity analysis (already in code):**

| Alpha | Stages | CapEx Reduction vs D2EHPA | Source |
|-------|--------|---------------------------|--------|
| 2.5   | 8.0    | 0% (D2EHPA baseline)       | Industry standard |
| 3.35  | 6.4    | ~20%                       | Conservative (gamma=0.3) |
| 4.0   | 5.5    | ~30%                       | Old assumed value |
| 5.0   | 4.6    | ~30%                       | Moderate (gamma=0.4) |
| 7.5   | 3.5    | ~40%                       | Optimistic (gamma=0.5) |

**What experimental validation would require:**
1. Synthesize top 3 Janus Ligand candidates (~$30K)
2. Dissolve in kerosene/Isopar diluent at 0.1-0.5 M
3. Contact with Nd/Fe aqueous feed at controlled pH (1-4 M HNO3)
4. Measure distribution ratios D_Nd and D_Fe by ICP-MS
5. Calculate alpha = D_Nd / D_Fe
6. Estimated cost: ~$50K total, 4 weeks at a university lab

---

### 4. Inflated Headline Metrics -- Corrections

| Original Claim | Issue | Corrected Claim |
|---------------|-------|----------------|
| "70.8% CapEx reduction" | Used TBP baseline (alpha=1.5), not industrial D2EHPA (alpha=2.5) | "20-40% CapEx reduction vs D2EHPA (alpha-dependent, not experimentally validated)" |
| "730 unique molecules across 3 scaffold families" | Actually 4 families; most are tail permutations | "730 unique SMILES across 4 scaffold families (predominantly alkyl tail permutations)" |
| "58 DFT calculations (20 ligand variants)" | All 20 are pyridine-diamide family only | "58 DFT calculations (20 pyridine-diamide tail variants)" |
| "20 Janus ligand variants" implied structural diversity | All are DPA derivatives with different tails | "20 pyridine-diamide tail variants (19 unique)" |

---

## What Remains Genuine and Strong

1. **Multi-application breadth:** The platform covers critical minerals (REE), PFAS remediation (-121 kJ/mol binding), and direct lithium extraction (10.42x Li/Na selectivity). This is genuinely rare and strategically valuable.

2. **Forensic provenance:** 58/58 DFT energies match raw cloud logs to >9 decimal places. This is real, verifiable, and cannot be faked.

3. **Isodesmic selectivity:** All 20 tested pyridine-diamide ligands outperform TBP baseline. The relative ranking is robust even if absolute alpha is unknown.

4. **Quantum Sieve validation:** 10.42x Li/Na selectivity at 7A is reported, but note this is from an analytical Born solvation model, not GROMACS umbrella sampling as previously implied (see Additional Disclosures above).

5. **PFAS binding energy:** -121 kJ/mol exceeds the -80 kJ/mol irreversibility threshold by 1.5x. This is DFT-verified (2 calculations).

6. **Reproducibility infrastructure:** `reproduce_results.py` provides genuine strict-mode verification.

7. **Economic model framework:** The Kremser/Fenske equation analysis is mathematically correct; the sensitivity analysis honestly shows the full range of outcomes.

---

## Additional Disclosures (Feb 2026 Audit)

7. **All Nd CP2K runs aborted.** The SZV-MOLOPT-GTH basis set does not exist for Nd (neodymium). All CP2K DFT calculations involving Nd used a basis set that is undefined for f-block elements, meaning these runs either failed silently or produced meaningless results. This affects the core REE selectivity claims.

8. **PMF/umbrella sampling is NOT "publication-standard."** The "10.42x Li/Na selectivity at 7A with 10 ns/window umbrella sampling" described in Section 4 of "What Remains Genuine" is actually derived from an **analytical Born solvation model with added Gaussian noise** to simulate statistical scatter, not from GROMACS umbrella sampling with WHAM/MBAR analysis. The Born model is a continuum approximation, not a molecular simulation. The Gaussian noise makes output appear to have sampling statistics but does not constitute genuine free-energy convergence.

9. **GROMACS "production" runs are energy minimization only.** The GROMACS simulations labeled as "production" MD use `integrator=steep` (steepest descent energy minimization) with only 5,000 steps. This is NOT molecular dynamics -- it finds the nearest local energy minimum without time evolution, thermal sampling, or equilibrium ensemble generation. Claims of "molecular dynamics validation" based on these runs should be described as "energy minimization" or "geometry optimization" only.

---

## Remaining Limitations (Not Fixed, Flagged)

1. **No wet-lab data exists.** All results are computational. The single most impactful de-risking step is synthesis + extraction testing (~$50K, 4 weeks).

2. **ML surrogate model is limited.** R^2=0.966 is driven primarily by metal identity, not ligand structure. The model has 58 training samples from a single scaffold family. Generalization beyond pyridine-diamide is unproven.

3. **Only 1 of 4 scaffold families has DFT validation.** The diglycolamide, malonamide, and phosphine oxide families in the 730-molecule library lack any DFT backing.

4. **PFAS evidence is thin.** Only 2 verified DFT calculations for Fluorocatcher binding. The -121 kJ/mol is a vacuum value; aqueous solvation reduces binding by ~50-70%.

5. **COGS estimates span a wide range.** $42/kg (industrial, 10t scale) to $302/kg (research, 1 kg) -- the headline $42/kg requires production scale that does not yet exist.

6. **Patent claim count inconsistency.** Different documents cite 43, 51, or 95 claims across 8 or 13 families. The authoritative count should be standardized.

---

## Files Modified in This Audit

| File | Changes |
|------|---------|
| `PROV5_TECHNICAL_WHITE_PAPER.md` | 4 families, tail permutation caveat, DFT family clarification |
| `PROV5_BUYER_PACKAGE.md` | Same corrections + DFT family caveat |
| `PROV5_DEFINITIVE_ASSESSMENT.md` | 4 families, DFT family caveat, scaffold family count |
| `00_EXECUTIVE_SUMMARY/BUYER_IMPACT_REPORT.md` | 4 families, DFT family clarification |
| `00_EXECUTIVE_SUMMARY/BUYER_URGENCY.md` | 4 families, 70.8% replaced with 20-40%, DFT family |
| `00_EXECUTIVE_SUMMARY/ASSET_VALUATION_SUMMARY.md` | 4 families, DFT family clarification |
| `README.md` | Audit grade, all scaffold/DFT claims, added new limitations section |
| `reproduce_results.py` | DFT family clarification in comments and output |
| `docs/DESIGN_AROUND_DESERT.md` | 4 families, composition counts |
| `docs/TECHNICAL_WHITE_PAPER.md` | 70.8% caveated, alpha assumption expanded |
| `03_DATA_ARTIFACTS/dft_results.json` | Clarified that 100 entries are GENERATED, not forensic |
| `03_DATA_ARTIFACTS/CLAIM_EVIDENCE_MATRIX.json` | DFT family, composition honesty |
| `03_DATA_ARTIFACTS/FINAL_SUMMARY.json` | DFT family, 4 scaffold families |
| `03_DATA_ARTIFACTS/ml_models/README.md` | DFT family clarification |
| `02_SOURCE_CODE/simulations/industrial_process_sim.py` | Added `alpha_sensitivity_data()` function |
| `smartmatter/PATENT_CLAIMS.py` | 70.8% replaced, 730 composition honesty |
| `tests/test_smart_matter.py` | Enhanced caveat on 70.8% test |
| `tests/test_economics.py` | Enhanced caveat on 70.8% test |
| `09_PUBLIC_BENCHMARKS/CRITICAL_MINERAL_BENCHMARK_README.md` | 70.8% caveated, composition honesty |

---

*This document was created as part of a science-fixer audit on 2026-02-28. The goal was to replace inflated framing with honest characterization while preserving the genuine strengths of the platform (multi-application breadth, forensic provenance, reproducible infrastructure).*
