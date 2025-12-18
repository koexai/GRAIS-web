# Simulated Gamma-Ray Burst Event Dataset A1 (Version 1.0.0)

## Summary

This dataset contains simulated gamma-ray burst (GRB) event data generated for astrophysical research and analysis. The collection includes 100 HDF5 files with approximately 171,558 individual gamma-ray detection events from simulated GRB sources. Each event provides comprehensive information including reconstructed energy, celestial coordinates, angular parameters, timing information, and Monte Carlo truth data.

The simulations represent filtered event data from gamma-ray detectors, suitable for developing and testing analysis pipelines, training machine learning models, and validating analysis methods for high-energy astrophysics research.

## Contents

```
dataset/A1/
├── GRB_Simulated_1_filtered.hdf5
├── GRB_Simulated_2_filtered.hdf5
├── ...
├── GRB_Simulated_9525_filtered.hdf5
└── (100 HDF5 files total)
```

**Dataset Statistics:**
- Total files: 100
- Valid files with events: 91
- Empty files (no events passing filters): 9
- Total events: 171,558
- Events per file: ranges from 0 to 49,847 (mean: ~1,716)

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
- **Energy:** GeV (gigaelectronvolts)
- **Angles:** Degrees
- **Time:** MET (Mission Elapsed Time) as Unix timestamp in seconds
- **Coordinates:** 
  - RA/DEC: Equatorial coordinates in degrees (J2000 epoch)
  - L/B: Galactic coordinates in degrees
- **Missing values:** Empty HDF5 groups indicate files with no events passing quality filters

### Data Variables

Each event record contains 22 variables organized into the following categories:

**Energy Information:**
- `ENERGY`: Reconstructed photon energy (GeV)
- `MCENERGY`: True Monte Carlo photon energy (GeV)

**Spatial Coordinates:**
- `RA`: Right Ascension (degrees, J2000)
- `DEC`: Declination (degrees, J2000)
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

For detailed variable descriptions, see the **Data Dictionary** section below.

## Data Dictionary

| Column | Type | Unit | Range/Values | Description |
|--------|------|------|--------------|-------------|
| ENERGY | float32 | GeV | >0 | Reconstructed photon energy from detector response |
| RA | float32 | degrees | 0-360 | Right Ascension in J2000 equatorial coordinates |
| DEC | float32 | degrees | -90 to 90 | Declination in J2000 equatorial coordinates |
| L | float32 | degrees | 0-360 | Galactic longitude |
| B | float32 | degrees | -90 to 90 | Galactic latitude |
| THETA | float32 | degrees | ≥0 | Off-axis angle from pointing direction |
| PHI | float32 | degrees | 0-360 | Azimuthal angle around pointing direction |
| ZENITH_ANGLE | float32 | degrees | 0-180 | Zenith angle of observation (0 = overhead) |
| EARTH_AZIMUTH_ANGLE | float32 | degrees | 0-360 | Azimuthal angle relative to Earth |
| TIME | float64 | seconds | >0 | Event time in Mission Elapsed Time (MET) format |
| EVENT_ID | int32 | n/a | ≥0 | Unique identifier for each event within simulation |
| RUN_ID | int32 | n/a | ≥0 | Identifier for the simulation run |
| RECON_VERSION | int16 | n/a | ≥0 | Version number of reconstruction algorithm used |
| CONVERSION_TYPE | int16 | n/a | {0, 1} | Photon conversion mechanism (0=front, 1=back) |
| LIVETIME | float64 | seconds | >0 | Effective observation time accounting for dead time |
| MC_SRC_ID | int32 | n/a | ≥0 | Monte Carlo source identifier |
| MCENERGY | float32 | GeV | >0 | True photon energy from Monte Carlo simulation |
| CONT_ANG | float64 | degrees | ≥0 | Angular separation between reconstructed and true position |
| WEIGHT | float64 | n/a | >0 | Statistical weight for combining events in analysis |
| GTI | int64 | n/a | {0, 1} | Good Time Interval flag (1=usable, 0=excluded) |
| probability | int64 | n/a | {0, 1} | Event quality flag (1=high quality, 0=low quality) |
| GRB | string | n/a | "GRB_Simulated_*" | Source identifier matching filename |

