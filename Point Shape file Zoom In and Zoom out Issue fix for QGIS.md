QGIS Web Map Marker Zoom Issue Fix
This repository addresses an issue with point shapefile markers moving or shifting when zooming in and out in a web map exported from QGIS using the qgis2web plugin. The problem is related to the configuration of the anchor, anchorXUnits, and anchorYUnits properties in the ol.style.Icon settings within the style.js script.

Problem Description
When zooming in and out on the web map, the point markers (icons) would move or shift location. This behavior occurred because the markers were anchored using the "pixels" unit, which caused them to behave inconsistently during zoom operations.

Causes of the Issue
The problem lies in the following properties in the style.js script:

anchor: [11, 11],
anchorXUnits: "pixels",
anchorYUnits: "pixels",


Anchoring with "pixels" makes the icon's position relative to a fixed pixel location, which can cause the icon to shift as the zoom level changes.

Solution
The anchorXUnits and anchorYUnits properties must be changed to "fraction" to fix this. This adjustment anchors the icon relative to its size, ensuring consistent positioning across zoom levels.

Original Code
```
var style_US_Metro_Counties_Centroids_2 = function(feature, resolution){
    var context = { feature: feature, variables: {} };
    var labelText = "";
    var style = [ new ol.style.Style({
        image: new ol.style.Icon({
            imgSize: [700, 700],
            scale: 0.03142857142857143,
            anchor: [11, 11],
            anchorXUnits: "pixels",
            anchorYUnits: "pixels",
            rotation: 0.0,
            src: "styles/red-marker.svg"
        }),
        text: createTextStyle(feature, resolution, labelText, "10px, sans-serif", "#000000", 'point', "", 0)
    })];
    return style;
};
```

Updated Code

var style_US_Metro_Counties_Centroids_2 = function(feature, resolution){
    var context = { feature: feature, variables: {} };
    var labelText = "";
    var style = [ new ol.style.Style({
        image: new ol.style.Icon({
            imgSize: [700, 700],
            scale: 0.03142857142857143,
            anchor: [0.5, 0.5],
            anchorXUnits: "fraction",
            anchorYUnits: "fraction",
            rotation: 0.0,
            src: "styles/red-marker.svg"
        }),
        text: createTextStyle(feature, resolution, labelText, "10px, sans-serif", "#000000", 'point', "", 0)
    })];
    return style;
};

Key Changes

Anchor Position: Updated from [11, 11] to [0.5, 0.5], making it relative to the icon’s center.
Anchor Units: Changed anchorXUnits and anchorYUnits from "pixels" to "fraction", which centers the icon relative to its size.

How to Use

Export your web map from QGIS using the qgis2web plugin.
Navigate to the style.js file located in the "Style" folder.
Replace the relevant code with the updated script provided above.
Open your map in a browser to see the corrected marker behavior during zoom operations.
