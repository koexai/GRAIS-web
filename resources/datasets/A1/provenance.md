# Provenance and Methods Documentation
## Simulated Gamma-Ray Burst Event Dataset A1

**Version:** 1.0.0  
**Date:** 2025-11-30  
**Organization:** Koexai S.r.l.

---

## 1. Data Acquisition

### 1.1 Source Description

This dataset originates from Monte Carlo simulations of gamma-ray burst (GRB) events designed to model high-energy photon detection by space-based or ground-based gamma-ray observatories. The simulations represent the complete detector response chain from primary photon generation through event reconstruction.

### 1.2 Simulation Framework

**Simulation Type:** Full Monte Carlo simulation of gamma-ray detection
**Physical Processes Modeled:**
- Gamma-ray production from astrophysical sources
- Atmospheric propagation (if applicable)
- Detector geometry and material interactions
- Trigger logic and event selection
- Background rejection algorithms

**Simulation Components:**
1. **Source Generation:** Gamma-ray burst sources with varied spectral indices, luminosities, and temporal profiles
2. **Photon Propagation:** Ray-tracing from source through observation geometry
3. **Detector Response:** Interaction with detector materials, energy deposition, signal generation
4. **Reconstruction:** Direction and energy reconstruction from detector signals
5. **Analysis Cuts:** Application of quality filters to select high-confidence events

### 1.3 Simulation Parameters

**Source Parameters:**
- Number of simulated GRBs: 100
- Source naming: Sequential numbering (GRB_Simulated_1 through GRB_Simulated_9525)
- Source positions: Distributed across the celestial sphere
- Energy range: Approximately 10 GeV to 10,000+ GeV
- Temporal characteristics: Transient burst profiles with varying durations

**Observational Parameters:**
- Observation mode: Pointed observations toward GRB positions
- Exposure time: Variable per simulation
- Detector configuration: Standard gamma-ray detector geometry
- Background: Cosmic ray background modeled and rejected

---

## 2. Data Processing Pipeline

### 2.1 Raw Simulation Output

**Initial Data Products:**
- Individual photon events with true (Monte Carlo) parameters
- Detector interactions and signal formation
- Trigger decisions and readout information
- Timing information in Mission Elapsed Time (MET) format

### 2.2 Event Reconstruction

**Reconstruction Algorithms:**

1. **Direction Reconstruction:**
   - Input: Detector hit patterns and signal amplitudes
   - Output: Reconstructed arrival direction (RA, DEC, L, B, THETA, PHI)
   - Method: Pattern recognition and maximum likelihood fitting
   - Quality metric: Containment angle (CONT_ANG)

2. **Energy Reconstruction:**
   - Input: Total energy deposition in detector
   - Output: Reconstructed photon energy (ENERGY)
   - Method: Calibrated energy estimator accounting for detector response
   - Validation: Comparison with Monte Carlo truth energy (MCENERGY)

3. **Time Reconstruction:**
   - Input: Detector trigger time stamps
   - Output: Event time (TIME) and effective livetime (LIVETIME)
   - Precision: Sub-second timing accuracy

### 2.3 Quality Filtering

**Filter Criteria:**

Events were required to pass all of the following quality cuts:

1. **Reconstruction Quality:**
   - Valid reconstruction solution obtained
   - Reconstruction convergence flags met
   - Angular and energy uncertainties within acceptable ranges

2. **Event Selection:**
   - Energy threshold: Minimum reconstructed energy cut
   - Angular cut: Maximum off-axis angle (THETA)
   - Containment requirement: Reasonable agreement between reconstructed and true positions

3. **Temporal Selection:**
   - Good Time Interval (GTI) requirement: GTI = 1
   - Livetime > 0 (excludes dead time periods)

4. **Quality Flags:**
   - Event probability = 1 (high-quality events only)
   - Valid RECON_VERSION identifier

