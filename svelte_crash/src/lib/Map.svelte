<script>
  import { onMount, onDestroy } from 'svelte';
  import { browser } from '$app/environment';
  import {ikotRoutePoints, ikotEveningRoutePoints, tokiRoutePoints, } from './jeepRoutes.js'

  let mapElement;
  let map;

  onMount(async () => {
    if(browser) {



// import leaflet
const leaflet = await import('leaflet');

// map initialization
let mapOptions = {
  center:[14.65504,121.06871],
  zoom:15
}

map = leaflet.map(mapElement, mapOptions)

leaflet.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
}).addTo(map);

leaflet.marker([14.654787833877382, 121.06231633947266]).addTo(map)




// Jeeps logic start
//----------------------------------------------------------------
class Jeep {
  constructor(map, route, index) {
    this.map = map
    this.route = route
    this.index = index
    this.marker = new L.Marker(this.route[0])
    // console.log(`${index}`);
  }

  usad() {
    // console.log(`usad, index = ${this.index}`);
    this.marker.remove(this.map)
    this.index += 1
    this.marker = new L.Marker(this.route[ (this.index ) %(this.route.length)]);
    this.marker.addTo(this.map);
  }
}


// setup (1 lang muna ginawa ko sa driverNum)
let driverNum = 1;
let coordinates = [];
let markers = [];//(nakamap dito yung kada isang markers para pwedeng maremove at update)
for (let i=0; i<driverNum; i++){
  coordinates[i] = [];
}

function addRoutes(map) {
  var ikotRoute = L.polyline(ikotRoutePoints, {color: 'yellow'}).addTo(map);
  var ikotEveningRoute = L.polyline(ikotEveningRoutePoints, {color: 'violet'}).addTo(map);
  var tokiRoute = L.polyline(tokiRoutePoints, {color: 'orange'}).addTo(map);

  var jeepRoutes = {
    "Ikot" : ikotRoute,
    "Ikot(Night)" : ikotEveningRoute,
    "Toki" : tokiRoute,
  }
  var layerControl = L.control.layers(null, jeepRoutes).addTo(map);

}


function displayMap() {
  let layer = new L.TileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png');
  map.addLayer(layer);

  //Make Jeeps that move around
  let Jeep1 = new Jeep(map, tokiRoutePoints, 500);  
  setInterval(function () {Jeep1.usad()}, 60);   
  let Jeep2 = new Jeep(map, ikotEveningRoutePoints, 100);  
  setInterval(function () {Jeep2.usad()}, 50);    
  let Jeep3 = new Jeep(map, ikotRoutePoints, 1000);  
  setInterval(function () {Jeep3.usad()}, 55);    

  addRoutes(map);

  map.on('click', function(ev){
    var latlng = map.mouseEventToLatLng(ev.originalEvent);
    // console.log(latlng.lat + ', ' + latlng.lng);
  });

}



displayMap()





// --------------------------------------------------------------------------------
// Jeeps logic end














    }
  });

  onDestroy(async () => {
    if(map) {
      console.log('Unloading Leaflet map.');
      map.remove();
    }
  });
</script>


<main>
  <div bind:this={mapElement}></div>
</main>

<style>
  /* leaflet css */
  @import 'leaflet/dist/leaflet.css';
  main div {
      height: 800px;
  }
</style>
