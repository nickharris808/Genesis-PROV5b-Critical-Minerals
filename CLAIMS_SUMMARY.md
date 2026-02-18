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

## Family 1: Janus Ligands for Rare Earth Extraction (Claims 1-15)

**Type:** Composition of Matter
**Strength:** STRONG (730 structures + 58 verified DFT + 200 expanded DFT)

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
**Strength:** STRONG (Born analytical + GROMACS PMF at 10 ns/window, <10% uncertainty)

Claims 29-38 cover ion-selective membrane compositions for direct lithium extraction from brine.

| Claim Range | Scope | Key Feature |
|------------|-------|-------------|
| 29-30 | Independent composition claims | Sub-nanometer pore membrane with ion-selective transport |
| 31 | Dependent: pore size | 0.65-0.75 nm pore diameter (computationally validated optimal range) |
| 32-34 | Dependent: material systems | Graphene oxide, polymer, and hybrid membrane compositions |
| 35-36 | Dependent: selectivity | Li+/Na+ selectivity at specified minimum ratio |
| 37-38 | Dependent: manufacturing | Fabrication methods for controlled pore size distribution |

**Blocking Claim (Claim 31):** Pore diameter 0.65-0.75 nm. Computational PMF analysis (GROMACS, 10 ns/window, 5 ions x 8 pore diameters) demonstrates this is the only pore size range achieving meaningful Li+/Na+ selectivity through hydration shell stripping.

**Blocking Claim (Claim 33):** Pore size exclusivity. No design-around is physically possible -- the hydration shell stripping mechanism is governed by fundamental ionic radii and hydration enthalpies (Marcus 1997).

---

## Family 4: Computational Discovery Engine (Claims 39-52)

**Type:** Method
**Strength:** STRONG (ML v8 + 200+ DFT + molecular fingerprints + ligand-out CV)

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
| Janus Ligands (1-15) | DFT binding energies | 58 verified + 200 expanded | 58/58 forensic match |
| Janus Ligands (1-15) | Molecular structures | 730 + 120 v2 | 730/730 valid |
| Ion-Selective Membranes (29-38) | PMF calculations | 5 ions x 8 pore diameters | 10 ns/window, <10% uncertainty |
| Computational Discovery (39-52) | ML surrogate | Ridge R-squared = 0.966 | Ligand-out CV validated |
| Computational Discovery (39-52) | Kremser model | 18-point sensitivity curve | First-principles derivation |

---

**Note:** This document discloses claim family structure and scope only. Full patent claim text, specific molecular structures, and detailed claim language are confidential IP and are not included in this public repository.

---

*Genesis Platform -- PROV 5b Claims Summary -- Non-Confidential -- February 2026*
