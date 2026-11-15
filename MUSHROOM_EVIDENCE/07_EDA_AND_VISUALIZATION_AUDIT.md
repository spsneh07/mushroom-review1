# 07. Exploratory Data Analysis & Visualization Audit

**Inspected Notebook Cells**: Cells 9, 10, 11, 12, 13  
**Figure Export Directory**: `MUSHROOM_EVIDENCE/figures/`  
**Visualization Libraries**: `matplotlib.pyplot` (version 3.11.0), `seaborn`  
**Audit Status**: VERIFIED  

---

## 1. Overview of Generated Figures

The audited notebook contains five inline visualizations rendered via Seaborn and Matplotlib. All five figures have been extracted directly from the base64 metadata of `mushrooms.ipynb` and stored as standalone PNG images under `MUSHROOM_EVIDENCE/figures/` without altering the original notebook.

| Plot # | Source Notebook Cell | Output Artifact Filename | Plot Type | Primary Variable(s) | Status |
|---|---|---|---|---|---|
| **1** | Cell 9 | `fig_01_class_distribution.png` | Count Plot (`sns.countplot`) | `class` (`e` vs. `p`) | **VERIFIED** |
| **2** | Cell 10 | `fig_02_odor_distribution.png` | Count Plot (`sns.countplot`) | `odor` (9 categories) | **VERIFIED** |
| **3** | Cell 11 | `fig_03_odor_vs_class.png` | Grouped Count Plot (`hue="class"`) | `odor` stratified by `class` | **VERIFIED** |
| **4** | Cell 12 | `fig_04_cap_color_vs_class.png` | Grouped Count Plot (`hue="class"`) | `cap-color` stratified by `class` | **VERIFIED** |
| **5** | Cell 13 | `fig_05_probability_events_bar.png` | Bar Chart (`sns.barplot`) | Event Frequencies ($A$, $B$, $A \cap B$) | **VERIFIED** |

---

## 2. Detailed Technical Audit of Individual Figures

### Figure 1: Edible vs. Poisonous Mushrooms Class Distribution
- **Notebook Cell**: Cell 9 (Execution Count: 4)
- **Source Code**:
  ```python
  import seaborn as sns
  import matplotlib.pyplot as plt

  sns.countplot(data=df, x="class")
  plt.title("Edible vs Poisonous Mushrooms")
  plt.xlabel("Class")
  plt.ylabel("Count")
  plt.show()
  ```
- **Exported File**: `figures/fig_01_class_distribution.png` (13,273 bytes)
- **What it Demonstrates**: Demonstrates the global class balance between the two target categories.
- **Actual Quantitative Observations**:
  - Edible (`e`): 4,208 instances (51.80%)
  - Poisonous (`p`): 3,916 instances (48.20%)
  - Confirms a well-balanced binary dataset with zero severe class skew.
- **Relevant Academic Report Section**: *Dataset Exploration & Target Balance Analysis*.
- **Suggested Academic Caption**:  
  *Figure 1: Class frequency distribution of edible ($e = 4,208$) versus poisonous ($p = 3,916$) mushroom specimens in the UCI Mushroom dataset ($N = 8,124$).*

---

### Figure 2: Mushroom Odor Distribution
- **Notebook Cell**: Cell 10 (Execution Count: 5)
- **Source Code**:
  ```python
  sns.countplot(data=df, x="odor")
  plt.title("Mushroom Odor Distribution")
  plt.xlabel("Odor")
  plt.ylabel("Count")
  plt.show()
  ```
- **Exported File**: `figures/fig_02_odor_distribution.png` (13,663 bytes)
- **What it Demonstrates**: Displays the unconditional marginal frequency of all 9 distinct odor categories.
- **Actual Quantitative Observations**:
  - `n` (none): 3,528 (43.43%)
  - `f` (foul — Event A): 2,160 (26.59%)
  - `s` (spicy): 576 (7.09%)
  - `y` (fishy): 576 (7.09%)
  - `a` (almond): 400 (4.92%)
  - `l` (anise): 400 (4.92%)
  - `p` (pungent): 256 (3.15%)
  - `c` (creosote): 192 (2.36%)
  - `m` (musty): 36 (0.44%)
- **Relevant Academic Report Section**: *Univariate Feature Distribution — Olfactory Profiles*.
- **Suggested Academic Caption**:  
  *Figure 2: Frequency distribution of nine nominal odor categories across all 8,124 mushroom specimens, highlighting odorless ($n$) and foul ($f$) as dominant phenotypes.*

---

### Figure 3: Odor vs. Mushroom Class
- **Notebook Cell**: Cell 11 (Execution Count: 6)
- **Source Code**:
  ```python
  sns.countplot(data=df, x="odor", hue="class")
  plt.title("Odor vs Mushroom Class")
  plt.xlabel("Odor")
  plt.ylabel("Count")
  plt.show()
  ```
