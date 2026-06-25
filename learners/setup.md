---
title: "Setup"
teaching: 5
exercises: 0
---

## What You Need for This Session

Session 3a builds directly on the DEM and Sentinel-2 data you worked with in Session 2a. Make sure you have the following ready before the session begins.

### Software

- **QGIS Desktop (LTR)** — should already be installed from Session 1a/2a.

### Accounts

- **Copernicus Data Space** — for accessing Sentinel-2 imagery. If you have not yet registered, create a free account at [https://dataspace.copernicus.eu](https://dataspace.copernicus.eu).

### Data

- **Sentinel-2 image for the NDVI exercise** — download the ZIP file `S2B_MSIL1C_20260617T170849_N0512_R112_T14SPG_20260617T203803.SAFE.zip` from the workshop's participant resources Google Drive under [**Day 1_Session 3a: Basic raster functions**](https://drive.google.com/drive/folders/1eLEVONp7J7PeEPeFn1pC6Bm6YG9qIC-c). Save it to a new folder on your desktop called **NDVI_Analysis** locally.
- **SRTM DEM tile** — you should already have this from Session 2a. If not, download one from [USGS EarthExplorer](https://earthexplorer.usgs.gov/) following the instructions in the Session 2a episode.

### Background Reading

The following article provides useful context on the satellite imagery sources and elevation products we use in this session:

##### Lindsay I, Mkrtchyan A. Free and Low-Cost Aerial Remote Sensing in Archaeology: An Overview of Data Sources and Recent Applications in the South Caucasus. Advances in Archaeological Practice. 2023;11(2):164-183. [https://doi.org/10.1017/aap.2023.3](https://www.cambridge.org/core/journals/advances-in-archaeological-practice/article/free-and-lowcost-aerial-remote-sensing-in-archaeology/3A08274856CA8DF67082948C2E063A81)

Two figures from this article are especially relevant to our session:

![Free and low-cost satellite imagery sources commonly used in archaeological and heritage applications.](table1.png "Table 1 from Mkrtchyan 2023 — Satellite Imagery Sources")

![SRTM digital elevation model of the Mount Aragats region of Armenia and examples of common elevation products derived from the model: (a) raw SRTM DEM data; (b) hillshade; (c) slope calculated as percent, with slopes greater than 15% in red; (d) contour lines.](figure5.png "Figure 5 from Mkrtchyan 2023 — DEM-Derived Products")

These examples illustrate the range of analysis you can perform with freely available satellite and elevation data — the same tools and data sources we use in this workshop.

---

### Checklist

- [ ] QGIS installed and opens successfully
- [ ] Copernicus Data Space account created and verified
- [ ] Sentinel-2 ZIP file downloaded and saved to your NDVI_Analysis folder
- [ ] SRTM DEM tile available from Session 2a