## Provenance and Methods

### Data Generation

**Source:** Monte Carlo simulations of gamma-ray burst events using standard astrophysical simulation frameworks. The simulations model the detection and reconstruction of high-energy photons from transient astrophysical sources.

**Simulation Process:**
1. Generation of primary gamma-ray photons with specified spectral and temporal characteristics
2. Propagation through detector geometry and interaction with detector material
3. Triggering and reconstruction of detected events
4. Calculation of reconstructed event parameters (energy, direction, timing)

### Processing and Filtering

**Quality Filtering:**
Events underwent automated quality selection based on multiple criteria:
- Reconstruction quality metrics
- Angular resolution requirements
- Energy threshold cuts
- Good Time Interval (GTI) selection
- Event probability thresholds

**Filtering Results:**
- Input: Raw simulated events from 100 GRB simulations
- Output: 171,558 events passing quality filters
- Rejection: Events failing quality criteria or occurring during instrumental dead time
- Empty files: 9 simulations produced no events meeting quality standards

### Derived Quantities

**Reconstructed Parameters:**
- `ENERGY`: Calculated from detector response using energy reconstruction algorithms
- `RA`, `DEC`, `L`, `B`: Derived from reconstructed photon direction
- `CONT_ANG`: Computed as angular separation between reconstructed position and Monte Carlo truth
- `WEIGHT`: Statistical weight accounting for acceptance, efficiency, and analysis cuts

### Software and Versions

- Simulation framework: Standard gamma-ray simulation tools
- Reconstruction version: Indicated in `RECON_VERSION` field
- File format: HDF5 with PyTables 2.1 format
- Python processing: pandas, h5py, numpy

### Validation

**Quality Assurance:**
- Automated checks on reconstruction parameters
- Statistical validation of energy and angular distributions  
- File integrity verification using HDF5 validation tools
- Consistency checks between related parameters (e.g., ENERGY vs MCENERGY)

## Quality and Limitations

### Known Quality Issues

1. **Variable Event Statistics:** The number of events varies significantly between files (0 to 49,847 events), reflecting different source parameters and observing conditions in the simulations.

2. **Empty Files:** Nine files contain no events passing quality filters. These are retained as placeholders but should be skipped in analysis workflows.

3. **Low-Statistics Files:** Several files contain very few events (1-2 events), which may not be suitable for statistical analyses requiring larger samples.

4. **Simulation Limitations:** As simulated data, this dataset may not fully capture all systematic uncertainties and instrumental effects present in real observations.

### Recommended Usage

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

### Data Quality Metrics

- **Energy Range:** Approximately 10 GeV to 10,000 GeV
- **Angular Resolution:** Represented by CONT_ANG distribution
- **Valid Events:** All retained events have GTI=1 and probability=1
- **Completeness:** 91 of 100 files contain events; 9 files are empty

## How to Cite

If you use this dataset in your research, please cite it as:

**Plain text:**
```
Koexai S.r.l. (2025). Simulated Gamma-Ray Burst Event Dataset A1 (Version 1.0.0). KOEXAI-GRB-A1-v1.0
```

**BibTeX:**
```bibtex
@dataset{koexai_a1_2025,
  author = {Koexai S.r.l.},
  title = {Simulated Gamma-Ray Burst Event Dataset A1},
  year = {2025},
  version = {1.0.0},
  publisher = {Koexai S.r.l.},
}
```

## License

This dataset is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.

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
**Address:** Via Josemaria Escrivá 6, Catania, Italy  

For questions about this dataset, analysis support, or collaboration opportunities, please contact us at the email above.

## Version History

- **v1.0.0** (2025-12-05): Initial release
  - 100 simulated GRB event files
  - 171,558 total events
  - Filtered and validated dataset ready for research use
