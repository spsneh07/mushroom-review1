# 02. Dataset Description & Ground Truth Verification

**Dataset**: UCI Agaricus-Lepiota Mushroom Dataset  
**Primary Data File**: `agaricus-lepiota.data`  
**Metadata File**: `agaricus-lepiota.names`  
**Verification Method**: Independent script-based parsing & cross-tabulation of raw data  
**Audit Status**: VERIFIED  

---

## 1. Ground Truth Specification & High-Level Metrics

| Characteristic | Official Documentation Claim (`agaricus-lepiota.names`) | Notebook Loaded Dimension (`mushrooms.ipynb`) | Independent Verification | Verification Status |
|---|---|---|---|---|
| **Total Instances (Rows)** | 8,124 | 8,124 (from `df.shape`) | 8,124 records | **VERIFIED** |
| **Total Features (Columns)** | 22 attributes + 1 class | 23 columns (loaded with 23 names) | 23 comma-separated fields per line | **VERIFIED** |
| **Target Variable** | Attribute 0: `class` (`e`=edible, `p`=poisonous) | Column `class` | Binary nominal | **VERIFIED** |
| **Edible Class Count ($e$)** | 4,208 (51.8%) | Not directly printed via value_counts, but visualized in Cell 9 | 4,208 instances (51.7971%) | **VERIFIED** |
| **Poisonous Class Count ($p$)**| 3,916 (48.2%) | 3,916 (printed via `len(poisonous)` in Cell 5) | 3,916 instances (48.2029%) | **VERIFIED** |
| **Missing Attribute Values** | 2,480 in Attribute #11 (`stalk-root`) denoted by `'?'` | Not cleaned/handled; not referenced in calculations | Exactly 2,480 instances of `'?'` in `stalk-root` | **VERIFIED** |

---

## 2. Complete Attribute Master Table & Observed Distributions

The dataset consists of 23 nominal attributes. Every single row in `agaricus-lepiota.data` has exactly 23 fields separated by commas.

