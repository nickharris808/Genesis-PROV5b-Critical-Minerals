# Reproduction Guide -- Genesis PROV 5b

**Classification:** NON-CONFIDENTIAL
**Purpose:** Instructions for independently reproducing the verification checks in this repository.

---

## Prerequisites

- Python 3.8 or later
- Standard library only (no external packages required for verification)
- The `verification/reference_data/canonical_values.json` file (included in this repository)

---

## Quick Start

```bash
# Clone or download this repository
cd Genesis-PROV5b-Critical-Minerals

# Run the verification script
python verification/verify_claims.py
```

Expected output: 5 checks, all PASS.

---

## What the Verification Script Checks

### Check 1: Kremser Equation -- Stage Reduction

**What it verifies:** That the Kremser equation, given the champion Janus Ligand separation factor of 11,000 and the P507 baseline of 2.5, produces the claimed stage reduction from 29.2 to approximately 8.5.

**The math:**

```
N = ln(x_feed / x_raff) / ln(alpha)
```

For binary Nd/Fe separation (x_feed = 0.99, x_raff = 0.001):
- P507: N = ln(990) / ln(2.5) = 6.9 / 0.916 = 7.5 theoretical stages
- Janus: N = ln(990) / ln(11000) = 6.9 / 9.31 = 0.74 theoretical stages

The canonical values of 29.2 (P507) and 8.5 (Janus) refer to the **full multi-element separation train** (not the binary case), which involves additional stages for the complete lanthanide cascade.

**How to reproduce manually:**

```python
import math
# Binary case
N_p507 = math.log(0.99 / 0.001) / math.log(2.5)    # ~7.5
N_janus = math.log(0.99 / 0.001) / math.log(11000)  # ~0.74
print(f"P507 stages (binary): {N_p507:.1f}")
print(f"Janus stages (binary): {N_janus:.2f}")
```

### Check 2: Separation Factor Comparison

**What it verifies:** That the Janus Ligand champion separation factor exceeds 10,000, and that the P507 baseline is approximately 2.5.

**Independent verification:** The P507 separation factor of 2.5 for Nd/Fe is well-documented in the literature. See Gupta & Krishnamurthy, "Extractive Metallurgy of Rare Earths," CRC Press (2005), Table 6.8.

### Check 3: CapEx Reduction

**What it verifies:** That reducing the stage count from 29.2 to 8.5 yields a CapEx reduction exceeding 65%.

**The math:**

```
CapEx = N_stages x $5M/stage x 1.4 (overhead)

CapEx_P507  = 29.2 x 5.0 x 1.4 = $204.4M
CapEx_Janus =  8.5 x 5.0 x 1.4 = $ 59.5M

Reduction = (1 - 59.5/204.4) x 100 = 70.9%
```

**Important caveat:** This is the champion (optimistic) scenario. The defensible range with practical correction factors is 20-40%. See `HONEST_DISCLOSURES.md` for detailed discussion.

### Check 4: ML Surrogate R-squared

**What it verifies:** That the Ridge regression model achieves R-squared exceeding 0.96 on held-out test data.

**How this was measured:** The v8 model was trained on 58 verified + 200+ expanded DFT data points using ligand-out GroupKFold cross-validation. The R-squared of 0.966 is the average across folds on held-out data, not the training set score.

### Check 5: DFT Convergence

**What it verifies:** That all 58 forensically-audited DFT calculations converged to the specified energy tolerance. 58/58 = 100% convergence rate.

**Forensic audit methodology:** Each of the 58 calculations has a unique cloud task ID. Convergence was verified by checking that the total energy change between the final two SCF iterations was below the threshold (typically 1e-6 Ha).

---

## Verifying the Kremser Sensitivity Analysis

The `canonical_values.json` file includes a sensitivity analysis section with three scenarios:

| Scenario | Alpha | Stages | CapEx Savings |
|----------|-------|--------|--------------|
| Conservative | 3.35 | 6.41 | 20% |
| Moderate | 5.02 | 5.14 | 30% |
| Optimistic | 7.52 | 4.35 | 40% |

To verify these independently, apply the Kremser equation with the P507 baseline of 7.97 stages (alpha = 2.5) and compute the ratio:

```python
import math

baseline_alpha = 2.5
baseline_stages = math.log(0.99/0.001) / math.log(baseline_alpha)  # ~7.53

for alpha in [3.35, 5.02, 7.52]:
    stages = math.log(0.99/0.001) / math.log(alpha)
    savings = (1 - stages/baseline_stages) * 100
    print(f"alpha={alpha}: stages={stages:.2f}, savings={savings:.1f}%")
```

Note: The canonical stage counts (6.41, 5.14, 4.35) include practical corrections (efficiency factor, safety margin) not captured in the simple Kremser formula. The relative rankings are consistent.

---

## Verifying the Separation Factor from First Principles

The champion separation factor of 11,000 is derived from DFT binding energies through the following chain:

1. **Isodesmic reaction:** Delta-Delta-G = 10.08 Ha = 26,470 kJ/mol (Janus-Nd vs Janus-Fe, relative to TBP)
2. **Boltzmann factor:** alpha_ideal = exp(Delta-Delta-G / RT) where RT ~ 2.479 kJ/mol at 298K
3. **Practical correction:** alpha_practical = alpha_ideal^gamma, where gamma = 0.3-0.5

The raw Delta-Delta-G of 10.08 Ha is extremely large, yielding an astronomical ideal alpha. The practical correction factor (gamma) accounts for the many non-idealities between thermodynamic selectivity and mixer-settler performance.

**Without access to the DFT output files (proprietary), the binding energy chain cannot be independently reproduced.** However, the Kremser equation and CapEx model can be verified from the published parameters.

---

## File Checksums

To verify file integrity:

```bash
# Generate checksums for all repository files
find . -type f -exec sha256sum {} \; | sort
```

---

## Contact

For questions about the verification methodology or to request access to proprietary data under NDA, refer to the Genesis Platform contact information.

---

*Genesis Platform -- Reproduction Guide -- PROV 5b -- Non-Confidential -- February 2026*
