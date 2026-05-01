## README: Machine Learning for Quantum Error Mitigation (ML-QEM)

This project implements an automated, hardware-aware framework designed to mitigate quantum noise in **Noisy Intermediate-Scale Quantum (NISQ)** devices. By utilizing classical machine learning, this framework predicts ideal quantum expectation values from a single noisy execution, bypassing the massive runtime overhead of traditional methods like Zero-Noise Extrapolation.

---

### Core Features

*   **Autonomous Backend Profiling:** A graph-based algorithm that queries IBM Quantum calibration data to dynamically identify the healthiest physical qubit clusters.
*   **Hardware-Aware Feature Engineering:** Integrates real-time device metrics—including $T_1$, $T_2$, readout error, and gate error—directly into the ML training matrix.
*   **Optimal Topology Mapping:** Uses Depth-First Search (DFS) to find linear paths (e.g., qubits [4, 5, 6, 7] on *ibm_marrakesh*) to eliminate destructive SWAP gate insertions.
*   **High-Depth Mitigation:** Validated on 178-layer Gray code Trotterized Hong-Ou-Mandel (HOM) circuits.
*   **Ensemble Performance:** Achieved a **78.17% reduction** in Mean Absolute Error (MAE) using a Random Forest Regressor.

---

### Project Architecture

The framework is organized into four primary modules:

1.  **Module 1: Automated QEM Selector:** Handles graph construction using `NetworkX`, applies coherence filtering, and ranks paths via a custom Composite Fidelity Cost Function.
2.  **Module 2: Circuit Builder:** Constructs parameterized HOM circuits using Gray code entangling bridges to minimize decoherence.
3.  **Module 3: Parallel Execution:** Manages dual execution paths on the `AerSimulator` (ideal targets) and IBM Quantum hardware via `SamplerV2` (noisy features).
4.  **Module 4: ML Pipeline:** Performs data preprocessing, 80/20 train-test splitting, and evaluates regression architectures (OLS, Gradient Boosting, and Random Forest).

---

### Installation & Usage

#### **Prerequisites**
*   Python 3.8+
*   IBM Quantum Account (API Token)

#### **Dependencies**
```bash
pip install qiskit qiskit-ibm-runtime qiskit-aer networkx pandas scikit-learn matplotlib
```

#### **Execution Flow**
1.  **Profile Hardware:** The `AutomatedQEMSelector` identifies the optimal qubit cluster based on current calibration.
2.  **Generate Data:** Run the parallel execution pipeline to generate `hom_ml_data_ibm_marrakesh.csv`.
3.  **Train & Mitigate:** Use the ML pipeline to train the Random Forest model and predict mitigated values in milliseconds.

---

### Results Summary

| Strategy | MAE | Improvement |
| :--- | :--- | :--- |
| **Raw Hardware (Baseline)** | 0.03326 | 0.00% |
| Gradient Boosting | 0.01517 | 54.40% |
| **Random Forest (Bagging)** | **0.00726** | **78.17%** |

*Note: Random Forest outperformed Gradient Boosting because its bagging mechanism effectively smooths out fundamental quantum shot noise.*

---

### Citation & Acknowledgments

**Author:** Ritwij Kumar (Roll No: 220121054)
**Supervisor:** Dr. Vibhav Bharadwaj, Department of Physics, IIT Guwahati

This project was submitted as part of the Phase I Bachelor Thesis Project (BTP) Report in April 2026.
