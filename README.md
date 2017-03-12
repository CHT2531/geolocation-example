#The Geolocation API and Google Maps

>The following provides an overview of what we can do with the geolocation API and the google maps API. It is an overview only, you should investigate https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/Using_geolocation and https://developers.google.com/maps/documentation/javascript/tutorial for complete info.

* Open index.html in a web browser. Open the console. Note that your current longitude and latitude are displayed in the console. Take a few minutes to understand the JavaScript code. 

##Creating a map

Look in the HTML page. In the head of the document there is a link to the google maps library. In the body of the document there is a div element with an id of map.

* In the JavaScript file add a global map variable at the top of the script

```
var map;
```

* In the JavaScript file add the following function:

```
function showMap(latLng){
    map = new google.maps.Map(document.getElementById('map'), 
        {
          center: latLng,
          mapTypeId: google.maps.MapTypeId.SATELLITE,
          zoom: 18,
           });
}
```

* Add the following code inside the *itWorks* function so that a map will be displayed if the geolocation API is successful.

```
var userLatLng = new google.maps.LatLng({lat: latitude, lng: longitude});
showMap(userLatLng);
```

* Note that we are using the user's current location to centre the Google map.

* Have a look at https://developers.google.com/maps/documentation/javascript/controls see if you can adjust properties of the map e.g. the zoom level, the visibility of the controls etc. 

##Adding a Marker
* Add the following function to the js file

```
function addMarker(latLng, label){
    var marker = new google.maps.Marker({
            position: latLng,
            map: map,
            label:label
          });
}
```
* Call this function from *itWorks* e.g.

```
addMarker(userLatLng,"A");
```

* Test this works
* Try adding another marker. Add the following code, also in the *itWorks* function.

```
var latLng = new google.maps.LatLng({lat: 53.622285, lng: -1.772280});
addMarker(latLng,"B")
```

* Test this works. You might have to zoom the map out so you can see the marker (it should be on Castle Hill).

##Centering the Map on Markers

* We can centre the map based on the location of a number of different markers. To do this, declare the following variable at the start of the JavaScript file:

```
var latlngbounds = new google.maps.LatLngBounds();
```

* Now modify the *addMarker* function so that it looks like the following:

```
function addMarker(latLng, label){
    var marker = new google.maps.Marker({
            position: latLng,
            map: map,
            label:label
          });
    latlngbounds.extend(latLng);
    map.setCenter(latlngbounds.getCenter());
    map.fitBounds(latlngbounds);
}
```

* *latlngBounds* defines a rectangular area on the map.
* As we add co-ordinates, *latlngbounds.extend(latLng)*, we change the edges of this rectangle.
* We can then use this LatLngBounds object to specify the center, *map.setCenter*, and zoom, *map.fitBounds*, for the map. See https://developers.google.com/maps/documentation/javascript/reference#LatLngBounds for more info.
* Again test this works.

##Calculating Distances
In this application it would be good to tell the user how far away Castle Hill is from their current location. We can load an additional library, the geometry library, that can help us do this (https://developers.google.com/maps/documentation/javascript/libraries). 

* In the HTML page, modify the link to the maps library by adding *libraries=geometry* in the query string. The URL should now look like:

```
 <script src="https://maps.googleapis.com/maps/api/js?libraries=geometry"></script>
```
* Add the following at the end of the *itWorks* function:

```
var distance = google.maps.geometry.spherical.computeDistanceBetween(userLatLng,latLng);
console.log(distance/1000+"km");
```

* Test this works. The distance between your current location and Castle Hill should be displayed in the console. 