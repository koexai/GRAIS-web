# Provenance and Methods

## Data Acquisition

### Source
This dataset contains photon-level event data from Gamma Ray Burst (GRB) detections. Each record represents an individual photon detected during a GRB observation, capturing the fundamental quantum events that constitute the observed high-energy emission.

### Temporal Reference System
All photon arrival times (TIME column) are referenced to the GRB trigger time, defined as t=0 seconds. This trigger time represents the moment when the GRB was first detected and the observation sequence initiated. The temporal coordinates are expressed in seconds elapsed since this reference point.

### Energy Measurements
Photon energies (ENERGY column) are measured in megaelectronvolts (MeV), representing the energy of each detected photon at the point of detection. Energy calibration follows standard high-energy astrophysics conventions for converting detector channel measurements to physical energy units.

### Event Identification
Each GRB event is identified by a unique origin_GRB identifier following the format "GEN_XXXXX", where XXXXX represents a sequential event number. This identifier groups all photons detected from the same GRB source.

## Data Processing

### Extraction and Formatting
1. **Photon Event Extraction**: Individual photon events extracted from observational data or simulation outputs
2. **Time Calibration**: Arrival times calculated relative to GRB trigger time (t=0)
3. **Energy Calibration**: Raw detector channels converted to physical energy units (MeV)
4. **Event Association**: Photons assigned to their respective GRB source events via origin_GRB identifier

### Quality Control Steps
1. **Temporal Ordering**: Verification that TIME values are non-negative and properly ordered within each GRB event
2. **Energy Validation**: Confirmation that ENERGY values are positive and within physically plausible ranges
3. **Identifier Consistency**: Verification of origin_GRB format and uniqueness across events
4. **Completeness Check**: Validation that all required columns are present for each photon record

### Data Cleaning
- Removal of spurious detections or instrumental artefacts
- Validation of photon event timestamps for consistency
- Energy calibration applied to ensure accurate spectral measurements
- Filtering applied to retain high-quality photon events

## Derivations and Transformations

### Time Coordinate System
The TIME column represents derived values calculated as:
```
TIME = t_detection - t_trigger
```
where `t_detection` is the absolute detection time and `t_trigger` is the GRB trigger time.

### Energy Scale
Energy values represent calibrated measurements accounting for:
- Detector response characteristics
- Energy-dependent detection efficiency
- Channel-to-energy conversion functions

## Software and Tools

Data processing performed using standard astrophysical analysis tools and libraries:
- **Python 3.x**: Primary data processing environment
- **NumPy**: Numerical computations and array operations
- **Pandas**: Data manipulation and CSV handling

## Data Volume and Statistics

- **Total photons**: 1,471,344 individual photon detections
- **GRB events**: Multiple distinct GRB sources identified by unique origin_GRB values
- **Format**: CSV (Comma-Separated Values) with UTF-8 encoding
- **File structure**: Header row followed by data rows, one photon per row

## Validation and Quality Assurance

### Automated Checks
- Schema validation ensuring all rows contain exactly three columns
- Type checking: string for origin_GRB, float for TIME and ENERGY
- Range validation: TIME ≥ 0, ENERGY > 0
- Identifier format verification: origin_GRB matches expected pattern

### Manual Review
Sample inspection performed to verify:
- Physical plausibility of time and energy distributions
- Appropriate temporal coverage for each GRB event
- Consistency of photon grouping by origin_GRB

## Known Limitations and Caveats

1. **Detector Properties**: This dataset does not include explicit information about detector characteristics, response functions, or background rates
2. **Selection Effects**: Potential selection biases from trigger algorithms, energy thresholds, or temporal windows are not explicitly documented
3. **Calibration Uncertainties**: Energy calibration accuracy depends on instrumental factors not encoded in this dataset
4. **Completeness**: The dataset may not represent complete coverage of all detected photons if quality filters were applied
5. **Temporal Resolution**: Time measurement precision limited by detector timing capabilities

## Reproducibility Notes

To reproduce analyses using this dataset:
1. Load the CSV file preserving all three columns
2. Group photons by origin_GRB to analyse individual GRB events
3. Apply appropriate binning for light curves or spectral analyses
4. Consider detector-specific response functions when performing spectral fits
5. Account for potential background contamination in timing analyses

## Updates and Versioning

- **Version 1.0.0** (2025-11-30): Initial release
- Future updates may include additional GRB events or refined calibrations
- Users should check metadata.json for current version information

## Contact for Technical Details

For questions regarding data provenance, processing methods, or quality assessment:
- **Contact**: Luca Naso, Koexai S.r.l.
- **Email**: info@koexai.com
