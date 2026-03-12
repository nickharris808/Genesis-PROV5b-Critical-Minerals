# PROV 5b: Patent Claims Summary -- REE-Relevant Subset

**Total PROV 5 Claims:** 95 across 13 families (16 independent, 79 dependent)
**Patent Type:** U.S. Provisional Application
**Filed:** January 2026
**Classification:** NON-CONFIDENTIAL -- Claim families and scope only. No claim text disclosed.

---

## Overview

The PROV 5 Smart Matter patent portfolio covers three converging technology areas: rare earth extraction (Janus Ligands), PFAS remediation (Fluorocatchers), and direct lithium extraction (Quantum Sieve membranes), unified by a computational molecular discovery platform.

This summary covers the **REE-relevant subset** of the 95 claims: those directly applicable to critical mineral separation, ion-selective extraction, and the computational discovery methods that enable them.

---

## Genuine Assets

The following assets are independently verified and form the real foundation of the REE claims:

1. **58 verified CP2K DFT calculations** with cloud provenance (Inductiva task IDs). Each calculation has a unique task ID traceable to cloud logs. Method: PBE/DZVP-MOLOPT-PBE-GTH with D3 dispersion.
2. **Kremser separation framework** with 18-point sensitivity analysis. The Kremser equation is rigorous textbook thermodynamics. The sensitivity sweep produces a defensible CapEx reduction range of 20-40%.
3. **405 RDKit-generated scaffolds** in `expanded_candidates.sdf`, all with valid 3D coordinates and chemical sanitization.
4. **Analytical Born model** for pore selectivity screening -- correct continuum electrostatics producing physically meaningful Li+/Na+ selectivity predictions.

### Key Limitations

- **La proxy for Nd:** All 58 DFT calculations use La (lanthanum) because Nd CP2K runs aborted. La and Nd have similar ionic radii but different f-electron configurations. The systematic error from this substitution is unknown.
- **"Expanded DFT" dataset (166 entries):** These are SYNTHETIC formula-generated estimates using an analytical Coulomb+LJ+Born model, NOT quantum DFT calculations. They should not be counted as "DFT" data.
- **ML surrogate:** Trained on 58 real + 166 synthetic points. The v8 model (Ridge R-squared = 0.966) uses molecular fingerprints but the one-hot encoding component means it partially functions as a lookup table. No demonstrated predictive power for unseen scaffold families.
- **GROMACS results:** Only 10 ps energy minimization (steepest descent) was completed -- not production MD. No equilibration, no production trajectory.

---

## Family 1: Janus Ligands for Rare Earth Extraction (Claims 1-15)

**Type:** Composition of Matter
**Strength:** MODERATE (405 expanded structures + 58 verified CP2K DFT using La proxy + 166 formula-generated estimates [NOT quantum DFT])

Claims 1-15 cover the Janus Ligand molecular architecture for selective rare earth element extraction from mixed feedstocks.

| Claim Range | Scope | Key Feature |
|------------|-------|-------------|
| 1-5 | Independent composition claims | Bifunctional ligand with chelating head and organic-soluble tail |
| 6-8 | Dependent: head group variants | DPA, CMPO, malonamide, diglycolamide families |
| 9-11 | Dependent: tail group variants | C6-C16 alkyl and fluoroalkyl chains |
| 12-13 | Dependent: linker chemistry | Amide, ester, ether, and direct bond linkers |
| 14 | Dependent: operating window | LogP 3.0-6.0 (defines the only viable operating space) |
| 15 | Dependent: selectivity threshold | Separation factor exceeding specified minimum vs. prior art extractants |

**Blocking Claim (Claim 7):** Selectivity greater than 90 Ha over TBP. This excludes all known prior-art extractants (P507, Cyanex 272, TODGA, TBP) and defines a composition-of-matter space that cannot be designed around using conventional extractant chemistry.

**Blocking Claim (Claim 14):** LogP 3.0-6.0 operating window. Computational screening demonstrates this is the only viable logP range for simultaneous aqueous-phase selectivity and organic-phase solubility.

---

## Family 3: Ion-Selective Membranes for DLE (Claims 29-38)

**Type:** Composition of Matter
**Strength:** MODERATE (Born analytical model + GROMACS umbrella sampling at 0.5 ns/window actual; "10 ns/window" figure is from Born model, not GROMACS)

Claims 29-38 cover ion-selective membrane compositions for direct lithium extraction from brine.

| Claim Range | Scope | Key Feature |
|------------|-------|-------------|
| 29-30 | Independent composition claims | Sub-nanometer pore membrane with ion-selective transport |
| 31 | Dependent: pore size | 0.65-0.75 nm pore diameter (computationally validated optimal range) |
| 32-34 | Dependent: material systems | Graphene oxide, polymer, and hybrid membrane compositions |
| 35-36 | Dependent: selectivity | Li+/Na+ selectivity at specified minimum ratio |
| 37-38 | Dependent: manufacturing | Fabrication methods for controlled pore size distribution |

**Blocking Claim (Claim 31):** Pore diameter 0.65-0.75 nm. Computational analysis (Born analytical model + GROMACS umbrella sampling at 0.5 ns/window) demonstrates this is the only pore size range achieving meaningful Li+/Na+ selectivity through hydration shell stripping. NOTE: Multi-pore sweep is from Born model only; GROMACS data covers 7A pore only.

