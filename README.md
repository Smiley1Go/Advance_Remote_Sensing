# Multi-Temporal SAR-Based Crop-Type Mapping: Sehore District, M.P

## Overview
This repository hosts the Google Earth Engine (GEE) code and documentation for mapping crop types (**Wheat** vs. **Soybean**) across the **Sehore district** using multi-temporal **Sentinel-1 Synthetic Aperture Radar (SAR)** imagery. By leveraging radar backscatter time series ($\text{VV}$ and $\text{VH}$ polarizations) across 12-day composite windows, a supervised Random Forest classifier was trained to differentiate seasonal phenological trajectories.

## Live Access Links
* **Google Earth Engine Code Script:** [View Source Code in GEE](https://code.earthengine.google.com/391ed4568e784d77da65058ce3fa7869)
*  **Webpage:** [Crop Detection](https://smiley1go.github.io/Advance_Remote_Sensing/)

## Methodology & Workflow
1. **Data Acquisition:** Filtered Sentinel-1 GRD IW mode scenes over the Sehore district boundary (June 2025 – April 2026).
2. **Preprocessing:** Applied speckle filtering, temporal gap-filling across 12-day composite windows, and extracted polarization difference bands alongside seasonal statistical features (totaling 82 predictor bands).
3. **Supervised Classification:** Sampled regional training polygons for Wheat ($n=115$) and Soybean ($n=113$) with a 5m buffer to match the 10-meter pixel grid. Trained a 100-tree Random Forest model.
4. **Validation:** Evaluated model performance using independent validation sets, confusion matrices, producer/user accuracies, and F1-scores.

## Class Legend
* **Wheat:** Orange (`#E69F00`)
* **Soybean:** Blue (`#0072B2`)

## Author
* **Name:** Joshby Joshy  
* **Programme:** M.Sc. Geoinformatics   
* **Institution:** Bharati Vidyapeeth (Deemed to be University), Pune (IEER)
