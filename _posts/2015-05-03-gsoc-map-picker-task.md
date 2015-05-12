---
layout: post
title:  "Embed Leaflet map as coordinates picker"
date:   2015-05-03 19:32:00
categories: 
- gsoc
- javascript
comments: true
tags:
- openstreetmap
- leafletjs
- javascript
- gsoc2015
---

![Image broken](/images/map-picker.png "The generator form with map picker embedded")

As a warm up task for my GSoC project, I want to embed a map to the API [generator form](http://api.fossasia.net/generator/index.html) as a convenient way for users to provide their location coordinates. When the user clicks on a location on the map, two input fields **Latitude** and **Longitude** will be automatically updated with fresh values. Here's how.

For this task you need [jquery](https://jquery.com/) and [leaflet](http://leafletjs.com/) - an openstreetmap library. You also want to prepare an empty `div` for the embed map :

{% highlight html %}
<html>
<head>
    <link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.css"/>
</head>
<body>
    <label for="latInput">Latitude</label>
    <input id="latInput"/>
    <label for="lngInput">Longitude</label>
    <input id="lngInput"/>
    <div id="map" style="height : 200px"></div>
    <script src="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.js"></script>
    <script src="https://code.jquery.com/jquery-1.11.3.min.js"></script>
<body>
</html>
{% endhighlight %}  Now, let's instanciate the `div` using the leaflet API. A tile layer must be provided, in this example I'm using [mapbox](https://www.mapbox.com) but there are other tile providers as well :

{% highlight javascript %}
var mapCenter = [22, 87];
var map = L.map('map', {center : mapCenter, zoom : 3});
L.tileLayer('https://{s}.tiles.mapbox.com/v3/{id}/{z}/{x}/{y}.png', {
    maxZoom: 18,
    attribution: 'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, ' +
    '<a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ' +
    'Imagery © <a href="http://mapbox.com">Mapbox</a>',
    id: 'examples.map-i875mjb7',
    noWrap : true
}).addTo(map);
{% endhighlight %}

When the user clicks on the map, I want to point a marker to their choice and display a popup with choosen coordinates. That's what `updateMarker` function is good for :  
{% highlight javascript %}
var marker = L.marker(mapCenter).addTo(map);
var updateMarker = function(lat, lng) {
    marker
        .setLatLng([lat, lng])
        .bindPopup("Your location :  " + marker.getLatLng().toString())
        .openPopup();
    return false;
};
{% endhighlight %}

The last missing piece is, on map click event, updating the lat and lng inputs : 

{% highlight javascript %}
map.on('click', function(e) {
    $('#latInput').val(e.latlng.lat);
    $('#lngInput').val(e.latlng.lng);
    updateMarker(e.latlng.lat, e.latlng.lng);
});
{% endhighlight %}

Now if you need the other way around - enter manually the input fields to update the marker, it could be easily done as well, by capturing a useful event associated with HTML `input` tag - the... `input` event :
{% highlight javascript %}
var updateMarkerByInputs = function() {
	return updateMarker( $('#latInput').val() , $('#lngInput').val());
}
$('#latInput').on('input', updateMarkerByInputs);
$('#lngInput').on('input', updateMarkerByInputs);
{% endhighlight %}

That's about it ! Check out this tutorial on [Gist](https://gist.github.com/zyzo/143e2d8ec67f7c316ae2) and [JSFiddle](http://jsfiddle.net/zyzo/d8wafp5q/).
