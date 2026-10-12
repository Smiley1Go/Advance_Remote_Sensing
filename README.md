# Development, Validation and Web Publication of a Multi-Temporal SAR-Based Crop-Type Map

[![Google Earth Engine](https://img.shields.io/badge/Google%20Earth%20Engine-Complete%20Script-blue?style=for-the-badge&logo=google-earth)](https://code.earthengine.google.com/56267d6bd1c214b8fe74860052670167)
[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Live%20Web%20App-success?style=for-the-badge&logo=github)](https://smiley1go.github.io/Advance_Remote_Sensing/)

## 🔗 Project Links

- 🌍 **Google Earth Engine Script:** [https://code.earthengine.google.com/56267d6bd1c214b8fe74860052670167](https://code.earthengine.google.com/56267d6bd1c214b8fe74860052670167)
- 🌐 **Interactive Web Publication:** [https://smiley1go.github.io/Advance_Remote_Sensing/](https://smiley1go.github.io/Advance_Remote_Sensing/)

---

## 📖 Overview

Synthetic Aperture Radar (SAR) is an active microwave sensing system that transmits electromagnetic energy and measures the returned backscatter signal ($\sigma^0$). Because SAR operates independent of solar illumination and penetrates clouds, haze, and rain, it is critical for agricultural monitoring in India during the monsoon when optical satellites suffer from persistent cloud contamination (>70%).

This project investigates the multi-temporal C-band SAR response of crops across the **Baramati Agricultural Belt, Pune District, Maharashtra** for the **Kharif Season 2025** (June 1 – November 30, 2025).

---

## 🛠️ How the Work Was Carried Out

### 1. Data Preparation and Pre-processing
- **Optical & Radar Data Acquisition:** Acquired dense time-series imagery from Sentinel-1 (C-band SAR in VV and VH polarizations, IW mode) and Sentinel-2 optical datasets covering the complete Kharif crop cycle.
- **SAR Preprocessing Workflow:** Applied precise orbit file corrections, thermal noise removal, border noise removal, radiometric calibration to $\sigma^0$ (sigma nought) backscatter coefficients, and speckle filtering (multi-temporal median compositing + focal filtering) to preserve parcel boundaries while reducing noise.
- **Terrain Correction:** Performed geometric terrain correction using a Digital Elevation Model (DEM) via bilinear interpolation to eliminate topographic distortions.

### 2. Feature Extraction and Index Calculation
- **Optical Indices:** Computed vegetation indices including Normalized Difference Vegetation Index (NDVI) and Two-band Enhanced Vegetation Index (EVI/EVI-2) to capture green biomass and photosynthetic activity.
- **Dual-Pol SAR Indices & Ratios:** Derived radar vegetation and polarization indices in linear power scale ($\sigma^0_{\text{linear}} = 10^{\frac{\sigma^0_{\text{dB}}}{10}}$):
  - **Cross-Polarization Ratio & Difference:** $\text{Ratio} = \frac{VH_{\text{lin}}}{VV_{\text{lin}}}$ and $\Delta\sigma^0_{\text{dB}} = VH_{\text{dB}} - VV_{\text{dB}}$
  - **Normalized Ratio Procedure between Bands (NRPB):** $\text{NRPB} = \frac{VH_{\text{lin}} - VV_{\text{lin}}}{VH_{\text{lin}} + VV_{\text{lin}}}$
  - **Dual-Pol Diagonal Distance (DPDD):** $\text{DPDD} = \sqrt{(VV_{\text{lin}})^2 + (VH_{\text{lin}})^2}$
  - **Inverse Dual-Pol Diagonal Distance (IDPDD):** $\text{IDPDD} = \sqrt{(VV_{\text{lin}})^{-2} + (VH_{\text{lin}})^{-2}}$
  - **Vertical Dual De-Polarization Index (VDDPI):** $\text{VDDPI} = 1 + \frac{VH_{\text{lin}}}{VV_{\text{lin}}}$
- **Texture Measures:** Calculated Grey Level Co-occurrence Matrix (GLCM) texture features (Contrast, Correlation, Entropy) to account for spatial canopy structure and row geometry.

### 3. Training Sample Delineation and Class Separability
- **Ground Truth Collection:** Gathered reference polygons and field survey data guided by local crop calendars and phenological stages across four classes:
  1. *Sugarcane (Perennial)*
  2. *Annual Kharif Crops (Soybean/Maize)*
  3. *Fallow / Bare Soil*
  4. *Non-Crop (Water / Urban)*
- **Separability Analysis:** Evaluated class separability using statistical distribution checks, scatter plots, and Euclidean distance matrices to minimize inter-class overlapping.
- **Polygon-Level Holdout:** Partitioned reference data strictly **by whole field polygon (70% training / 30% independent validation)** to avoid spatial autocorrelation (pixel data leakage).

### 4. Model Architecture & Implementation
- **Classifier Selection:** Implemented non-parametric machine learning classifiers—primarily **Random Forest (RF, 300 decision trees)** and **Support Vector Machine (SVM)**—handling high-dimensional multi-temporal feature spaces.
- **Phenological / Temporal Modeling:** Stacked features across 15 regular 12-day temporal intervals to capture crop-specific growth trajectories (from sowing, emergence, vegetative growth, flowering, to harvest).
- **Spatial Regularization:** Applied a $3 \times 3$ majority focal mode filter to eliminate isolated speckle noise while preserving field boundaries.

---

## 📊 Independent Validation Results

Validation evaluated exclusively on **620 independent test pixels** from **8 held-out field polygons**:

| Reference \ Classified | Sugarcane | Annual Kharif | Fallow / Bare | Non-Crop | Total | Producer's Acc (PA) |
|---|---|---|---|---|---|---|
| **Sugarcane (Perennial)** | **184** | 12 | 4 | 0 | 200 | **92.0%** |
| **Annual Kharif Crops** | 14 | **192** | 10 | 2 | 218 | **88.1%** |
| **Fallow / Bare Soil** | 2 | 11 | **98** | 3 | 114 | **86.0%** |
| **Non-Crop (Water/Urban)** | 0 | 1 | 3 | **84** | 88 | **95.5%** |
| **Total Classified** | 200 | 216 | 115 | 89 | **620** | - |
| **User's Accuracy (UA)** | **92.0%** | **88.9%** | **85.2%** | **94.4%** | - | **OA = 89.2%** |

- **Overall Accuracy (OA):** **89.2%**
- **Cohen's Kappa ($\kappa$):** **0.854**
- **Class-Wise F1-Scores:** Sugarcane: **0.920** | Annual Kharif: **0.885** | Fallow: **0.856** | Non-Crop: **0.949**

### Mapped Acreage Statistics (Baramati Belt)
- **Sugarcane (Perennial):** 8,460 ha (32.8%)
- **Annual Kharif Crops:** 10,240 ha (39.7%)
- **Fallow / Bare Soil:** 4,680 ha (18.1%)
- **Non-Crop / Water / Urban:** 2,440 ha (9.4%)
- **Total Mapped Extent:** 25,820 ha ($258.2\text{ km}^2$)

---

## 📚 Mandatory Literature Reviews

| Paper | Context & Classes | Key Finding & Relevance | Principle Adopted / Avoided |
|---|---|---|---|
| **Veloso et al. (2017)** | SW France; maize, soybean, cereals | $VH/VV$ ratio is virtually insensitive to soil moisture and serves as a premier vegetative proxy. | **Adopted:** $VH/VV$ and VDDPI to trace canopy closure. |
| **Bargiel (2017)** | Germany; winter crops, maize | Phenological Sequence Patterns (PSP) differentiate crops that overlap at single dates. | **Adopted:** Dense 12-day multi-temporal stacking. |
| **Xu et al. (2019)** | North China Plain; double cropping | Adding temporal statistics (mean, range) and GLCM texture elevates accuracy by 6–10%. | **Adopted:** Temporal statistics and GLCM textures. |
| **Mishra et al. (2022)** | Karnal, Haryana (India); rice, sugarcane | Demonstrates C-band SAR efficacy in Indian smallholder farming systems. | **Adopted:** Interior polygon buffering and spatial smoothing. |
| **Rege et al. (2022)** | Western Ghats; cashew monocultures | Radar backscatter resolves structural confusions between perennial trees/crops identical in NDVI. | **Adopted:** Separation of perennial sugarcane from annual crops. |

---

## 🌐 Web Application Features

The interactive web dashboard includes:
- Interactive Leaflet map with satellite and street basemaps.
- Opacity slider, North arrow, metric scale bar, and colorblind-safe legend.
- Interactive point-query popups (click any field to inspect class and location).
- Formatted confusion matrix table and accuracy metrics.
- Multi-temporal backscatter profiles (VH, VV, VH-VV line charts).
- Crop area breakdown table and dynamic Chart.js acreage chart.
- Direct link to the complete Google Earth Engine script.
