---
title: "Flood Analysis with False Color Composites and NDWI"
teaching: 30
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions

- What is a false color composite and what does it reveal?
- How do I create a false color composite from Sentinel-2 bands in QGIS?
- What is NDWI and how does it differ from NDVI?
- How can NDWI help identify flood extent more accurately than a false color composite alone?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand how false color composites remap spectral bands to RGB channels to highlight landscape features
- Build a virtual raster in QGIS to create a false color composite from Sentinel-2 bands
- Calculate NDWI using the Raster Calculator to delineate water from non-water
- Compare FCC, NDWI, and satellite basemap imagery to assess flood extent

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

In the previous episode we used NDVI to identify vegetation. In this episode we shift focus to water — specifically, how to identify the extent of flooding using two complementary techniques: **False Color Composites (FCC)** and the **Normalized Difference Water Index (NDWI)**.

### What Is a False Color Composite?

A true color image maps the red, green, and blue bands to their corresponding RGB screen channels, producing an image that looks like a normal photograph. A **false color composite** swaps one of those bands — typically replacing blue with near-infrared — so that features invisible to the human eye become visible on screen.

In a standard NRG false color composite (NIR → Red channel, Red → Green channel, Green → Blue channel):

- **Vegetation** appears **red** — because healthy vegetation strongly reflects near-infrared light, which is now displayed in the red channel
- **Water** appears **dark blue to black** — because water absorbs most near-infrared light
- **Bare soil and infrastructure** appear **grey to white**

![Diagram showing how true color and false color composites map spectral bands to RGB screen channels differently.](assets/01_FCC_Example.png "False Color Composite Example — Source: Lamb 2001")

![Chart showing reflectance values across wavelengths for different surface types, illustrating why band substitution reveals hidden features.](assets/02_Reflectance_Figure.png "Spectral Reflectance — Source: Khan et al. 2018")

![A false color composite example from Sentinel-2 imagery.](assets/03_FCC_Screen_Example.png "False Color Composite — Source: European Space Agency")

### What Is NDWI?

The **Normalized Difference Water Index (NDWI)** operates similarly to NDVI, but targets water rather than vegetation. It uses Sentinel-2 Bands 3 (Green) and 8 (Near-Infrared):

**NDWI = (Green − NIR) / (Green + NIR)**

Water absorbs near-infrared light but reflects green light, so water features produce **positive** NDWI values while non-water features produce **negative** values. This makes NDWI particularly useful for mapping flood extent, where muddy floodwater can appear ambiguous in both true and false color images.

---

## Step 1: Set Up Your Project

1. Create a folder on your desktop called **Flood_Analysis**.
2. Open QGIS, close any pop-ups, and go to **Project → Save As**. Save the project as `Flood_Project` inside your Flood_Analysis folder.

---

## Step 2: Download and Extract the Sentinel-2 Image

1. Navigate to the workshop's shared resources Google Drive. Under **Day 1_Session 3a: Basic raster functions**, download the ZIP file:
   `S2A_MSIL1C_20220831T055651_N0510_R091_T42RVR_20240719T213641.SAFE.zip`

2. Save the ZIP file to your **Flood_Analysis** folder and extract it:
   - **Windows:** Right-click → **Extract All**
   - **Mac:** Double-click the ZIP file

This image captures the Sindh province of Pakistan during the devastating 2022 floods.

---

## Step 3: Load the Required Bands

To create both the FCC and the NDWI, we need three bands:

| Band | Wavelength | File name |
|------|-----------|-----------|
| **Band 3** (Green) | ~560 nm | `T42RVR_20220831T055651_B03.jp2` |
| **Band 4** (Red) | ~665 nm | `T42RVR_20220831T055651_B04.jp2` |
| **Band 8** (NIR) | ~842 nm | `T42RVR_20220831T055651_B08.jp2` |

In the **Browser Panel**, navigate to:
**Project Home → S2A_MSIL1C…SAFE → GRANULE → L1C_T42RVR_… → IMG_DATA**

Drag **B03**, **B04**, and **B08** into the Layers Panel.

![Bands 3, 4, and 8 loaded into QGIS.](assets/04_Bands_Loaded.png "Bands Loaded")

---

## Step 4: Create the False Color Composite

To combine three single-band rasters into one multi-band image, we use a **virtual raster**.

1. From the menu bar, select **Raster → Miscellaneous → Build Virtual Raster**.
2. Click the three dots next to **Input Layers** and arrange the bands by dragging them into this order:
   - **B08** (NIR — will map to the red channel)
   - **B04** (Red — will map to the green channel)
   - **B03** (Green — will map to the blue channel)
3. Click **Select All**, then click the back arrow in the top-left corner of the pop-up.

![The Build Virtual Raster dialog with bands arranged in NIR-Red-Green order.](assets/05_Virtual_Raster_Popup.png "Virtual Raster Setup")

