# Dataset A2: Simulated Gamma-Ray Burst Events via Generative AI - Version 1.0.0 | Release Date: 2025-12-05

## Overview
Welcome to the A2 Synthetic Dataset, a cutting-edge collection of Gamma-Ray Burst (GRB) events developed by Koexai S.r.l. Unlike traditional Monte Carlo simulations, this dataset leverages Generative Artificial Intelligence to produce synthetic event samples. By learning from the complex statistical distributions of temporal and spectral patterns, our model generates high-fidelity GRB data that mirrors the characteristics of real-world observations.

## 1. Data Generation & Simulation
The foundation of Dataset A2 is a generative model trained to understand the underlying "DNA" of a gamma-ray burst. Instead of calculating individual particle interactions, the model samples from a learned latent representation to create new, synthetic event sequences.

Key features of this generation process include:
- Statistical Learning: The model was trained specifically to reproduce the intricate temporal profiles (light curves) and energy spectra (power-law shapes) found in high-energy astrophysics.
- Scope: The synthetic events cover a wide celestial distribution and an energy range of approximately 30 MeV to 300 GeV.
- Implicit Modeling: While there is no explicit simulation of detector geometry or spacecraft pointing, these effects are indirectly encoded into the data through the patterns present in the original training sets.

## 2. Process & Pipeline
The transition from a trained model to a usable dataset follows a streamlined "Inference-to-File" pipeline:
- Model Inference: During the sampling phase, the AI generates two primary data products: the photon arrival time and the reconstructed photon energy (ENERGY).
- Post-Generation Validation: Every generated sequence is checked against physical boundaries to ensure that energy values and temporal distributions remain within scientifically plausible limits.
- Data Packaging: To ensure maximum accessibility, Dataset A2 is provided in CSV format. Each simulated GRB source is saved as an individual file (following the GEN_*.csv convention), making it easy to load into any data analysis environment without specialized HDF5 libraries.

## 3. Data Quality & Model Limitations
As this dataset is a product of Generative AI, users should be mindful of the following characteristics:
- Model-Dependent Biases: The synthetic events reflect the patterns of the data the model was trained on. Consequently, rare or extreme physical phenomena that were not well-represented in the training data may not appear in this dataset.
- Physical Fidelity: There is no explicit recalculation of instrument response functions (IRFs). Temporal "fine-structures" in the bursts are limited by the capacity and resolution of the generative model.
- Simplified Logic: Background modeling and trigger logic are idealized, as they are learned statistically rather than simulated through direct physical interaction.

## 4. Technical Specifications
The dataset is optimized for a modern Python stack. To interact with the files, we recommend:
- Python 3.7+
- Core Libraries: NumPy, Pandas, Astropy, and PyTables/h5py.

## 5. Provenance & Support
Every event in this release is fully traceable. Each entry contains an id that links it back to the original GRB, ensuring the lineage of the data is never lost.

### Contact Information
For technical support, custom data requests, or to report inconsistencies, please contact the Koexai S.r.l. Data Team:
- **Email:** info@koexai.com
- **Web:** www.grais.koexai.com