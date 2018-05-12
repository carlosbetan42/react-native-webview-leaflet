# React Native Webview Leaflet

## A Leaflet map component with no native code for React Native applications.

### Why use this?

This component is useful if you want to display HTML elements on an interactive map. Since the elements are just HTML items, you can use SVG's, emojis, text, images, etc., and they can even be animated.

### Why not use this?

This component may not be useful if you expect your users to tap on the map quickly since there is a bug in React Native that prevents the map from responding to multiple taps that occure less than ~1.5 seconds apart.

[![npm](https://img.shields.io/npm/v/react-native-webview-leaflet.svg)](https://www.npmjs.com/package/react-native-webview-leaflet)
[![npm](https://img.shields.io/npm/dm/react-native-webview-leaflet.svg)](https://www.npmjs.com/package/react-native-webview-leaflet)
[![npm](https://img.shields.io/npm/dt/react-native-webview-leaflet.svg)](https://www.npmjs.com/package/react-native-webview-leaflet)
[![npm](https://img.shields.io/npm/l/react-native-webview-leaflet.svg)](https://github.com/react-native-component/react-native-webview-leaflet/blob/master/LICENSE)

![Image](https://thumbs.gfycat.com/CraftyKnobbyApe-size_restricted.gif)

## Installation

Install using npm or yarn like this:

```
npm install --save react-native-webview-leaflet
```

or

```
yarn add react-native-webview-leaflet
```

## Usage

and import like so

```
import WebViewLeaflet from 'react-native-webview-leaflet';
```

Add the following component to your code with optional callback functions for leaflet.js map events.

```
 <WebViewLeaflet
  currentPosition={this.state.coords}
  locations={this.state.locations}
  onMapClicked={this.onMapClicked}
  onMarkerClicked={this.onMarkerClicked}
  onWebViewReady={this.onWebViewReady}
  panToLocation={false}
  zoom={10}
  showZoomControls={true}
  onZoomLevelsChange={this.onZoomLevelsChange}
  onResize={this.onResize}
  onUnload={this.onUnload}
  onViewReset={this.onViewReset}
  onLoad={this.onLoad}
  onZoomStart={this.onZoomStart}
  onMoveStart={this.onMoveStart}
  onZoom={this.onZoom}
  onMove={this.onMove}
  onZoomEnd={this.onZoomEnd}
  onMoveEnd={this.onMoveEnd}
  showMapAttribution={true}
  defaultIconSize={[16, 16]}
  onCurrentPositionClicked={this.onCurrentPositionClicked}
  currentPositionMarkerStyle= {{
    icon: '🍇',
    animation: {
      name: 'beat',
      duration: 0.25,
      delay: 0,
      interationCount: 'infinite',
      direction: 'alternate'
    },
    size: [36, 36]
  }}
/>
```

|
This component accepts the following props:

| Name                       | Required | Description                                                                                                                                                                                                                                                                                                                                    |
| -------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| currentPosition            | no       | The center of the map in an array fo the form [latitude, longitude]                                                                                                                                                                                                                                                                            |
| onMapClicked               | no       | a function that will be called when the map is tapped. It will receive the location of the tap in an array of the form [latitude, longitude]                                                                                                                                                                                                   |
| onMarkerClicked            | no       | a function that will be called when a marker is tapped. It will receive the ID of the marker as shown in the location.id object below.                                                                                                                                                                                                         |
| onWebViewReady             | no       | a function that is called when the map is ready for display. For example, it can be used to hide an activity indicator                                                                                                                                                                                                                         |
| locations                  | no       | see below                                                                                                                                                                                                                                                                                                                                      |
| zoom                       | no       | initial map zoom level. "Lower zoom levels means that the map shows entire continents, while higher zoom levels means that the map can show details of a city." (source: leaflet website)                                                                                                                                                      |
| showMapAttribution         | no       | Removes the attribution link from the map to prevent the user from inadvertently clicking on it. This value defaults to true. Remember to give proper credit by displaying attribution information somewhere in you application. The standard OSM attribution is OSM Mapnik <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a> |
| defaultIconSize            | no       | Array with 2 integers [width, height] denoting the map marker icon size. Default is [36, 36] for 36x36 React Native density-independent pixels|
| onCurrentPositionClicked   | no       | Function that will be called with the marker depicting the device's current location is clicked.                                                                                                                                                                                                                                                |
| currentPositionMarkerStyle | no       | An object describing the own location marker's icon, animation, and size. The default current position marker object is shown below.  Note that the current position icon can be sized independently of individual markers.|


## Setting Map Bounds
You can also set the map's bounds by setting a ref to the WebViewLeaflet component and calling it's `fitBounds` method with accepts two arguments.  The first argument is an array that contains two points describing the maximum and minimun latitude and longitued of the desired bounds.  The second argument is a array containing the bound's horizontal and vertical padding.

Set a ref in the WebViewLeaflet component like so: 
~~~
<WebViewLeaflet
  ref={(component) => (this.webViewLeaflet = component)}
  other properties
/>
~~~
Then call the component's `fitBounds` method:
~~~
this.webViewLeaflet.fitBounds([
  [bounds.minLat, bounds.minLng],
  [bounds.maxLat, bounds.maxLng]
], [75, 75]);
~~~


### Default Current Position Marker Object
~~~
icon: '❤️',
    animation: {
      name: 'beat',
      duration: 0.25,
      delay: 0,
      interationCount: 'infinite',
      direction: 'alternate'
    },
    size: [36, 36]
~~~

Additonally, there are several optional props that allow you to fire a callback function in response to leaflet's 11 "Map state change events".  The prop corresponding to each event is shown in the above sample.  More information on map state change events can be found [here](http://leafletjs.com/reference-1.3.0.html#map-event).

Each of these callbacks will receive an object containing the maps bounds, center, and zoom level in the following format:

~~~
{
  bounds:{
    \_northEast:{lat: 36.961963124877656, lng: -75.99380493164064}
    \_southWest:{lat: 36.590171021747466, lng: -76.55960083007814}
  },
  center:{lat: 36.7762924811868, lng: -76.27670288085939},
  zoom:10
}
~~~


### Marker Locations
The locations property expects an array of objects with the following format:

~~~
{
  id: uuidV1(), // The ID attached to the marker. It will be returned when onMarkerClicked is called
  coords: bounce, // Latitude and Longitude of the marker
  icon: '🍇', // HTML element that will be displayed as the marker.  It can also be text or an SVG string.

  // The child object, "animation", controls the optional animation that will be attached to the marker.
  // See below for a list of available animations
  animation: {
    name: animations[Math.floor(Math.random() * animations.length)],
    duration: Math.floor(Math.random() _ 3) + 1,
    delay: Math.floor(Math.random()) _ 0.5,
    interationCount
  }
  // optional size for this individual icon
  // will default to the WebViewLeaflet `defaultIconSize` property if not provided
  size: [64, 64],
}
~~~
### Available Animations
Animations for "bounce", "fade", "pulse", "jump", "waggle", "spin", and "beat" cane be specified in the animation.name property of an individual location.

### Animation Information
Animations are kept in the file [markers.css](https://github.com/reggie3/react-native-webview-leaflet/blob/master/web/markers.css)  They are just keyframe animations like this:
```
@keyframes spin {
  50% {
    transform: rotateZ(-20deg);
    animation-timing-function: ease;
  }
  100% {
    transform: rotateZ(360deg);
  }
}
```
### The Format of the Object Received by onMove, onMoveEnd, onZoom, and onZoomEnd Callback Functions
```
{
  center: {lat, lng},
  bounds: {
    \_northEast: {lat, lng},
    \_southWest: {lat, lng},
  }
}
```
## Changelog

### 0.4.0
* Breaking Change: Deprecated `mapCenterCoord` property and replaced with `currentPosition` property
* Added custom map icon size option for individual map marker icons
* Added default icon size option for 
* Added own position marker click event
* Added support for SVG markers
* Added custom current position marker option
* Added functionality to fit map to bounds

### 0.3.38
* Fixed coordinate validation bug
### 0.3.0
* Added props for each leaflet map state change event (resize, load, move, etc.)
### 0.2.7
* Added inital set of tests
### 0.2.6
* Added props for zoom, zoomEnd, move, and moveEnd listeners.
### 0.2.0
* Removed requirement to download JavaScript files from GitHub in order for the package to work.  JavaScript files are now inline with the HTML which enables the package to work without an Internet connection.
* Added getMapCallback
### 0.0.86
* Added ability to set map zoom level on initial render
### 0.0.85
* First version suitable for install using npm or yarn


## LICENSE

MIT
```
