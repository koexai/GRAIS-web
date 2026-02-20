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

**Source Parameters:**
- Number of simulated GRBs: 10000
- Source naming: Sequential numbering (GRB_Simulated_1 through GRB_Simulated_10000)
- Source positions: Distributed across the celestial sphere
- Energy range: Approximately 30 MeV to 300 GeV

**Observational Parameters:**
- Observation mode: Pointed observations toward GRB positions
- Exposure time: Variable per simulation
- Detector configuration: Standard gamma-ray detector geometry
- Background: Cosmic ray background modeled and rejected

### 1.3 Simulation Software

Simulations were performed using the Fermi Science Tools and the gtobssim utility.

Instrument response functions (IRFs): P8R3_SOURCE_V3

Spacecraft pointing history was taken from weekly spacecraft files:
lat_spacecraft_weekly_w***_p310_v001.fits

The simulation accounts for:
- Energy-dependent effective area
- Point Spread Function (PSF)
- Energy dispersion
- Livetime variations

---

## 2. Data Processing Pipeline

### 2.1 Raw Simulation Output

**Initial Data Products:**
- Individual photon events with true (Monte Carlo) parameters
- Timing information in Mission Elapsed Time (MET) format

### 2.2 Event Reconstruction

**Reconstruction Algorithms:**

1. **Direction Reconstruction:**
   - Input: Detector hit patterns and signal amplitudes
   - Output: Reconstructed arrival direction (RA, DEC, L, B, THETA, PHI)
   - Method: Pattern recognition and maximum likelihood fitting
   - Quality metric: Containment angle and weight

2. **Energy Reconstruction:**
   - Input: Total energy deposition in detector
   - Output: Reconstructed photon energy (ENERGY)
   - Method: Calibrated energy estimator accounting for detector response
   - Validation: Comparison with Monte Carlo truth energy (MCENERGY)

3. **Time Reconstruction:**
   - Input: Detector trigger time stamps
   - Output: Event time (TIME) and effective livetime (LIVETIME)

### 2.3 Data Organization

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

## 3. Validation and Quality Assurance

### 3.1 Automated Validation

**Pre-Release Checks:**
1. HDF5 file integrity verification
2. Schema validation for all 22 required fields
3. Data type consistency checks
4. Range validation for physical quantities:
   - Energy > 0
   - -90 ≤ DEC ≤ 90
   - 0 ≤ RA < 360
   - Angles within valid ranges

### 3.2 Statistical Validation

**Distribution Checks:**
- Energy spectrum: Power-law or broken power-law shape expected
- Angular distributions: Consistency with point source model
- Time distributions: Consistency with GRB temporal profiles
- Coordinate distributions: Sky coverage as expected from simulation design

## 4. Known Issues and Limitations

### 4.1 Simulation Limitations

**Scope:**
- Systematic uncertainties may be simplified compared to real detectors
- Detector response based on nominal performance models

**Physical Limitations:**
- Simplified background modeling
- Idealized trigger logic

### 4.2 Statistical Limitations

- Large variation in events per file
- 9 simulations with zero surviving events
- Some files have very low statistics (1-2 events)

### 4.3 Reconstruction Limitations

**Angular Resolution:**
- Containment angle (CONT_ANG) varies with energy and position
- Systematic biases may exist at high off-axis angles
- Resolution degrades for low-energy events

**Energy Reconstruction:**
- Energy-dependent systematic uncertainties
- Possible bias at spectrum edges
- Resolution limits precision of spectral measurements

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

### 6.2 Traceability

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
