# Dataset A1: Simulated Gamma-Ray Burst Events with gtobssim - Version 1.0.0 | Release Date: 2025-12-05

## Overview
Welcome to the A1 Simulated Dataset, a high-fidelity collection of 10,000 Gamma-Ray Burst (GRB) events developed by Koexai S.r.l. This resource is designed to bridge the gap between theoretical astrophysics and practical data science by providing a realistic environment for testing reconstruction algorithms and machine learning models.

The dataset captures the entire lifecycle of a high-energy photon—from its generation in the depths of space to its eventual capture and reconstruction by specialized gamma-ray detectors.

## 1. Data Generation & Simulation
Rather than simple mathematical models, this dataset is built upon complex Monte Carlo simulations that replicate the physical interactions of photons within a detector.

Using the Fermi Science Tools and the gtobssim utility, we simulated 10,000 unique sources (labeled GRB_Simulated_1 through 10000). These sources are distributed across the celestial sphere, covering an energy spectrum from 30 MeV to 300 GeV. To ensure maximum realism, the simulations incorporate:
- Instrument Response Functions (IRFs): Specifically the P8R3_SOURCE_V3 standard.
- Spacecraft Dynamics: Real-world pointing history from weekly spacecraft files was used to model exposure and livetime variations.
- Environmental Factors: Cosmic ray backgrounds were modeled and subsequently rejected, mirroring the challenges of real-space observations.

## 2. Processing & Reconstruction Pipeline
The raw simulation output underwent a rigorous reconstruction process to transform detector "hits" into usable astronomical data:
- Directional Analysis: Arrival directions (RA, Dec) were determined through pattern recognition and maximum likelihood fitting. Each event includes a quality metric based on its containment angle.
- Energy Estimation: Since detectors rarely capture a photon's energy perfectly, we applied calibrated estimators to reconstruct the likely energy (ENERGY) from the total deposition, validating it against the "ground truth" (MCENERGY).
- Data Packaging: The final output is organized into HDF5 files (one per GRB). We utilized the PyTables framework to ensure that even with 22 distinct fields per event, queries remain lightning-fast and memory-efficient.

## 3. Data Quality & Known Characteristics
While we strive for perfection, users should be aware of specific statistical and physical nuances within the data:
- Statistical Variance: Due to the nature of random simulations, the number of surviving events per file varies. Approximately 9 simulations resulted in zero surviving events after filtering, and several others have very low statistics (1–2 events).
- Resolution Limits: Angular resolution (CONT_ANG) is not constant; it degrades at lower energies and higher off-axis angles.
- Energy Biases: Minor systematic uncertainties may exist at the extreme edges of the energy spectrum (near 30 MeV or 300 GeV).

## 4. Technical Specifications
The dataset is optimized for a modern Python stack. To interact with the files, we recommend:
- Python 3.7+
- Core Libraries: NumPy, Pandas, Astropy, and PyTables/h5py.

Structure: Each file (GRB_Simulated_<ID>_filtered.hdf5) contains an /events group stored as a compound dtype table.

## 5. Provenance & Support
Every event in this release is fully traceable. Each entry contains a RUN_ID and EVENT_ID that links it back to the original simulation parameters, ensuring the lineage of the data is never lost.

### Contact Information
For technical support, custom data requests, or to report inconsistencies, please contact the Koexai S.r.l. Data Team:
- **Email:** info@koexai.com
- **Web:** www.grais.koexai.com