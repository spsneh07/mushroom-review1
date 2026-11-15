# Biological Classification Challenge

## Probability-Based Independence Analysis Using the UCI Mushroom Dataset

An empirical probability and statistical analysis examining feature independence and conditional independence on the **UCI Mushroom Dataset** (*Agaricus-Lepiota*).

---

## 📌 Project Overview

This project investigates the probabilistic dependencies among distinct biological characteristics of mushrooms and their edibility status. In particular, we evaluate whether morphological and sensory features (such as cap color and odor) occur independently across the population, and whether observing toxicity (poisonous vs. edible) renders these characteristics conditionally independent.

### Analyzed Events

We define the primary events of interest:
- **Event A**: Mushroom exhibits a **foul odor** (`odor = 'f'`).
- **Event B**: Mushroom has a **brown cap color** (`cap-color = 'n'`).
- **Event C**: Mushroom is **poisonous** (`class = 'p'`).

---

## 📐 Mathematical Formulation

### 1. Test of Independence (Phase 1)
Two events $A$ and $B$ are statistically independent if and only if the joint probability equals the product of their individual (marginal) probabilities:

$$P(A \cap B) = P(A) \times P(B)$$

If $P(A \cap B) \neq P(A) \times P(B)$, the events are **dependent**.

### 2. Test of Conditional Independence (Phase 2)
Two events $A$ and $B$ are conditionally independent given event $C$ if knowledge of $C$ provides all relevant information about their joint occurrence:

$$P(A \cap B \mid C) = P(A \mid C) \times P(B \mid C)$$

If $P(A \cap B \mid C) \neq P(A \mid C) \times P(B \mid C)$, events $A$ and $B$ remain **dependent even when conditioned on $C$**.

---

## 📊 Dataset Summary

The dataset used is the benchmark **UCI Mushroom Dataset** (`agaricus-lepiota.data`):
- **Total Samples**: 8,124 mushroom records
- **Features**: 22 categorical attributes (cap shape, cap surface, cap color, odor, gill features, stalk features, veil, rings, habitat, etc.)
- **Target Class**:
  - `e`: Edible (4,208 samples, ~51.8%)
  - `p`: Poisonous (3,916 samples, ~48.2%)

---

## 🔬 Experimental Results

### Phase 1: Marginal Probabilities & Independence Test

| Metric | Formula / Calculation | Value |
|---|---|---|
| Total population ($N$) | Total rows in dataset | 8,124 |
| Foul odor ($A$) | Count of `odor == 'f'` | 2,160 |
| Brown cap ($B$) | Count of `cap-color == 'n'` | 2,284 |
| Joint occurrence ($A \cap B$) | Count of foul odor and brown cap | 288 |
| $P(A)$ | $2160 / 8124$ | **0.2659** |
| $P(B)$ | $2284 / 8124$ | **0.2811** |
| **$P(A \cap B)$** | $288 / 8124$ | **0.0355** |
| **$P(A) \times P(B)$** | $0.2659 \times 0.2811$ | **0.0747** |

> **Conclusion**: $P(A \cap B) = 0.0355 \neq 0.0747 = P(A) \times P(B)$.  
> **Events A (Foul Odor) and B (Brown Cap) are statistically DEPENDENT.**

---

### Phase 2: Conditional Probabilities & Conditional Independence Test

Conditioning the analysis strictly on the poisonous subset ($C$: $N_C = 3,916$):

| Metric | Calculation | Value |
|---|---|---|
| Poisonous mushrooms ($C$) | Count of `class == 'p'` | 3,916 |
| $P(A \mid C)$ | Mean of foul odor among poisonous | **0.5516** |
| $P(B \mid C)$ | Mean of brown cap among poisonous | **0.2605** |
| **$P(A \cap B \mid C)$** | Mean of joint event among poisonous | **0.0735** |
| **$P(A \mid C) \times P(B \mid C)$** | $0.5516 \times 0.2605$ | **0.1437** |

> **Conclusion**: $P(A \cap B \mid C) = 0.0735 \neq 0.1437 = P(A \mid C) \times P(B \mid C)$.  
> **Events A and B are NOT CONDITIONALLY INDEPENDENT given C (Poisonous).**

---

## 📈 Key Visualizations

The Jupyter Notebook generates exploratory data analysis plots:
1. **Class Distribution**: Edible (`e`) vs. Poisonous (`p`) count comparison.
2. **Odor Distribution**: Frequency across all 9 odor categories (`f`, `n`, `y`, `s`, `a`, `l`, `p`, `c`, `m`).
3. **Odor vs. Class**: Highlights that certain odors (such as foul `f`, pungent `p`, fishy `y`) correlate exclusively with poisonous mushrooms.
4. **Cap Color vs. Class**: Distribution of cap colors across edible and poisonous classes.
5. **Probability Event Counts**: Direct comparison bar chart of $A$, $B$, and $A \cap B$.

---

## 📁 Repository Structure

```
├── agaricus-lepiota.data    # Raw UCI Mushroom dataset (8,124 records)
├── agaricus-lepiota.names   # UCI attribute information and documentation
├── mushrooms.ipynb          # Jupyter notebook with complete data analysis & plots
├── README.md                # Project documentation and findings
├── README                   # Original UCI repository archive note (1988)
├── Index                    # Archive index file
├── expanded.Z               # Compressed original archive data
└── .gitignore               # Standard Python / Jupyter git ignore file
```

---

## 🚀 Getting Started

### Prerequisites

Ensure you have Python 3.8+ installed along with the required libraries:

```bash
pip install pandas matplotlib seaborn notebook
```

### Running the Analysis

Launch the Jupyter notebook:

```bash
jupyter notebook mushrooms.ipynb
```

Execute all cells sequentially to reproduce the probability calculations and visualizations.

---

## 📚 References

- **Dataset**: UCI Machine Learning Repository — [Mushroom Data Set](https://archive.ics.uci.edu/ml/datasets/mushroom)
- **Donor**: Jeff Schlimmer (1987), based on *The Audubon Society Field Guide to North American Mushrooms* (1981).
