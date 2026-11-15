# 04. Probability Mathematical Formulation

**Audited Domain**: Probability Theory & Categorical Feature Association  
**Inspected File**: `mushrooms.ipynb` (Cells 0, 4, 6, 7, 8)  
**Audit Status**: VERIFIED  

---

## 1. Probability Space Specification

The mathematical framework utilized by the project is rooted in the classical frequentist definition of probability over a finite sample space.

Let the sample space $\Omega$ represent the set of all observed mushroom specimens in the dataset:
$$\Omega = \{\omega_1, \omega_2, \dots, \omega_N\}, \quad \text{where } N = |\Omega| = 8,124$$

Each specimen $\omega \in \Omega$ is endowed with a uniform probability measure under the empirical distribution:
$$P(\{\omega\}) = \frac{1}{N} = \frac{1}{8,124}$$

---

## 2. Formal Definition of Random Events

Three discrete categorical events are defined as subsets of $\Omega$:

1. **Event $A$ (Foul Odor)**:
   $$A = \{\omega \in \Omega \mid \text{odor}(\omega) = \text{'f'}\}$$
   - Measures the occurrence of foul-smelling mushrooms.

2. **Event $B$ (Brown Cap Color)**:
   $$B = \{\omega \in \Omega \mid \text{cap-color}(\omega) = \text{'n'}\}$$
   - Measures the occurrence of brown-capped mushrooms.

3. **Condition $C$ (Poisonous Class)**:
   $$C = \{\omega \in \Omega \mid \text{class}(\omega) = \text{'p'}\}$$
   - Measures the toxicity status of the mushroom.

---

## 3. Probability Definitions Used in Project

### A. Marginal Probability
The marginal probability of an event $E \subseteq \Omega$ is the ratio of favorable outcomes to the total sample size:
$$P(E) = \frac{|E|}{|\Omega|} = \frac{1}{N} \sum_{i=1}^N \mathbb{I}(\omega_i \in E)$$
Where $\mathbb{I}(\cdot)$ denotes the indicator function.

In the notebook implementation (Cell 4):
$$P(A) = \frac{\text{count\_A}}{N}, \quad P(B) = \frac{\text{count\_B}}{N}$$

### B. Joint Probability
The joint probability of two events $A$ and $B$ represents the likelihood that both characteristics appear simultaneously in the same specimen:
$$P(A \cap B) = \frac{|A \cap B|}{|\Omega|} = \frac{1}{N} \sum_{i=1}^N \mathbb{I}(\omega_i \in A \text{ and } \omega_i \in B)$$

In the notebook implementation (Cell 4):
$$P(A \cap B) = \frac{\text{count\_AB}}{N}$$

### C. Statistical Independence
Two events $A$ and $B$ are defined to be **statistically independent** if and only if the joint probability equals the product of their individual marginal probabilities:

$$\boxed{P(A \cap B) = P(A) \times P(B)}$$

- **Equivalence**: If $P(B) > 0$, this is equivalent to $P(A \mid B) = P(A)$.
- **Criterion for Dependence**:
  $$\text{If } |P(A \cap B) - P(A) \times P(B)| > 0 \implies A \text{ and } B \text{ are DEPENDENT.}$$

### D. Conditional Probability
The conditional probability of an event $E$ given condition $C$ (where $P(C) > 0$) is defined by Kolmogorov's axiom:
$$P(E \mid C) = \frac{P(E \cap C)}{P(C)} = \frac{|E \cap C|}{|C|} = \frac{\sum_{\omega \in C} \mathbb{I}(\omega \in E)}{|C|}$$

In the notebook implementation (Cell 6):
$$P(A \mid C) = \frac{|A \cap C|}{|C|}, \quad P(B \mid C) = \frac{|B \cap C|}{|C|}$$
Computed directly by evaluating `.mean()` over the sliced subset `df[df["class"] == "p"]`.

### E. Conditional Independence
Two events $A$ and $B$ are **conditionally independent given $C$** if and only if:

$$\boxed{P(A \cap B \mid C) = P(A \mid C) \times P(B \mid C)}$$

- **Physical Interpretation**: Knowing that a mushroom is poisonous ($C$) provides complete information regarding the joint occurrence of foul odor ($A$) and brown cap ($B$), such that knowing the cap color provides no additional information about the odor.
- **Criterion for Conditional Dependence**:
  $$\text{If } |P(A \cap B \mid C) - P(A \mid C) \times P(B \mid C)| > 0 \implies A \text{ and } B \text{ are NOT CONDITIONALLY INDEPENDENT given } C.$$

---

## 4. Notation & Equation Audit

The equations and mathematical typography in the notebook use standard LaTeX math markup:
- Cell 0 & Cell 8:
  ```latex
  \[
  P(A \cap B) = P(A) \times P(B)
  \]
  ```
  ```latex
  \[
  P(A \cap B \mid C) = P(A \mid C) \times P(B \mid C)
  \]
  ```
- Implementation uses floating-point precision with tolerance `1e-10`:
  ```python
  if abs(P_AB - P_A * P_B) < 1e-10:
  if abs(P_AB_given_C - P_A_given_C * P_B_given_C) < 1e-10:
  ```
- **Auditor Note**: No extraneous Bayesian priors, probability density functions, or parametric distribution assumptions (e.g., Gaussian, Poisson) are made. The framework is strictly empirical, discrete, and finite.
