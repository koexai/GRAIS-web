# Dataset B1: Gamma-Ray Burst Candidates - Version 1.0.0 | Release Date: 2025-12-05

## Overview
The B1 Candidate Dataset represents the "discovery" layer of our pipeline. Unlike Datasets A1 and A2, which provide ground-truth simulations and synthetic samples, Dataset B1 is the result of applying advanced anomaly detection to processed gamma-ray data. Developed by Koexai S.r.l., this dataset contains statistically significant transient candidates that deviate from the expected background, identified using the GRAIS detection framework.

## 1. Detection Framework
The identification of candidates in Dataset B1 is driven by an semi-supervised machine learning approach. The goal is to isolate sudden, localized increases in photon activity that suggest a transient event like a Gamma-Ray Burst.

The detection process utilizes a multi-stage architecture:
- Stage-1 (CNN Screening): A Convolutional Neural Network scans structured tensors of spatial pixels and temporal windows. This stage acts as a high-speed filter to flag preliminary anomalies.
- Stage-2 (DeepSet Refinement): Candidates are then processed by a DeepSet model, which specializes in evaluating sets of events. This stage refines the anomaly score and significantly reduces the rate of false positives.
- Likelihood Scoring: Every candidate in this dataset is assigned a probability score (prob_stage2), indicating the model's confidence that the cluster of photons represents a non-background event.

## 2. Processing pipeline
The journey from raw photons to a B1 candidate involves several critical preprocessing and inference steps:
- Preparation: Photon data is segmented into time windows and binned into spatial sky pixels. These are normalized based on instrument exposure to ensure that detection is not biased by where the telescope was pointing.
- Feature Extraction: Aggregated counts are transformed into structured tensors—essentially "images" of the sky across different time slices—for the models to analyze.
- Thresholding: To ensure data quality, only candidates whose anomaly scores exceed a strict predefined threshold are retained.
- HDF5 Packaging: All identified candidates are compiled into a single, high-performance HDF5 file. This format uses internal compression and a compound dtype table to store spatial, temporal, and scoring metadata efficiently.

## 3. Validation & Usage Constraints
Dataset B1 is a powerful tool for transient research, but it should be handled with an understanding of its probabilistic nature:
- No Spectral Fitting: At this stage, no physical spectral likelihood fitting or Bayesian block analysis has been applied. These candidates are "raw" detections awaiting further astrophysical characterization.
- Purity vs. Completeness: The selection threshold is tuned for a specific balance. Increasing sensitivity would capture more bursts but would also increase the presence of false positives.

## 4. Technical Specifications
The dataset is optimized for a modern Python stack. To interact with the files, we recommend:
- Python 3.7+
- Core Libraries: NumPy, Pandas, Astropy, and PyTables/h5py.

## 5. Provenance & Support
Every event in this release is fully traceable.

### Contact Information
For technical support, custom data requests, or to report inconsistencies, please contact the Koexai S.r.l. Data Team:
- **Email:** info@koexai.com
- **Web:** www.grais.koexai.com