**Filtering Results:**
- Input events: Full simulation output from 100 GRB sources
- Output events: 171,558 events passing all quality cuts
- Rejection rate: Variable per simulation
- Empty simulations: 9 sources produced zero events passing cuts

### 2.4 Data Organization

**File Generation:**
- One HDF5 file created per simulated GRB source
- Filename convention: `GRB_Simulated_<ID>_filtered.hdf5`
- Internal structure: Events stored in `/events` group as PyTables Table
- Metadata: File attributes include PyTables version, class information

**Data Packaging:**
- Format: HDF5 (Hierarchical Data Format version 5)
- Compression: HDF5 internal compression applied
- Table structure: Compound dtype with 22 fields per event
- Index optimization: PyTables indexing for efficient queries

---

## 3. Derived Quantities

### 3.1 Coordinate Transformations

**Equatorial to Galactic:**
- Input: RA, DEC (J2000 equatorial coordinates)
- Output: L, B (Galactic coordinates)
- Method: Standard IAU coordinate transformation
- Library: Astropy or equivalent

**Angular Parameters:**
- THETA: Off-axis angle computed from reconstructed direction and telescope pointing
- PHI: Azimuthal angle in telescope reference frame
- ZENITH_ANGLE: Zenith angle in local horizon coordinates
- EARTH_AZIMUTH_ANGLE: Azimuth in Earth-centered reference frame

### 3.2 Statistical Weights

**Weight Calculation:**
The WEIGHT field provides statistical weights for proper event combination in analyses:

```
WEIGHT = (Acceptance × Efficiency) / (Simulation_Rate)
```

Components:
- **Acceptance:** Geometric acceptance of detector for event direction
- **Efficiency:** Combined trigger and reconstruction efficiency
- **Simulation_Rate:** Rate normalization from Monte Carlo

Usage: Events should be weighted by WEIGHT when computing rates, spectra, or light curves.

### 3.3 Quality Metrics

**Containment Angle (CONT_ANG):**
- Definition: Angular separation between reconstructed and true photon direction
- Calculation: Great circle distance on celestial sphere
- Unit: Degrees
- Interpretation: Smaller values indicate better angular reconstruction

**Reconstruction Version (RECON_VERSION):**
- Tracks algorithm version used for event reconstruction
- Allows identification of events processed with different algorithm versions
- Integer identifier linked to specific reconstruction software release

---

## 4. Validation and Quality Assurance

### 4.1 Automated Validation

**Pre-Release Checks:**
1. HDF5 file integrity verification
2. Schema validation for all 22 required fields
3. Data type consistency checks
4. Range validation for physical quantities:
   - Energy > 0
   - -90 ≤ DEC ≤ 90
   - 0 ≤ RA < 360
   - Angles within valid ranges

### 4.2 Statistical Validation

**Distribution Checks:**
- Energy spectrum: Power-law or broken power-law shape expected
- Angular distributions: Consistency with point source model
- Time distributions: Consistency with GRB temporal profiles
- Coordinate distributions: Sky coverage as expected from simulation design

**Parameter Correlations:**
- ENERGY vs MCENERGY: Strong correlation expected
- CONT_ANG distribution: Validates angular resolution
- WEIGHT distribution: Reasonable range without outliers

### 4.3 Cross-Checks

**Internal Consistency:**
- GTI and probability flags: All retained events have GTI=1, probability=1
- GRB identifier: Matches filename for all events
- TIME ordering: Events properly time-ordered within each file
- Coordinate consistency: RA/DEC and L/B transformations validated

**Empty File Verification:**
- 9 files confirmed to have no events table
- Empty files result from no events passing quality cuts
- File IDs: 10, 511, 519, 2502, 2503, 2505, 2509, 2522, 9517
- Retained as placeholders for completeness

---

## 5. Known Issues and Limitations

### 5.1 Simulation Limitations

**Scope:**
- Simulated data only; does not include real instrumental backgrounds
- Systematic uncertainties may be simplified compared to real detectors
- Detector response based on nominal performance models

