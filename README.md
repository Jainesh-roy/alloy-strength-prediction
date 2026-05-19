# ML-Driven Yield Strength and hardness Prediction for AA7xxx Alloys

An end-to-end Materials Informatics pipeline that utilizes machine learning to predict the room-temperature yield strength of heat-treated AA7xxx series aluminum alloys based on chemical composition and thermal processing parameters.

## 📌 Project Overview
Traditional alloy development loops rely on resource-intensive, iterative laboratory casting, rolling, and destructive tensile testing. This project establishes a physics-informed predictive pipeline using advanced regression models and materials genomics to accelerate the alloy design lifecycle, substituting physical iterations with instantaneous AI inference.

---

## 🛠️ Tech Stack & Libraries
* **Language:** Python
* **Materials Genomics:** `pymatgen` (Python Materials Genomics)
* **Data Processing & EDA:** `pandas`, `numpy`, `matplotlib`, `seaborn`
* **Machine Learning Frameworks:** `scikit-learn`, `xgboost`
* **Explainable AI (XAI):** `shap` (Shapley Additive exPlanations)
* **Interactive UI:** `ipywidgets`

---

## 🧬 Core Engineering Pipeline

### 1. Data Curation & Handling
* Synthesized a 350-sample dataset modeling localized compositional spaces (`Zn`, `Mg`, `Cu`) and aging heat treatment schedules ($120^\circ\text{C}$, $135^\circ\text{C}$, $150^\circ\text{C}$).
* Engineered a targeted, robust data imputation framework to cleanly resolve processing temperature anomalies using group-based median operations.

### 2. Materials Genomics Feature Engineering
* Leveraged the `pymatgen` backend to parse raw compositional formulas (e.g., `Al0.878Zn0.085Mg0.022Cu0.015`).
* Computed elemental atomic fractions, collective structural masses, and mean atomic numbers ($Z$) to convert text formulas into high-value crystal-chemical descriptors.

### 3. Model Evaluation & Performance
We benchmarked several machine learning architectures using **Root Mean Squared Error (RMSE)** as the primary governing metric:

| Model Architecture | MAE (MPa) | RMSE (MPa) | $R^2$ Score | Status vs. Target ($\text{RMSE} < 50 \text{ MPa}$) |
| :--- | :---: | :---: | :---: | :---: |
| **Optimized Random Forest** | $\pm 42.82$ | — | $0.646$ | Baseline Comparison |
| **XGBoost Regressor** | — | **48.41** | **0.655** | **✅ PASSED** |
| **Kernel Ridge Regression**| — | **59.89** | **0.469** | **❌ FAILED** |

* **Key Insight:** Tree-based models (XGBoost) outclassed distance-based models (Kernel Ridge Regression) due to their inherent ability to handle highly non-linear, multi-scale physical boundaries (e.g., fractional compositions alongside massive raw temperatures) without failing on scale discrepancies.

### 4. Explainable AI (SHAP Insights)
SHAP analysis verified that the machine learning models organically deduced real-world thermodynamic and metallurgical constraints:
* **Solute Dominance:** Identified Zinc (`WT_Zn`) and Magnesium (`WT_Mg`) as the primary co-dependent drivers of strength, accurately verifying the physics of reinforcing intermetallic $\eta$-phase ($\text{MgZn}_2$) precipitate nucleation.
* **Aging Kinetics:** Successfully mapped the degradation curve of mechanical properties at extended aging times, accounting for precipitate over-coarsening and loss of matrix coherency (**Ostwald ripening**).

---

## 🚀 How to Run the Project

1. Clone the repository:
   ```bash
   git clone [https://github.com/YOUR_USERNAME/alloy-strength-informatics.git](https://github.com/YOUR_USERNAME/alloy-strength-informatics.git)
   cd alloy-strength-informatics
