<template>

<div >
    <l-map
    
   :center="center"
   :zoom="zoom"
   class="map"
   ref="map"
   @update:center="centerUpdated" 
   @update:zoom="zoomUpdated" 
    >
    <l-tile-layer
    :url="url"
    >

    </l-tile-layer>

    <l-marker :lat-lng="markerLatLng" :icon="icon" >
        <l-icon
          :icon-size="dynamicSize"
          :icon-anchor="dynamicAnchor"
          icon-url="marker.png" >
</l-icon>
    </l-marker>
   <!--  <l-tooltip>
      <h1>{{clnombreEntidad}}</h1>
      <h1>{{Geolocalizar.potencialPromedio}}</h1>
      <h1>{{Geolocalizar.alimentos}}</h1>
      <h1>{{Geolocalizar.calor}}</h1>
      <h1>{{Geolocalizar.elecMin}}</h1>
      <h1>{{Geolocalizar.elecMax}}</h1>
    </l-tooltip> -->
</l-map>
</div>
</template>
<script>

import L from 'leaflet';
import {LMap, LTileLayer, LMarker, LIcon} from 'vue2-leaflet';




export default {
   /*  beforeRouteEnter:(to,from,next)=>{
    console.log("[IN-component] beforeEnter guard");
    let isAuth = sessionStorage.getItem("username");
    let role = sessionStorage.getItem("rol");    
    return isAuth && role==='Especialista' ? next() : next("/login")  
  }, */
    name: 'Mapa',
    components:{
        LMap,
        LTileLayer, 
        LMarker,
        LIcon,
       
    },
    data (){
        return {
        url: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png',
        center: [21.93271597736655, -79.43706927182542],
        zoom: 20,
        
       
   
        markerLatLng: [21.93271597736655, -79.43706927182542],
        icon: L.icon({
        iconUrl: 'marker.png',
        iconSize: [7, 7],
        iconAnchor: [7, 7]
      }),
      staticAnchor: [16, 37],
      customText: 'Foobar',
      iconSize: 64
        }
    },
    computed: {
    dynamicSize () {
      return [this.iconSize, this.iconSize * 1.15];
    },
    dynamicAnchor () {
      return [this.iconSize / 2, this.iconSize * 1.15];
    }
  },
    methods:{
        centerUpdated(center){
            this.center = center;
            },
        zoomUpdated(zoom){
            this.zoom = zoom;
            },
    }
}
</script>
<style scoped>
.map{
    position: absolute;
    width: 100%;
    height: 100%;
    overflow: hidden;
}
</style>