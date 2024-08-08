<script setup>

import {onMounted, ref} from 'vue';
import L from 'leaflet';

const lat = ref(0);
const lng = ref(0);
const map = ref();
const mapContainer = ref();

onMounted(() => {
  map.value = L.map(mapContainer.value).setView([32.5317397, -117.0195290], 13);

  L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
}).addTo(map.value);
})

function getLocation(){
    if(navigator.geolocation){
        navigator.geolocation.watchPosition((position) => {
          lat.value = position.coords.latitude;
          lng.value = position.coords.longitude;
          map.value.setView([lat.value, lng.value], 13);

          L.marker([lat.value, lng.value],{draggable : true})
          .addTo(map.value)
          .on("dragend",(event)=>{
            console.log(event);
          });
      })
    }
}



</script>

<template>
  <button @click="getLocation()">Get Location</button>
  {{ lat }} , {{ lng }}

  <div ref="mapContainer" style="width: 40rem;height:40rem"></div>
</template>

<style scoped>

</style>
