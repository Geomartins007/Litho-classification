
# 🗺️ Lithological Classification Using Self-Organizing Maps (SOM)
### Integrating Sentinel-2 Multispectral and Airborne Geophysical Data for Arctic Terrain Mapping

![Python](https://img.shields.io/badge/Python-3.12-blue.svg)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)
![Status](https://img.shields.io/badge/Status-Complete-success.svg)

---

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Study Area](#-study-area)
- [Methodology](#-methodology)
- [Results](#-results)
- [Key Findings](#-key-findings)
- [Limitations](#-limitations)
- [References](#-references)
- [Acknowledgments](#-acknowledgments)

---

## 🎯 Project Overview

This project demonstrates the application of **Self-Organizing Maps (SOM)** combined with **K-means clustering** for lithological classification in the **Malmbjerg area, East Greenland**. The workflow integrates:

- **Sentinel-2 multispectral satellite data** (13 bands)
- **Airborne geophysical resistivity measurements** (140 kHz and 40 kHz)
- **Machine learning techniques** (SOM + K-means)
- **Comprehensive geologic interpretation**

The project successfully identifies **9 distinct geophysical-spectral domains** corresponding to intrusive units, alteration zones, host rocks, and transitional lithologies.

### 🔬 Research Context

The Malmbjerg area hosts a significant **molybdenum-tungsten (Mo–W) deposit** within an alkaline intrusive complex. This project aims to provide an objective, data-driven approach to lithologic discrimination in Arctic terrains where:
- ❄️ Snow and ice coverage limits traditional mapping
- 🏔️ Access and exposure are restricted
- 🛰️ Remote sensing offers the primary data source

---

## 🌍 Study Area

**Location:** Malmbjerg Mo–W Deposit, Central East Greenland  
**Coordinates:** ~71.96°N, -24.27°E  
**Geology:** Alkaline intrusive complex emplaced into Paleozoic sedimentary and metamorphic host rocks  
**Mineralization:** Hydrothermal Mo-bearing zones with distinct alteration signatures  

### Geologic Setting

The Malmbjerg intrusive complex exhibits:
- Multiple **intrusive phases** with varying compositions
- **Hydrothermal alteration** associated with Mo mineralisation
- Structural control on mineralization
- **Sharp lithologic contacts** and transitional boundaries

---

## 📊 Datasets

### 1. **Sentinel-2 Multispectral Data** (13 Bands)
- **Bands:** B1-B9, B8a, B11, B12 (excluding B10)
- **Resolution:** 10m, 20m, and 60m (resampled)
- **Date:** August 1, 2019
- **Preprocessing:** NDSI masking to remove snow/ice coverage

| Band | Name | Wavelength (nm) | Resolution (m) | Purpose |
|------|------|----------------|----------------|---------|
| B1 | Coastal Aerosol | 443 | 60 | Atmospheric correction |
| B2 | Blue | 490 | 10 | Water body identification |
| B3 | Green | 560 | 10 | Vegetation monitoring |
| B4 | Red | 665 | 10 | Vegetation discrimination |
| B5 | Red Edge 1 | 705 | 20 | Vegetation chlorophyll |
| B6 | Red Edge 2 | 740 | 20 | Vegetation stress |
| B7 | Red Edge 3 | 783 | 20 | Vegetation LAI |
| B8 | NIR | 842 | 10 | Biomass estimation |
| B8a | Narrow NIR | 865 | 20 | Water vapor absorption |
| B9 | Water Vapor | 945 | 60 | Atmospheric correction |
| B11 | SWIR 1 | 1610 | 20 | **Alteration minerals** |
| B12 | SWIR 2 | 2190 | 20 | **Lithologic discrimination** |

### 2. **Airborne Geophysical Data**
- **Type:** Resistivity measurements
- **Frequencies:** 140 kHz and 40 kHz
- **Preprocessing:** Log-transformation for normalization
- **Integration:** Co-located with Sentinel-2 pixels

### 3. **Ancillary Data**
- **Shapefile:** Study area boundary
- **Published Geologic Map:** Bedini (2012) for validation

---

## 🔬 Methodology

The workflow follows a systematic approach to unsupervised lithologic classification:

### 1. **Data Preprocessing**
```
├── Load Sentinel-2 multispectral data (13 bands)
├── Compute NDSI (Normalized Difference Snow Index)
├── Apply NDSI threshold masking (removes snow/ice)
├── Load airborne resistivity data (2 frequencies)
├── Perform log-transformation on resistivity
└── Merge datasets based on geographic coordinates
```

### 2. **Snow/Ice Masking**
- **NDSI Formula:** `(Green - SWIR1) / (Green + SWIR1)`
- **Threshold:** NDSI > 0.4 (removed as snow/ice)
- **Rationale:** Snow obscures lithologic signals and dominates clustering

### 3. **Data Integration & Normalization**
- **Feature Vector:** 15 dimensions (13 spectral + 2 resistivity)
- **Standardization:** Standard Scaler (zero mean, unit variance)
- **Data Points:** 273,306 pixels

### 4. **Self-Organizing Map (SOM) Training**
- **Architecture:** 20×20 hexagonal grid (400 neurons)
- **Topology:** Hexagonal neighborhood
- **Distance Function:** Gaussian
- **Learning Rate:** Decay from initial to final
- **Iterations:** Sufficient for convergence
- **Random Seed:** 42 (reproducibility)

### 5. **K-means Clustering**
- **Input:** 400 SOM neuron weights
- **Method:** Elbow method + Davies-Bouldin Index
- **Optimal Clusters:** k = 9
- **Evaluation:** Silhouette score, DB index, cluster compactness

### 6. **Visualization & Interpretation**
- RGB composites (before/after masking)
- NDSI histogram
- SOM U-matrix (neuron distances)
- Weight vectors visualization
- Training progress (quantization error)
- Davies-Bouldin index plot
- Kohonen map (cluster topology)
- **Geographic cluster map** with geologic interpretation

---

## 📈 Results

### Clustering Performance

| Metric | Value | Interpretation |
|--------|-------|----------------|
| **Optimal Clusters (k)** | 9 | Davies-Bouldin Index minimum |
| **Davies-Bouldin Index** | ~0.85 | Good cluster separation |
| **Quantization Error** | Converged | SOM topology preserved |
| **Geographic Coherence** | High | Spatially continuous domains |

### Identified Lithologic Domains

The 9 SOM-derived clusters represent distinct geophysical-spectral domains:

#### **Cluster 1: Core Intrusive Units**
- **Characteristics:** High resistivity + subdued spectral variability
- **Interpretation:** Massive, homogeneous alkaline intrusives (least altered)
- **Spatial Pattern:** Central intrusive complex

#### **Cluster 3: Altered & Mineralised Zones**
- **Characteristics:** Reduced resistivity + elevated SWIR response
- **Interpretation:** Hydrothermal alteration halos, Mo-mineralised zones
- **Spatial Pattern:** Structurally controlled, associated with known deposits

#### **Cluster 8: Host Rocks**
- **Characteristics:** Variable resistivity + distinct spectral signature
- **Interpretation:** Sedimentary and metasedimentary country rocks
- **Spatial Pattern:** Surrounding the intrusive complex

#### **Transitional Clusters (2, 4, 5, 6, 7, 9)**
- **Characteristics:** Gradual changes in properties
- **Interpretation:** Lithologic contacts, alteration gradients, mixed units
- **Spatial Pattern:** Along boundaries between major domains

### Validation Against Published Map

Strong spatial agreement with Bedini (2012) geologic map demonstrates:
- ✅ Successful recovery of major lithologic boundaries
- ✅ Identification of alteration zones
- ✅ Discrimination between intrusive phases and host rocks
- ✅ Geologically realistic cluster topology

---

## 🔑 Key Findings

### Scientific Contributions

1. **Methodological Innovation:**
   - Successfully integrated multispectral and geophysical data for lithologic classification
   - Demonstrated SOM effectiveness in Arctic remote sensing applications
   - Validated unsupervised learning against ground-truth geology

2. **Geologic Insights:**
   - Identified 9 lithologic domains consistent with known geology
   - Mapped alteration zones associated with Mo mineralisation
   - Revealed transitional boundaries reflecting natural geologic gradients

3. **Practical Applications:**
   - **Early-stage exploration:** Rapid lithologic reconnaissance in remote areas
   - **Alteration mapping:** Targeting hydrothermal systems
   - **Arctic terrain mapping:** Workflow applicable to other glaciated regions

### Advantages of This Approach

| Advantage | Description |
|-----------|-------------|
| **Objective** | Data-driven, reduces interpreter bias |
| **Multi-source** | Integrates complementary datasets (spectral + geophysical) |
| **Unsupervised** | No training data required |
| **Topology-preserving** | SOM maintains natural feature space relationships |
| **Scalable** | Applicable to large datasets |
| **Reproducible** | Fully documented workflow |

---

## ⚠️ Limitations

### Technical Limitations

1. **Sentinel-2 Data:**
   - Surface-only information (weathering affects spectral response)
   - Sensitive to illumination conditions and atmospheric effects
   - 10-20m resolution may miss small-scale features

2. **Airborne Resistivity:**
   - Integrates subsurface properties (limited depth investigation)
   - Non-unique lithologic signatures (multiple rock types can have similar resistivity)
   - Flight line coverage may have gaps

3. **SOM Clusters:**
   - Represent **lithologic domains**, not formal map units
   - Require geologic interpretation and ground-truthing
   - Cluster boundaries are data-driven, not strictly geologic contacts

### Environmental Challenges

- ❄️ **Snow/ice coverage:** NDSI masking reduces data coverage
- 🏔️ **Topographic shadowing:** Affects spectral reflectance
- 🌤️ **Cloud cover:** Limits usable Sentinel-2 acquisitions

---

## 📚 References

### Primary Reference
**Bedini, E. (2012).** *Mapping alteration minerals at Malmbjerg molybdenum deposit, central East Greenland, by Kohonen self-organising maps and matched filter analysis of HyMap data.* International Journal of Remote Sensing, 33:4, 939-961.  
**DOI:** [10.1080/01431161.2010.542202](https://doi.org/10.1080/01431161.2010.542202)

### Methodological References

- **Kohonen, T. (2001).** *Self-Organizing Maps*. Springer Series in Information Sciences, Vol. 30, Springer, Berlin.
- **Davies, D. L., & Bouldin, D. W. (1979).** *A cluster separation measure.* IEEE Transactions on Pattern Analysis and Machine Intelligence, 1(2), 224-227.
- **Drusch, M., et al. (2012).** *Sentinel-2: ESA's Optical High-Resolution Mission for GMES Operational Services.* Remote Sensing of Environment, 120, 25-36.

### Related Work

- **Bedini, E., et al. (2009).** *Use of HyMap imaging spectrometer data to map mineralogy in the Rodalquilar caldera, southeast Spain.* International Journal of Remote Sensing, 30(2), 327-348.
- **Cracknell, M. J., & Reading, A. M. (2014).** *Geological mapping using remote sensing data: A comparison of five machine learning algorithms.* Computers & Geosciences, 66, 1-12.
- **Harris, J.R., et al. (2011).** *Remote predictive mapping.* Canadian Journal of Remote Sensing, 37(1), 45-60.

---

## 🙏 Acknowledgments

### Original Work
This Python implementation is based on original R code prepared by:
- **Prof. Thorkild Rasmussen**
- **Dr. Sara Kasmaeeyazdi**

I hereby thank and recognize their foundational work. This code was rewritten for learning, research purposes, and to demonstrate Python-based geospatial workflows.

### Data Sources
- **Sentinel-2 Data:** European Space Agency (ESA) Copernicus Programme
- **Airborne Geophysical Data:** (Source to be specified)
- **Geological Survey of Greenland (GEUS):** Geologic mapping and reference data

### Software & Libraries
- **MiniSom:** Pythonic implementation of SOM by Giuseppe Vettigli
- **Scikit-learn:** Machine learning library
- **Geopandas:** Geospatial data processing
- **Matplotlib/Seaborn:** Visualization

---

<div align="center">
  <sub>Built with ❤️ for the remote sensing and geoscience community</sub>
</div>