4. Check the box **Place each input file into a separate band**.
5. Under **Virtual**, click the three dots and select **Save to File**. Save it as `Flood_Output` in your Flood_Analysis folder.
6. Click **Run**.

Once complete, uncheck B03, B04, and B08 in the Layers Panel to see the false color composite.

![False color composite of the Sindh province during the 2022 floods.](assets/06_FCC.png "False Color Composite Result")

The red areas are vegetation, dark blue and black areas are water, and grey-to-white areas are bare soil and infrastructure.

---

## Step 5: Compare Against a Satellite Basemap

Add a basemap to compare the FCC against post-flood satellite imagery.

1. If you have not already installed **QuickMapServices**, go to **Plugins → Manage and Install Plugins**, search for **QuickMapServices**, and install it.
2. Open the QMS panel (the globe-with-magnifying-glass icon in the toolbar), search for `imagery`, and add **Esri Satellite (ArcGIS/World_Imagery)**.

Toggle the `Flood_Output` layer on and off to compare the flooded landscape to post-flood imagery.

:::::::::::::::::::::::::::::::::::: challenge

### Exercise 1: Interpret the False Color Composite

1. How accurate is the FCC at showing flood extent? Can you identify areas that were clearly underwater?
2. What does the FCC struggle to distinguish? Look carefully at lighter blue areas — are they floodwater, or bare soil?
3. What was the approximate extent of flooding in this region?

::::::::::::::::::::::::::::::::::::::::::::::::

---

## Step 6: Calculate NDWI

The FCC provides a useful overview, but it has a limitation: muddy floodwater often appears as a lighter blue-grey that is difficult to distinguish from bare soil or infrastructure. NDWI solves this by isolating water based on its spectral signature rather than relying on visual color interpretation.

1. From the menu bar, select **Raster → Raster Calculator**.
2. Enter the NDWI formula in the expression box (double-click band names to insert them):

```
( "T42RVR_20220831T055651_B03@1" - "T42RVR_20220831T055651_B08@1" ) / ( "T42RVR_20220831T055651_B03@1" + "T42RVR_20220831T055651_B08@1" )
```

![The Raster Calculator with the NDWI expression entered.](assets/07_NDWI_Raster_Calculator.png "NDWI Raster Calculator")

3. Click the three-dot button next to **Output layer** and save it as `NDWI_Output` in your Flood_Analysis folder.
4. Click **OK** to run the calculation.

---

## Step 7: Apply Symbology to the NDWI Output

1. Right-click the `NDWI_Output` layer → **Properties** → **Symbology** tab.
2. Change the **Render type** to **Singleband pseudocolor**.
3. Set **Interpolation** to **Discrete**.
4. Set the **Color ramp** to **Blues**.
5. Set the **Mode** to **Equal Interval** and reduce **Classes** to **2**.
6. In the **Value** column, set the first value to `0` — this creates a clean split between water (positive values, blue) and non-water (negative values).
7. Click **Apply**.

![NDWI results with two-class blue symbology applied.](assets/08Initial_NDWI_Results.png "Initial NDWI Results")

---

## Step 8: Overlay NDWI on the False Color Composite

To directly compare how much water the NDWI detected versus what the FCC shows, we can make the non-water class transparent so only detected water is visible over the FCC.

1. Reopen the **Symbology** tab for `NDWI_Output`.
2. Right-click the color swatch for the first value (the non-water class) → **Change Opacity** → set to **0**.
3. Click **Apply**.
4. In the Layers Panel, make sure `Flood_Output` (the FCC) is turned on and positioned **below** `NDWI_Output`.

![NDWI water detection overlaid on the false color composite, showing detected flood extent in blue over the FCC.](assets/09_NDWI_Results_Opacity0.png "NDWI Over FCC")

The blue areas are pixels the NDWI identified as water. You can now see exactly where the NDWI detected flooding that may have been ambiguous in the FCC alone.

:::::::::::::::::::::::::::::::::::: challenge

### Exercise 2: Compare All Three Layers

Toggle between the NDWI overlay, the FCC, and the Esri satellite basemap to answer the following:

1. Does the NDWI detect flood extent that the FCC missed? Where?
2. Are there areas where the NDWI may have incorrectly classified non-water as water (false positives)?
3. What are the strengths and limitations of each method? When would you use FCC versus NDWI?

::::::::::::::::::::::::::::::::::::::::::::::::

---

::::::::::::::::::::::::::::::::::::: keypoints

- False color composites remap spectral bands to RGB channels, making vegetation (red), water (dark blue), and bare soil (grey/white) visually distinct.
- FCC is a quick visual tool but struggles with muddy or shallow floodwater that appears similar to bare soil.
- NDWI uses the ratio of green and near-infrared reflectance to isolate water features, providing a more reliable delineation of flood extent.
- Overlaying NDWI on an FCC combines the strengths of both methods — spectral precision from NDWI and spatial context from the FCC.

::::::::::::::::::::::::::::::::::::::::::::::::