| Col # | Column Name | Official Categories (`agaricus-lepiota.names`) | Observed Unique Values | Observed Value Frequencies | Missing Values (`'?'`) |
|---|---|---|---|---|---|
| 0 | `class` *(Target)* | edible=`e`, poisonous=`p` | 2 | `e`: 4208, `p`: 3916 | 0 |
| 1 | `cap-shape` | bell=`b`, conical=`c`, convex=`x`, flat=`f`, knobbed=`k`, sunken=`s` | 6 | `x`: 3656, `f`: 3152, `k`: 828, `b`: 452, `s`: 32, `c`: 4 | 0 |
| 2 | `cap-surface` | fibrous=`f`, grooves=`g`, scaly=`y`, smooth=`s` | 4 | `y`: 3244, `s`: 2556, `f`: 2320, `g`: 4 | 0 |
| 3 | `cap-color` *(Event B)* | brown=`n`, buff=`b`, cinnamon=`c`, gray=`g`, green=`r`, pink=`p`, purple=`u`, red=`e`, white=`w`, yellow=`y` | 10 | `n`: 2284, `g`: 1840, `e`: 1500, `y`: 1072, `w`: 1040, `b`: 168, `p`: 144, `c`: 44, `u`: 16, 'r': 16 | 0 |
| 4 | `bruises` | bruises=`t`, no=`f` | 2 | `f`: 4748, `t`: 3376 | 0 |
| 5 | `odor` *(Event A)* | almond=`a`, anise=`l`, creosote=`c`, fishy=`y`, foul=`f`, musty=`m`, none=`n`, pungent=`p`, spicy=`s` | 9 | `n`: 3528, `f`: 2160, `y`: 576, `s`: 576, `a`: 400, `l`: 400, `p`: 256, `c`: 192, `m`: 36 | 0 |
| 6 | `gill-attachment` | attached=`a`, descending=`d`, free=`f`, notched=`n` | 2 | `f`: 7914, `a`: 210 | 0 |
| 7 | `gill-spacing` | close=`c`, crowded=`w`, distant=`d` | 2 | `c`: 6812, `w`: 1312 | 0 |
| 8 | `gill-size` | broad=`b`, narrow=`n` | 2 | `b`: 5612, `n`: 2512 | 0 |
| 9 | `gill-color` | black=`k`, brown=`n`, buff=`b`, chocolate=`h`, gray=`g`, green=`r`, orange=`o`, pink=`p`, purple=`u`, red=`e`, white=`w`, yellow=`y` | 12 | `b`: 1728, `p`: 1492, `w`: 1202, `n`: 1048, `g`: 752, `h`: 732, `u`: 492, `k`: 408, `e`: 96, `y`: 86, `o`: 64, `r`: 24 | 0 |
| 10 | `stalk-shape` | enlarging=`e`, tapering=`t` | 2 | `t`: 4608, `e`: 3516 | 0 |
| 11 | `stalk-root` | bulbous=`b`, club=`c`, cup=`u`, equal=`e`, rhizomorphs=`z`, rooted=`r`, missing=`?` | 5 | `b`: 3776, `?`: 2480, `e`: 1120, `c`: 556, `r`: 192 | **2,480 (30.53%)** |
| 12 | `stalk-surface-above-ring`| fibrous=`f`, scaly=`y`, silky=`k`, smooth=`s` | 4 | `s`: 5176, `k`: 2372, `f`: 552, `y`: 24 | 0 |
| 13 | `stalk-surface-below-ring`| fibrous=`f`, scaly=`y`, silky=`k`, smooth=`s` | 4 | `s`: 4936, `k`: 2304, `f`: 600, `y`: 284 | 0 |
| 14 | `stalk-color-above-ring` | brown=`n`, buff=`b`, cinnamon=`c`, gray=`g`, orange=`o`, pink=`p`, red=`e`, white=`w`, yellow=`y` | 9 | `w`: 4464, `p`: 1872, `g`: 576, `n`: 448, `b`: 432, `o`: 192, `e`: 96, `c`: 36, `y`: 8 | 0 |
| 15 | `stalk-color-below-ring` | brown=`n`, buff=`b`, cinnamon=`c`, gray=`g`, orange=`o`, pink=`p`, red=`e`, white=`w`, yellow=`y` | 9 | `w`: 4384, `p`: 1872, `g`: 576, `n`: 512, `b`: 432, `o`: 192, `e`: 96, `c`: 36, `y`: 24 | 0 |
| 16 | `veil-type` | partial=`p`, universal=`u` | 1 | `p`: 8124 | 0 |
| 17 | `veil-color` | brown=`n`, orange=`o`, white=`w`, yellow=`y` | 4 | `w`: 7924, `n`: 96, `o`: 96, `y`: 8 | 0 |
| 18 | `ring-number` | none=`n`, one=`o`, two=`t` | 3 | `o`: 7488, `t`: 600, `n`: 36 | 0 |
| 19 | `ring-type` | cobwebby=`c`, evanescent=`e`, flaring=`f`, large=`l`, none=`n`, pendant=`p`, sheathing=`s`, zone=`z` | 5 | `p`: 3968, `e`: 2776, `l`: 1296, `f`: 48, 'n': 36 | 0 |
| 20 | `spore-print-color` | black=`k`, brown=`n`, buff=`b`, chocolate=`h`, green=`r`, orange=`o`, purple=`u`, white=`w`, yellow=`y` | 9 | `w`: 2388, `n`: 1968, `k`: 1872, `h`: 1632, `r`: 72, `u`: 48, `o`: 48, `y`: 48, `b`: 48 | 0 |
| 21 | `population` | abundant=`a`, clustered=`c`, numerous=`n`, scattered=`s`, several=`v`, solitary=`y` | 6 | `v`: 4040, `y`: 1712, `s`: 1248, `n`: 400, `a`: 384, `c`: 340 | 0 |
| 22 | `habitat` | grasses=`g`, leaves=`l`, meadows=`m`, paths=`p`, urban=`u`, waste=`w`, woods=`d` | 7 | `d`: 3148, `g`: 2148, `p`: 1144, `l`: 832, `u`: 368, `m`: 292, `w`: 192 | 0 |

---

## 3. Key Ground Truth Insights for Machine Learning & Probability

1. **Zero-Variance Feature**:
   `veil-type` has **only 1 observed class**: `'p'` (partial) for all 8,124 records. Although the metadata defines `universal=u`, no instances of `u` exist in the dataset. This feature has zero variance and provides zero mutual information.
2. **Missing Attribute Exclusivity**:
   Missing values occur **strictly and exclusively** in attribute #11 (`stalk-root`), where 2,480 instances (30.53%) are represented as `'?'`. All other 22 attributes have complete observations.
3. **Target Exclusivity of Foul Odor ($A$)**:
   Out of all 8,124 mushrooms, exactly 2,160 have foul odor (`odor == 'f'`). Cross-tabulation reveals that **all 2,160 are poisonous (`class == 'p'`)**. Exactly 0 edible mushrooms have a foul odor ($P(A \mid \text{edible}) = 0$).
