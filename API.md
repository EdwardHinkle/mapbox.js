# Getting Started

This API documentation covers the MapBox Javascript API, an API for adding
MapBox maps to webpages.

## Prerequisites

In order to use this API, you'll need to understand basic Javascript and mapping concepts.
If you'd like to learn Javascript, start with [an interactive course](http://www.codecademy.com/tracks/javascript),
[a book](http://eloquentjavascript.net/) or [a printed book](http://www.amazon.com/dp/0596517742/?tag=stackoverfl08-20).
If you'd like to learn more about maps, [we've provided a helpful article explaining how web maps work](http://mapbox.com/developers/guide/).

## MapBox.js & Leaflet

The Javascript API is implemented as a [Leaflet](http://leafletjs.com/) plugin. Leaflet
is an open-source library that provides the basic ability to embed a map, like
a MapBox map or a map from OpenStreetMap, into a page. [The Leaflet API](http://leafletjs.com/reference.html)
handles much of the fundamental operations of using maps, so this API documentation is
meant to be used in conjunction with the [Leaflet](http://leafletjs.com/reference.html) API
reference.

The MapBox API includes Leaflet and makes it easier to integrate Leaflet with MapBox's
maps and services.

## Getting Started with the API

Here's a simple page that you can set up with MapBox.js:

    <html>
    <head>
      <link href='http://api.tiles.mapbox.com/mapbox.js/v1.0.0beta0.0/mapbox.css' rel='stylesheet' />
      <!--[if lte IE 8]>
        <link href='http://api.tiles.mapbox.com/mapbox.js/v1.0.0beta0.0/mapbox.ie.css' rel='stylesheet' />
      <![endif]-->
      <script src='http://api.tiles.mapbox.com/mapbox.js/v1.0.0beta0.0/mapbox.js'></script>
    </head>
    <body>
      <div id='map' class='dark'></div>
      <script type='text/javascript'>
      var map = L.mapbox.map('map', 'examples.map-y7l23tes')
          .setView([37.9, -77], 5);
      </script>
    </body>
    </html>

The necessary Javascript and CSS files for the map are hosted on MapBox's servers, so they're
served from a worldwide content-distribution network. There's no API key required to include
the Javascript API - you'll identify with MapBox's services simply by using your own custom
maps.

## Reading this Documentation

This documentation is organized by _methods_ in the Javascript API. Each method
is shown with potential arguments, and their types. For instance, the `setFilter`
method on `L.mapbox.markerLayer` is documented as:

    markerLayer.setFilter(filter: function)

The format `filter: function` means that the single argument to `setFilter`, a filter
function, should be a Javascript function. Other kinds of arguments include
`object`, `string`, or `Element`.

When the API has a Javascript constructor function that returns an object, the constructor
is documented with its full name and the functions on the object are named with just
the type of the object. For instance, `L.mapbox.markerLayer` documents a function that
returns a layer for markers. The methods on that object are then documented as
`markerLayer.setFilter`, `markerLayer.getGeoJSON`, and so on.

# Map

## L.mapbox.map(element: Element, id: string | url: string | tilejson: object, [options: object])

Create and automatically configure a map with layers, markers, and
interactivity.

_Arguments_:

The first argument is required and must be the id of an element, or a DOM element
reference.

The second argument is required and must be:

* An `id` string `examples.map-foo`
* A URL to TileJSON, like `http://a.tiles.mapbox.com/v3/examples.map-0l53fhk2.json`
* A TileJSON object, from your own Javascript code

The third argument is optional. If provided, it is the same options
as provided to [L.Map](http://leafletjs.com/reference.html#map-options)
with the following additions:

* `tileLayer` (boolean). Whether or not to add a `L.mapbox.tileLayer` based on
  the TileJSON. Default: `true`.
* `markerLayer` (boolean). Whether or not to add a `L.mapbox.markerLayer` based on
  the TileJSON. Default: `true`.
* `gridLayer` (boolean). Whether or not to add a `L.mapbox.gridLayer` based on
  the TileJSON. Default: `true`.
* `legendControl` (boolean). Whether or not to add a `L.mapbox.legendControl`.
  Default: `true`.

# Layers

## L.mapbox.tileLayer(id: string | url: string | tilejson: object, [options: object])

You can add a tiled layer to your map with `L.mapbox.tileLayer()`, a simple
interface to layers from MapBox and elsewhere.

_Arguments_:

The first argument is required and must be:

* An `id` string `examples.map-foo`
* A URL to TileJSON, like `http://a.tiles.mapbox.com/v3/examples.map-0l53fhk2.json`
* A TileJSON object, from your own Javascript code

The second argument is optional. If provided, it is the same options
as provided to [L.TileLayer](http://leafletjs.com/reference.html#tilelayer)
with one addition:

* `retinaVersion`, if provided, is an alternative value for the first argument
  to `L.mapbox.tileLayer` which, if retina is detected, is used instead.

_Example_:

    // the second argument is optional
    var layer = L.mapbox.tileLayer('examples.map-20v6611k');

    // you can also provide a full url to a tilejson resource
    var layer = L.mapbox.tileLayer('http://a.tiles.mapbox.com/v3/examples.map-0l53fhk2.json');

    // if provided,you can support retina tiles
    var layer = L.mapbox.tileLayer('examples.map-20v6611k', {
        detectRetina: true,
        // if retina is detected, this layer is used instead
        retinaVersion: 'examples.map-zswgei2n'
    });

_Returns_ a `L.mapbox.tileLayer` object.


### tileLayer.loadURL(url: string, [callback: function])

Load tiles from a map with its tiles described by a TileJSON object at the
given `url`. If a callback function are provided as the second argument, it's
called after the request completes and the changes are applied to the layer.

_Arguments_:

1. `string` a map id

_Returns_: the layer object

### tileLayer.loadID(id: string, [callback: function])

Load tiles from a map with the given `id` on MapBox. If a callback function
are provided as the second argument, it's called after the request completes
and the changes are applied to the layer.

_Arguments_:

1. `string` a map id

_Returns_: the layer object

### tileLayer.setTileJSON(tilejson: object)

Set the TileJSON object that determines this layer's tile source, zoom bounds
and other metadata

_Arguments_:

1. `object` a TileJSON object

_Returns_: the TileJSON object

### tileLayer.getTileJSON()

Returns this layer's TileJSON object which determines its tile source,
zoom bounds and other metadata.

_Arguments_: none

_Returns_: the TileJSON object

### tileLayer.setFormat(format: string)

Set the image format of tiles in this layer. You can use lower-quality tiles
in order to load maps faster

_Arguments_:

1. `string` an image format. valid options are: 'png', 'png32', 'png64', 'png128', 'png256', 'jpg70', 'jpg80', 'jpg90'

_Returns_: the layer object

## L.mapbox.gridLayer(id: string | url: string | tilejson: object, [options: object])

An `L.mapbox.gridLayer` loads [UTFGrid](http://mapbox.com/developers/utfgrid/) tiles of
interactivity into your map, which you can easily access with `L.mapbox.gridControl`.

_Arguments_:

The first argument is required and must be:

* An `id` string `examples.map-foo`
* A URL to TileJSON, like `http://a.tiles.mapbox.com/v3/examples.map-0l53fhk2.json`
* A TileJSON object, from your own Javascript code

_Example_:

    // the second argument is optional
    var layer = L.mapbox.gridLayer('examples.map-20v6611k');

_Returns_ a `L.mapbox.gridLayer` object.


### gridLayer.loadURL(url: string, [callback: function])

Load tiles from a map with its tiles described by a TileJSON object at the
given `url`. If a callback function are provided as the second argument, it's
called after the request completes and the changes are applied to the layer.

_Arguments_:

1. `string` a map id

_Returns_: the layer object

### gridLayer.loadID(id: string, [callback: function])

Load tiles from a map with the given `id` on MapBox. If a callback function
are provided as the second argument, it's called after the request completes
and the changes are applied to the layer.

_Arguments_:

1. `string` a map id

_Returns_: the layer object

### gridLayer.getTileJSON()

Returns this layer's TileJSON object which determines its tile source,
zoom bounds and other metadata.

_Arguments_: none

_Returns_: the TileJSON object

### gridLayer.setTileJSON(tilejson: object)

Set the TileJSON object that determines this layer's tile source, zoom bounds
and other metadata

_Arguments_:

1. `object` a TileJSON object

_Returns_: the TileJSON object

### gridLayer.getData(latlng: LatLng, callback: function)

Load data for a given latitude, longitude point on the map, and call the callback
function with that data, if any.

_Arguments_:

1. `latlng` an L.LatLng object
2. `callback` a function that is called with the grid data as an argument

_Returns_: the L.mapbox.gridLayer object

## L.mapbox.markerLayer(id: string | url: string | tilejson: object, [options: object])

`L.mapbox.markerLayer` provides an easy way to integrate [GeoJSON](http://www.geojson.org/)
from MapBox and elsewhere into your map.

_Arguments_:

1. required and must be:

* An `id` string `examples.map-foo`
* A URL to TileJSON, like `http://a.tiles.mapbox.com/v3/examples.map-0l53fhk2.json`
* A GeoJSON object, from your own Javascript code

The second argument is optional. If provided, it is the same options
as provided to [L.FeatureGroup](http://leafletjs.com/reference.html#featuregroup)
with one addition:

_Example_:

    var markerLayer = L.mapbox.markerLayer(geojson)
        .addTo(map);

_Returns_ a `L.mapbox.markerLayer` object.

### markerLayer.loadURL(url: string, [callback: function])

Load tiles from a map with its tiles described by a TileJSON object at the
given `url`. If a callback function are provided as the second argument, it's
called after the request completes and the changes are applied to the layer.

_Arguments_:

1. `string` a map id

_Returns_: the layer object

### markerLayer.loadID(id: string, [callback: function])

Load tiles from a map with the given `id` on MapBox. If a callback function
are provided as the second argument, it's called after the request completes
and the changes are applied to the layer.

_Arguments_:

1. `string` a map id

_Returns_: the layer object

### markerLayer.setFilter(filter: function)

Sets the filter function for this data layer.

_Arguments_:

1. a function that takes GeoJSON features and
  returns true to show and false to hide features.

_Example_:

    var markerLayer = L.mapbox.markerLayer(geojson)
        // hide all markers
        .setFilter(function() { return false; })
        .addTo(map);

_Returns_ the markerLayer object.

### markerLayer.getFilter()

Gets the filter function for this data layer.

_Arguments_: none

_Example_:

    var markerLayer = L.mapbox.markerLayer(geojson)
        // hide all markers
        .setFilter(function() { return false; })
        .addTo(map);

    // get the filter function
    var fn = markerLayer.getFilter()

_Returns_ the filter function.

### markerLayer.setGeoJSON(geojson: object)

Set the contents of a markers layer: run the provided
features through the filter function and then through the factory function to create elements
for the map. If the layer already has features, they are replaced with the new features.
An empty array will clear the layer of all features.

_Arguments:_

* `features`, an array of [GeoJSON feature objects](http://geojson.org/geojson-spec.html#feature-objects),
  or omitted to get the current value.

_Returns_ the markerLayer object

### markerLayer.getGeoJSON()

Get the contents of this layer as GeoJSON data.

_Arguments:_ none

_Returns_ the GeoJSON represented by this layer

# Geocoding

## L.mapbox.geocoder(id: string | url: string)

A low-level interface to geocoding, useful for more complex uses and reverse-geocoding.

1. (required) must be:

* An `id` string `examples.map-foo`
* A URL to TileJSON, like `http://a.tiles.mapbox.com/v3/examples.map-0l53fhk2.json`

_Returns_ a `L.mapbox.geocoder` object.

### geocoder.query(queryString: string, callback: function)

Queries the geocoder with a query string, and returns its result, if any.

_Arguments_:

1. (required) a query, expressed as a string, like 'Arkansas'
2. (required) a callback

The callback is called with arguments

1. An error, if any
2. The result. This is an object with the following members:

        { results: // raw results
        latlng: // a map-friendly latlng array
        bounds: // geojson-style bounds of the first result
        lbounds: // leaflet-style bounds of the first result
        }

_Returns_: the geocoder object. The return value of this function is not useful - you must use a callback to get results.

### geocoder.reverseQuery(location: object, callback: function)

Queries the geocoder with a location, and returns its result, if any.

_Arguments_:

1. (required) a query, expressed as an object:

         [lon, lat] // an array of lon, lat
         { lat: 0, lon: 0 } // a lon, lat object
         { lat: 0, lng: 0 } // a lng, lat object

The first argument can also be an array of objects in that
form to geocode more than one item.

2. (required) a callback

The callback is called with arguments

1. An error, if any
2. The result. This is an object of the raw result from MapBox.

_Returns_: the geocoder object. The return value of this function is not useful - you must use a callback to get results.

# Controls

## L.mapbox.legendControl()

A map control that shows legends added to maps in MapBox. Legends are auto-detected from active layers.

_Arguments_:

1. (optional) an options object. Beyond the default options for map controls,
   this object has one special parameter:

* `sanitizer`: A function that accepts a string containing legend data, and returns a
  sanitized result for HTML display. The default will remove dangerous script content,
  and is recommended.

_Returns_: a `mapbox.Legend` object.

## L.mapbox.gridControl()

Interaction is what we call interactive parts of maps that are created with
the powerful [tooltips & regions system](http://mapbox.com/tilemill/docs/crashcourse/tooltips/)
in TileMill. Under the hood, it's powered by
the [open UTFGrid specification.](https://github.com/mapbox/utfgrid-spec).

_Arguments_:

* The first argument must be a layer created with `L.mapbox.gridLayer()`
* The second argument can be an options object. Valid options are:

* `sanitizer`: A function that accepts a string containing interactivity data, and returns a
  sanitized result for HTML display. The default will remove dangerous script content,
  and is recommended.
* `template`: A string in the [Moustache](http://mustache.github.io/) template
  language that will be evaluated with data from the grid to produce HTML for the
  interaction.
* `mapping`: an object of the types of interaction showed on each interaction. The default is

        mapping: {
          mousemove: ['teaser'],
          click: ['full'],
          mouseout: [function() { return ''; }]
        }

Each mapping is from an event type, like `mousemove`, to an array of options
to try. To fall-back the `teaser` formatter to `full`, one could write
`['teaser', 'full']`.

_Returns_: a `mapbox.gridControl` object.

## L.mapbox.geocoderControl(id: string | url: string)

Adds geocoder functionality as well as a UI element to a map. This uses
the [MapBox Geocoding API](http://mapbox.com/developers/api/#geocoding).

This function is currently in private beta:
[contact MapBox](http://mapbox.com/about/contact/) before using this functionality.

_Arguments_:

1. (required) either:

* An `id` string `examples.map-foo`
* A URL to TileJSON, like `http://a.tiles.mapbox.com/v3/examples.map-0l53fhk2.json`

_Example_

    var map = L.map('map')
        .setView([37, -77], 5)
        .addControl( L.mapbox.geocoder('examples.map-vyofok3q'));

_Returns_ a `L.mapbox.geocoderControl` object.

### geocoderControl.setURL(url: string)

Set the url used for geocoding.

_Arguments_:

1. a geocoding url

_Returns_: the geocoder control object

### geocoderControl.setID(id: string)

Set the map id used for geocoding.

_Arguments_:

1. a map id to geocode from

_Returns_: the geocoder control object

### geocoderControl.setTileJSON(tilejson: object)

Set the TileJSON used for geocoding.

_Arguments_:

1. A TileJSON object

_Returns_: the geocoder object

### geocoderControl.setErrorHandler(errorhandler: function)

Set the function called if a geocoding request returns an error.

_Arguments_:

1. a function that takes an error object - typically an XMLHttpRequest, and
   handles it.

_Returns_: the geocoder control object

### geocoderControl.getErrorHandler()

Returns the current function used by this geocoderControl for error handling.

_Arguments_: none

_Returns_: the geocoder control's error handler

# Markers

## L.mapbox.marker.icon(feature: object)

A core icon generator used in `L.mapbox.marker.style`

_Arguments_:

1. A GeoJSON feature object

_Returns_:

A `L.Icon` object with custom settings for `iconUrl`, `iconSize`, `iconAnchor`,
and `popupAnchor`.

## L.mapbox.marker.style(feature: object | latlon: object)

An icon generator for use in conjunction with `pointToLayer` to generate
markers from the [MapBox Markers API](http://mapbox.com/developers/api/#markers)
and support the [simplestyle-spec](https://github.com/mapbox/simplestyle-spec) for
features.

_Arguments_:

1. A GeoJSON feature object
2. The latitude, longitude position of the marker

_Examples_:

    L.geoJson(geoJson, {
        pointToLayer: L.mapbox.marker.style,
    });

_Returns_:

A `L.Marker` object with the latitude, longitude position and a styled marker

# Theming

## Dark theme

Mapbox.js implements a simple, light style on all interaction elements. A dark theme
is available by applying `class="dark"` to the map div.

_Example_:

    <div id="map" class="dark"></div>

