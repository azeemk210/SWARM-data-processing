# **SWARM Data Processing Workflow**

This document outlines the step-by-step process to extract, compute, and process SWARM magnetic field data using various scripts and repositories.

## **Step 1: Identify AOI and Nearby Tracks**
- **Script:** `plot_data_from_cdf_raw_data.ipynb`
- **Objective:** Plot the Area of Interest (AOI) and find the closest dataset tracks for extraction.
- **Output:** Identified dataset track to extract in `.txt` format.

---

## **Step 2: Extract Data from .CDF Files and Compute IGRF Model**
- **Script:** `extract_data_from_cdf_to_txt.ipynb`
- **Objective:** Extract data from `.cdf` files and compute **B_N, B_E, B_C** using the **IGRF Model**.
- **Output:** A `.txt` file with the following header:
  ```
  Timestamp,Latitude,Longitude,Altitude,B_N_SWARM,B_E_SWARM,B_C_SWARM,B_N_IGRF,B_E_IGRF,B_C_IGRF (nT)
  ```
- **Modification:** Change the sign of **B_C_IGRF** to match the original dataset.

---

## **Step 3: Extract Coordinates and Prepare for Geomagnetic Calculation**
- **Script:** `extract_coordinates_from_txt_file_to_use_in_geomag.ipynb`
- **Objective:** Extract coordinates from the `.txt` file and prepare data for geomagnetic calculations.
- **Task:** Extract every **100th point** from **HR files** for interpolation (since fetching the entire dataset via geomag is time-consuming). No need for LR dataset.

---

## **Step 4: Compute Geomagnetic Coordinates**
- **Repository:** `geo2mag`
- **Command:** Run the following command to compute geomagnetic coordinates:
  ```sh
  python GeomagConvert.py 2025 -f d:\Data\SWARM_data\geomag\geo2mag\santorini_island\coordinates_SW_OPER_MAGC_LR_1B_20250128T000000_20250128T235959_0606_MDR_MAG_LR.txt geomag
  ```
- **Output:** A `.txt` file with the following header:
  ```
  glat glon mlat mlon year
  ```

---

## **Step 5: Interpolate Missing Data for HR Files**
- **Script:** `interpolate_data.ipynb`
- **Objective:** Interpolate missing geomagnetic data points for HR files.

---

## **Step 6: Insert mlat and mlon into the Existing Dataset**
- **Script:** `insert_mlat_mlon_in_existing_txt_file.ipynb`
- **Objective:** Insert **mlat** and **mlon** into the `.txt` file extracted from the original `.cdf` file.
- **Columns Placement:** Insert as **Column 4 & 5** in the dataset.

---

## **Final Output**
At the end of this workflow, you will have:
- A **fully processed `.txt` file** containing the extracted, computed, and interpolated magnetic field data with geomagnetic coordinates.
- Data ready for further analysis or visualization.

---

## **Additional Notes**
- Ensure that all required Python dependencies and repositories (`geo2mag`, IGRF models, etc.) are installed and configured properly.
- Always verify the consistency of `.txt` files before proceeding to the next step.
- Use **HR data interpolation** to reduce processing time while maintaining accuracy.

🚀 **Follow these steps carefully to ensure accurate data processing!**