**Physical Limitations:**
- No atmospheric absorption for ground-based detectors (if applicable)
- Simplified background modeling
- Idealized trigger logic

### 5.2 Statistical Limitations

**Event Statistics:**
- Large variation in events per file (0 to 49,847)
- 9 simulations with zero surviving events
- Some files have very low statistics (1-2 events)

**Usage Implications:**
- Low-event files not suitable for per-source spectral analysis
- Combined analysis of multiple files recommended
- Bootstrap or ensemble methods needed for uncertainty estimation

### 5.3 Reconstruction Limitations

**Angular Resolution:**
- Containment angle (CONT_ANG) varies with energy and position
- Systematic biases may exist at high off-axis angles
- Resolution degrades for low-energy events

**Energy Reconstruction:**
- Energy-dependent systematic uncertainties
- Possible bias at spectrum edges
- Resolution limits precision of spectral measurements

---

## 6. Dependencies

### 6.1 Data Processing

**Core Libraries:**
- Python 3.7+
- NumPy (array operations)
- pandas (data manipulation)
- PyTables / h5py (HDF5 I/O)
- Astropy (coordinate transformations)

**File Format:**
- HDF5 library version 1.8+
- PyTables format version 2.1

### 6.2 Reproducibility

**Simulation Seeds:**
- Random number generator seeds not preserved
- Results are statistically equivalent but not bit-reproducible

**Processing Pipeline:**
- Deterministic processing from raw simulation output
- Filtering criteria consistently applied across all files
- No manual curation or selection bias

---

## 7. Data Lineage

### 7.1 Processing History

```
Monte Carlo Generation
         ↓
Raw Simulation Output
         ↓
Event Reconstruction
         ↓
Quality Filtering
         ↓
File Organization (HDF5)
         ↓
Validation and Verification
         ↓
Dataset A1 (this release)
```

### 7.2 Traceability

**Event Tracking:**
- EVENT_ID: Unique within each simulation run
- RUN_ID: Links events to specific simulation run
- MC_SRC_ID: Connects to source input parameters
- GRB: Source name for aggregation across dataset

**Provenance Metadata:**
- File creation dates: 2025-12-04
- Processing date: 2025-12-04
- Dataset release date: 2025-12-05
- Version: 1.0.0

---

## 8. Updates and Versioning

### 8.1 Version Policy

This dataset follows semantic versioning (MAJOR.MINOR.PATCH):
- MAJOR: Incompatible structural changes
- MINOR: New data or backward-compatible additions
- PATCH: Bug fixes or documentation updates

### 8.2 Change Log

**Version 1.0.0 (2025-12-05):**
- Initial release
- 100 simulated GRB event files
- 171,558 total filtered events
- Complete documentation and validation

### 8.3 Future Plans

Potential future releases may include:
- Additional simulated sources
- Extended energy range
- Alternative reconstruction algorithms
- Background event datasets
- Real observational data for comparison

---

## 9. Contact and Support

**Technical Questions:**
Koexai S.r.l.  
Email: info@koexai.com  
Website: https://www.koexai.com
LinkedIn: https://www.linkedin.com/company/koexai/

**Data Requests:**
For access to raw simulation output, alternative filtering criteria, or custom simulations, please contact the data provider.

**Bug Reports:**
If you identify data quality issues or inconsistencies, please report them to info@koexai.com with:
- File name(s) affected
- Description of the issue
- Steps to reproduce (if applicable)

---

## 10. References

**Data Format:**
- HDF5 Group: https://www.hdfgroup.org/
- PyTables: https://www.pytables.org/

**Coordinate Systems:**
- IAU SOFA Library: http://www.iausofa.org/
- Astropy Coordinates: https://docs.astropy.org/en/stable/coordinates/

**Gamma-Ray Astronomy:**
- Gamma-ray detection principles and analysis methods documented in relevant astrophysics literature

---

**Document Version:** 1.0  
**Last Updated:** 2025-11-30  
**Authors:** Koexai S.r.l. Data Team
