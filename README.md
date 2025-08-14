# P-Factor-Raster-Creation-for-WaTEM-SEDEM
For me, this is a headache raster to create.
# Notes
P-factor Raster Creation & Cleanup (Python in VS Code)
Goal

Generate a P-factor raster aligned to the DTM.rst reference grid, with:

Data type: Float32

Allowed values: {0.02, 0.20, 0.60}

NoData value: 0

Format: Idrisi RST (with .rdc header)

CRS, extent, resolution, rows/columns: exactly matching DTM.rst

Why this was tricky

Original PFac.tif had either scale/offset metadata or was stored as an integer type, causing values to collapse to 0 and 1 when exported.

Alignment steps produced non-finite values (NaN, -inf) in some pixels, which polluted statistics.

Some workflows accidentally used bilinear resampling, which is wrong for categorical/stepped rasters like P-factor — it created unwanted intermediate values.

NoData handling was inconsistent: some rasters contained -9999 but had NoData metadata set to 0.

Final Python Workflow in VS Code
1. Align the P raster to the DTM grid

Before cleaning, we used Python’s gdal.Warp() inside VS Code to:

Force Float32 output type

Use nearest resampling

Set dstNodata=0

Apply the exact CRS, bounds, resolution, and dimensions from DTM.rst



Metadata policy

Data type: Float32 (keeps 0.02/0.20/0.60 exactly; avoids integer rounding).

NoData: 0 (used consistently in data and header).

Resampling: nearest for all categorical/stepped rasters (P, routing, stream links, land use).
(Use bilinear/cubic only for continuous rasters like precipitation/elevation.)
