# Provenance and Methods Documentation
## Gamma-Ray Burst Candidate Dataset B1

**Version:** 1.0.0  
**Date:** 2025-11-30  
**Organization:** Koexai S.r.l.

---

## 1. Data Acquisition

### 1.1 Source Description

This dataset originates from the application of a trained anomaly detection model to processed gamma-ray event data consistent with Fermi-LAT observations. The dataset contains candidate gamma-ray burst events identified as statistically anomalous with respect to the learned background and nominal event distributions. B1 is the output of a machine learning inference stage applied to preprocessed event data.

### 1.2 Detection Framework

**Detection Type:** Unsupervised / Semi-supervised anomaly detection

**Input Data:**
- Processed photon event data (time, energy, sky position)
- Structured in spatial pixels and temporal windows

**Model Objective:**
- Identify statistically significant deviations from learned background behavior
- Assign anomaly likelihood scores to event clusters

**Output Data:**
- List of candidate transient events
- Associated anomaly scores and metadata

### 1.3 Detection Software

The anomaly detection model was implemented within the GRAIS pipeline. Core components include:
- Preprocessing and feature extraction stage
- Stage-1 CNN screening
- Stage-2 DeepSet refinement candidates list

The model was trained using real and/or simulated data consistent with Dataset A1 and A2 statistical properties.

---

## 2. Data Processing Pipeline

### 2.1 Input Data Preparation

Prior to anomaly detection, input event data undergo:
- Time window segmentation
- Spatial binning into sky pixels
- Energy selection within defined analysis range
- Exposure-consistent normalization

### 2.2 Anomaly Detection Inference

Processing Steps:

1. **Feature Extraction:**
   - Aggregation of photon counts in spatial-temporal bins
   - Construction of structured tensors for model input

2. **Stage-1 Model Screening:**
   - Convolutional neural network–based anomaly scoring
   - Preliminary candidate selection

3. **Stage-2 Refinement:**
   - DeepSet-based anomaly scoring
   - Secondary model evaluation (prob_stage2)
   - Suppression of false positives

4. **Thresholding:**
- Candidates retained if anomaly score exceeds predefined threshold

### 2.3 Data Organization

**File Generation:**
- One HDF5 file created containing all candidates
- Internal structure: Events stored in `/events` group as PyTables Table
- Metadata: File attributes include PyTables version, class information

**Data Packaging:**
- Format: HDF5 (Hierarchical Data Format version 5)
- Compression: HDF5 internal compression applied
- Table structure: Compound dtype with 6 fields per event

---

## 3. Validation and Quality Assurance

### 3.1 Automated Validation

**Pre-Release Checks:**

- File integrity verification
- Schema validation of required fields
- Data type consistency checks
- Range validation:

### 3.2 Statistical Validation

**Distribution Checks:**
- Distribution of anomaly scores inspected for stability
- Cross-comparison between Stage-1 and Stage-2 scores
- Spot-check validation against known simulated transients (where available)
- Verification of spatial clustering consistency

## 4. Known Issues and Limitations

### 4.1 Model Limitations

- Anomaly detection does not guarantee astrophysical origin
- False positives may be present
- Detection sensitivity depends on training data domain

### 4.2 Statistical Limitations

- Candidate density varies across time and sky position
- Detection performance influenced by exposure variations
- Threshold selection impacts purity vs completeness trade-off

### 4.3 Physical Interpretation Limitations

- No spectral likelihood fitting is applied at this stage
- No temporal Bayesian block analysis included
- Further astrophysical validation required for confirmation

---

## 5. Data Processing

**Core Libraries:**
- Python 3.7+
- NumPy (array operations)
- pandas (data manipulation)
- PyTables / h5py (HDF5 I/O)
- Astropy (coordinate transformations)

**File Format:**
- HDF5 library version 1.8+
- PyTables format version 2.1

---

## 6. Data Lineage

### 6.1 Processing History

```
Processed Event Dataset
           ↓
Stage-1 CNN
           ↓
Stage-2 DeepSet
           ↓
Thresholding and Candidate Selection
           ↓
Dataset B1 (this release)
```

### 6.2 Traceability

**Event Tracking:**
- Unique candidate identifier
- Pixel index linking to sky bin
- Time window identifier

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