- **Exported File**: `figures/fig_03_odor_vs_class.png` (14,813 bytes)
- **What it Demonstrates**: Bivariate relationship between odor categories and toxicity classification.
- **Actual Quantitative Observations**:
  - **Exclusively Edible Odors**: `a` (almond: 400 e, 0 p), `l` (anise: 400 e, 0 p).
  - **Exclusively Poisonous Odors**: `f` (foul: 0 e, 2,160 p), `s` (spicy: 0 e, 576 p), `y` (fishy: 0 e, 576 p), `p` (pungent: 0 e, 256 p), `c` (creosote: 0 e, 192 p), `m` (musty: 0 e, 36 p).
  - **Mixed Odor**: `n` (none: 3,408 e, 120 p).
  - **Critical Analytical Takeaway**: Foul odor (`f`, Event A) is a **100% deterministic indicator** of poisonous toxicity ($P(C \mid A) = 1.0$).
- **Relevant Academic Report Section**: *Bivariate Associations & Deterministic Feature Detection*.
- **Suggested Academic Caption**:  
  *Figure 3: Stratified count plot of mushroom odors by class, illustrating that foul odor ($f$) occurs exclusively within poisonous specimens ($n = 2,160$), while almond ($a$) and anise ($l$) are uniquely edible.*

---

### Figure 4: Cap Color vs. Mushroom Class
- **Notebook Cell**: Cell 12 (Execution Count: 7)
- **Source Code**:
  ```python
  sns.countplot(data=df, x="cap-color", hue="class")
  plt.title("Cap Color vs Mushroom Class")
  plt.xlabel("Cap Color")
  plt.ylabel("Count")
  plt.show()
  ```
- **Exported File**: `figures/fig_04_cap_color_vs_class.png` (14,736 bytes)
- **What it Demonstrates**: Bivariate distribution of mushroom cap colors stratified across edible and poisonous classes.
- **Actual Quantitative Observations**:
  - Brown (`n`, Event B): 1,264 edible vs. 1,020 poisonous (Total = 2,284).
  - Gray (`g`): 1,032 edible vs. 808 poisonous (Total = 1,840).
  - Red (`e`): 624 edible vs. 876 poisonous (Total = 1,500).
  - Yellow (`y`): 400 edible vs. 672 poisonous (Total = 1,072).
  - White (`w`): 720 edible vs. 320 poisonous (Total = 1,040).
  - Buff (`b`): 48 edible vs. 120 poisonous (Total = 168).
  - Pink (`p`): 56 edible vs. 88 poisonous (Total = 144).
  - Cinnamon (`c`): 32 edible vs. 12 poisonous (Total = 44).
  - Purple (`u`): 16 edible vs. 0 poisonous (Total = 16).
  - Green (`r`): 16 edible vs. 0 poisonous (Total = 16).
  - **Analytical Takeaway**: Unlike odor, cap color is **non-deterministic**; brown caps occur in substantial numbers across both edible (~55.3%) and poisonous (~44.7%) subsets.
- **Relevant Academic Report Section**: *Morphological Feature Evaluation & Ambiguity Analysis*.
- **Suggested Academic Caption**:  
  *Figure 4: Frequency of ten mushroom cap colors stratified by edibility, demonstrating that brown caps ($n$) are distributed across both edible ($1,264$) and poisonous ($1,020$) classes.*

---

### Figure 5: Probability Event Counts
- **Notebook Cell**: Cell 13 (Execution Count: 8)
- **Source Code**:
  ```python
  plot_data = pd.DataFrame({
      "Event": ["Foul Odor", "Brown Cap", "Foul + Brown"],
      "Count": [A.sum(), B.sum(), (A & B).sum()]
  })

  sns.barplot(data=plot_data, x="Event", y="Count")
  plt.title("Probability Events")
  plt.xlabel("Event")
  plt.ylabel("Number of Mushrooms")
  plt.show()
  ```
- **Exported File**: `figures/fig_05_probability_events_bar.png` (14,501 bytes)
- **What it Demonstrates**: Direct visual comparison of the absolute specimen counts corresponding to marginal events $A$, $B$, and joint intersection $A \cap B$.
- **Actual Quantitative Observations**:
  - Foul Odor ($A$): 2,160 specimens
  - Brown Cap ($B$): 2,284 specimens
  - Foul + Brown ($A \cap B$): 288 specimens
  - **Visual Insight**: Highlights the dramatic suppression in joint co-occurrence (288) compared to marginal expectations under independence ($8,124 \times 0.0747 \approx 607.3$ expected instances).
- **Relevant Academic Report Section**: *Empirical Event Comparison & Dependency Visualization*.
- **Suggested Academic Caption**:  
  *Figure 5: Absolute specimen count comparison for marginal events Foul Odor ($A = 2,160$), Brown Cap ($B = 2,284$), and their joint intersection ($A \cap B = 288$).*
