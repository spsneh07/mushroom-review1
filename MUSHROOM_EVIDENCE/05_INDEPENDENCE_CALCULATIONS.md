# 05. Phase 1: Marginal & Joint Independence Calculations

**Analysis Phase**: Phase 1 — Global Statistical Independence Check  
**Inspected Notebook Cells**: Cell 3 (Event definition), Cell 4 (Probability calculations), Cell 7 (Decision logic)  
**Audit Status**: VERIFIED  

---

## 1. Event Definitions & Population Base

The global population consists of all records in `agaricus-lepiota.data`:
$$N = |\Omega| = 8,124$$

The boolean events are implemented in Cell 3:
```python
A = df["odor"] == "f"          # Event A: foul odor
B = df["cap-color"] == "n"     # Event B: brown cap
```

---

## 2. Calculation Verification Matrix

Every step of the Phase 1 calculation has been audited, cross-verifying the raw frequency counts, exact fractional representations, 64-bit floating-point outputs, and the rounded values presented in documentation.

| Probability Term | Mathematical Formula | Exact Fraction | Evaluated Float64 (Notebook Output) | Documentation Rounded Value (README) | Status |
|---|---|---|---|---|---|
| **$P(A)$** | $\frac{|A|}{N} = \frac{\sum A}{N}$ | $\frac{2,160}{8,124} = \frac{180}{677}$ | `0.26587887740029542` | `0.2659` (4 d.p.) | **VERIFIED** |
| **$P(B)$** | $\frac{|B|}{N} = \frac{\sum B}{N}$ | $\frac{2,284}{8,124} = \frac{571}{2031}$ | `0.28114229443623828` | `0.2811` (4 d.p.) | **VERIFIED** |
| **$P(A \cap B)$** | $\frac{|A \cap B|}{N} = \frac{\sum (A \land B)}{N}$ | $\frac{288}{8,124} = \frac{24}{677}$ | `0.03545051698670606` | `0.0355` (4 d.p.) | **VERIFIED** |
| **$P(A) \times P(B)$** | $\frac{2160}{8124} \times \frac{2284}{8124}$ | $\frac{4,933,440}{65,999,376} = \frac{102,780}{1,374,987}$ | `0.07474979763445036` | `0.0747` (4 d.p.) | **VERIFIED** |

---

## 3. Discrepancy & Precision Audit

### A. Raw Notebook Output vs. README.md:
- **Notebook Output (Cell 4)**:
  ```
  Total: 8124
  Foul odor: 2160
  Brown cap: 2284
  Foul + Brown: 288
  P(A) = 0.2658788774002954
  P(B) = 0.2811422944362383
  P(A ∩ B) = 0.03545051698670606
  P(A) × P(B) = 0.07474979763445036
  ```
- **Documentation Text (`README.md`)**:
  - $P(A) \approx 0.2659$
  - $P(B) \approx 0.2811$
  - $P(A \cap B) \approx 0.0355$
  - $P(A) \times P(B) \approx 0.0747$
- **Auditor Finding**:
  There is **no mathematical discrepancy**. The README values represent standard four-decimal-place rounding of the exact float64 figures computed in Cell 4.

---

## 4. Test of Independence Evaluation

### Mathematical Comparison:
$$\Delta = P(A \cap B) - P(A) \times P(B)$$
$$\Delta = 0.03545051698670606 - 0.07474979763445036 = -0.03929928064774430$$
$$|\Delta| = 0.03929928064774430$$

### Decision Logic (Cell 7):
```python
if abs(P_AB - P_A * P_B) < 1e-10:
    print("A and B are INDEPENDENT")
else:
    print("A and B are DEPENDENT")
```
- **Condition Evaluated**: `0.03929928064774430 < 1e-10` $\to$ **`False`**.
- **Printed Verdict**: `"A and B are DEPENDENT"`.
- **Statistical Interpretation**:
  If events $A$ and $B$ were independent, observing a brown cap would not alter the probability of having a foul odor. However:
  $$P(A \mid B) = \frac{P(A \cap B)}{P(B)} = \frac{288}{2,284} \approx 0.1261$$
  Whereas the unconditional probability is:
  $$P(A) = \frac{2,160}{8,124} \approx 0.2659$$
  Because $P(A \mid B) \approx 0.1261 \ll P(A) \approx 0.2659$, mushrooms with brown caps are **significantly less likely** to have a foul odor than a randomly chosen mushroom from the overall population.

---

## 5. Supporting $2 \times 2$ Contingency Table

To verify the complete joint probability distribution across the entire dataset:

| Event Status | Brown Cap ($B$) | Not Brown Cap ($B^c$) | Marginal Total |
|---|---|---|---|
| **Foul Odor ($A$)** | **288** ($A \cap B$) | **1,872** ($A \cap B^c$) | **2,160** ($n_A$) |
| **Not Foul Odor ($A^c$)** | **1,996** ($A^c \cap B$) | **3,968** ($A^c \cap B^c$) | **5,964** ($n_{A^c}$) |
| **Marginal Total** | **2,284** ($n_B$) | **5,840** ($n_{B^c}$) | **8,124** ($N$) |

*(Auditor verification note: A formal Pearson's Chi-Square test on this table yields $\chi^2 = 317.0714, df = 1, p = 6.29 \times 10^{-71}$, confirming extreme statistical significance for the dependency, although this test was not executed in the student's notebook).*
