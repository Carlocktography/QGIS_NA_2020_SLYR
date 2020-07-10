# QGIS_NA_2020_SLYR
Presentation Outline Public Draft

# Proposed Talk
In this session, I would like to cover an introduction to SLYR, North Road, Nyall Dawson, and background on the tool.  
I would also like to provide examples of conversions from proprietary ESRI projects and styles to QGIS projects and styles.  
In addition, I would like to talk about how this tool can benefit those in mixed GIS environments, those who wish to transition to FOSS, or those consuming many government-provided data often found in these formats.  
Other tips, tricks, and future developments will likely be covered as well.  
I will be working with Nyall Dawson on the content and scope, so this is my best projection as of now.  
I will provide updates if it changes.

# Requested Talking Points
*Kurt Meneke*  
I think the sheer number of symbology and layout features/improvements that have been developed as a result of SLYR is interesting, and worth a mention.

# Prior Art
*ArcGIS to QGIS In One Easy Lesson with SLYR - Ant Scott - FOSS4GUK Online 2020*  
https://youtu.be/h4fC7ryyyTw

# Talking Over
*Nyall Dawson*  
Intro  
Brett/North Road  
Open-Source SLYR  
Limitations:  
Annotations – QGIS 3.16  
ArcGIS Pro mapx/lyrx/stylex  
TIN – QGIS 3.18+  
ArcScene  
Usage in Mixed Environments  
Editing in QGIS for Portal/Online  

# Nyall Notes
*Nyall Dawson*  
Well - it's a complex situation. In summary:
- QGIS relies on the GDAL library for accessing GDB databases
- GDAL has two different drivers for GDB files. One is an open-source
driver, developed from a reverse-engineering effort into GDB files
(ESRI have not released specifications for the format). The other
driver uses a closed source ESRI SDK for opening GDBs. Both drivers
have issues! The default behavior is to use the open-source driver (it
requires no extra software installation).
- Issues with the open-source driver include:
   - The driver can't read the spatial indices present in gdb files,
so they are slow to open (this is likely being resolved soon, thanks
to a QGIS.org funding grant)
   - The driver is read-only -- you can't use it to edit or create new
gdb tables
   - If a GBD has been compressed in ArcMap, then the driver can't
read the table
   - Some extra information which can be stored in a GBD is not
currently respected (e.g. field domains and relationship definitions)
- While the closed SDK
From Nyall Dawson to Everyone:  08:50 PM
driver does allow edits to be made to GDB
files, it has severe issues, including:
   - The driver is unstable when used in a multi-threaded environment
(such as QGIS)
   - The driver can only read GDBs created in certain ArcMap versions
   - Limited support for CRSes

Furthermore, neither driver is able to read raster layers stored in
GDB files, so there's currently NO way these can be read using
open-source software. (The format has been reverse engineered, but has
not yet been implemented as a GDAL driver).

So overall, it's rather murky. The issue is compounded because the
ESRI software has such poor support for anything which isn't a
shapefile or GDB. If you're lucky, on a good day you'll be able to
open a Geopackage in ArcMap, but it's read-only at best. Basically,
the end result is:

- If you need a data file to be openable and editable in both
packages, you have the choice of either using Shapefiles (with their
inherent limitations), or installing the closed source ESRI SDK driver
for GDAL (and risk th
From Nyall Dawson to Everyone:  08:50 PM
risk the dangerous issues with this driver)
- If you only want QGIS to read vector data, you can use geodatabases
(with the open-source driver) and do all the editing in ArcMap alone.
- If you only want read-only access for ArcGIS, you can sometimes use
Geopackages and do the editing in QGIS.
- Avoid storing rasters in GDB files if you'll need to access them
from open-source tools

Basically, the only reliable approach which exists today is to use Shapefiles!

There's a few steps to rectifying this issue, listed in order of priority:

1. (hopefully addressed soon by a QGIS.org grant) implement the
spatial index support in the open-source GDB driver, so that reading
vector files from GDB tables is fast. This would get us first class
support for reading GDB vector tables "out of the box" in open source
software
2. Implement a GDAL driver for GDB raster layers. This would allow
QGIS/other open source tools to load raster layers stored in GDBs. (A
representative from GDAL and I asked for a QGIS.org grant to cover
From Nyall Dawson to Everyone:  08:50 PM
this work, but unfortunately it was not funded)
3. Implement GDB vector editing support in the open-source driver.
This would then allow GDBs to be safely used as a common data file for
use across both QGIS and ArcMap, with full reading/writing
capabilities in both software.
4. Implement support for handling the remaining "extra" capabilities
of GDB databases, by exposing field domains and relationship
information from GDBs in QGIS
(5. Low priority: Reverse engineer the specifications for compressed
GDB files, and add support of these to the GDAL driver.  There's a
basic understanding of how the compression works, but work remains to
verify this understanding and develop a decompression tool)
(6. Low priority: Add support for creating raster layers in GDB files
to GDAL. This is a low priority task, given that there's plenty of
alternative raster formats which work well in both software packages
(such as GeoTIFF))

# Data Mining
https://github.com/qgis/QGIS/pulls?q=is:pr+SLYR  
2020-07-01 19:00 EST  
#
[FEATURE] New line symbol type: Hash line #9644  
https://github.com/qgis/QGIS/pull/9644  

Handle text formats in style manager #3028  
https://github.com/qgis/QGIS/pull/30281  

New algorithm "Combine style databases" #30608  
https://github.com/qgis/QGIS/pull/31315  

"Center of segment" placement mode for marker and hash line symbol layers #31315  
https://github.com/qgis/QGIS/pull/31315  

New class QgsBookmarkManager #31524  
https://github.com/qgis/QGIS/pull/31524  

Add option to set color for rendering nodata pixels in raster layers #31728  
https://github.com/qgis/QGIS/pull/31728  

[FEATURE] Random marker fill symbol layer type #32241  
https://github.com/qgis/QGIS/pull/32241  

[FEATURE] Add spacing option for vector layer bar chart diagrams #32984  
https://github.com/qgis/QGIS/pull/32984  

[FEATURE][diagrams] Add option to control pie diagram angular direction #32986  
https://github.com/qgis/QGIS/pull/32986  

Diagrams - add option to show axis for histogram plots, many fixes #33029  
https://github.com/qgis/QGIS/pull/33029  

[FEATURE][diagrams] New diagram type "stacked bars" #33043  
https://github.com/qgis/QGIS/pull/33043  

[diagrams] Paint effect support for diagram renderer #33044  
https://github.com/qgis/QGIS/pull/33044  

Rework picture item UI and behavior #35160  
https://github.com/qgis/QGIS/pull/35160  

Allow scalebar line style to be set using standard QGIS line symbols #35225  
https://github.com/qgis/QGIS/pull/35225  

Add "stepped line" and "hollow" scalebar styles #35238  
https://github.com/qgis/QGIS/pull/35238  

Add numeric formatter "fraction" style #35244  
https://github.com/qgis/QGIS/pull/35244  

[FEATURE][layouts] New item type for marker symbols #35576  
https://github.com/qgis/QGIS/pull/35576  

Allow marker items to sync rotation with maps #35591  
https://github.com/qgis/QGIS/pull/35591  

Allow plugins to register custom "Project Open" handlers #35606  
https://github.com/qgis/QGIS/pull/35606  

Allow configuring legend patch shapes by double-clicking on legend items #35863  
https://github.com/qgis/QGIS/pull/35863  

Allow control over the horizontal spacing before legend group/subgroup/symbols #35974  
https://github.com/qgis/QGIS/pull/35974  

Allow overriding the legend patch size on a per-item basis #36013  
https://github.com/qgis/QGIS/pull/36013  

Allow custom QgsDataItem types a chance to create a info widget #36014  
https://github.com/qgis/QGIS/pull/36014  

Allow customisation of division and subdivision symbols as distinct from scalebar tick horizontal symbol #36222  
https://github.com/qgis/QGIS/pull/36222  

Expose control over layer legend splitting behavior on a layer-by-layer basis #36224  
https://github.com/qgis/QGIS/pull/36224  

Add support for field aliases to OGRFieldDefn, read alias in openfilegdb driver #2729  
https://github.com/OSGeo/gdal/pull/2729  
