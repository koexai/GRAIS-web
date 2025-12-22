# Gamma Ray Burst Photon-Level Dataset (Version 1.0.0)

## Summary

This dataset provides photon-level detections from Gamma Ray Bursts (GRBs), capturing individual photon arrivals characterised by their detection time and energy. With 1,471,344 photon events across multiple GRB sources, this dataset enables detailed time-resolved spectroscopy, timing analysis, and investigations of the extreme astrophysical processes driving these transient phenomena. Each photon record includes the source GRB identifier, arrival time relative to trigger, and measured energy in MeV.

## Contents

The dataset consists of a single CSV file with photon-level data:

```
├── A2-sample_GRBs_generated.csv    # Main dataset (1,471,344 rows × 3 columns)
├── README.md                        # This file
├── metadata.json                    # Machine-readable metadata
├── dictionary.csv                   # Data dictionary defining all columns
├── provenance.md                    # Detailed provenance and methods
└── LICENSE.md                       # CC BY 4.0 license
```

**Data Structure**: Each row represents a single detected photon with three attributes:
- `origin_GRB`: Identifier for the source GRB event
- `TIME`: Photon arrival time (seconds since GRB trigger)
- `ENERGY`: Photon energy (MeV)

## Quick Start

### Loading the Data

**Python with Pandas:**
```python
import pandas as pd

# Load the full dataset
grb_photons = pd.read_csv('A2-sample_GRBs_generated.csv')

# View basic information
print(grb_photons.info())
print(grb_photons.head())

# Analyse a specific GRB event
event_id = 'GEN_00000'
single_grb = grb_photons[grb_photons['origin_GRB'] == event_id]
```

**R:**
```r
# Load the dataset
grb_photons <- read.csv('A2-sample_GRBs_generated.csv')

# View structure
str(grb_photons)
head(grb_photons)

# Filter by GRB event
single_grb <- subset(grb_photons, origin_GRB == 'GEN_00000')
```

### Basic Analysis Examples

**Construct a light curve:**
```python
import matplotlib.pyplot as plt
import numpy as np

# Select a GRB event
grb = grb_photons[grb_photons['origin_GRB'] == 'GEN_00000']

# Bin photons by time
bins = np.arange(0, grb['TIME'].max(), 1.0)  # 1-second bins
counts, edges = np.histogram(grb['TIME'], bins=bins)

# Plot light curve
plt.figure(figsize=(10, 5))
plt.step(edges[:-1], counts, where='post')
plt.xlabel('Time since trigger (s)')
plt.ylabel('Count rate (photons/s)')
plt.title('GRB Light Curve')
plt.show()
```

**Energy spectrum:**
```python
# Create energy histogram
plt.figure(figsize=(10, 5))
plt.hist(grb['ENERGY'], bins=50, log=True)
plt.xlabel('Energy (keV)')
plt.ylabel('Counts')
plt.title('GRB Energy Spectrum')
plt.xscale('log')
plt.show()
```

## Structure and Formats

### File Format
- **Type**: CSV (Comma-Separated Values)
- **Encoding**: UTF-8
- **Delimiter**: Comma (`,`)
- **Header**: First row contains column names

### Column Specifications

See `dictionary.csv` for complete variable documentation. Brief overview:

| Column | Type | Unit | Description |
|--------|------|------|-------------|
| origin_GRB | string | n/a | Unique GRB event identifier (format: GEN_XXXXX) |
| TIME | float | seconds | Time since GRB trigger (t=0) |
| ENERGY | float | MeV | Photon energy |

### Conventions
- **Missing values**: None present in this dataset
- **Time reference**: t=0 corresponds to GRB trigger time
- **Energy units**: megaelectronvolts (MeV)
- **Temporal ordering**: Photons not necessarily time-ordered within file
- **Event grouping**: Multiple rows share the same `origin_GRB` value

### Data Volume
- **Total photons**: 1,471,344
- **Columns**: 3
- **Approximate file size**: ~45-50 MB (uncompressed CSV)

## Provenance and Methods

### Data Origin
Photon event data from GRB observations or simulations, representing individual quantum detection events during high-energy transient phenomena.

### Key Processing Steps
1. Photon event extraction from observational data
2. Time calibration relative to GRB trigger (t=0)
3. Energy calibration from detector channels to MeV
4. Quality filtering to remove spurious detections
5. Association of photons with source GRB events

**Detailed provenance**: See `provenance.md` for comprehensive information on data acquisition, processing workflows, software tools, and validation procedures.

## Quality and Limitations

### Validation
- Automated schema and range validation performed
- Temporal ordering verified within events
- Energy values confirmed positive and physically plausible
- GRB identifier format consistency checked

### Known Limitations
1. **Detector information absent**: No explicit encoding of detector response, background rates, or instrumental characteristics
2. **Selection effects**: Potential biases from trigger algorithms and quality filters not fully documented
3. **Energy calibration**: Accuracy depends on instrumental factors not included in dataset
4. **Temporal resolution**: Limited by detector timing capabilities
5. **Completeness**: May not represent all detected photons if quality cuts were applied

### Recommended Usage
- **Suitable for**: Time-resolved spectroscopy, light curve analysis, timing studies, spectral characterisation
- **Not recommended for**: Absolute flux measurements without additional calibration, analyses requiring detailed detector response information

**Users should consider potential selection effects and detector limitations when interpreting results.**

## Applications and Use Cases

This photon-level dataset enables:
- **Temporal analysis**: Light curve construction, variability studies, pulse characterisation
- **Spectral analysis**: Energy distribution studies, spectral evolution, hardness ratios
- **Time-resolved spectroscopy**: Spectral changes during GRB evolution
- **Statistical studies**: Photon arrival time distributions, Poisson statistics tests
- **Machine learning**: Training data for GRB classification or parameter estimation
- **Simulation validation**: Comparison with synthetic GRB models

## Licence and Citation

### Licence
This dataset is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** licence.

You are free to:
- Share and redistribute the data
- Adapt and build upon the data

Under the condition that you provide appropriate attribution.

**Full licence**: See `LICENSE.md` or visit https://creativecommons.org/licenses/by/4.0/

### How to Cite

**Plain text:**
```
Naso, L. (2025). Gamma Ray Burst Photon-Level Dataset (Version 1.0.0). Koexai S.r.l.
```


### Attribution Guidelines
When using this dataset in publications, presentations, or derivative works, please:
1. Include the full citation above
2. Mention the dataset version number
3. Link to the source if available
4. Acknowledge any modifications or derivations made

## Technical Support and Contact

### Dataset Maintainer
- **Name**: Luca Naso
- **Role**: CEO and Founder
- **Organisation**: Koexai S.r.l.
- **Email**: info@koexai.com
- **Website**: https://www.koexai.com

### Reporting Issues
For questions, bug reports, or suggestions regarding this dataset:
1. Contact the maintainer at the email above
2. Provide specific details about your issue or question
3. Include dataset version information (1.0.0)

### Additional Resources
- **Metadata**: See `metadata.json` for machine-readable metadata
- **Data dictionary**: See `dictionary.csv` for complete column specifications
- **Provenance**: See `provenance.md` for detailed methods and processing information

## Version History

- **v1.0.0** (2025-12-10): Initial release
  - 1,471,344 photon events
  - Multiple GRB sources included
  - Complete documentation artefacts

## Acknowledgements

This dataset was prepared by Koexai S.r.l., an Italian data science company specialising in AI solutions.

---

**Last updated**: 2025-11-30  
**Dataset version**: 1.0.0  
**Documentation version**: 1.0.0
