# HS-FRAG: Hybrid Segmentation for Fragmented Croplands

HS-FRAG is a QGIS plugin for delineating fragmented croplands. It combines edge-based and region-based segmentation in a single workflow: Sobel edge detection followed by Watershed region segmentation. The plugin automates these steps, so a segmentation can be produced from satellite imagery, a study-area boundary, and a threshold value, and repeated across multiple time steps with minimal manual effort.

## Requirements

- QGIS (with the OSGeo4W installation on Windows)
- Python packages: `scikit-learn` (required for the machine learning-based segmentation algorithms)

## Installation

### 1. Install scikit-learn

Open the **OSGeo4W Shell** and run:

```bash
python -m pip install scikit-learn --user
```

### 2. Install the plugin

1. Download this repository as a ZIP file and extract it, or use the provided `HS-FRAG.zip`.
2. Open QGIS and go to **Plugins > Manage and Install Plugins**.
3. Select **Install from ZIP** in the left panel.
4. Browse to `HS-FRAG.zip` and click **Install Plugin**.
5. The HS-FRAG icon will appear on the top toolbar once installation is complete.

## Usage

Click the HS-FRAG icon to open the plugin dialog. Provide the following inputs:

| Field | Description |
|---|---|
| Overall Region | Vector file (shapefile) defining the boundary of the study area. |
| Non Agri Regions | Vector file (shapefile) of non-agricultural areas, such as buildings, barren land, and roads, which are removed from the study area to prevent incorrect segmentation. |
| Input Files | Satellite imagery in `.tif` format. Bands should be provided one at a time. |
| Threshold | Watershed merging threshold. Basins are merged when their height difference (from seed to saddle) is below this value. Lower values produce more segments (risk of over-segmentation), while higher values produce fewer, larger segments (risk of under-segmentation). |
| Output Folder | Directory where segmentation and evaluation outputs will be saved. |
| Reference | Reference vector or raster data used for the evaluation measures. |

Test data is provided in the `SourceFiles` folder.

## Outputs

All outputs are written to the selected output folder:

- `Cropped image.tif`: study-area imagery clipped to the overall region
- `Difference.shp`: study area with non-agricultural regions removed
- `Masked.tif`: masked imagery
- `output.tif`: Sobel edge image
- `Average.tif`: averaged image used for segmentation
- `Segment.sdat`: watershed segmentation raster
- `borders.tif`: segment borders
- `Segment_vector.shp`: final vectorized segments

## Evaluation Measures

Segmentation results are compared against the reference data using three measures:

- **Generalization:** the ratio of the number of segments in the segmented output to the number of reference segments. Results are saved as a `.txt` file.
- **Geometric:** the ratio of area and perimeter of the segments to those of the reference segments. Results include histograms, plots with R² values, and an Excel file in the `evaluation` folder.
- **Spatial Correctness:** the percentage of segmented borders or pixels lying on and outside the reference borders. Results are saved as an Excel file.


## Contact

Sravani Duvvuri - sravanidvr@gmail.com
