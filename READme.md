# High-Fidelity Analysis & AI Prediction of Twin Supersonic Impinging Jets

**[ACFM 2025]** Flow Analysis of Twin Supersonic Impinging Jets Using IDDES with Adaptive Mesh Refinement and Flow Prediction by Operator Learning Approach.

[![Status](https://img.shields.io/badge/Status-Under_Review-yellow.svg)]()
[![License](https://img.shields.io/badge/License-MIT-blue.svg)]()

---

## Overview
Vertical Take-Off and Landing (VTOL) aircraft, such as the F-35B, experience complex flow interactions during hover, including supersonic jet impingement, fountain upwash, and ground vortex formation. These phenomena induce lift loss, thermal stress, and structural fatigue.

This project presents a comprehensive framework to analyze and predict these unsteady flows by bridging High-Fidelity CFD with Scientific Machine Learning (SciML):

1.  **High-Fidelity Simulation:** Conducted IDDES (Improved Delayed Detached Eddy Simulation) with Adaptive Mesh Refinement (AMR) to resolve multi-scale turbulent structures.
2.  **Rapid Prediction:** Developed a Fourier Neural Operator (FNO) surrogate model to predict unsteady flow fields in milliseconds, overcoming the extreme computational cost of traditional CFD.

---

## Computational Scale & Data Availability
**Computational Cost:**
The full CFD simulation requires HPC-scale resources (approx. 47 million cells, 160 CPU cores). Generating 0.2 ms of physical time data takes approximately 2 days.

**Data Availability:**
Due to the massive size of the high-fidelity CFD datasets and ongoing research restrictions.

| Metric | High-Fidelity CFD (IDDES) | AI Surrogate (FNO) | Improvement |
| :--- | :--- | :--- | :--- |
| **Method** | k-omega SST IDDES + AMR | Fourier Neural Operator | - |
| **Resolution** | ~47 Million Cells (Max) | 200 x 200 | - |
| **Compute** | 160 CPU Cores | 1 GPU (RTX 3060Ti) | - |
| **Time Cost** | ~2 Days (for 0.2 ms physical time) | 0.741 Seconds | ~230,000x Speedup |

---

## Methodology

### 1. Hybrid RANS-LES with AMR
We utilized IDDES to resolve the "grey area" between RANS and LES. To capture unsteady vortical structures efficiently, we implemented a custom Adaptive Mesh Refinement (AMR) criterion based on resolving 80% of the Turbulent Kinetic Energy (TKE).

* **Solver:** ANSYS Fluent (User Defined Functions for AMR)
* **Schemes:** AUSM+ Flux Splitting, Dual Time Stepping (2nd order)
* **AMR Criteria:** Balanced criterion based on integral length scale and grid size.

### 2. Operator Learning (FNO)
We trained a Fourier Neural Operator (FNO) to map the flow state from time t to t+dt in an auto-regressive manner. Unlike CNNs, FNO learns the resolution-independent operator in the Fourier domain.

* **Architecture:** 4 Fourier Layers, 12 Modes (Height/Width), GeLU Activation.
* **Training Data:** 203 High-fidelity IDDES snapshots.

---

## Key Results

### Unsteady Flow Physics
The simulation successfully captured complex interactions such as the Mach disk, plate shock, and the instability wave in the shear layer.
* **Fountain Upwash:** Captured the intense vortex formation and flow deflection caused by the collision of two wall jets.

### AI Prediction Accuracy
The FNO model demonstrated exceptional accuracy in predicting instantaneous pressure and velocity fields compared to the ground truth (CFD).

* **Pressure Field Error:** Relative L2 error ~0.5% at the nozzle exit.
* **Wall Jet Prediction:** Relative L2 error ~1.5% at the impingement wall.

<p align="center">
<img width="2032" height="847" alt="image" src="https://github.com/user-attachments/assets/38862f3c-864e-4dc9-a312-b27895ba5253" />




</p>

---

