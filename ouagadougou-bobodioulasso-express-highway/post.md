---
published: 2026-10-29
title: Watching Burkina Faso's First Expressway From Orbit With Sentinel-2
excerpt: A free, repeatable workflow using the Copernicus Browser and Google Earth Engine to monitor landscape change along the Ouagadougou–Bobo-Dioulasso expressway construction.
tags: [beginner, guide, remote-sensing, javascript]
---

[[toc]]

![note] This is an introductory guide that uses [Google Earth Engine (GEE)](https://code.earthengine.google.com/) and the [Copernicus Browser](https://browser.dataspace.copernicus.eu). Code used in GEE will appear as a drop-down menu in order to not bother those who are not interested in that. Tools used in Copernicus Browser are mentioned in-text.

# Introduction

On 16 December 2025, President Ibrahim Traoré [launched the construction](https://faso7.com/2025/12/16/burkina-faso-le-capitaine-ibrahim-traore-pose-la-premiere-pierre-de-la-construction-de-lautoroute-ouaga-bobo/) of the **Autoroute Ouagadougou–Bobo-Dioulasso**, Burkina Faso's first expressway: 332 km joining the country's two largest cities. The project proceeds under the state-led Faso Mêbo initiative and is financed by the state itself—roughly CFA 200 billion, about US$357 million, was allocated in the [2026 budget](https://www.africanews.com/2025/12/21/burkina-faso-starts-building-first-expressway-in-push-to-modernize/) rather than borrowed from multilateral lenders. As designed, the expressway carries [up to eight lanes](https://en.wikipedia.org/wiki/Ouagadougou%E2%80%93Bobo-Dioulasso_Expressway), and a 140 km/h design speed outside built-up areas. [Reports by 15 February 2026](https://www.presidencedufaso.bf/autoroute-ouaga-bobo-fin-des-travaux-de-deblayage-place-au-terrassement/) announced _clearing_ finished along the full length, and _terracing_ beginning on the cleared sections. There are no publicly available reports regarding the conclusion of _terracing_ and/or starting of _asphalting_.

![The Ouagadougou–Bobo-Dioulasso expressway alignment drawn over the Copernicus Browser basemap. Ouagadougou at the top-right corner, and Bobo-Dioulasso at the bottom-left. Screenshot 22 October 2026.](entire-300km-road/copernicus-road-line-22-10-26.JPG "Figure 1. Roughly 300 km alignment drawn by hand using the _draw a line_ tool in [Copernicus Browser](https://browser.dataspace.copernicus.eu) in a Sentinel-2 image from 22 October 2026. Ouagadougou at the top-right, and Bobo-Dioulasso at the bottom-left.") 

The line drawn on the image above can be exported as a `GeoJSON` file to be used later in [Google Earth Engine (GEE)](https://code.earthengine.google.com/). The exact `GeoJSON` for that line is:

```dropdown GeoJSON "GeoJSON file for 300 km road line"
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {},
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [-1.6852, 12.300507], [-1.73687, 12.284489], [-1.767168, 12.277779], [-1.810942, 12.267631],
          [-1.826649, 12.264947], [-1.879349, 12.264612], [-1.897459, 12.263186], [-1.912479, 12.259244],
          [-1.947155, 12.242637], [-1.966553, 12.22477], [-1.981401, 12.212019], [-1.991701, 12.20707],
          [-2.015219, 12.200023], [-2.030926, 12.19566], [-2.04217, 12.190291], [-2.068863, 12.168813],
          [-2.100019, 12.143473], [-2.107487, 12.13911], [-2.137356, 12.126523], [-2.161388, 12.115865],
          [-2.178469, 12.106131], [-2.192202, 12.094885], [-2.207737, 12.084478], [-2.223873, 12.078351],
          [-2.238979, 12.075078], [-2.25503, 12.073399], [-2.261124, 12.071385], [-2.279148, 12.05913],
          [-2.293139, 12.047883], [-2.303095, 12.042175], [-2.31185, 12.038649], [-2.320948, 12.032269],
          [-2.328329, 12.025386], [-2.335968, 12.017998], [-2.351761, 12.003055], [-2.366867, 11.994827],
          [-2.402401, 11.98492], [-2.433643, 11.974005], [-2.465401, 11.958891], [-2.507458, 11.944113],
          [-2.535782, 11.93286], [-2.570286, 11.898932], [-2.613029, 11.868863], [-2.632256, 11.852567],
          [-2.656116, 11.837111], [-2.677746, 11.822157], [-2.707272, 11.815436], [-2.732677, 11.806699],
          [-2.753792, 11.799306], [-2.783833, 11.796785], [-2.802372, 11.795441], [-2.820568, 11.789727],
          [-2.834988, 11.787543], [-2.862797, 11.787207], [-2.903137, 11.791912], [-2.913265, 11.79208],
          [-2.922363, 11.791576], [-2.929916, 11.7904], [-2.957554, 11.782334], [-3.002529, 11.769058],
          [-3.024673, 11.765192], [-3.042698, 11.765192], [-3.049564, 11.766369], [-3.061924, 11.765192],
          [-3.106899, 11.746202], [-3.140202, 11.72889], [-3.180542, 11.703509], [-3.20097, 11.694432],
          [-3.223286, 11.688717], [-3.248005, 11.687036], [-3.276672, 11.689053], [-3.305168, 11.690902],
          [-3.314095, 11.689221], [-3.329029, 11.681825], [-3.358383, 11.657616], [-3.411598, 11.601458],
          [-3.428936, 11.588174], [-3.456745, 11.572703], [-3.489704, 11.554708], [-3.520432, 11.534525],
          [-3.571758, 11.50694], [-3.628063, 11.466903], [-3.670635, 11.435777], [-3.703079, 11.411884],
          [-3.733292, 11.391523], [-3.763332, 11.370319], [-3.828735, 11.323025], [-3.870277, 11.298618],
          [-3.892422, 11.283972], [-3.935852, 11.266296], [-3.952503, 11.255521], [-3.969498, 11.247945],
          [-3.982372, 11.245419], [-3.999367, 11.235149], [-4.027004, 11.229592], [-4.037476, 11.226898],
          [-4.06889, 11.211407], [-4.120045, 11.188674], [-4.132061, 11.18278], [-4.172058, 11.157519]
        ]
      }
    }
  ]
}
```

# Sentinel-2

In this guide we monitor the expressway construction from space using Sentinel-2, a satellite mission of European Copernicus that repeatedly photographs the same spot on Earth roughly every five days. [Sentinel-2 records reflected energy in visible, near-infrared, red-edge, and short-wave infrared wavelengths](https://gisgeography.com/sentinel-2-bands-combinations/). These different slices of the electromagnetic spectrum are also called _spectral bands_. 

## Bands used in this analysis

Sentinel-2 has 13 spectral bands. In this analysis we will use the following five:

```
B2  = blue (B)
B3  = green (G)
B4  = red (R)
B8  = near-infrared (NIR)
B11 = short-wave infrared 1  (SWIR1)
```

Each type of surface absorbs and reflects light differently; for example, healthy vegetation absorbs much of the red light used in photosynthesis but reflects strongly the near-infrared light. Bare and dry soil often reflects relatively strongly in visible and short-wave infrared wavelengths. Whereas water and moisture-rich vegetation absorb short-wave infrared, so they often appear dark in SWIR bands.

Images built from B2, B3 and B4 (RGB) are also called _true color_ images. Figure 2 shows a `.gif` created using one true color (RGB) image per month, spanning from October 2025 to October 2026, of a 20 km section of the expressway west of Tanghin-Dassouri.

![true_color_aoi](/20km_section_near_Tanghin-Dassouri/monthly_true_color_section_road_20cc_oct25-oct26.gif "Figure 2. Roughly 20 km section of the expressway, west of Tanghin-Dassouri. True color (RGB) Sentinel-2 images from October 2025 to October 2026. `GIF` generated using the _create timelapse animation_ tool from Copernicus Browser.")

This result already is quite informative. According to reports, the _clearing_ of the expressway took place between December 2025 and February 2026. The timelapse above is consistent with that; at least for this 20 km section west of Tanghin-Dassouri. The image on 29 of December 2025 shows the clearing beginning, and by 02 of February 2026, we can see this entire section has been cleared. Then, the image from 23 of April 2026 shows what seems to be the beginning of the _terracing_ operation, although this operation seems to pause by the 11 of August 2026. We see no signs of the completion of _terracing_ and/or beginning of _asphalting_.

Even though we can draw a conclusion from the timelapse above regarding the construction of the expressway, we would also like to introduce different band combinations apart from the RGB image; and touch upon how to analyse this data programmatically.

## Indices

To highlight different desired features of the landscape we often build different _indices_ by combining these bands in some way. For example, we mentioned healthy vegetation absorbs red light (B4) and reflects near-infrared light (B8); therefore, the contrast between these two bands is useful to detect vegetation (or lack thereof) in our data. We combine these two bands into the **Normalized Difference Vegetation Index (NDVI)**:

```
NDVI = (B8-B4) / (B8 + B4).
```

If healthy vegetation absorbs red (B4 ↓) and reflects near-infrared  (B8 ↑), then higher NDVI values generally indicate denser or more active vegetation, while lower values can indicate sparse vegetation, exposed ground, water or built surfaces. In this visualization, NDVI values are mapped to a color gradient, from black for the lowest values, through beige and white around zero, to dark green for the highest ones. In Figure 3 we show the same `.gif` as in Figure 2, but using NDVI.

![ndvi_aoi](/20km_section_near_Tanghin-Dassouri/monthly_NDVI_section_road_20cc_oct25-oct26.gif "Figure 3. Roughly 20 km section of the expressway, west of Tanghin-Dassouri. NDVI Sentinel-2 visualization from October 2025 to October 2026. `GIF` generated using the _create timelapse animation_ tool from Copernicus Browser.")

In this animation, the different phases of the expressway construction are even clearer than before. In the beginning of the timelapse (October 2025), most of the image has a green color, indicating vegetation. Then, we clearly see the evolution of the _clearing_ stage, where the road starts appearing as a tone of beige. After that, the beginning of the _terracing_ phase becomes very clear; the parts of the road where terracing works (such as digging) took place become white; and we can also see a couple white blobs appearing (and growing) next to the road; these could indicate borrow pits (areas excavated to supply material for the road embankment), material stockpiles, or construction camps. By the end of the timelapse, we see some white parts start becoming beige again, which could indicate vegetation growing back.

Next, we have the **Normalized Difference Moisture Index (NDMI)**:
```
NDMI = (B8 - B11) / (B8 + B11).
```

NDMI contrasts near-infrared with short-wave infrared. Water in leaves and soil absorbs strongly in SWIR, so higher values (blue) generally indicate wetter or denser canopies, and lower values (red) drier vegetation or dry, exposed ground. It responds to both canopy water and soil moisture, so a low value does not by itself say which one changed. In this visualization, NDMI values are mapped to a diverging color scale, from red for the lowest values, through yellow around zero, to blue for the highest ones.

![ndmi_aoi](/20km_section_near_Tanghin-Dassouri/monthly_NDMI_section_road_20cc_oct25-oct26.gif "Figure 4. Roughly 20 km section of the expressway, west of Tanghin-Dassouri. NDMI Sentinel-2 visualization from October 2025 to October 2026. `GIF` generated using the _create timelapse animation_ tool from Copernicus Browser.")

Here the road construction progress remains clear; following the same trend as we saw in the true color and NDVI animations. It becomes specially evident during the _green season_ (August, September and October of 2026). While the surrounding is blue, we see the road as orange and red, with the low value areas being the area where terracing seems to have taken place.

Finally, we have the **Bare Soil Index (BSI)**. Where NDVI is built to find vegetation, BSI is built to find exposed ground. It sets the bands that bare soil reflects strongly (SWIR1 and red) against the bands where vegetation stands out (NIR and blue):

```
BSI = ((B11 + B4) - (B8 + B2)) / ((B11 + B4) + (B8 + B2)).
```
Higher BSI values generally indicate more exposed bare soil, while lower values indicate more vegetation cover. Since clearing and terracing both expose soil along the corridor, BSI is a natural companion to NDVI. In this visualization, BSI is applied to the red channel, with NIR (B08) in the green channel and SWIR (B11) in the blue channel; vegetation appears in green, bare ground in red, and water in black.

![bsi_aoi](/20km_section_near_Tanghin-Dassouri/monthly_BSI_section_road_20cc_oct25-oct26.gif "Figure 5. Roughly 20 km section of the expressway, west of Tanghin-Dassouri. BSI Sentinel-2 visualization from October 2025 to October 2026. `GIF` generated using the _create timelapse animation_ tool from Copernicus Browser.")

In this animation, the _clearing_ stage is actually less clear than in the previous two. At the beginning of the timelapse (October 2025), the whole landscape is dry, so the bare soil of the cleared corridor looks much like the bare soil that surrounds it, and the road does not stand out. The contrast appears once the wet season passes and vegetation around grows back: the surroundings turn green while the corridor stays red, and the road becomes very clear. The [potential] _terracing_ signal, however, is subtler than with the other indices; the terraced parts of the road appear only slightly darker red than the rest of the corridor, instead of the strong contrast we saw in NDVI and NDMI. 

![note] You may also come across the **Normalized Difference Built-up Index (NDBI)**. With Sentinel-2 it is `(B11 - B8) / (B11 + B8)`, which is exactly `-NDMI`. It carries no extra information, so we leave it out.

There are hundreds more different indices that are used by combining the 13 bands of Sentinel-2. But for this first guide, we showcased NDVI, NDMI and BSI as they are widely used to monitor vegetation, moisture and bare soil.

# Google Earth Engine

In this section we plot the monthly evolution of the NDVI, NDMI and BSI indices using the free Sentinel-2 surface-reflectance dataset `COPERNICUS/S2_SR_HARMONIZED` in GEE. SR stands for _Surface reflectance_, which means that the atmospheric effects have already been corrected. _Harmonized_ means that values from images processed with different versions of the Sentinel-2 processing baseline have been normalized. Our workflow is divided into five steps:

1. Defining general settings, functions and areas of interest (AOIs).
2. Filtering unfit scenes and masking clouds.
3. Computing NDVI, NDMI and BSI for every image.
4. Combining the images into monthly composites.
5. Extracting the values along the road and plotting them.

## 1. General settings

Here we define important parameters such as (but not limited to):

- the start date of our time series (01 June 2024), giving us about 18 months before construction began;

- the resolution for the pixels on the ground (20 m);

- the Coordinate Reference System (CRS) to use across the analysis: EPSG:32630;

- the coordinates of the lines of the long (~300 km) and short (~20 km) roads that were exported as a `GeoJSON` from Copernicus Browser, which we call routes, and which are what we extract statistics along;

- the AOI, a corridor that adds a buffer of 50 m on each side of both routes. This corridor is only used to select and clip the imagery; the statistics themselves are computed along the route centerlines, not across the full corridor; and

- the end date of our time series (29 October 2026), fixed so the results can be reproduced no matter when the script is run.

```dropdown javascript "[JS Code] General Settings"
var START_DATE = '2024-06-01';
var END_DATE = '2026-10-29'

// Analysis scale in meters. 20 m matches the native resolution of B11
var ANALYSIS_SCALE = 20;

// Adds a mean +/- 1 spatial SD envelope to the index time series
// to give a sense of the spatial variability
var SHOW_INDEX_SD = true;

// Formats a number to a fixed number of decimals (default 3) for printing; 
// returns 'n/a' if the value is missing or not a number.
function fmt(x, dec) {
  if (x === null || x === undefined || isNaN(x)) return 'n/a';
  return Number(x).toFixed(dec === undefined ? 3 : dec);
}

// Returns true only for real, finite numbers (not null, undefined, NaN or Infinity); 
// used to keep missing values out of the charts.
function validNumber(x) {
  return typeof x === 'number' && isFinite(x);
}

var MONTH_NAMES = [
  'JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN',
  'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'
];

// Turns {year: 2025, month: 10} into a short label like 'OCT25' 
// for chart axes and printouts.
function monthLabel(m) {
  return MONTH_NAMES[m.month - 1] + String(m.year).slice(-2);
}

// Coordinates of short road
var SHORT_ROUTE = ee.Geometry.LineString([[-1.759701,12.279121],[-1.783218,12.274718],[-1.805921,12.268973],[-1.819654,12.265744],[-1.829138,12.26478],[-1.839094,12.264402],[-1.852655,12.26478],[-1.867418,12.264864],[-1.881924,12.264528],[-1.89394,12.263689],[-1.903811,12.261928],[-1.915569,12.257944],[-1.934838,12.248718],[-1.943893,12.24423]]);

// Coordinates of long road
var LONG_ROUTE = ee.Geometry.LineString([
  [-1.6852, 12.300507], [-1.73687, 12.284489], [-1.767168, 12.277779], [-1.810942, 12.267631],
  [-1.826649, 12.264947], [-1.879349, 12.264612], [-1.897459, 12.263186], [-1.912479, 12.259244],
  [-1.947155, 12.242637], [-1.966553, 12.22477], [-1.981401, 12.212019], [-1.991701, 12.20707],
  [-2.015219, 12.200023], [-2.030926, 12.19566], [-2.04217, 12.190291], [-2.068863, 12.168813],
  [-2.100019, 12.143473], [-2.107487, 12.13911], [-2.137356, 12.126523], [-2.161388, 12.115865],
  [-2.178469, 12.106131], [-2.192202, 12.094885], [-2.207737, 12.084478], [-2.223873, 12.078351],
  [-2.238979, 12.075078], [-2.25503, 12.073399], [-2.261124, 12.071385], [-2.279148, 12.05913],
  [-2.293139, 12.047883], [-2.303095, 12.042175], [-2.31185, 12.038649], [-2.320948, 12.032269],
  [-2.328329, 12.025386], [-2.335968, 12.017998], [-2.351761, 12.003055], [-2.366867, 11.994827],
  [-2.402401, 11.98492], [-2.433643, 11.974005], [-2.465401, 11.958891], [-2.507458, 11.944113],
  [-2.535782, 11.93286], [-2.570286, 11.898932], [-2.613029, 11.868863], [-2.632256, 11.852567],
  [-2.656116, 11.837111], [-2.677746, 11.822157], [-2.707272, 11.815436], [-2.732677, 11.806699],
  [-2.753792, 11.799306], [-2.783833, 11.796785], [-2.802372, 11.795441], [-2.820568, 11.789727],
  [-2.834988, 11.787543], [-2.862797, 11.787207], [-2.903137, 11.791912], [-2.913265, 11.79208],
  [-2.922363, 11.791576], [-2.929916, 11.7904], [-2.957554, 11.782334], [-3.002529, 11.769058],
  [-3.024673, 11.765192], [-3.042698, 11.765192], [-3.049564, 11.766369], [-3.061924, 11.765192],
  [-3.106899, 11.746202], [-3.140202, 11.72889], [-3.180542, 11.703509], [-3.20097, 11.694432],
  [-3.223286, 11.688717], [-3.248005, 11.687036], [-3.276672, 11.689053], [-3.305168, 11.690902],
  [-3.314095, 11.689221], [-3.329029, 11.681825], [-3.358383, 11.657616], [-3.411598, 11.601458],
  [-3.428936, 11.588174], [-3.456745, 11.572703], [-3.489704, 11.554708], [-3.520432, 11.534525],
  [-3.571758, 11.50694], [-3.628063, 11.466903], [-3.670635, 11.435777], [-3.703079, 11.411884],
  [-3.733292, 11.391523], [-3.763332, 11.370319], [-3.828735, 11.323025], [-3.870277, 11.298618],
  [-3.892422, 11.283972], [-3.935852, 11.266296], [-3.952503, 11.255521], [-3.969498, 11.247945],
  [-3.982372, 11.245419], [-3.999367, 11.235149], [-4.027004, 11.229592], [-4.037476, 11.226898],
  [-4.06889, 11.211407], [-4.120045, 11.188674], [-4.132061, 11.18278], [-4.172058, 11.157519]
]);

// Routes used for statistics and charts. pointShape sets the chart
// marker: short route = squares, long route = triangles.
var TRANSECTS = [
  {key: 'short', label: 'Short route', geometry: SHORT_ROUTE, pointShape: 'square'},
  {key: 'long',  label: 'Long route',  geometry: LONG_ROUTE,  pointShape: 'triangle'}
];

// Consistent colors for routes across all time-series charts.
var ROUTE_COLORS = {
  short: '#0072B2',  // Blue
  long:  '#D55E00'   // Vermilion
};

// Both lines as one FeatureCollection, so a single reduceRegions()
// request returns the statistics of both routes.
var TRANSECT_FC = ee.FeatureCollection(TRANSECTS.map(function (t) {
  return ee.Feature(t.geometry, {key: t.key});
}));

// Corridor: both routes buffered by 50 m on each side (100 m wide).
var aoi = SHORT_ROUTE.buffer(50).union(LONG_ROUTE.buffer(50));

// Common grid for statistics. UTM zone 30N covers both routes.
var ANALYSIS_CRS = 'EPSG:32630';

Map.centerObject(aoi, 8);

Map.addLayer(aoi, {color: 'FF0000'}, 'AOI: 100 m corridors');
Map.addLayer(SHORT_ROUTE, {color: 'FFFF00'}, 'Short route centerline');
Map.addLayer(LONG_ROUTE, {color: '00FFFF'}, 'Long route centerline');
```

## 2. Cloud filtering and masking

Clouds and their shadows hide the ground and interfere with our indexes. There are two main ways to deal with them:

1. **Scene filter**. Any scene that has cloud coverage above 20% are excluded. Note that this considers the entire scene, not just our AOI. So a scene might still have less than 20% cloud cover but have clouds on top of the area we are interested in.

2. **Pixel mask**. We mask individual bad pixels using the **Scene Classification Layer (SCL)** that already comes with each image. Masked pixels are not used to compute any indices. The different SCL classes are as follows:

|SCL class|Meaning|
|-:|-|
|1| Saturated or defective|
|3| Cloud shadows|
|7| Unclassified|
|8| Medium-probability clouds|
|9| High-probability clouds|
|10| Thin [cirrus clouds](https://en.wikipedia.org/wiki/Cirrus_cloud)|
|11| Snow or ice|

The masked areas are also expanded by 20 m, to reduce contamination at the edges of clouds and shadows.

```dropdown javascript "[Code] Cloud filtering and masking"
var endJs = new Date(END_DATE + 'T00:00:00Z');

var currentMonth = {
  year: endJs.getUTCFullYear(),
  month: endJs.getUTCMonth() + 1
};

var currentLbl = monthLabel(currentMonth);

// Stage 1 (scene level)
var s2Scenes = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
  .filterBounds(aoi)
  .filterDate(START_DATE, END_DATE)
  .filter(ee.Filter.lte('CLOUDY_PIXEL_PERCENTAGE', 20));

// Stage 2 (pixel level)
var CLOUD_BUFFER_METERS = 20;

function maskS2CloudsAndShadows(image) {
  var scl = image.select('SCL');

  var badPixels = scl.eq(1)
    .or(scl.eq(3))
    .or(scl.eq(7))
    .or(scl.eq(8))
    .or(scl.eq(9))
    .or(scl.eq(10))
    .or(scl.eq(11));

  var bufferedBadPixels = badPixels.focalMax({
    radius: CLOUD_BUFFER_METERS,
    units: 'meters'
  });

  return image
    .updateMask(bufferedBadPixels.not())
    .copyProperties(image, ['system:time_start']);
}

var s2 = s2Scenes.map(maskS2CloudsAndShadows);
```

## 3. Computing the indices

For every image we scale the bands to reflectance (the dataset stores them as integers multiplied by 10000, so we scale them back multiplying by 0.0001), and compute NDVI, NDMI and BSI. Pixels where a formula would divide by zero are also masked.

```dropdown javascript "[Code] Computing indices"
var ANALYSIS_BANDS = ['NDVI', 'NDMI', 'BSI'];

function makeAnalysisImage(img) {
  var ref = img.select([
    'B2', 'B3', 'B4', 'B8', 'B11'
  ]).multiply(0.0001);

  var blue = ref.select('B2');
  var red = ref.select('B4');
  var nir = ref.select('B8');
  var swir1 = ref.select('B11');

  // NDVI = (NIR - Red) / (NIR + Red)
  var ndviDen = nir.add(red);

  var ndvi = nir.subtract(red)
    .divide(ndviDen)
    .updateMask(ndviDen.neq(0))
    .rename('NDVI');

  // NDMI = (NIR - SWIR1) / (NIR + SWIR1)
  var ndmiDen = nir.add(swir1);

  var ndmi = nir.subtract(swir1)
    .divide(ndmiDen)
    .updateMask(ndmiDen.neq(0))
    .rename('NDMI');

  // BSI = ((SWIR1 + Red) - (NIR + Blue)) /
  //      ((SWIR1 + Red) + (NIR + Blue))
  var bsiDen = swir1.add(red).add(nir).add(blue);

  var bsi = swir1.add(red)
    .subtract(nir.add(blue))
    .divide(bsiDen)
    .updateMask(bsiDen.neq(0))
    .rename('BSI');

  return ndvi
    .addBands(ndmi)
    .addBands(bsi)
    .select(ANALYSIS_BANDS)
    .copyProperties(img, ['system:time_start']);
}
```

## 4. Monthly composites

Next, we build one composite per calendar month, from June 2024 up to October 2026. Building composites means merging all images acquired in a month into one, to avoid gaps left by cloud masking (where one image is masked, another may have valid data) and to avoid the noise of any single acquisition (residual atmosphere, viewing geometry, sensor noise). The indices are computed image by image and then, for every pixel, we take the median across all observations of that month. For example, imagine for the month of June 2024 we have 3 images; and consider that the NDVI for the first pixel in the first row are: 0.4, 0.2, 0.5. We then take the [median](https://statistics.laerd.com/statistical-guides/measures-central-tendency-mean-mode-median.php) value of that pixel to build our composite: 0.4. We do the same for every pixel to have our composite for that month. The median is less sensitive than the mean to the occasional outlier (e.g., cloud or shadow) that slipped through the mask, but reliability depends on number and quality of valid observations. With only one or two images in a month, the median is simply that image (or the mean of the two) and offers no robustness.

In our code we also print, for every month, how many image tiles intersect each route (long or short), how many distinct dates they cover, and which dates those are. Note that one acquisition date can produce several tiles, because the road crosses adjacent tiles, so the number of distinct dates is the better measure of how many independent observations go into each composite.

```dropdown javascript "[Code] Monthly composites"
// Every calendar month from START_DATE through the current month.
var MONTHS = (function () {
  var list = [];
  var start = new Date(START_DATE + 'T00:00:00Z');

  var cur = new Date(Date.UTC(
    currentMonth.year,
    currentMonth.month - 1,
    1
  ));

  while (cur >= start) {
    list.unshift({
      year: cur.getUTCFullYear(),
      month: cur.getUTCMonth() + 1
    });

    cur.setUTCMonth(cur.getUTCMonth() - 1);
  }

  return list;
})();

if (MONTHS.length === 0) {
  throw new Error('Current date is before the configured start date.');
}

// Image count and acquisition dates per month.
var metaFC = ee.FeatureCollection(MONTHS.map(function (m) {
  var start = ee.Date.fromYMD(m.year, m.month, 1);
  var end = start.advance(1, 'month');

  var col = s2.filterDate(start, end);

  var shortCount = s2Scenes
    .filterDate(start, end)
    .filterBounds(SHORT_ROUTE.buffer(50))
    .size();

  var longCount = s2Scenes
    .filterDate(start, end)
    .filterBounds(LONG_ROUTE.buffer(50))
    .size();

  var dates = col.aggregate_array('system:time_start')
    .map(function (ms) {
      return ee.Date(ms).format('YYYY-MM-dd');
    })
    .distinct()
    .sort();

  return ee.Feature(null, {
    t: start.millis(),
    n: col.size(),
    short_n: shortCount,
    long_n: longCount,
    dates: dates
  });
})).sort('t');

print('Loading monthly metadata...');

var meta = metaFC.getInfo();

meta.features.forEach(function (f) {
  var p = f.properties;

  var d = new Date(p.t);
  var m = {
    year: d.getUTCFullYear(),
    month: d.getUTCMonth() + 1
  };

  print(
    monthLabel(m) +
    ': Short route = ' + p.short_n +
    ' tiles; Long route = ' + p.long_n +
    ' tiles; Combined AOI = ' + p.n +
    ' tiles, ' + p.dates.length + ' distinct dates. Dates:',
    p.dates
  );
});

// One median composite per non-empty month.
var composites = [];
var labelsKept = [];

meta.features.forEach(function (f) {
  var p = f.properties;

  if (!p.n) return;

  var d = new Date(p.t);

  var m = {
    year: d.getUTCFullYear(),
    month: d.getUTCMonth() + 1
  };

  var start = ee.Date.fromYMD(m.year, m.month, 1);

  var comp = s2
    .filterDate(start, start.advance(1, 'month'))
    .map(makeAnalysisImage)
    .median()
    .clip(aoi)
    .set('label', monthLabel(m))
    .set('t', p.t);

  composites.push(comp);
  labelsKept.push(monthLabel(m));
});

print('Built ' + composites.length + ' monthly composites.');

if (composites.length === 0) {
  throw new Error('No non-empty monthly composites were found.');
}

// Mark the latest month if it is the partial current month.
var hasPartial =
  labelsKept[labelsKept.length - 1] === currentLbl;
```

## 5. Extracting the values and plotting them

For every monthly composite we compute the [mean](https://statistics.laerd.com/statistical-guides/measures-central-tendency-mean-mode-median.php) and [standard deviation](https://en.wikipedia.org/wiki/Standard_deviation) of each index over the pixels that the road lines pass through, at 20 m resolution. The mean gives the typical value along the road in that month, the standard deviation tells us how much the value varies along the road for that month composite. 

Both routes are processed in a single request, and the months are requested one after the other. This keeps each request small, which avoids GEE's computation limits, especially for the ~300 km line. Once all months are done, the code builds one chart per index.

```dropdown javascript "[Code] Extracting values and plotting"
var INDEX_PRODUCTS = [
  {key: 'NDVI', name: 'NDVI'},
  {key: 'NDMI', name: 'NDMI'},
  {key: 'BSI',  name: 'BSI'}
];

var meanStd = ee.Reducer.mean().combine({
  reducer2: ee.Reducer.stdDev(),
  sharedInputs: true
});

// Each row: {order, label, values: {short: {...}, long: {...}}}
var monthlyValueRows = [];

function monthDisplayLabel(index) {
  var label = labelsKept[index];

  if (index === composites.length - 1 && hasPartial) {
    label += ' *';
  }

  return label;
}

// Calculate mean and standard deviation for both route lines.
function reduceImageStats(image) {
  return image.reduceRegions({
    collection: TRANSECT_FC,
    reducer: meanStd,
    scale: ANALYSIS_SCALE,
    crs: ANALYSIS_CRS,
    tileScale: 4
  });
}

// Convert evaluated features into properties keyed by route.
function statsByRoute(result) {
  var out = {};

  ((result && result.features) || []).forEach(function (f) {
    out[f.properties.key] = f.properties;
  });

  return out;
}

// Print the monthly spatial mean +/- 1 SD for the three indices.
function printIndexStats(properties, routeLabel, label) {
  print(
    '[MONTHLY VALUE] ' + label + ' | ' + routeLabel +
    ' | NDVI ' +
      fmt(properties.NDVI_mean) +
      ' ± ' + fmt(properties.NDVI_stdDev) +
    ' | NDMI ' +
      fmt(properties.NDMI_mean) +
      ' ± ' + fmt(properties.NDMI_stdDev) +
    ' | BSI ' +
      fmt(properties.BSI_mean) +
      ' ± ' + fmt(properties.BSI_stdDev)
  );
}

function printAllRoutes(values, label) {
  TRANSECTS.forEach(function (t) {
    printIndexStats(values[t.key] || {}, t.label, label);
  });
}

// One statistics request per month, covering both routes.
// Once all monthly values have been retrieved, build the charts.
function loadMonthlyStats(index) {
  if (index >= composites.length) {
    print('Monthly statistics complete. Building charts...');

    INDEX_PRODUCTS.forEach(function (product, productIndex) {
      makeValueTimeSeries(product, productIndex + 1);
    });

    print('All monthly statistics and charts complete.');
    return;
  }

  var label = monthDisplayLabel(index);

  print(
    'Loading monthly values ' +
    (index + 1) + '/' + composites.length +
    ': ' + label
  );

  reduceImageStats(composites[index]).evaluate(
    function (result, error) {
      var row = {
        order: index,
        raw: labelsKept[index],
        label: label,
        values: error ? {} : statsByRoute(result)
      };

      if (error) {
        print('Statistics failed for ' + label + ':', error);
        print(
          'This month will appear as a gap where values are unavailable.'
        );
      } else {
        printAllRoutes(row.values, label);
      }

      monthlyValueRows.push(row);

      // Run the next request only after this one finishes.
      loadMonthlyStats(index + 1);
    }
  );
}

function makeValueTimeSeries(product, plotNumber) {
  var band = product.key;
  var chartName = product.name;

  var columns = [
    {id: 'month', label: 'Month', type: 'string'}
  ];

  var series = {};
  var getters = [];
  var seriesCount = 0;

  function addSeries(column, options, getter, isMean) {
    columns.push(column);
    series[seriesCount++] = options;
    getters.push({
      get: getter,
      isMean: isMean
    });
  }

  TRANSECTS.forEach(function (t) {
    function stat(values, name) {
      var props = values[t.key];
      return props ? props[name] : null;
    }

    var routeColor = ROUTE_COLORS[t.key];

    // Spatial mean.
    addSeries(
      {
        id: t.key + '_' + band,
        label: t.label + ': spatial mean',
        type: 'number'
      },
      {
        color: routeColor,
        lineWidth: 3,
        pointSize: 6,
        pointShape: t.pointShape
      },
      function (values) {
        return stat(values, band + '_mean');
      },
      true
    );

    if (SHOW_INDEX_SD) {
      // Upper and lower boundaries share one legend entry.
      var dashed = {
        color: routeColor,
        lineWidth: 1,
        lineDashStyle: [4, 4],
        pointSize: 0
      };

      function envelope(values, sign) {
        var mean = stat(values, band + '_mean');
        var sd = stat(values, band + '_stdDev');

        return (validNumber(mean) && validNumber(sd))
          ? mean + sign * sd
          : null;
      }

      // Upper SD boundary: labelled.
      addSeries(
        {
          id: t.key + '_upper',
          label: t.label + ': mean ± 1 spatial SD',
          type: 'number'
        },
        dashed,
        function (values) {
          return envelope(values, 1);
        },
        false
      );

      // Lower SD boundary: no additional legend entry.
      addSeries(
        {
          id: t.key + '_lower',
          label: '',
          type: 'number'
        },
        dashed,
        function (values) {
          return envelope(values, -1);
        },
        false
      );
    }
  });

  var hasValues = false;

  // Look up the computed rows by month label.
  var rowsByLabel = {};
  monthlyValueRows.forEach(function (row) {
    rowsByLabel[row.raw] = row;
  });

  // One row per calendar month in MONTHS, including months with no images
  // (these show on the x-axis with a gap in the lines).
  var rows = MONTHS.map(function (m) {
    var raw = monthLabel(m);
    var row = rowsByLabel[raw];

    var label = row
      ? row.label
      : (raw === currentLbl ? raw + ' *' : raw);
    var values = row ? row.values : {};

    var cells = [{v: label}];

    getters.forEach(function (g) {
      var value = g.get(values);

      if (g.isMean && validNumber(value)) {
        hasValues = true;
      }

      cells.push({
        v: validNumber(value) ? value : null
      });
    });

    return {c: cells};
  });

  if (!hasValues) {
    print(
      plotNumber + '. ' + chartName +
      ': no valid monthly values.'
    );
    return;
  }

  var chart = ui.Chart({
    cols: columns,
    rows: rows
  })
    .setChartType('LineChart')
    .setOptions({
      title:
        chartName + ': monthly composite values along the roads ' +
        '(blue squares = short route, orange triangles = long route)',
      titleTextStyle: {fontSize: 30, bold: true},

      hAxis: {
        title: 'Month (* = partial current month)',
        titleTextStyle: {fontSize: 22, italic: false},
        textStyle: {fontSize: 20},
        slantedText: true,
        slantedTextAngle: 45
      },

      vAxis: {
        title: chartName,
        titleTextStyle: {fontSize: 22, italic: false},
        textStyle: {fontSize: 20},
        baseline: 0
      },

      legend: {
        position: 'bottom',
        textStyle: {fontSize: 20}
      },

      interpolateNulls: false,
      series: series
    });

  print(
    plotNumber + '. ' + chartName + ' monthly values',
    chart
  );
}

// Start processing
loadMonthlyStats(0);

```

It's important to note here that GEE computes everything _lazily_. Meaning that nothing is computed when the code runs per se. So when we do operations like `s2.map(...)`, or `.median()`, or `.clip(aoi)`, we are just building a recipe, not yet computing anything. Pixels are computed only when a result is requested, which here is `reduceRegions`. This means that we are not computing the indices for the entire image, we are computing them only for the pixels along the route lines, which are the regions passed to `reduceRegions`.

# Results & final remarks

Figures 6, 7 and 8 show the monthly composite values of NDVI, NDMI and BSI from June 2024 to October 2026, respectively. 

![NDVI](/TIMESERIES/NDVI.png "Figure 6. Mean and ±1 standard deviation of the NDVI computed for the ~300 km road (orange) and the ~ 20 km section (blue). Gaps in the graph represent months without images available due to cloud coverage.")

![NDMI](/TIMESERIES/NDMI.png "Figure 7. Mean and ±1 standard deviation of the NDMI computed for the ~300 km road (orange) and the ~ 20 km section (blue). Gaps in the graph represent months without images available due to cloud coverage.")

![BSI](/TIMESERIES/BSI.png "Figure 8. Mean and ±1 standard deviation of the BSI computed for the ~300 km road (orange) and the ~ 20 km section (blue). Gaps in the graph represent months without images available due to cloud coverage.")

So, what can we tell from these results? 

**Seasonality**: The most prominent feature, which is the peaks in the graphs around September and October of 2024 and 2025. These months come right after the rainy season, which in this region peaks around July-August. Vegetation responds to rainfall with a lag of a month or two, so NDVI and NDMI reach their maximum at the end of the rains, when vegetation is at its greenest and wettest. And BSI at its minimum: most of the soil is hidden beneath vegetation.

Now, if we focus in 2026, we see that the peak from the wet months is not really present. For the long road results (orange), we see the peak starting to form, but not really reaching the values it did in the previous years; while the short road results (blue) drops to its lowest (or maximum if looking at BSI). These results are consistent with what we saw earlier: some work was done in this path. 

If we compare the entire expressway results vs the section west of Tanghin-Dassouri from September 2026 onwards, we see that the short road shows more prevalence of soil: lower NDVI and NDMI values and higher BSI values. This suggests that this section had more exposed soil, consistent with more earthworks, which allowed less vegetation to grow back during the rainy season. 

To conclude, it is worth pointing out that this analysis shows land-surface changes that are consistent with road construction, but not a direct measure of such. Furthermore, the shorter route is a subset of the longer route, which means they are not independent observations. This work is a descriptive, introductory work rather than a controlled test.



![note]Please contact us at hello@soberana-sensing.org if you have something to add, or questions, or anything really! We are happy to connect.

# Resources

- _Google Earth Engine_. (n.d.). Google. https://code.earthengine.google.com/
- _Copernicus Browser_. (n.d.). European Space Agency. https://browser.dataspace.copernicus.eu/
- Faso7. (2025, December 16). _Burkina Faso : le capitaine Ibrahim Traoré pose la première pierre de la construction de l'autoroute Ouaga-Bobo_. https://faso7.com/2025/12/16/burkina-faso-le-capitaine-ibrahim-traore-pose-la-premiere-pierre-de-la-construction-de-lautoroute-ouaga-bobo/
- Africanews. (2025, December 21). _Burkina Faso starts building first expressway in push to modernize_. https://www.africanews.com/2025/12/21/burkina-faso-starts-building-first-expressway-in-push-to-modernize/
- _Ouagadougou–Bobo-Dioulasso expressway_. (n.d.). Wikipedia. https://en.wikipedia.org/wiki/Ouagadougou%E2%80%93Bobo-Dioulasso_Expressway
- Présidence du Faso. (2026, February 15). _Autoroute Ouaga-Bobo : fin des travaux de déblayage, place au terrassement_. https://www.presidencedufaso.bf/autoroute-ouaga-bobo-fin-des-travaux-de-deblayage-place-au-terrassement/
- _Sentinel-2 bands combinations_. (n.d.). GIS Geography. https://gisgeography.com/sentinel-2-bands-combinations/
- _Cirrus cloud_. (n.d.). Wikipedia. https://en.wikipedia.org/wiki/Cirrus_cloud
- _Measures of central tendency: Mean, median and mode_. (n.d.). Laerd Statistics. https://statistics.laerd.com/statistical-guides/measures-central-tendency-mean-mode-median.php
- _Standard deviation_. (n.d.). Wikipedia. https://en.wikipedia.org/wiki/Standard_deviation