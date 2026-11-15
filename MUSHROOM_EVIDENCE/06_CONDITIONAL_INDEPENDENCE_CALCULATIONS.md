# 06. Phase 2: Conditional Independence Calculations

**Analysis Phase**: Phase 2 — Conditional Independence Check Given Event $C$ (Poisonous)  
**Inspected Notebook Cells**: Cell 3 (Condition $C$), Cell 5 (Subsetting), Cell 6 (Conditional probabilities), Cell 7 (Decision logic)  
**Audit Status**: VERIFIED  

---

## 1. Conditioning Subpopulation Specification

Event $C$ restricts the analysis strictly to the subset of poisonous mushrooms:
$$C = \{\omega \in \Omega \mid \text{class}(\omega) = \text{'p'}\}$$

### Subpopulation Size (Cell 5):
```python
poisonous = df[df["class"] == "p"]
print("Poisonous mushrooms:", len(poisonous))
```
- **Observed Subpopulation Size**: $N_C = |C| = 3,916$ specimens (48.20% of total dataset).
- **Verification**: Exactly 3,916 records in `agaricus-lepiota.data` have `'p'` in column 0.

---

## 2. Calculation Verification Matrix

Every step of the Phase 2 conditional calculation has been audited, cross-verifying the raw frequency counts, exact fractional representations, 64-bit floating-point outputs, and the rounded values presented in documentation.

| Probability Term | Mathematical Formula | Exact Fraction | Evaluated Float64 (Notebook Output) | Documentation Rounded Value (README) | Status |
|---|---|---|---|---|---|
| **$P(A \mid C)$** | $\frac{|A \cap C|}{|C|} = \frac{n_{A \cap C}}{N_C}$ | $\frac{2,160}{3,916} = \frac{540}{979}$ | `0.5515832482124617` | `0.5516` (4 d.p.) | **VERIFIED** |
| **$P(B \mid C)$** | $\frac{|B \cap C|}{|C|} = \frac{n_{B \cap C}}{N_C}$ | $\frac{1,020}{3,916} = \frac{255}{979}$ | `0.2604698672114402` | `0.2605` (4 d.p.) | **VERIFIED** |
| **$P(A \cap B \mid C)$** | $\frac{|A \cap B \cap C|}{|C|} = \frac{n_{AB \cap C}}{N_C}$ | $\frac{288}{3,916} = \frac{72}{979}$ | `0.0735444330949949` | `0.0735` (4 d.p.) | **VERIFIED** |
| **$P(A \mid C) \times P(B \mid C)$** | $\frac{2160}{3916} \times \frac{1020}{3916}$ | $\frac{2,203,200}{15,335,056} = \frac{137,700}{958,441}$ | `0.1436708154179548` | `0.1437` (4 d.p.) | **VERIFIED** |

---

## 3. Discrepancy & Precision Audit

### A. Raw Notebook Output vs. README.md:
- **Notebook Output (Cell 6)**:
  ```
  P(A|C) = 0.5515832482124617
  P(B|C) = 0.2604698672114402
  P(A ∩ B|C) = 0.0735444330949949
  P(A|C) × P(B|C) = 0.1436708154179548
  ```
- **Documentation Text (`README.md`)**:
  - $P(A \mid C) \approx 0.5516$
  - $P(B \mid C) \approx 0.2605$
  - $P(A \cap B \mid C) \approx 0.0735$
  - $P(A \mid C) \times P(B \mid C) \approx 0.1437$
- **Auditor Finding**:
  There is **no mathematical discrepancy**. The README values represent standard four-decimal-place rounding of the exact float64 figures computed in Cell 6.

---

## 4. Test of Conditional Independence Evaluation

### Mathematical Comparison:
$$\Delta_{cond} = P(A \cap B \mid C) - P(A \mid C) \times P(B \mid C)$$
$$\Delta_{cond} = 0.0735444330949949 - 0.1436708154179548 = -0.07012638232295990$$
$$|\Delta_{cond}| = 0.07012638232295990$$

### Decision Logic (Cell 7):
```python
if abs(P_AB_given_C - P_A_given_C * P_B_given_C) < 1e-10:
    print("A and B are CONDITIONALLY INDEPENDENT given C")
else:
    print("A and B are NOT CONDITIONALLY INDEPENDENT given C")
```
- **Condition Evaluated**: `0.07012638232295990 < 1e-10` $\to$ **`False`**.
- **Printed Verdict**: `"A and B are NOT CONDITIONALLY INDEPENDENT given C"`.
- **Domain Interpretation**:
  Even after controlling for toxicity by restricting our observation exclusively to poisonous mushrooms, cap color and foul odor remain strongly dependent. In fact, the absolute difference widened from $0.0393$ in the unconditioned population to $0.0701$ in the poisonous subpopulation:
  $$P(A \mid B \cap C) = \frac{P(A \cap B \cap C)}{P(B \cap C)} = \frac{288}{1,020} \approx 0.2824$$
  Whereas:
  $$P(A \mid C) = \frac{2,160}{3,916} \approx 0.5516$$
  Among poisonous mushrooms, having a brown cap cuts the likelihood of having a foul odor roughly in half (from ~55.2% down to ~28.2%). Hence, $A$ and $B$ are **not** conditionally independent given $C$.

---

## 5. Supporting $2 \times 2$ Poisonous Subpopulation Contingency Table

Restricting all counts exclusively to the 3,916 poisonous mushrooms ($C$):

| Condition: Poisonous ($C$) | Brown Cap ($B$) | Not Brown Cap ($B^c$) | Marginal Total in $C$ |
|---|---|---|---|
| **Foul Odor ($A$)** | **288** ($A \cap B \mid C$) | **1,872** ($A \cap B^c \mid C$) | **2,160** ($n_{A \mid C}$) |
| **Not Foul Odor ($A^c$)** | **732** ($A^c \cap B \mid C$) | **1,024** ($A^c \cap B^c \mid C$) | **1,756** ($n_{A^c \mid C}$) |
| **Marginal Total in $C$** | **1,020** ($n_{B \mid C}$) | **2,896** ($n_{B^c \mid C}$) | **3,916** ($N_C$) |

*(Auditor verification note: A formal Pearson's Chi-Square test on this conditioned sub-table yields $\chi^2 = 402.7322, df = 1, p = 1.40 \times 10^{-89}$, again demonstrating extreme statistical significance for the conditional dependency).*