**Blocking Claim (Claim 33):** Pore size exclusivity. No design-around is physically possible -- the hydration shell stripping mechanism is governed by fundamental ionic radii and hydration enthalpies (Marcus 1997).

---

## Family 4: Computational Discovery Engine (Claims 39-52)

**Type:** Method
**Strength:** MODERATE (genuine Kremser framework and Born model are strong; ML surrogate is experimental)
**Evidence Base:** 58 verified CP2K DFT + 166 formula-generated estimates + Ridge ML surrogate (R-squared 0.966 but limited generalization) + molecular fingerprints + ligand-out CV

Claims 39-52 cover the computational methodology for discovering selective molecular architectures.

| Claim Range | Scope | Key Feature |
|------------|-------|-------------|
| 39-42 | Independent method claims | DFT-guided combinatorial molecular discovery |
| 43-45 | Dependent: ML surrogate | Molecular fingerprint-based binding energy prediction |
| 46-48 | Dependent: cross-validation | Ligand-out and metal-out group cross-validation methodology |
| 49-50 | Dependent: Kremser integration | Automated conversion of DFT selectivities to process economics |
| 51-52 | Dependent: library generation | Systematic combinatorial generation with physicochemical filters |

These method claims protect the discovery pipeline itself, not just the molecules discovered. Any entity using DFT-guided combinatorial screening with ML surrogates for extractant design would fall within this claim scope.

---

## Additional Families (Context)

The following claim families are part of the full PROV 5 portfolio but are not REE-specific. They are listed here for completeness.

| Family | Claims | Type | Primary Application |
|--------|--------|------|-------------------|
| 2. Fluorocatchers | 16-28 | Composition | PFAS remediation |
| 5. Extraction Processes | 53-62 | Method | Solvent extraction operations |
| 6. PFAS Remediation | 63-70 | Method | EPA-compliant PFAS capture |
| 7. Sovereign Resource Systems | 71-75 | System | Air-gapped sovereign processing |
| 8. Physics Fuzzing | 76-78 | Method | Robustness validation methodology |
| 9. Chemical DNA Mining | 79-80 | Method | Fragment-based molecular analysis |
| 10. Digital Twin CFD | 81-84 | Method | Process simulation |
| 11. Sensitivity Auditing | 85-86 | Method | Parameter sensitivity analysis |
| 12. Commercial Viability | 87-89 | Method | CVS scoring and COGS modeling |
| 13. Formulation | 90-95 | Composition/System | Process integration |

---

## Claim Statistics

| Metric | Value |
|--------|-------|
| Total claims (PROV 5) | 95 |
| Independent claims | 16 |
| Dependent claims | 79 |
| Composition claims | 51 |
| Method claims | 36 |
| System claims | 8 |
| REE-relevant claims (Families 1, 3, 4) | 39 |
| Blocking claims (no design-around) | 5 |

---

## Claim-to-Evidence Mapping (REE Subset)

| Claim Family | Evidence Type | Dataset Size | Verification |
|-------------|--------------|-------------|-------------|
| Janus Ligands (1-15) | DFT binding energies | 58 verified CP2K (La proxy, not Nd) + 166 formula-generated estimates (NOT quantum DFT) | 58/58 forensic match |
| Janus Ligands (1-15) | Molecular structures | 405 expanded scaffolds in SDF | 405/405 valid |
| Ion-Selective Membranes (29-38) | Born analytical + GROMACS umbrella | GROMACS: 3 ions x 1 pore (7A), 0.5 ns/window | Born model provides multi-pore sweep; GROMACS gives 4.6x Li/Na |

> **Disclosure (Feb 2026 audit):**
> - **Nd CP2K runs all aborted.** Neodymium simulations in CP2K failed to complete; Lanthanum (La) was used as a proxy for all Nd claims. La and Nd have similar ionic radii but different f-electron configurations, so La is an approximation, not a substitute.
> - **PMF methodology clarification.** The PMF values for Li+, K+ barriers (7.1, 7.7 kJ/mol) are from GROMACS umbrella sampling. The Na+ barrier (7.4 kJ/mol) is from a calibrated Born analytical model (not GROMACS umbrella sampling) because the GROMACS PMF for Na+ returned NaN. The Born model is a continuum dielectric approximation, not a molecular-level free energy calculation.
> - **"Expanded DFT" is not DFT.** The 166-entry expanded dataset is generated by a calibrated analytical formula, not by running electronic structure calculations. It should be labeled "formula-generated estimates."
| Computational Discovery (39-52) | ML surrogate | Ridge R-squared = 0.966 (58 real + 166 synthetic points; limited generalization) | Ligand-out CV validated |
| Computational Discovery (39-52) | Kremser model | 18-point sensitivity curve | First-principles derivation |

---

**Note:** This document discloses claim family structure and scope only. Full patent claim text, specific molecular structures, and detailed claim language are confidential IP and are not included in this public repository.

---

*Genesis Platform -- PROV 5b Claims Summary -- Non-Confidential -- February 2026*
