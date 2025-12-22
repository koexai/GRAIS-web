# Simulated Gamma-Ray Burst Event Dataset A1 (Version 1.0.0)

## Summary

This dataset contains simulated gamma-ray burst (GRB) event data generated for astrophysical research and analysis. The collection includes 100 HDF5 files with 171558 individual gamma-ray detection events from simulated GRB sources. Each event provides comprehensive information including reconstructed energy, celestial coordinates, angular parameters, timing information, and Monte Carlo truth data.

The simulations represent filtered event data from gamma-ray detectors, suitable for developing and testing analysis pipelines, training machine learning models and validating analysis methods for high-energy astrophysics research.

## Contents

```
dataset/A1/
├── GRB_Simulated_1_filtered.hdf5
├── GRB_Simulated_2_filtered.hdf5
├── ...
├── GRB_Simulated_9525_filtered.hdf5
└── (100 HDF5 files total)
```

## Quick Start

### Reading the Data

The dataset uses HDF5 format and can be read using Python with pandas and h5py:

```python
import pandas as pd

# Read a single file
df = pd.read_hdf('GRB_Simulated_*_filtered.hdf5', 'events')

# Display basic information
print(df.info())
print(df.head())

# Access specific columns
energies = df['ENERGY']
coordinates = df[['RA', 'DEC']]
```

### Requirements

- Python 3.7+
- pandas
- h5py
- pytables (tables)

Install dependencies:
```bash
pip install pandas h5py tables
```

## Structure and Formats

### File Format
- **Format:** HDF5 (Hierarchical Data Format version 5)
- **Encoding:** Binary HDF5 with PyTables structure
- **Compression:** HDF5 internal compression
- **Key path:** Data stored at `/events` within each file

### File Naming Convention
Files follow the pattern `GRB_Simulated_<ID>_filtered.hdf5` where `<ID>` is a unique numeric identifier for each simulated GRB source.

### Coordinate Systems and Units
- **Energy:** MeV (gigaelectronvolts)
- **Angles:** Degrees
- **Time:** MET (Mission Elapsed Time) as Unix timestamp in seconds
- **Coordinates:** 
  - RA/DEC: Equatorial coordinates in degrees
  - L/B: Galactic coordinates in degrees

### Data Variables

Each event record contains 22 variables organized into the following categories:

**Energy Information:**
- `ENERGY`: Reconstructed photon energy (MeV)
- `MCENERGY`: True Monte Carlo photon energy (MeV)

**Spatial Coordinates:**
- `RA`: Right Ascension (degrees)
- `DEC`: Declination (degrees)
- `L`: Galactic longitude (degrees)
- `B`: Galactic latitude (degrees)

**Angular Parameters:**
- `THETA`: Off-axis angle (degrees)
- `PHI`: Azimuthal angle (degrees)
- `ZENITH_ANGLE`: Observation zenith angle (degrees)
- `EARTH_AZIMUTH_ANGLE`: Earth azimuth angle (degrees)
- `CONT_ANG`: Containment angle between reconstructed and true position (degrees)

**Timing:**
- `TIME`: Event detection time (MET seconds)
- `LIVETIME`: Effective observation livetime (seconds)

**Event Identification:**
- `EVENT_ID`: Unique event identifier (integer)
- `RUN_ID`: Simulation run identifier (integer)
- `MC_SRC_ID`: Monte Carlo source identifier (integer)
- `GRB`: Source name string (e.g., "GRB_Simulated_1")

**Quality and Reconstruction:**
- `RECON_VERSION`: Reconstruction algorithm version (integer)
- `CONVERSION_TYPE`: Photon conversion type flag (integer)
- `GTI`: Good Time Interval flag (1 = good, 0 = excluded)
- `probability`: Event quality probability (integer)
- `WEIGHT`: Statistical weight for analysis (float)

For detailed variable descriptions, see the **Data Dictionary** file.

## Provenance and Methods

For detailed informations about provenance of the dataset and methods used for obtaining it, see the **Provenance and Methods** file.

## Recommended Usage

**Suitable for:**
- Algorithm development and testing
- Machine learning model training
- Analysis pipeline validation
- Educational purposes
- Method comparison studies

**Not recommended for:**
- Direct comparison with observational data without systematic corrections
- Studies requiring precise instrumental backgrounds
- Analyses sensitive to detector systematics not included in simulations
- Statistical studies using low-event-count files (<10 events)

## How to Cite

If you use this dataset in your research, please cite it as indicated in **Citation** section. This dataset is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.

You are free to:
- Share — copy and redistribute the material in any medium or format
- Adapt — remix, transform, and build upon the material for any purpose, even commercially

Under the following terms:
- Attribution — You must give appropriate credit, provide a link to the license, and indicate if changes were made

Full license text: https://creativecommons.org/licenses/by/4.0/

## Contact

**Maintainer:** Koexai S.r.l.  
**Email:** info@koexai.com  
**Website:** https://www.koexai.com 
**LinkedIn:** https://www.linkedin.com/company/koexai/

For questions about this dataset, analysis support, or collaboration opportunities, please contact us at the email above.

## Version History

- **v1.0.0** (2025-11-30): Initial release
  - 100 simulated GRB event files as an example
  - 171558 total events
  - Filtered and validated dataset ready for research use
