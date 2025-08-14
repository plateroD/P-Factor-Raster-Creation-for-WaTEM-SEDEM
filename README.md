# P-Factor-Raster-Creation-for-WaTEM-SEDEM
For me, this is a headache raster to create.
# Notes
P-factor Raster (Idrisi RST) — Creation Notes
Goal

Create a P-factor raster on the exact grid/CRS of DTM.rst, with:

Type: Float32

Values: categorical/stepped in {0.02, 0.20, 0.60}

NoData: 0

Format: Idrisi RST (+ companion RDC header)

Reference grid:
C:\Users\dplatero\watem-sedem-master\watem_sedem\Harris_Grove\Input\DTM.rst

Source:
PFac.tif (classed P values)

Why this was tricky (a.k.a. where it went wrong)

The source TIFF (or an intermediate) had Scale/Offset or integer storage, which made values collapse to 0/1 on export.

During alignment, some operations produced non-finite values (-inf, NaN) that then polluted stats.

NoData handling was inconsistent (-9999 present but dataset NoData set to 0), so stats/means looked crazy.

Bilinear resampling was used at one point (smooths categories → unintended mid-values 0.2–0.6). P is categorical/stepped and must use nearest.



Metadata policy

Data type: Float32 (keeps 0.02/0.20/0.60 exactly; avoids integer rounding).

NoData: 0 (used consistently in data and header).

Resampling: nearest for all categorical/stepped rasters (P, routing, stream links, land use).
(Use bilinear/cubic only for continuous rasters like precipitation/elevation.)
