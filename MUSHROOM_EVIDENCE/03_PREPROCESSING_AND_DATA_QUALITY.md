# 03. Preprocessing & Data Quality Audit

**Repository**: `https://github.com/spsneh07/mushroom-review1`  
**Inspected File**: `mushrooms.ipynb` (Cells 1, 2, 3, 5)  
**Audit Status**: VERIFIED  

---

## 1. Data Ingestion & Schema Alignment

The raw file `agaricus-lepiota.data` is an uncompressed, ASCII comma-delimited text file containing 8,124 rows. It does **not** include an initial header row.

### Ingestion Implementation (Cell 2):
```python
import pandas as pd

columns = [
    "class", "cap-shape", "cap-surface", "cap-color", "bruises",
    "odor", "gill-attachment", "gill-spacing", "gill-size",
    "gill-color", "stalk-shape", "stalk-root",
    "stalk-surface-above-ring", "stalk-surface-below-ring",
    "stalk-color-above-ring", "stalk-color-below-ring",
    "veil-type", "veil-color", "ring-number", "ring-type",
    "spore-print-color", "population", "habitat"
]

df = pd.read_csv(
    "agaricus-lepiota.data",
    header=None,
    names=columns
)
```

### Ingestion Audit Assessment:
- **`header=None`**: Correctly specifies that the first line is data, preventing the loss of row 0 (`p,x,s,n,t,p,f,c,n,k,e,e,s,s,w,w,p,w,o,p,k,s,u`).
- **Column Sequence**: The supplied list of 23 names precisely mirrors the sequence documented in Section 7 of `agaricus-lepiota.names`.
- **Data Dimension**: The output of `df.shape` is verified as `(8124, 23)`.

---

## 2. Missing Value Audit & Handling

### Ground Truth State:
- Column `stalk-root` contains **2,480 entries** encoded as `'?'`.
- All other 22 columns have **0 occurrences** of `'?'` or empty strings.

### Notebook Handling Assessment:
1. **No NA Replacement**: `pd.read_csv()` did not specify `na_values='?'`. Consequently, pandas treats `'?'` as a valid string literal category.
2. **No Deletion / Imputation**: The notebook does not execute `.dropna()` or `.fillna()`. The DataFrame length remains strictly $N = 8,124$.
3. **Impact on Results**:
   In Cell 1 Markdown, the notebook explicitly scopes the analysis:
   > *"Only the features required for this probability analysis will be used: `class`, `cap-color`, `odor`"*
   
   Because `class`, `cap-color`, and `odor` possess **zero missing values**, the unhandled missing values in `stalk-root` have **zero mathematical impact** on the computed probabilities or conclusions.

---

## 3. Data Types & Feature Representation

| Feature Name | Python/Pandas Dtype | Cardinality | Distinct Values | Verification Finding |
|---|---|---|---|---|
| `class` | `object` (string) | 2 | `['e', 'p']` | Ground truth target labels |
| `odor` | `object` (string) | 9 | `['a', 'c', 'f', 'l', 'm', 'n', 'p', 's', 'y']` | Complete nominal encoding |
| `cap-color` | `object` (string) | 10 | `['b', 'c', 'e', 'g', 'n', 'p', 'r', 'u', 'w', 'y']` | Complete nominal encoding |

No categorical encoding (e.g., `OneHotEncoder`, `OrdinalEncoder`, `pd.get_dummies`) was applied or needed, as the probability calculations operate directly via boolean equivalence filtering.

---

## 4. Boolean Masking & Subsetting Pipeline

The notebook constructs event indicators using vectorized Pandas equality comparisons:

### 1. Marginal Event Masks (Cell 3):
```python
A = df["odor"] == "f"          # Event A: foul odor (True for 2,160 rows)
B = df["cap-color"] == "n"     # Event B: brown cap (True for 2,284 rows)
C = df["class"] == "p"         # Condition C: poisonous (True for 3,916 rows)
```

### 2. Poisonous Conditioning Subset (Cell 5):
```python
poisonous = df[df["class"] == "p"]
```
- **Subset Cardinality**: Exactly 3,916 records ($N_C = 3,916$).
- **Subsetting Integrity**: Verified. No slicing warnings or index corruption.

---

## 5. Summary of Data Quality Verification

| Quality Dimension | Evaluation | Evidence |
|---|---|---|
| **Syntactic Validity** | 100% | 8,124 rows parsed cleanly without malformed lines. |
| **Completeness (Active Features)** | 100% | 0 missing values across `class`, `odor`, and `cap-color`. |
| **Completeness (Global Dataset)**| 69.47% | 30.53% missing values restricted entirely to `stalk-root`. |
| **Data Consistency** | 100% | Class sums ($4,208 + 3,916 = 8,124$) and marginal counts match official UCI specifications. |
