# Provenance and Methods Documentation
## Simulated Gamma-Ray Burst Event Dataset A2

**Version:** 1.0.0  
**Date:** 2025-11-30  
**Organization:** Koexai S.r.l.

---

## 1. Data Acquisition

### 1.1 Source Description

This dataset consists of synthetic gamma-ray burst events generated using a trained Generative Artificial Intelligence model.

### 1.2 Simulation Framework

**Simulation Type:** Statistically learned temporal and spectral patterns

**Source Parameters:**
- Number of simulated GRBs: 5000
- Source naming: Sequential numbering (GEN_1 through GEN_5000)
- Source positions: Distributed across the celestial sphere
- Energy range: Approximately 30 MeV to 300 GeV

---

## 2. Data Processing Pipeline

### 2.1 Raw Simulation Output

**Initial Data Products:**
- Photon arrival time
- Reconstructed photon energy (ENERGY)

### 2.2 2.3 Generation Process

The generative model was trained to reproduce the statistical distributions of:
- GRB temporal profiles
- Energy spectra
During inference, the trained model produces synthetic event samples by sampling from the learned latent representation.
No explicit detector response simulation (e.g., IRFs or spacecraft pointing history) is applied during generation unless implicitly encoded through training data.

### 2.3 Data Organization

- One csv file created per simulated GRB source
- Filename convention: `GEN_*.csv`

---

## 3. Validation and Quality Assurance

### 3.1 Pre-Release Checks:

1. File integrity verification
2. Schema validation for all required fields
3. Data type consistency checks
4. Range validation for physical quantities

### 3.2 Distribution Checks

- Energy spectrum: Power-law or broken power-law shape expected
- Time distributions: Consistency with GRB temporal profiles

## 4. Known Issues and Limitations

### 4.1 Model-Dependent Biases

The dataset reflects only patterns present in the training data. Rare or extreme physical scenarios may not be reproduced. Systematic detector effects are only indirectly encoded via training data.

### 4.2 Physical Fidelity

- No explicit instrument response recalculation
- No independent background modeling
- Temporal fine-structure limited by model capacity
- Systematic uncertainties may be simplified compared to real detectors
- Detector response based on nominal performance models
- Simplified background modeling
- Idealized trigger logic

---

## 5. Data Processing

**Core Libraries:**
- Python 3.7+
- NumPy (array operations)
- pandas (data manipulation)
- PyTables / h5py (HDF5 I/O)
- Astropy (coordinate transformations)

---

## 6. Data Lineage

### 6.1 Processing History

```
Training Data Preparation
            ↓
GenAI Model Training
            ↓
Model Inference / Sampling
            ↓
Post-Generation Validation
            ↓
Dataset A2 (this release)
```

### 6.2 Traceability

**Event Tracking:**
- origin_GRB: Unique identifier for the Gamma Ray Burst source event from which the photon was detected

**Provenance Metadata:**
- File creation dates: 2025-12-04
- Processing date: 2025-12-04
- Dataset release date: 2025-12-05
- Version: 1.0.0

---

## 7. Contact and Support

**Technical Questions:**
Koexai S.r.l.  
Email: info@koexai.com  
Website: https://www.grais.koexai.com

**Data Requests:**
For access to raw simulation output, alternative filtering criteria, or custom simulations, please contact the data provider.

**Bug Reports:**
If you identify data quality issues or inconsistencies, please report them to info@koexai.com with:
- File name(s) affected
- Description of the issue
- Steps to reproduce (if applicable)

**Document Version:** 1.0  
**Last Updated:** 2025-11-30  
**Authors:** Koexai S.r.l. Data Team
