# 01. Repository Structure & Artifact Inventory

**Repository**: `https://github.com/spsneh07/mushroom-review1`  
**Working Directory**: `c:\Users\snehp\Downloads\mushroom`  
**Audit Status**: VERIFIED  

---

## 1. File Inventory and Verification Table

The repository contains a combination of original 1988–1996 UCI Machine Learning archive artifacts, a Jupyter Notebook implementing probability analysis, and git repository configuration documents.

| File Name | Size (Bytes) | Line Count | SHA-256 Checksum | Category / Purpose | Status |
|---|---|---|---|---|---|
| `agaricus-lepiota.data` | 373,704 | 8,125 | `e65d082030501a3ebcbcd7c9f7c71aa9d28fdfff463bf4cf4716a3fe13ac360e` | Raw CSV-formatted dataset (8,124 instances + 1 trailing newline) | **VERIFIED** |
| `agaricus-lepiota.names` | 6,816 | 149 | `79e440566900901a5900febc1f3c9659e3746e306a5fa1aed4d084a1e4da76fd` | Official UCI documentation (metadata, attributes, references) | **VERIFIED** |
| `mushrooms.ipynb` | 107,314 | 461 | `f5056e54fd55812d739f0680a29d88794fb118e13c5ade88a7a488533567378a` | Jupyter Notebook containing probability analysis & EDA plots | **VERIFIED** |
| `README.md` | 5,484 | 140 | `be6362cfe174f51339d5d88c1a49f5b6f0ebeb268313deab4b5ff43d130653cf` | Project overview, mathematical formulation, and findings | **VERIFIED** |
| `README` | 853 | 22 | `982356018292e0d0cdfb310083aba2f0ef98db8961a87e5b7043f170b8106d22` | Historical UCI note from David W. Aha (dated 9/27/1988) | **VERIFIED** |
| `Index` | 193 | 8 | `b901c28403c8eeec6e72e76608411a14485af9f4567175ae5d7ee119649435d7` | UCI repository manifest dated 02 Dec 1996 | **VERIFIED** |
| `expanded.Z` | 111,577 | 352 | `cc1723a2bb79329843d902680467b0adde972ecdc171c65138f08c03135885dc` | Unix-compressed historical expanded dataset archive (1990) | **VERIFIED** |
| `.gitignore` | 256 | 22 | `c3262fd975770567e5199b39207e24c63dddaddc65e10e91e0e6f9710c4d1af9` | Git ignore rules for Python, Jupyter checkpoints, virtualenvs | **VERIFIED** |

---

## 2. Granular Inspection of Non-Code Artifacts

### A. Historical Archive File: `README`
- **Author**: David W. Aha (UCI Department of Information and Computer Science).
- **Date**: September 27, 1988.
- **Content Summary**:
  Explains the provenance of the database:
  1. The database was initially lost due to backup downtime at UCI and restored via a copy from Wayne Iba.
  2. Jeff Schlimmer (original donor) subsequently mailed a compressed version utilizing 1-character acronyms for attribute values (reducing archive size from 120KB to 50KB).
- **Audit Significance**: Explains why the attributes in `agaricus-lepiota.data` are represented as single-character nominal symbols rather than full English descriptive strings.

### B. Historical Archive File: `Index`
- **Timestamp recorded**: 02 Dec 1996.
- **Manifest entries**:
  - `02 Dec 1996      193 Index`
  - `25 Jun 1990   111577 expanded.Z`
  - `26 Feb 1990     4167 agaricus-lepiota.names`
  - `30 May 1989      853 README`
  - `30 May 1989   373704 agaricus-lepiota.data`
- **Integrity Check**: The file sizes for `expanded.Z` (111,577 B), `README` (853 B), and `agaricus-lepiota.data` (373,704 B) exactly match the sizes recorded in 1989–1996. `agaricus-lepiota.names` in the repository is 6,816 bytes because it incorporates the 1997 Wlodzislaw Duch logical rule benchmark additions.

### C. Compressed Archive File: `expanded.Z`
- **Type**: Unix compress (`Lempel-Ziv-Welch` / `compress` format).
- **Role**: Contains the expanded (uncompressed 1-character converted to full strings) or alternative format of the dataset from the 1990 distribution.
- **Audit Note**: The Jupyter notebook does not load or process `expanded.Z`; it operates exclusively on `agaricus-lepiota.data`.

---

## 3. Git Version Control State

- **Active Branch**: `main`
- **Remote Origin**: `https://github.com/spsneh07/mushroom-review1.git`
- **Upstream Tracking**: `origin/main` (clean working directory, 0 uncommitted changes).
- **Commit History**:
  1. `0161e71`: *"Initial commit: mushroom classification notebook and dataset"* (Root commit importing initial repository contents).
  2. `66f8860`: *"Add project README.md with analysis breakdown and .gitignore"* (Added structured markdown documentation and ignore specifications).
