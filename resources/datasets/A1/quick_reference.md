# Quick Reference Guide
## Simulated Gamma-Ray Burst Event Dataset A1

**Version:** 1.0.0 | **Release Date:** 2025-11-30

---

## At a Glance

| Property | Value |
|----------|-------|
| **Dataset Type** | Simulated gamma-ray burst events |
| **Format** | HDF5 files |
| **Number of Files** | 100 |
| **Total Events** | 171,558 |
| **Variables per Event** | 22 |
| **Energy Range** | ~10 GeV to ~10,000 GeV |
| **License** | CC-BY-4.0 |
| **Organization** | Koexai S.r.l. |

---

## Quick Start

### Installation
```bash
pip install pandas h5py tables
```

### Load Data
```python
import pandas as pd

# Read a single file
df = pd.read_hdf('GRB_Simulated_*_filtered.hdf5', 'events')

# Display info
print(df.info())
print(df.head())
```

### Access Specific Variables
```python
# Energy
energies = df['ENERGY']          # Reconstructed energy (GeV)
mc_energies = df['MCENERGY']     # True energy (GeV)

# Coordinates
ra = df['RA']                     # Right Ascension (deg)
dec = df['DEC']                   # Declination (deg)

# Time
times = df['TIME']                # Event time (MET seconds)
```

---

## Key Variables

### Essential Observables
- **ENERGY** - Reconstructed photon energy (GeV)
- **RA, DEC** - Celestial coordinates (degrees, J2000)
- **TIME** - Event time (MET seconds)

### Monte Carlo Truth
- **MCENERGY** - True photon energy (GeV)
- **MC_SRC_ID** - Source identifier
- **CONT_ANG** - Reconstruction quality (degrees)

### Quality Flags
- **GTI** - Good Time Interval (1 = good)
- **probability** - Event quality (1 = high quality)
- **WEIGHT** - Statistical weight for analysis

---

## File Structure

```
datasetA1/
├── GRB_Simulated_1_filtered.hdf5    (2 events)
├── GRB_Simulated_2_filtered.hdf5    (1 event)
├── ...
├── GRB_Simulated_9505_filtered.hdf5 (49,847 events)
└── GRB_Simulated_9525_filtered.hdf5 (4,046 events)
```

**Note:** 9 files are empty (no events passed quality filters)

---

## Important Notes

### ✓ Data Quality
- All events have `GTI=1` and `probability=1`
- Events passed automated quality filtering
- Variable event counts per file (0 to 49,847)

### ⚠ Limitations
- **Empty files:** 9 files contain no events
- **Low statistics:** Some files have <10 events
- **Simulated data:** Does not include all real-world systematics

### ✓ Best Practices
- Use WEIGHT field for proper statistical analysis
- Check file event count before per-file analysis
- Combine multiple files for robust statistics
- Compare ENERGY vs MCENERGY for validation

---

## Citation

```
Koexai S.r.l. (2025). Simulated Gamma-Ray Burst Event Dataset A1 
(Version 1.0.0). KOEXAI-GRB-A1-v1.0
```

**BibTeX:**
```bibtex
@dataset{koexai_grb_a1_2025,
  author = {Koexai S.r.l.},
  title = {Simulated Gamma-Ray Burst Event Dataset A1},
  year = {2025},
  version = {1.0.0},
  identifier = {KOEXAI-GRB-A1-v1.0}
}
```

---

## Complete Documentation

📄 **README.md** - Comprehensive overview and user guide  
📊 **dictionary.csv** - Complete variable descriptions  
📝 **provenance.md** - Detailed methods and processing  
⚙️ **metadata.json** - Machine-readable metadata  
✅ **FAIR_compliance_checklist.md** - Quality assurance

---

## Common Tasks

### Load Multiple Files
```python
import glob
import pandas as pd

files = glob.glob('datasetA1/*.hdf5')
dfs = []

for f in files:
    try:
        df = pd.read_hdf(f, 'events')
        dfs.append(df)
    except:
        pass  # Skip empty files

combined = pd.concat(dfs, ignore_index=True)
```

### Filter by Energy
```python
# Events above 100 GeV
high_energy = df[df['ENERGY'] > 100]

# Energy range
energy_range = df[(df['ENERGY'] >= 10) & (df['ENERGY'] <= 1000)]
```

### Weighted Analysis
```python
# Weighted histogram
import numpy as np

bins = np.logspace(1, 4, 30)  # Log bins from 10 to 10000 GeV
hist, edges = np.histogram(df['ENERGY'], bins=bins, weights=df['WEIGHT'])
```

### Check Reconstruction Quality
```python
# Plot containment angle distribution
import matplotlib.pyplot as plt

plt.hist(df['CONT_ANG'], bins=50)
plt.xlabel('Containment Angle (degrees)')
plt.ylabel('Events')
plt.title('Angular Reconstruction Quality')
plt.show()
```

### Sky Map
```python
# Plot event positions
import matplotlib.pyplot as plt

plt.scatter(df['RA'], df['DEC'], s=1, alpha=0.5)
plt.xlabel('RA (degrees)')
plt.ylabel('DEC (degrees)')
plt.title('Event Sky Distribution')
plt.grid(True, alpha=0.3)
plt.show()
```

---

## Data Dictionary Quick Reference

| Variable | Type | Unit | Description |
|----------|------|------|-------------|
| ENERGY | float32 | GeV | Reconstructed energy |
| RA | float32 | deg | Right Ascension |
| DEC | float32 | deg | Declination |
| L | float32 | deg | Galactic longitude |
| B | float32 | deg | Galactic latitude |
| TIME | float64 | s | Event time (MET) |
| MCENERGY | float32 | GeV | True energy |
| CONT_ANG | float64 | deg | Reconstruction quality |
| WEIGHT | float64 | - | Statistical weight |
| GRB | string | - | Source identifier |

See `dictionary.csv` for complete descriptions of all 22 variables.

---

## Support

**Contact:** Koexai S.r.l.  
**Email:** info@koexai.com  
**Web:** https://www.koexai.com

**For Help With:**
- Data access issues
- Analysis questions
- Bug reports
- Collaboration opportunities

---

## License

**Creative Commons Attribution 4.0 International (CC BY 4.0)**

✓ Share and adapt freely  
✓ Commercial use allowed  
✓ Must give appropriate credit

Full license: https://creativecommons.org/licenses/by/4.0/

---

**Last Updated:** 2025-11-30 
**Document Version:** 1.0
