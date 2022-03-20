# Mapping Earthquakes with JavaScript & APIs

## Overview

As a data visualization specialist at the Disaster Reporting Network, we were asked to build insightful data visualizations with interactive featureson earthquakes around the world.  Being able to present earthquake information on interactive maps should enable the company to draw people in who might otherwise be bored with information being displayed via a standard list or spreadsheet.

![Initial Earthquake Map](https://github.com/Jeffstr00/Mapping_Earthquakes/blob/main/Resources/map1.png)

After reading in JSON files containing the latest earthquake information, we used JavaScript to customize and Leaflet/Mapbox to create a map which highlights earthquakes all over the globe.  Earthquake magnitudes are intuitively conveyed using both marker size and color (green = mild, red = severe).  While this was a good start, we were then asked to add a few more options to the map: tectonic plate location, displays specifically for 4.5+ magnitude earthquakes, and additional maps to choose from.

## Results

### Tectonic Plate Layer

![Tectonic Plate Map](https://github.com/Jeffstr00/Mapping_Earthquakes/blob/main/Resources/map_tectonic_plates.png)

```  d3.json("https://raw.githubusercontent.com/fraxen/tectonicplates/master/GeoJSON/PB2002_boundaries.json").then(function(data) {
    L.geoJson(data, {
      opacity: .6,
      color: "#91522A",
      weight: 2.5
  }).addTo(tectonicPlates);

  tectonicPlates.addTo(map);   
```

To add an overlay with tectonic plates to our map, we used the above code to read in tectonic plate locations, used them as the data to add them to the map, and customized the look by adjusting the opacity, color, and weight (line size) in the layout.  

### Major Earthquake Layer

![Major Earthquakes Map](https://github.com/Jeffstr00/Mapping_Earthquakes/blob/main/Resources/map_major_earthquakes.png)

Luckily for us, the Major Earthquakes map wasn't too different from the standard Earthquakes one, so we were able to create it by refactoring our old code.  This time, our getColor function was simplified to:

```  function getColor(magnitude) {
    if (magnitude > 6) {
      return "#ff160c";
    }
    if (magnitude > 5) {
      return "#ea2c2c";
    }
    return "#ea822c";
  }   
```

This added a very bright red for earthquakes of the highest (6+) magnitude so that they would visually stand out.  

### Additional Maps

To enhance the visualization options, we opted to add two additional maps: outdoors and dark.  Outdoors (seen in the previous section's image) was similar to the old streets map, but when dealing with earthquakes, we aren't really concerned with driving on roads, so an outdoors-focused one made more sense.  The contrast between the background on the dark map and the bright earthquake markers really made the earthquakes "pop" and gave them a violent feel.  For illustrating earthquakes, this was perfect.

Like with our previous maps, we added the dark one using (with our API_KEY residing in a separate config file):

```
let dark = L.tileLayer('https://api.mapbox.com/styles/v1/mapbox/dark-v10/tiles/{z}/{x}/{y}?access_token={accessToken}', {
	attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
	maxZoom: 18,
	accessToken: API_KEY
});   
```

## Summary

In the end, we were able to use HTML, JavaScript, APIs/JSON files, and Leaflet/Mapbox to create and display up-to-date maps featuring earthquakes all over the world.  New additions to the map included tectonic plate mapping, major earthquake differentiation to hightlight especially large earthquakes, and new maps which hopefully make the visualizations even more striking.  Easily displaying these maps to people via their computers, phones, etc. aims to get and keep more people interested in both earthquakes and the Disaster Reporting Network as a whole.