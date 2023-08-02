<template>
  <header class="header-container">
    <h1 class="primary-title">IP Address Tracker</h1>
    <form class="header__form" @submit.prevent="getIpInfo">
      <input
        type="text"
        class="input"
        placeholder="Search for any IP address or domain"
        v-model="ipQuery"
      />
      <button type="submit" class="btn">
        <img src="../assets/icon-arrow.svg" alt="Arrow Icons" />
      </button>
    </form>
    <IpInfo v-if="ipDetails" :ipDetails="ipDetails" />
  </header>

  <div id="map"></div>
</template>

<script setup>
// Imports
import { ref, onMounted } from "vue";
import IpInfo from "./IpInfo.vue";
import markerIcon from "../assets/icon-location.svg";
import leaflet from "leaflet";
import axios from "axios";

// Variables
const ipQuery = ref("");
const ipDetails = ref(null);
let map;

// Functions
onMounted(() => {
  // Map Initialisation
  map = leaflet.map("map").setView([51.505, -0.09], 13);

  // Map Tile Layer
  leaflet
    .tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
      maxZoom: 19,
      attribution: "© OpenStreetMap",
    })
    .addTo(map);

  getIpInfo();
});

const getIpInfo = async () => {
  try {
    const IpData = await axios.get(
      `https://geo.ipify.org/api/v2/country,city?apiKey=${
        import.meta.env.VITE_GEOLOCATION_API
      }&ipAddress=${ipQuery.value}`
    );

    // IP Details Results
    const result = IpData.data;

    ipDetails.value = {
      IpAddress: result.ip,
      city: result.location.city,
      timeZone: result.location.timezone,
      isp: result.isp,
      lat: result.location.lat,
      lng: result.location.lng,
    };

    // Map Location
    map.setView([ipDetails.value.lat, ipDetails.value.lng], 13);

    // Map Marker
    const marker = leaflet.icon({
      iconUrl: markerIcon,
      iconSize: [32, 40],
      iconAnchor: [22, 94],
      popupAnchor: [-3, -76],
    });

    leaflet
      .marker([ipDetails.value.lat, ipDetails.value.lng], { icon: marker })
      .addTo(map);
  } catch (error) {
    console.log(error);
  }
};
</script>
